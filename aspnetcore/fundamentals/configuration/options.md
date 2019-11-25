---
title: Шаблон параметров в ASP.NET Core
author: guardrex
description: Узнайте, как использовать шаблон параметров для представления групп связанных параметров в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/18/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 4192bab8acef7c4f7bdf1ac481c468cd0a835420
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239798"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="c7756-103">Шаблон параметров в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7756-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="c7756-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="c7756-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c7756-105">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="c7756-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="c7756-106">Когда [параметры конфигурации](xref:fundamentals/configuration/index) изолируются по сценарию в отдельных классах, в приложениях соблюдаются два важных принципа программной инженерии.</span><span class="sxs-lookup"><span data-stu-id="c7756-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="c7756-107">[Принцип изоляции интерфейсов или инкапсуляция](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation). Сценарии (классы), которые зависят от параметров конфигурации, зависят только от используемых ими параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="c7756-108">[Разделение ответственности](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns). Параметры для разных частей приложения не зависят друг от друга и не связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="c7756-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="c7756-109">В параметрах также предусмотрен механизм для проверки данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="c7756-110">Дополнительные сведения см. в разделе [Проверка параметров](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="c7756-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="c7756-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7756-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="c7756-112">Пакет</span><span class="sxs-lookup"><span data-stu-id="c7756-112">Package</span></span>

<span data-ttu-id="c7756-113">Пакет [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) неявно упоминается в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7756-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="c7756-114">Интерфейсы параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-114">Options interfaces</span></span>

<span data-ttu-id="c7756-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> используется для извлечения параметров и управления уведомлениями о параметрах для экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="c7756-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="c7756-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> поддерживается в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="c7756-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="c7756-117">уведомления об изменениях;</span><span class="sxs-lookup"><span data-stu-id="c7756-117">Change notifications</span></span>
* <span data-ttu-id="c7756-118">[именованные параметры](#named-options-support-with-iconfigurenamedoptions);</span><span class="sxs-lookup"><span data-stu-id="c7756-118">[Named options](#named-options-support-with-iconfigurenamedoptions)</span></span>
* <span data-ttu-id="c7756-119">[перезагружаемые конфигурации](#reload-configuration-data-with-ioptionssnapshot);</span><span class="sxs-lookup"><span data-stu-id="c7756-119">[Reloadable configuration](#reload-configuration-data-with-ioptionssnapshot)</span></span>
* <span data-ttu-id="c7756-120">объявление определенных параметров недействительными (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>).</span><span class="sxs-lookup"><span data-stu-id="c7756-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="c7756-121">Сценарии [пост-конфигурации](#options-post-configuration) позволяют задать или изменить параметры после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="c7756-122">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsFactory%601> отвечает за создание экземпляров параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="c7756-123">Он имеет единственный метод <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="c7756-124">При реализации по умолчанию принимаются все зарегистрированные интерфейсы <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>. Также сначала выполняются все основные настройки, а затем действия после конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="c7756-125">Она различает интерфейсы <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> и <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и вызывает только соответствующий интерфейс.</span><span class="sxs-lookup"><span data-stu-id="c7756-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="c7756-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> используется интерфейсом <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для записи экземпляров `TOptions` в кэш.</span><span class="sxs-lookup"><span data-stu-id="c7756-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="c7756-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> делает экземпляры параметров в мониторе недействительными, что приводит к повторному вычислению значений (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="c7756-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="c7756-128">Значения можно также вводить вручную с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="c7756-129">Метод <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> используется, если необходимо повторно создать все именованные экземпляры по требованию.</span><span class="sxs-lookup"><span data-stu-id="c7756-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="c7756-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> полезно использовать в сценариях, когда параметры должны заново вычисляться при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="c7756-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="c7756-131">Дополнительные сведения см. в разделе [Повторная загрузка данных конфигурации с помощью IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="c7756-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="c7756-132"><xref:Microsoft.Extensions.Options.IOptions%601> можно использовать, чтобы обеспечить поддержку определенных параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="c7756-133">Но <xref:Microsoft.Extensions.Options.IOptions%601> не поддерживается в описанных выше сценариях <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="c7756-134">Вы можете продолжать использовать <xref:Microsoft.Extensions.Options.IOptions%601> в существующих платформах и библиотеках, которые уже используют интерфейс <xref:Microsoft.Extensions.Options.IOptions%601>. В таком случае вам не потребуются сценарии, предоставляемые <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="c7756-135">Конфигурация общих параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-135">General options configuration</span></span>

<span data-ttu-id="c7756-136">Конфигурация общих параметров демонстрируется в примере &num;1 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="c7756-137">Класс параметров должен быть неабстрактным с открытым конструктором без параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="c7756-138">Приведенный ниже класс (`MyOptions`) имеет два свойства: `Option1` и `Option2`.</span><span class="sxs-lookup"><span data-stu-id="c7756-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="c7756-139">Задавать значения по умолчанию необязательно, однако конструктор класса в этом примере задает значение по умолчанию для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="c7756-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="c7756-140">Значение по умолчанию для `Option2` устанавливается путем прямой инициализации (*Models/MyOptions.cs*).</span><span class="sxs-lookup"><span data-stu-id="c7756-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="c7756-141">Класс `MyOptions` добавляется в контейнер службы с помощью интерфейса <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> и привязывается к конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="c7756-142">В следующей страничной модели применяется [внедрение зависимостей конструктора](xref:mvc/controllers/dependency-injection) с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для доступа к параметрам (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="c7756-143">В файле *appsettings.json* образца задаются значения для свойств `option1` и `option2`:</span><span class="sxs-lookup"><span data-stu-id="c7756-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="c7756-144">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="c7756-145">При использовании настраиваемого компонента <xref:System.Configuration.ConfigurationBuilder> для загрузки конфигурации параметров из файла настроек необходимо убедиться, что базовый путь задан правильно:</span><span class="sxs-lookup"><span data-stu-id="c7756-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="c7756-146">При загрузке конфигурации параметров из файла настроек с помощью <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> явно задавать базовый путь не требуется.</span><span class="sxs-lookup"><span data-stu-id="c7756-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="c7756-147">Настройка простых параметров с помощью делегата</span><span class="sxs-lookup"><span data-stu-id="c7756-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="c7756-148">Настройка простых параметров с помощью делегата демонстрируется в примере &num;2 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="c7756-149">Используйте делегат для задания значений параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-149">Use a delegate to set options values.</span></span> <span data-ttu-id="c7756-150">В образце приложения используется класс `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="c7756-151">В приведенном ниже коде в контейнер службы добавляется еще одна служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="c7756-152">Она использует делегат для настройки привязки к `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="c7756-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="c7756-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c7756-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="c7756-154">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="c7756-155">Поставщики конфигурации доступны в пакетах NuGet и применяются в порядке регистрации.</span><span class="sxs-lookup"><span data-stu-id="c7756-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="c7756-156">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c7756-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="c7756-157">При каждом вызове <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601> добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="c7756-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="c7756-158">В предыдущем примере значения `Option1` и `Option2` задаются в файле *appsettings.json*, однако значения `Option1` и `Option2` переопределяются настроенным делегатом.</span><span class="sxs-lookup"><span data-stu-id="c7756-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="c7756-159">Если включено более одной службы конфигурации, последний источник конфигурации *имеет приоритет* и определяет значение конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="c7756-160">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="c7756-161">Конфигурация подпараметров</span><span class="sxs-lookup"><span data-stu-id="c7756-161">Suboptions configuration</span></span>

<span data-ttu-id="c7756-162">Конфигурация подпараметров демонстрируется в примере &num;3 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="c7756-163">В приложениях должны создаваться классы приложений, относящиеся к определенным группам сценариев (классам).</span><span class="sxs-lookup"><span data-stu-id="c7756-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="c7756-164">Части приложения, которым требуются значения конфигурации, должны иметь доступ только к используемым ими значениям конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="c7756-165">При привязке параметров к конфигурации каждое свойство, относящееся к типу параметров, привязывается к ключу конфигурации в формате `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="c7756-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="c7756-166">Например, свойство `MyOptions.Option1` привязывается к ключу `Option1`, который считывается из свойства `option1` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="c7756-167">В приведенном ниже коде в контейнер службы добавляется третья служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="c7756-168">Она привязывает `MySubOptions` к разделу `subsection` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="c7756-169">Методу `GetSection` нужно пространство имен <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="c7756-169">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="c7756-170">В файле *appsettings.json* образца определяется член `subsection` с ключами для `suboption1` и `suboption2`.</span><span class="sxs-lookup"><span data-stu-id="c7756-170">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="c7756-171">Класс `MySubOptions` определяет свойства `SubOption1` и `SubOption2` для хранения значений параметров (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-171">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="c7756-172">Метод `OnGet` страничной модели возвращает строку со значениями параметров (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-172">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="c7756-173">Когда приложение выполняется, метод `OnGet` возвращает строку со значениями класса подпараметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-173">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="c7756-174">Внедрение параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-174">Options injection</span></span>

<span data-ttu-id="c7756-175">Внедрение параметров демонстрируется в примере 4 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-175">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="c7756-176">Внедрите <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в:</span><span class="sxs-lookup"><span data-stu-id="c7756-176">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="c7756-177">Страница Razor или представление MVC с директивой Razor [@inject](xref:mvc/views/razor#inject).</span><span class="sxs-lookup"><span data-stu-id="c7756-177">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="c7756-178">Модель страницы или представления.</span><span class="sxs-lookup"><span data-stu-id="c7756-178">A page or view model.</span></span>

<span data-ttu-id="c7756-179">Следующий пример из примера приложения внедряет <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в модель страницы (*Pages/index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-179">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="c7756-180">В примере приложения показано, как внедрить `IOptionsMonitor<MyOptions>` с директивой `@inject`.</span><span class="sxs-lookup"><span data-stu-id="c7756-180">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="c7756-181">Когда запускается приложение, значения параметров появляются на отображаемой странице:</span><span class="sxs-lookup"><span data-stu-id="c7756-181">When the app is run, the options values are shown in the rendered page:</span></span>

![Значения параметров Option1: value1_from_json и Option2: –1 загружаются из модели и путем внедрения в представление.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="c7756-183">Повторная загрузка данных конфигурации с помощью IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="c7756-183">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="c7756-184">Повторная загрузка данных конфигурации с помощью <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> демонстрируется в примере &num;5 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-184">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="c7756-185">С помощью <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> параметры вычисляются один раз на каждый запрос при обращении к ним и кэшируются на все время существования запроса.</span><span class="sxs-lookup"><span data-stu-id="c7756-185">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="c7756-186">Разница между `IOptionsMonitor` и `IOptionsSnapshot`:</span><span class="sxs-lookup"><span data-stu-id="c7756-186">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="c7756-187">`IOptionsMonitor` — это [одноэлементная служба](xref:fundamentals/dependency-injection#singleton), которая получает текущие значения параметров в любое время, что особенно полезно в одноэлементных зависимостях.</span><span class="sxs-lookup"><span data-stu-id="c7756-187">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="c7756-188">`IOptionsSnapshot` — это [служба с заданной областью действия](xref:fundamentals/dependency-injection#scoped), предоставляющая моментальный снимок параметров на момент создания объекта `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="c7756-188">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="c7756-189">Моментальные снимки параметров предназначены для использования с временными зависимостями и зависимостями с заданной областью действия.</span><span class="sxs-lookup"><span data-stu-id="c7756-189">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="c7756-190">В приведенном ниже примере демонстрируется создание экземпляра <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> после изменения файла *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="c7756-190">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="c7756-191">Несколько запросов к серверу возвращают константные значения, предоставляемые файлом *appsettings.json*, пока файл не изменится и конфигурация не загрузится повторно.</span><span class="sxs-lookup"><span data-stu-id="c7756-191">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="c7756-192">Ниже показаны начальные значения `option1` и `option2`, загруженные из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-192">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="c7756-193">Измените значения в файле *appsettings.json* на `value1_from_json UPDATED` и `200`.</span><span class="sxs-lookup"><span data-stu-id="c7756-193">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="c7756-194">Сохраните файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-194">Save the *appsettings.json* file.</span></span> <span data-ttu-id="c7756-195">Обновите окно браузера, чтобы увидеть обновленные значения параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-195">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="c7756-196">Поддержка именованных параметров в IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="c7756-196">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="c7756-197">Включение поддержки именованных параметров в <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> демонстрируется в примере &num;6 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-197">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="c7756-198">Поддержка *именованных параметров* позволяет приложению различать конфигурации именованных параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-198">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="c7756-199">В примере приложения именованные параметры должны быть объявлены с помощью элемента [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), который вызывает метод расширения [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="c7756-199">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="c7756-200">Пример приложения обращается к именованным параметрам с помощью метода <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="c7756-201">При запуске образца приложения возвращаются именованные параметры:</span><span class="sxs-lookup"><span data-stu-id="c7756-201">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="c7756-202">Значения `named_options_1` предоставляются из конфигурации, которая загружается из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="c7756-203">Значения `named_options_2` предоставляются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c7756-203">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="c7756-204">Делегатом `named_options_2` в `ConfigureServices` для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="c7756-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="c7756-205">Значение по умолчанию для `Option2` предоставляется классом `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="c7756-205">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="c7756-206">Настройка всех параметров с помощью метода ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="c7756-206">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="c7756-207">Настройте все экземпляры параметров с помощью метода <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="c7756-208">Приведенный ниже код настраивает `Option1` для всех экземпляров конфигурации с общим значением.</span><span class="sxs-lookup"><span data-stu-id="c7756-208">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="c7756-209">Добавьте следующий код в метод `Startup.ConfigureServices` вручную:</span><span class="sxs-lookup"><span data-stu-id="c7756-209">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="c7756-210">Если запустить образец приложения после добавления, результат будет следующим:</span><span class="sxs-lookup"><span data-stu-id="c7756-210">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="c7756-211">Все параметры являются именованными экземплярами.</span><span class="sxs-lookup"><span data-stu-id="c7756-211">All options are named instances.</span></span> <span data-ttu-id="c7756-212">Существующие экземпляры <xref:Microsoft.Extensions.Options.IConfigureOptions%601> считаются нацеленными на экземпляр `Options.DefaultName`, который имеет значение `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="c7756-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="c7756-213">Интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> также реализует интерфейс <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="c7756-214">Реализация <xref:Microsoft.Extensions.Options.IOptionsFactory%601> по умолчанию содержит логику для надлежащего использования каждого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="c7756-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="c7756-215">Именованный параметр `null` предназначен для всех именованных экземпляров, а не для какого-то определенного (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> и <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> следуют этому соглашению).</span><span class="sxs-lookup"><span data-stu-id="c7756-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="c7756-216">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="c7756-216">OptionsBuilder API</span></span>

<span data-ttu-id="c7756-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> используется для настройки экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="c7756-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="c7756-218">`OptionsBuilder` упрощает создание именованных параметров, так как он является единственным параметром для первоначального вызова `AddOptions<TOptions>(string optionsName)` и не должен появляться во всех последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="c7756-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="c7756-219">Проверка параметров и перегрузки `ConfigureOptions`, принимающие зависимости службы, доступны только через `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c7756-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="c7756-220">Использование служб внедрения зависимостей для настройки параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-220">Use DI services to configure options</span></span>

<span data-ttu-id="c7756-221">Вы можете получить доступ к другим службам, доступным в результате внедрения зависимостей при настройке параметров, двумя способами.</span><span class="sxs-lookup"><span data-stu-id="c7756-221">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="c7756-222">Передайте делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) в [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="c7756-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="c7756-223">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) предоставляет перегрузки метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), которые позволяют использовать до пяти служб для настройки параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-223">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="c7756-224">Создайте собственный тип, реализующий <xref:Microsoft.Extensions.Options.IConfigureOptions%601> или <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, и зарегистрируйте этот тип как службу.</span><span class="sxs-lookup"><span data-stu-id="c7756-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="c7756-225">Рекомендуем передать делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), так как создать службу сложнее.</span><span class="sxs-lookup"><span data-stu-id="c7756-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="c7756-226">Создание собственного типа эквивалентно тому, что делает платформа при использовании метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="c7756-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="c7756-227">При вызове метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) регистрируется временный универсальный интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, имеющий конструктор, который принимает указанные универсальные типы службы.</span><span class="sxs-lookup"><span data-stu-id="c7756-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="c7756-228">Проверка параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-228">Options validation</span></span>

<span data-ttu-id="c7756-229">Эта функция позволяет проверить параметры при их настройке.</span><span class="sxs-lookup"><span data-stu-id="c7756-229">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="c7756-230">Вызовите `Validate` с использованием метода проверки, который возвращает `true`, если параметры являются допустимыми, и `false`, если они являются недопустимыми:</span><span class="sxs-lookup"><span data-stu-id="c7756-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");

// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
}
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="c7756-231">В предыдущем примере для именованного экземпляра параметров задается `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="c7756-231">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="c7756-232">Экземпляр параметров по умолчанию — `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="c7756-232">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="c7756-233">Проверка выполняется при создании экземпляра параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-233">Validation runs when the options instance is created.</span></span> <span data-ttu-id="c7756-234">Экземпляр параметров гарантированно проходит проверку при первом обращении.</span><span class="sxs-lookup"><span data-stu-id="c7756-234">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7756-235">Проверка параметров не защищает от изменений, вносимых после исходной настройки и проверки параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-235">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="c7756-236">Метод `Validate` принимает `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="c7756-236">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="c7756-237">Чтобы полностью настроить проверку, реализуйте `IValidateOptions<TOptions>`, чтобы:</span><span class="sxs-lookup"><span data-stu-id="c7756-237">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="c7756-238">выполнить проверку нескольких типов параметров — `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`;</span><span class="sxs-lookup"><span data-stu-id="c7756-238">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="c7756-239">выполнить проверку, зависящую от другого типа параметра — `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`.</span><span class="sxs-lookup"><span data-stu-id="c7756-239">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="c7756-240">`IValidateOptions` проверяет:</span><span class="sxs-lookup"><span data-stu-id="c7756-240">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="c7756-241">определенный именованный экземпляр параметров;</span><span class="sxs-lookup"><span data-stu-id="c7756-241">A specific named options instance.</span></span>
* <span data-ttu-id="c7756-242">все параметры, при которых `name` имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="c7756-242">All options when `name` is `null`.</span></span>

<span data-ttu-id="c7756-243">Возвращает `ValidateOptionsResult` из реализации интерфейса:</span><span class="sxs-lookup"><span data-stu-id="c7756-243">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="c7756-244">Проверка на основе заметок к данным доступна в пакете [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) с помощью вызова метода <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> в `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="c7756-244">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="c7756-245">`Microsoft.Extensions.Options.DataAnnotations` неявным образом упоминается в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7756-245">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
using Microsoft.Extensions.DependencyInjection;

private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}

[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="c7756-246">Безотложная проверка (быстрое прекращение при запуске) находится на этапе рассмотрения для включения в будущие выпуски.</span><span class="sxs-lookup"><span data-stu-id="c7756-246">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="c7756-247">Пост-конфигурация параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-247">Options post-configuration</span></span>

<span data-ttu-id="c7756-248">Задайте пост-конфигурацию с помощью <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-248">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="c7756-249">Пост-конфигурация применяется после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-249">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="c7756-250">Для последующей настройки именованных параметров доступен метод <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-250"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="c7756-251">Для последующей настройки всех экземпляров конфигурации служит метод <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-251">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="c7756-252">Доступ к параметрам во время запуска</span><span class="sxs-lookup"><span data-stu-id="c7756-252">Accessing options during startup</span></span>

<span data-ttu-id="c7756-253"><xref:Microsoft.Extensions.Options.IOptions%601> и <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> можно использовать в методе `Startup.Configure`, так как службы создаются до выполнения метода `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c7756-253"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="c7756-254">Не используйте <xref:Microsoft.Extensions.Options.IOptions%601> или <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c7756-254">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c7756-255">Из-за очередности регистрации служб состояние параметров может быть несогласованным.</span><span class="sxs-lookup"><span data-stu-id="c7756-255">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="c7756-256">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="c7756-256">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="c7756-257">Когда [параметры конфигурации](xref:fundamentals/configuration/index) изолируются по сценарию в отдельных классах, в приложениях соблюдаются два важных принципа программной инженерии.</span><span class="sxs-lookup"><span data-stu-id="c7756-257">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="c7756-258">[Принцип изоляции интерфейсов или инкапсуляция](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation). Сценарии (классы), которые зависят от параметров конфигурации, зависят только от используемых ими параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-258">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="c7756-259">[Разделение ответственности](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns). Параметры для разных частей приложения не зависят друг от друга и не связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="c7756-259">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="c7756-260">В параметрах также предусмотрен механизм для проверки данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-260">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="c7756-261">Дополнительные сведения см. в разделе [Проверка параметров](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="c7756-261">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="c7756-262">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7756-262">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7756-263">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c7756-263">Prerequisites</span></span>

<span data-ttu-id="c7756-264">Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет в пакет [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="c7756-264">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="c7756-265">Интерфейсы параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-265">Options interfaces</span></span>

<span data-ttu-id="c7756-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> используется для извлечения параметров и управления уведомлениями о параметрах для экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="c7756-266"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="c7756-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> поддерживается в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="c7756-267"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="c7756-268">уведомления об изменениях;</span><span class="sxs-lookup"><span data-stu-id="c7756-268">Change notifications</span></span>
* <span data-ttu-id="c7756-269">[именованные параметры](#named-options-support-with-iconfigurenamedoptions);</span><span class="sxs-lookup"><span data-stu-id="c7756-269">[Named options](#named-options-support-with-iconfigurenamedoptions)</span></span>
* <span data-ttu-id="c7756-270">[перезагружаемые конфигурации](#reload-configuration-data-with-ioptionssnapshot);</span><span class="sxs-lookup"><span data-stu-id="c7756-270">[Reloadable configuration](#reload-configuration-data-with-ioptionssnapshot)</span></span>
* <span data-ttu-id="c7756-271">объявление определенных параметров недействительными (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>).</span><span class="sxs-lookup"><span data-stu-id="c7756-271">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="c7756-272">Сценарии [пост-конфигурации](#options-post-configuration) позволяют задать или изменить параметры после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-272">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="c7756-273">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsFactory%601> отвечает за создание экземпляров параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-273"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="c7756-274">Он имеет единственный метод <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-274">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="c7756-275">При реализации по умолчанию принимаются все зарегистрированные интерфейсы <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>. Также сначала выполняются все основные настройки, а затем действия после конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-275">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="c7756-276">Она различает интерфейсы <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> и <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и вызывает только соответствующий интерфейс.</span><span class="sxs-lookup"><span data-stu-id="c7756-276">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="c7756-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> используется интерфейсом <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для записи экземпляров `TOptions` в кэш.</span><span class="sxs-lookup"><span data-stu-id="c7756-277"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="c7756-278"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> делает экземпляры параметров в мониторе недействительными, что приводит к повторному вычислению значений (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="c7756-278">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="c7756-279">Значения можно также вводить вручную с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-279">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="c7756-280">Метод <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> используется, если необходимо повторно создать все именованные экземпляры по требованию.</span><span class="sxs-lookup"><span data-stu-id="c7756-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="c7756-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> полезно использовать в сценариях, когда параметры должны заново вычисляться при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="c7756-281"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="c7756-282">Дополнительные сведения см. в разделе [Повторная загрузка данных конфигурации с помощью IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="c7756-282">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="c7756-283"><xref:Microsoft.Extensions.Options.IOptions%601> можно использовать, чтобы обеспечить поддержку определенных параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-283"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="c7756-284">Но <xref:Microsoft.Extensions.Options.IOptions%601> не поддерживается в описанных выше сценариях <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-284">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="c7756-285">Вы можете продолжать использовать <xref:Microsoft.Extensions.Options.IOptions%601> в существующих платформах и библиотеках, которые уже используют интерфейс <xref:Microsoft.Extensions.Options.IOptions%601>. В таком случае вам не потребуются сценарии, предоставляемые <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-285">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="c7756-286">Конфигурация общих параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-286">General options configuration</span></span>

<span data-ttu-id="c7756-287">Конфигурация общих параметров демонстрируется в примере &num;1 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-287">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="c7756-288">Класс параметров должен быть неабстрактным с открытым конструктором без параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-288">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="c7756-289">Приведенный ниже класс (`MyOptions`) имеет два свойства: `Option1` и `Option2`.</span><span class="sxs-lookup"><span data-stu-id="c7756-289">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="c7756-290">Задавать значения по умолчанию необязательно, однако конструктор класса в этом примере задает значение по умолчанию для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="c7756-290">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="c7756-291">Значение по умолчанию для `Option2` устанавливается путем прямой инициализации (*Models/MyOptions.cs*).</span><span class="sxs-lookup"><span data-stu-id="c7756-291">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="c7756-292">Класс `MyOptions` добавляется в контейнер службы с помощью интерфейса <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> и привязывается к конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-292">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="c7756-293">В следующей страничной модели применяется [внедрение зависимостей конструктора](xref:mvc/controllers/dependency-injection) с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для доступа к параметрам (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-293">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="c7756-294">В файле *appsettings.json* образца задаются значения для свойств `option1` и `option2`:</span><span class="sxs-lookup"><span data-stu-id="c7756-294">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="c7756-295">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-295">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="c7756-296">При использовании настраиваемого компонента <xref:System.Configuration.ConfigurationBuilder> для загрузки конфигурации параметров из файла настроек необходимо убедиться, что базовый путь задан правильно:</span><span class="sxs-lookup"><span data-stu-id="c7756-296">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="c7756-297">При загрузке конфигурации параметров из файла настроек с помощью <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> явно задавать базовый путь не требуется.</span><span class="sxs-lookup"><span data-stu-id="c7756-297">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="c7756-298">Настройка простых параметров с помощью делегата</span><span class="sxs-lookup"><span data-stu-id="c7756-298">Configure simple options with a delegate</span></span>

<span data-ttu-id="c7756-299">Настройка простых параметров с помощью делегата демонстрируется в примере &num;2 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-299">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="c7756-300">Используйте делегат для задания значений параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-300">Use a delegate to set options values.</span></span> <span data-ttu-id="c7756-301">В образце приложения используется класс `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-301">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="c7756-302">В приведенном ниже коде в контейнер службы добавляется еще одна служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-302">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="c7756-303">Она использует делегат для настройки привязки к `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="c7756-303">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="c7756-304">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c7756-304">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="c7756-305">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-305">You can add multiple configuration providers.</span></span> <span data-ttu-id="c7756-306">Поставщики конфигурации доступны в пакетах NuGet и применяются в порядке регистрации.</span><span class="sxs-lookup"><span data-stu-id="c7756-306">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="c7756-307">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c7756-307">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="c7756-308">При каждом вызове <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601> добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="c7756-308">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="c7756-309">В предыдущем примере значения `Option1` и `Option2` задаются в файле *appsettings.json*, однако значения `Option1` и `Option2` переопределяются настроенным делегатом.</span><span class="sxs-lookup"><span data-stu-id="c7756-309">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="c7756-310">Если включено более одной службы конфигурации, последний источник конфигурации *имеет приоритет* и определяет значение конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-310">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="c7756-311">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-311">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="c7756-312">Конфигурация подпараметров</span><span class="sxs-lookup"><span data-stu-id="c7756-312">Suboptions configuration</span></span>

<span data-ttu-id="c7756-313">Конфигурация подпараметров демонстрируется в примере &num;3 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-313">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="c7756-314">В приложениях должны создаваться классы приложений, относящиеся к определенным группам сценариев (классам).</span><span class="sxs-lookup"><span data-stu-id="c7756-314">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="c7756-315">Части приложения, которым требуются значения конфигурации, должны иметь доступ только к используемым ими значениям конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-315">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="c7756-316">При привязке параметров к конфигурации каждое свойство, относящееся к типу параметров, привязывается к ключу конфигурации в формате `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="c7756-316">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="c7756-317">Например, свойство `MyOptions.Option1` привязывается к ключу `Option1`, который считывается из свойства `option1` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-317">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="c7756-318">В приведенном ниже коде в контейнер службы добавляется третья служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-318">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="c7756-319">Она привязывает `MySubOptions` к разделу `subsection` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-319">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="c7756-320">Методу `GetSection` нужно пространство имен <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="c7756-320">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="c7756-321">В файле *appsettings.json* образца определяется член `subsection` с ключами для `suboption1` и `suboption2`.</span><span class="sxs-lookup"><span data-stu-id="c7756-321">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="c7756-322">Класс `MySubOptions` определяет свойства `SubOption1` и `SubOption2` для хранения значений параметров (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-322">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="c7756-323">Метод `OnGet` страничной модели возвращает строку со значениями параметров (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-323">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="c7756-324">Когда приложение выполняется, метод `OnGet` возвращает строку со значениями класса подпараметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-324">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="c7756-325">Внедрение параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-325">Options injection</span></span>

<span data-ttu-id="c7756-326">Внедрение параметров демонстрируется в примере 4 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-326">Options injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="c7756-327">Внедрите <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в:</span><span class="sxs-lookup"><span data-stu-id="c7756-327">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="c7756-328">Страница Razor или представление MVC с директивой Razor [@inject](xref:mvc/views/razor#inject).</span><span class="sxs-lookup"><span data-stu-id="c7756-328">A Razor page or MVC view with the [@inject](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="c7756-329">Модель страницы или представления.</span><span class="sxs-lookup"><span data-stu-id="c7756-329">A page or view model.</span></span>

<span data-ttu-id="c7756-330">Следующий пример из примера приложения внедряет <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в модель страницы (*Pages/index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-330">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="c7756-331">В примере приложения показано, как внедрить `IOptionsMonitor<MyOptions>` с директивой `@inject`.</span><span class="sxs-lookup"><span data-stu-id="c7756-331">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="c7756-332">Когда запускается приложение, значения параметров появляются на отображаемой странице:</span><span class="sxs-lookup"><span data-stu-id="c7756-332">When the app is run, the options values are shown in the rendered page:</span></span>

![Значения параметров Option1: value1_from_json и Option2: –1 загружаются из модели и путем внедрения в представление.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="c7756-334">Повторная загрузка данных конфигурации с помощью IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="c7756-334">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="c7756-335">Повторная загрузка данных конфигурации с помощью <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> демонстрируется в примере &num;5 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-335">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="c7756-336">С помощью <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> параметры вычисляются один раз на каждый запрос при обращении к ним и кэшируются на все время существования запроса.</span><span class="sxs-lookup"><span data-stu-id="c7756-336">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="c7756-337">Разница между `IOptionsMonitor` и `IOptionsSnapshot`:</span><span class="sxs-lookup"><span data-stu-id="c7756-337">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="c7756-338">`IOptionsMonitor` — это [одноэлементная служба](xref:fundamentals/dependency-injection#singleton), которая получает текущие значения параметров в любое время, что особенно полезно в одноэлементных зависимостях.</span><span class="sxs-lookup"><span data-stu-id="c7756-338">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="c7756-339">`IOptionsSnapshot` — это [служба с заданной областью действия](xref:fundamentals/dependency-injection#scoped), предоставляющая моментальный снимок параметров на момент создания объекта `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="c7756-339">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="c7756-340">Моментальные снимки параметров предназначены для использования с временными зависимостями и зависимостями с заданной областью действия.</span><span class="sxs-lookup"><span data-stu-id="c7756-340">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="c7756-341">В приведенном ниже примере демонстрируется создание экземпляра <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> после изменения файла *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="c7756-341">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="c7756-342">Несколько запросов к серверу возвращают константные значения, предоставляемые файлом *appsettings.json*, пока файл не изменится и конфигурация не загрузится повторно.</span><span class="sxs-lookup"><span data-stu-id="c7756-342">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="c7756-343">Ниже показаны начальные значения `option1` и `option2`, загруженные из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-343">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="c7756-344">Измените значения в файле *appsettings.json* на `value1_from_json UPDATED` и `200`.</span><span class="sxs-lookup"><span data-stu-id="c7756-344">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="c7756-345">Сохраните файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-345">Save the *appsettings.json* file.</span></span> <span data-ttu-id="c7756-346">Обновите окно браузера, чтобы увидеть обновленные значения параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-346">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="c7756-347">Поддержка именованных параметров в IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="c7756-347">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="c7756-348">Включение поддержки именованных параметров в <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> демонстрируется в примере &num;6 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-348">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="c7756-349">Поддержка *именованных параметров* позволяет приложению различать конфигурации именованных параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-349">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="c7756-350">В примере приложения именованные параметры должны быть объявлены с помощью элемента [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), который вызывает метод расширения [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="c7756-350">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="c7756-351">Пример приложения обращается к именованным параметрам с помощью метода <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-351">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="c7756-352">При запуске образца приложения возвращаются именованные параметры:</span><span class="sxs-lookup"><span data-stu-id="c7756-352">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="c7756-353">Значения `named_options_1` предоставляются из конфигурации, которая загружается из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-353">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="c7756-354">Значения `named_options_2` предоставляются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c7756-354">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="c7756-355">Делегатом `named_options_2` в `ConfigureServices` для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="c7756-355">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="c7756-356">Значение по умолчанию для `Option2` предоставляется классом `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="c7756-356">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="c7756-357">Настройка всех параметров с помощью метода ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="c7756-357">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="c7756-358">Настройте все экземпляры параметров с помощью метода <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-358">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="c7756-359">Приведенный ниже код настраивает `Option1` для всех экземпляров конфигурации с общим значением.</span><span class="sxs-lookup"><span data-stu-id="c7756-359">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="c7756-360">Добавьте следующий код в метод `Startup.ConfigureServices` вручную:</span><span class="sxs-lookup"><span data-stu-id="c7756-360">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="c7756-361">Если запустить образец приложения после добавления, результат будет следующим:</span><span class="sxs-lookup"><span data-stu-id="c7756-361">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="c7756-362">Все параметры являются именованными экземплярами.</span><span class="sxs-lookup"><span data-stu-id="c7756-362">All options are named instances.</span></span> <span data-ttu-id="c7756-363">Существующие экземпляры <xref:Microsoft.Extensions.Options.IConfigureOptions%601> считаются нацеленными на экземпляр `Options.DefaultName`, который имеет значение `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="c7756-363">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="c7756-364">Интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> также реализует интерфейс <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-364"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="c7756-365">Реализация <xref:Microsoft.Extensions.Options.IOptionsFactory%601> по умолчанию содержит логику для надлежащего использования каждого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="c7756-365">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="c7756-366">Именованный параметр `null` предназначен для всех именованных экземпляров, а не для какого-то определенного (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> и <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> следуют этому соглашению).</span><span class="sxs-lookup"><span data-stu-id="c7756-366">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="c7756-367">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="c7756-367">OptionsBuilder API</span></span>

<span data-ttu-id="c7756-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> используется для настройки экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="c7756-368"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="c7756-369">`OptionsBuilder` упрощает создание именованных параметров, так как он является единственным параметром для первоначального вызова `AddOptions<TOptions>(string optionsName)` и не должен появляться во всех последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="c7756-369">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="c7756-370">Проверка параметров и перегрузки `ConfigureOptions`, принимающие зависимости службы, доступны только через `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c7756-370">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="c7756-371">Использование служб внедрения зависимостей для настройки параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-371">Use DI services to configure options</span></span>

<span data-ttu-id="c7756-372">Вы можете получить доступ к другим службам, доступным в результате внедрения зависимостей при настройке параметров, двумя способами.</span><span class="sxs-lookup"><span data-stu-id="c7756-372">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="c7756-373">Передайте делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) в [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="c7756-373">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="c7756-374">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) предоставляет перегрузки метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), которые позволяют использовать до пяти служб для настройки параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-374">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="c7756-375">Создайте собственный тип, реализующий <xref:Microsoft.Extensions.Options.IConfigureOptions%601> или <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, и зарегистрируйте этот тип как службу.</span><span class="sxs-lookup"><span data-stu-id="c7756-375">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="c7756-376">Рекомендуем передать делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), так как создать службу сложнее.</span><span class="sxs-lookup"><span data-stu-id="c7756-376">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="c7756-377">Создание собственного типа эквивалентно тому, что делает платформа при использовании метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="c7756-377">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="c7756-378">При вызове метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) регистрируется временный универсальный интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, имеющий конструктор, который принимает указанные универсальные типы службы.</span><span class="sxs-lookup"><span data-stu-id="c7756-378">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="c7756-379">Проверка параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-379">Options validation</span></span>

<span data-ttu-id="c7756-380">Эта функция позволяет проверить параметры при их настройке.</span><span class="sxs-lookup"><span data-stu-id="c7756-380">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="c7756-381">Вызовите `Validate` с использованием метода проверки, который возвращает `true`, если параметры являются допустимыми, и `false`, если они являются недопустимыми:</span><span class="sxs-lookup"><span data-stu-id="c7756-381">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");

// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
}
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="c7756-382">В предыдущем примере для именованного экземпляра параметров задается `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="c7756-382">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="c7756-383">Экземпляр параметров по умолчанию — `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="c7756-383">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="c7756-384">Проверка выполняется при создании экземпляра параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-384">Validation runs when the options instance is created.</span></span> <span data-ttu-id="c7756-385">Экземпляр параметров гарантированно проходит проверку при первом обращении.</span><span class="sxs-lookup"><span data-stu-id="c7756-385">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7756-386">Проверка параметров не защищает от изменений, вносимых после исходной настройки и проверки параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-386">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="c7756-387">Метод `Validate` принимает `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="c7756-387">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="c7756-388">Чтобы полностью настроить проверку, реализуйте `IValidateOptions<TOptions>`, чтобы:</span><span class="sxs-lookup"><span data-stu-id="c7756-388">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="c7756-389">выполнить проверку нескольких типов параметров — `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`;</span><span class="sxs-lookup"><span data-stu-id="c7756-389">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="c7756-390">выполнить проверку, зависящую от другого типа параметра — `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`.</span><span class="sxs-lookup"><span data-stu-id="c7756-390">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="c7756-391">`IValidateOptions` проверяет:</span><span class="sxs-lookup"><span data-stu-id="c7756-391">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="c7756-392">определенный именованный экземпляр параметров;</span><span class="sxs-lookup"><span data-stu-id="c7756-392">A specific named options instance.</span></span>
* <span data-ttu-id="c7756-393">все параметры, при которых `name` имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="c7756-393">All options when `name` is `null`.</span></span>

<span data-ttu-id="c7756-394">Возвращает `ValidateOptionsResult` из реализации интерфейса:</span><span class="sxs-lookup"><span data-stu-id="c7756-394">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="c7756-395">Проверка на основе заметок к данным доступна в пакете [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) с помощью вызова метода <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> в `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="c7756-395">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="c7756-396">`Microsoft.Extensions.Options.DataAnnotations` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c7756-396">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}

[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="c7756-397">Безотложная проверка (быстрое прекращение при запуске) находится на этапе рассмотрения для включения в будущие выпуски.</span><span class="sxs-lookup"><span data-stu-id="c7756-397">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="c7756-398">Пост-конфигурация параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-398">Options post-configuration</span></span>

<span data-ttu-id="c7756-399">Задайте пост-конфигурацию с помощью <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-399">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="c7756-400">Пост-конфигурация применяется после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-400">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="c7756-401">Для последующей настройки именованных параметров доступен метод <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-401"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="c7756-402">Для последующей настройки всех экземпляров конфигурации служит метод <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-402">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="c7756-403">Доступ к параметрам во время запуска</span><span class="sxs-lookup"><span data-stu-id="c7756-403">Accessing options during startup</span></span>

<span data-ttu-id="c7756-404"><xref:Microsoft.Extensions.Options.IOptions%601> и <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> можно использовать в методе `Startup.Configure`, так как службы создаются до выполнения метода `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c7756-404"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="c7756-405">Не используйте <xref:Microsoft.Extensions.Options.IOptions%601> или <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c7756-405">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c7756-406">Из-за очередности регистрации служб состояние параметров может быть несогласованным.</span><span class="sxs-lookup"><span data-stu-id="c7756-406">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="c7756-407">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="c7756-407">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="c7756-408">Когда [параметры конфигурации](xref:fundamentals/configuration/index) изолируются по сценарию в отдельных классах, в приложениях соблюдаются два важных принципа программной инженерии.</span><span class="sxs-lookup"><span data-stu-id="c7756-408">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="c7756-409">[Принцип изоляции интерфейсов или инкапсуляция](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation). Сценарии (классы), которые зависят от параметров конфигурации, зависят только от используемых ими параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-409">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="c7756-410">[Разделение ответственности](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns). Параметры для разных частей приложения не зависят друг от друга и не связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="c7756-410">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="c7756-411">В параметрах также предусмотрен механизм для проверки данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-411">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="c7756-412">Дополнительные сведения см. в разделе [Проверка параметров](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="c7756-412">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="c7756-413">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7756-413">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7756-414">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c7756-414">Prerequisites</span></span>

<span data-ttu-id="c7756-415">Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет в пакет [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="c7756-415">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="c7756-416">Интерфейсы параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-416">Options interfaces</span></span>

<span data-ttu-id="c7756-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> используется для извлечения параметров и управления уведомлениями о параметрах для экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="c7756-417"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="c7756-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> поддерживается в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="c7756-418"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="c7756-419">уведомления об изменениях;</span><span class="sxs-lookup"><span data-stu-id="c7756-419">Change notifications</span></span>
* <span data-ttu-id="c7756-420">[именованные параметры](#named-options-support-with-iconfigurenamedoptions);</span><span class="sxs-lookup"><span data-stu-id="c7756-420">[Named options](#named-options-support-with-iconfigurenamedoptions)</span></span>
* <span data-ttu-id="c7756-421">[перезагружаемые конфигурации](#reload-configuration-data-with-ioptionssnapshot);</span><span class="sxs-lookup"><span data-stu-id="c7756-421">[Reloadable configuration](#reload-configuration-data-with-ioptionssnapshot)</span></span>
* <span data-ttu-id="c7756-422">объявление определенных параметров недействительными (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>).</span><span class="sxs-lookup"><span data-stu-id="c7756-422">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="c7756-423">Сценарии [пост-конфигурации](#options-post-configuration) позволяют задать или изменить параметры после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-423">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="c7756-424">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsFactory%601> отвечает за создание экземпляров параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-424"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="c7756-425">Он имеет единственный метод <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-425">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="c7756-426">При реализации по умолчанию принимаются все зарегистрированные интерфейсы <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>. Также сначала выполняются все основные настройки, а затем действия после конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-426">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="c7756-427">Она различает интерфейсы <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> и <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и вызывает только соответствующий интерфейс.</span><span class="sxs-lookup"><span data-stu-id="c7756-427">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="c7756-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> используется интерфейсом <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для записи экземпляров `TOptions` в кэш.</span><span class="sxs-lookup"><span data-stu-id="c7756-428"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="c7756-429"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> делает экземпляры параметров в мониторе недействительными, что приводит к повторному вычислению значений (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="c7756-429">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="c7756-430">Значения можно также вводить вручную с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-430">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="c7756-431">Метод <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> используется, если необходимо повторно создать все именованные экземпляры по требованию.</span><span class="sxs-lookup"><span data-stu-id="c7756-431">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="c7756-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> полезно использовать в сценариях, когда параметры должны заново вычисляться при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="c7756-432"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="c7756-433">Дополнительные сведения см. в разделе [Повторная загрузка данных конфигурации с помощью IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="c7756-433">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="c7756-434"><xref:Microsoft.Extensions.Options.IOptions%601> можно использовать, чтобы обеспечить поддержку определенных параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-434"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="c7756-435">Но <xref:Microsoft.Extensions.Options.IOptions%601> не поддерживается в описанных выше сценариях <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-435">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="c7756-436">Вы можете продолжать использовать <xref:Microsoft.Extensions.Options.IOptions%601> в существующих платформах и библиотеках, которые уже используют интерфейс <xref:Microsoft.Extensions.Options.IOptions%601>. В таком случае вам не потребуются сценарии, предоставляемые <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-436">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="c7756-437">Конфигурация общих параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-437">General options configuration</span></span>

<span data-ttu-id="c7756-438">Конфигурация общих параметров демонстрируется в примере &num;1 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-438">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="c7756-439">Класс параметров должен быть неабстрактным с открытым конструктором без параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-439">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="c7756-440">Приведенный ниже класс (`MyOptions`) имеет два свойства: `Option1` и `Option2`.</span><span class="sxs-lookup"><span data-stu-id="c7756-440">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="c7756-441">Задавать значения по умолчанию необязательно, однако конструктор класса в этом примере задает значение по умолчанию для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="c7756-441">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="c7756-442">Значение по умолчанию для `Option2` устанавливается путем прямой инициализации (*Models/MyOptions.cs*).</span><span class="sxs-lookup"><span data-stu-id="c7756-442">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="c7756-443">Класс `MyOptions` добавляется в контейнер службы с помощью интерфейса <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> и привязывается к конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-443">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="c7756-444">В следующей страничной модели применяется [внедрение зависимостей конструктора](xref:mvc/controllers/dependency-injection) с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для доступа к параметрам (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-444">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="c7756-445">В файле *appsettings.json* образца задаются значения для свойств `option1` и `option2`:</span><span class="sxs-lookup"><span data-stu-id="c7756-445">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="c7756-446">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-446">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="c7756-447">При использовании настраиваемого компонента <xref:System.Configuration.ConfigurationBuilder> для загрузки конфигурации параметров из файла настроек необходимо убедиться, что базовый путь задан правильно:</span><span class="sxs-lookup"><span data-stu-id="c7756-447">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="c7756-448">При загрузке конфигурации параметров из файла настроек с помощью <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> явно задавать базовый путь не требуется.</span><span class="sxs-lookup"><span data-stu-id="c7756-448">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="c7756-449">Настройка простых параметров с помощью делегата</span><span class="sxs-lookup"><span data-stu-id="c7756-449">Configure simple options with a delegate</span></span>

<span data-ttu-id="c7756-450">Настройка простых параметров с помощью делегата демонстрируется в примере &num;2 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-450">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="c7756-451">Используйте делегат для задания значений параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-451">Use a delegate to set options values.</span></span> <span data-ttu-id="c7756-452">В образце приложения используется класс `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-452">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="c7756-453">В приведенном ниже коде в контейнер службы добавляется еще одна служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-453">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="c7756-454">Она использует делегат для настройки привязки к `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="c7756-454">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="c7756-455">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="c7756-455">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="c7756-456">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-456">You can add multiple configuration providers.</span></span> <span data-ttu-id="c7756-457">Поставщики конфигурации доступны в пакетах NuGet и применяются в порядке регистрации.</span><span class="sxs-lookup"><span data-stu-id="c7756-457">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="c7756-458">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c7756-458">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="c7756-459">При каждом вызове <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601> добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="c7756-459">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="c7756-460">В предыдущем примере значения `Option1` и `Option2` задаются в файле *appsettings.json*, однако значения `Option1` и `Option2` переопределяются настроенным делегатом.</span><span class="sxs-lookup"><span data-stu-id="c7756-460">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="c7756-461">Если включено более одной службы конфигурации, последний источник конфигурации *имеет приоритет* и определяет значение конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-461">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="c7756-462">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-462">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="c7756-463">Конфигурация подпараметров</span><span class="sxs-lookup"><span data-stu-id="c7756-463">Suboptions configuration</span></span>

<span data-ttu-id="c7756-464">Конфигурация подпараметров демонстрируется в примере &num;3 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-464">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="c7756-465">В приложениях должны создаваться классы приложений, относящиеся к определенным группам сценариев (классам).</span><span class="sxs-lookup"><span data-stu-id="c7756-465">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="c7756-466">Части приложения, которым требуются значения конфигурации, должны иметь доступ только к используемым ими значениям конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7756-466">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="c7756-467">При привязке параметров к конфигурации каждое свойство, относящееся к типу параметров, привязывается к ключу конфигурации в формате `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="c7756-467">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="c7756-468">Например, свойство `MyOptions.Option1` привязывается к ключу `Option1`, который считывается из свойства `option1` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-468">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="c7756-469">В приведенном ниже коде в контейнер службы добавляется третья служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-469">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="c7756-470">Она привязывает `MySubOptions` к разделу `subsection` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-470">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="c7756-471">Методу `GetSection` нужно пространство имен <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="c7756-471">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="c7756-472">В файле *appsettings.json* образца определяется член `subsection` с ключами для `suboption1` и `suboption2`.</span><span class="sxs-lookup"><span data-stu-id="c7756-472">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="c7756-473">Класс `MySubOptions` определяет свойства `SubOption1` и `SubOption2` для хранения значений параметров (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-473">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="c7756-474">Метод `OnGet` страничной модели возвращает строку со значениями параметров (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-474">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="c7756-475">Когда приложение выполняется, метод `OnGet` возвращает строку со значениями класса подпараметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-475">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="c7756-476">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления</span><span class="sxs-lookup"><span data-stu-id="c7756-476">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="c7756-477">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления, демонстрируются в примере &num;4 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-477">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="c7756-478">Параметры могут передаваться в модель представления или путем внедрения <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> непосредственно в представление (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-478">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="c7756-479">В примере приложения показано, как внедрить `IOptionsMonitor<MyOptions>` с директивой `@inject`.</span><span class="sxs-lookup"><span data-stu-id="c7756-479">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="c7756-480">Когда запускается приложение, значения параметров появляются на отображаемой странице:</span><span class="sxs-lookup"><span data-stu-id="c7756-480">When the app is run, the options values are shown in the rendered page:</span></span>

![Значения параметров Option1: value1_from_json и Option2: –1 загружаются из модели и путем внедрения в представление.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="c7756-482">Повторная загрузка данных конфигурации с помощью IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="c7756-482">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="c7756-483">Повторная загрузка данных конфигурации с помощью <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> демонстрируется в примере &num;5 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-483">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="c7756-484">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> поддерживает повторную загрузку параметров с минимальными затратами на обработку.</span><span class="sxs-lookup"><span data-stu-id="c7756-484"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="c7756-485">Параметры вычисляются один раз на каждый запрос при обращении к ним и кэшируются на все время существования запроса.</span><span class="sxs-lookup"><span data-stu-id="c7756-485">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="c7756-486">В приведенном ниже примере демонстрируется создание экземпляра <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> после изменения файла *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="c7756-486">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="c7756-487">Несколько запросов к серверу возвращают константные значения, предоставляемые файлом *appsettings.json*, пока файл не изменится и конфигурация не загрузится повторно.</span><span class="sxs-lookup"><span data-stu-id="c7756-487">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="c7756-488">Ниже показаны начальные значения `option1` и `option2`, загруженные из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-488">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="c7756-489">Измените значения в файле *appsettings.json* на `value1_from_json UPDATED` и `200`.</span><span class="sxs-lookup"><span data-stu-id="c7756-489">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="c7756-490">Сохраните файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-490">Save the *appsettings.json* file.</span></span> <span data-ttu-id="c7756-491">Обновите окно браузера, чтобы увидеть обновленные значения параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-491">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="c7756-492">Поддержка именованных параметров в IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="c7756-492">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="c7756-493">Включение поддержки именованных параметров в <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> демонстрируется в примере &num;6 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="c7756-493">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="c7756-494">Поддержка *именованных параметров* позволяет приложению различать конфигурации именованных параметров.</span><span class="sxs-lookup"><span data-stu-id="c7756-494">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="c7756-495">В примере приложения именованные параметры должны быть объявлены с помощью элемента [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), который вызывает метод расширения [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="c7756-495">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="c7756-496">Пример приложения обращается к именованным параметрам с помощью метода <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="c7756-496">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="c7756-497">При запуске образца приложения возвращаются именованные параметры:</span><span class="sxs-lookup"><span data-stu-id="c7756-497">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="c7756-498">Значения `named_options_1` предоставляются из конфигурации, которая загружается из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c7756-498">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="c7756-499">Значения `named_options_2` предоставляются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c7756-499">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="c7756-500">Делегатом `named_options_2` в `ConfigureServices` для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="c7756-500">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="c7756-501">Значение по умолчанию для `Option2` предоставляется классом `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="c7756-501">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="c7756-502">Настройка всех параметров с помощью метода ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="c7756-502">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="c7756-503">Настройте все экземпляры параметров с помощью метода <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-503">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="c7756-504">Приведенный ниже код настраивает `Option1` для всех экземпляров конфигурации с общим значением.</span><span class="sxs-lookup"><span data-stu-id="c7756-504">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="c7756-505">Добавьте следующий код в метод `Startup.ConfigureServices` вручную:</span><span class="sxs-lookup"><span data-stu-id="c7756-505">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="c7756-506">Если запустить образец приложения после добавления, результат будет следующим:</span><span class="sxs-lookup"><span data-stu-id="c7756-506">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="c7756-507">Все параметры являются именованными экземплярами.</span><span class="sxs-lookup"><span data-stu-id="c7756-507">All options are named instances.</span></span> <span data-ttu-id="c7756-508">Существующие экземпляры <xref:Microsoft.Extensions.Options.IConfigureOptions%601> считаются нацеленными на экземпляр `Options.DefaultName`, который имеет значение `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="c7756-508">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="c7756-509">Интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> также реализует интерфейс <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-509"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="c7756-510">Реализация <xref:Microsoft.Extensions.Options.IOptionsFactory%601> по умолчанию содержит логику для надлежащего использования каждого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="c7756-510">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="c7756-511">Именованный параметр `null` предназначен для всех именованных экземпляров, а не для какого-то определенного (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> и <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> следуют этому соглашению).</span><span class="sxs-lookup"><span data-stu-id="c7756-511">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="c7756-512">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="c7756-512">OptionsBuilder API</span></span>

<span data-ttu-id="c7756-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> используется для настройки экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="c7756-513"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="c7756-514">`OptionsBuilder` упрощает создание именованных параметров, так как он является единственным параметром для первоначального вызова `AddOptions<TOptions>(string optionsName)` и не должен появляться во всех последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="c7756-514">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="c7756-515">Проверка параметров и перегрузки `ConfigureOptions`, принимающие зависимости службы, доступны только через `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c7756-515">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="c7756-516">Использование служб внедрения зависимостей для настройки параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-516">Use DI services to configure options</span></span>

<span data-ttu-id="c7756-517">Вы можете получить доступ к другим службам, доступным в результате внедрения зависимостей при настройке параметров, двумя способами.</span><span class="sxs-lookup"><span data-stu-id="c7756-517">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="c7756-518">Передайте делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) в [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="c7756-518">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="c7756-519">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) предоставляет перегрузки метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), которые позволяют использовать до пяти служб для настройки параметров:</span><span class="sxs-lookup"><span data-stu-id="c7756-519">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="c7756-520">Создайте собственный тип, реализующий <xref:Microsoft.Extensions.Options.IConfigureOptions%601> или <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, и зарегистрируйте этот тип как службу.</span><span class="sxs-lookup"><span data-stu-id="c7756-520">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="c7756-521">Рекомендуем передать делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), так как создать службу сложнее.</span><span class="sxs-lookup"><span data-stu-id="c7756-521">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="c7756-522">Создание собственного типа эквивалентно тому, что делает платформа при использовании метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="c7756-522">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="c7756-523">При вызове метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) регистрируется временный универсальный интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, имеющий конструктор, который принимает указанные универсальные типы службы.</span><span class="sxs-lookup"><span data-stu-id="c7756-523">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="c7756-524">Пост-конфигурация параметров</span><span class="sxs-lookup"><span data-stu-id="c7756-524">Options post-configuration</span></span>

<span data-ttu-id="c7756-525">Задайте пост-конфигурацию с помощью <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-525">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="c7756-526">Пост-конфигурация применяется после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="c7756-526">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="c7756-527">Для последующей настройки именованных параметров доступен метод <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-527"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="c7756-528">Для последующей настройки всех экземпляров конфигурации служит метод <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="c7756-528">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="c7756-529">Доступ к параметрам во время запуска</span><span class="sxs-lookup"><span data-stu-id="c7756-529">Accessing options during startup</span></span>

<span data-ttu-id="c7756-530"><xref:Microsoft.Extensions.Options.IOptions%601> и <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> можно использовать в методе `Startup.Configure`, так как службы создаются до выполнения метода `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c7756-530"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="c7756-531">Не используйте <xref:Microsoft.Extensions.Options.IOptions%601> или <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c7756-531">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c7756-532">Из-за очередности регистрации служб состояние параметров может быть несогласованным.</span><span class="sxs-lookup"><span data-stu-id="c7756-532">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="c7756-533">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c7756-533">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
