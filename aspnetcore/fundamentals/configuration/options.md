---
title: Шаблон параметров в ASP.NET Core
author: guardrex
description: Узнайте, как использовать шаблон параметров для представления групп связанных параметров в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: eb0b7f3f4596b63cf3142017c5c5fe4923aac3a4
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378744"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="a3359-103">Шаблон параметров в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a3359-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="a3359-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="a3359-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a3359-105">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="a3359-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="a3359-106">Когда [параметры конфигурации](xref:fundamentals/configuration/index) изолируются по сценарию в отдельных классах, в приложениях соблюдаются два важных принципа программной инженерии.</span><span class="sxs-lookup"><span data-stu-id="a3359-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="a3359-107">[Принцип изоляции интерфейсов или инкапсуляция](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation). Сценарии (классы), которые зависят от параметров конфигурации, зависят только от используемых ими параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="a3359-108">[Разделение ответственности](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns). Параметры для разных частей приложения не зависят друг от друга и не связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="a3359-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="a3359-109">В параметрах также предусмотрен механизм для проверки данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="a3359-110">Дополнительные сведения см. в разделе [Проверка параметров](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="a3359-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="a3359-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a3359-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="a3359-112">Пакет</span><span class="sxs-lookup"><span data-stu-id="a3359-112">Package</span></span>

<span data-ttu-id="a3359-113">Пакет [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) неявно упоминается в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3359-113">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="a3359-114">Интерфейсы параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-114">Options interfaces</span></span>

<span data-ttu-id="a3359-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> используется для извлечения параметров и управления уведомлениями о параметрах для экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a3359-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="a3359-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> поддерживается в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="a3359-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="a3359-117">уведомления об изменениях;</span><span class="sxs-lookup"><span data-stu-id="a3359-117">Change notifications</span></span>
* <span data-ttu-id="a3359-118">[именованные параметры](#named-options-support-with-iconfigurenamedoptions);</span><span class="sxs-lookup"><span data-stu-id="a3359-118">[Named options](#named-options-support-with-iconfigurenamedoptions)</span></span>
* <span data-ttu-id="a3359-119">[перезагружаемые конфигурации](#reload-configuration-data-with-ioptionssnapshot);</span><span class="sxs-lookup"><span data-stu-id="a3359-119">[Reloadable configuration](#reload-configuration-data-with-ioptionssnapshot)</span></span>
* <span data-ttu-id="a3359-120">объявление определенных параметров недействительными (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>).</span><span class="sxs-lookup"><span data-stu-id="a3359-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="a3359-121">Сценарии [пост-конфигурации](#options-post-configuration) позволяют задать или изменить параметры после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="a3359-122">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsFactory%601> отвечает за создание экземпляров параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="a3359-123">Он имеет единственный метод <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="a3359-124">При реализации по умолчанию принимаются все зарегистрированные интерфейсы <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>. Также сначала выполняются все основные настройки, а затем действия после конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="a3359-125">Она различает интерфейсы <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> и <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и вызывает только соответствующий интерфейс.</span><span class="sxs-lookup"><span data-stu-id="a3359-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="a3359-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> используется интерфейсом <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для записи экземпляров `TOptions` в кэш.</span><span class="sxs-lookup"><span data-stu-id="a3359-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="a3359-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> делает экземпляры параметров в мониторе недействительными, что приводит к повторному вычислению значений (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="a3359-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="a3359-128">Значения можно также вводить вручную с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="a3359-129">Метод <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> используется, если необходимо повторно создать все именованные экземпляры по требованию.</span><span class="sxs-lookup"><span data-stu-id="a3359-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="a3359-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> полезно использовать в сценариях, когда параметры должны заново вычисляться при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="a3359-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="a3359-131">Дополнительные сведения см. в разделе [Повторная загрузка данных конфигурации с помощью IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="a3359-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="a3359-132"><xref:Microsoft.Extensions.Options.IOptions%601> можно использовать, чтобы обеспечить поддержку определенных параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="a3359-133">Но <xref:Microsoft.Extensions.Options.IOptions%601> не поддерживается в описанных выше сценариях <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="a3359-134">Вы можете продолжать использовать <xref:Microsoft.Extensions.Options.IOptions%601> в существующих платформах и библиотеках, которые уже используют интерфейс <xref:Microsoft.Extensions.Options.IOptions%601>. В таком случае вам не потребуются сценарии, предоставляемые <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="a3359-135">Конфигурация общих параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-135">General options configuration</span></span>

<span data-ttu-id="a3359-136">Конфигурация общих параметров демонстрируется в примере &num;1 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="a3359-137">Класс параметров должен быть неабстрактным с открытым конструктором без параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="a3359-138">Приведенный ниже класс (`MyOptions`) имеет два свойства: `Option1` и `Option2`.</span><span class="sxs-lookup"><span data-stu-id="a3359-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="a3359-139">Задавать значения по умолчанию необязательно, однако конструктор класса в этом примере задает значение по умолчанию для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a3359-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="a3359-140">Значение по умолчанию для `Option2` устанавливается путем прямой инициализации (*Models/MyOptions.cs*).</span><span class="sxs-lookup"><span data-stu-id="a3359-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="a3359-141">Класс `MyOptions` добавляется в контейнер службы с помощью интерфейса <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> и привязывается к конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="a3359-142">В следующей страничной модели применяется [внедрение зависимостей конструктора](xref:mvc/controllers/dependency-injection) с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для доступа к параметрам (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="a3359-143">В файле *appsettings.json* образца задаются значения для свойств `option1` и `option2`:</span><span class="sxs-lookup"><span data-stu-id="a3359-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="a3359-144">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="a3359-145">При использовании настраиваемого компонента <xref:System.Configuration.ConfigurationBuilder> для загрузки конфигурации параметров из файла настроек необходимо убедиться, что базовый путь задан правильно:</span><span class="sxs-lookup"><span data-stu-id="a3359-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="a3359-146">При загрузке конфигурации параметров из файла настроек с помощью <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> явно задавать базовый путь не требуется.</span><span class="sxs-lookup"><span data-stu-id="a3359-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="a3359-147">Настройка простых параметров с помощью делегата</span><span class="sxs-lookup"><span data-stu-id="a3359-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="a3359-148">Настройка простых параметров с помощью делегата демонстрируется в примере &num;2 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="a3359-149">Используйте делегат для задания значений параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-149">Use a delegate to set options values.</span></span> <span data-ttu-id="a3359-150">В образце приложения используется класс `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="a3359-151">В приведенном ниже коде в контейнер службы добавляется еще одна служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a3359-152">Она использует делегат для настройки привязки к `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="a3359-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="a3359-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a3359-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="a3359-154">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="a3359-155">Поставщики конфигурации доступны в пакетах NuGet и применяются в порядке регистрации.</span><span class="sxs-lookup"><span data-stu-id="a3359-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="a3359-156">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a3359-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="a3359-157">При каждом вызове <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601> добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="a3359-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="a3359-158">В предыдущем примере значения `Option1` и `Option2` задаются в файле *appsettings.json*, однако значения `Option1` и `Option2` переопределяются настроенным делегатом.</span><span class="sxs-lookup"><span data-stu-id="a3359-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="a3359-159">Если включено более одной службы конфигурации, последний источник конфигурации *имеет приоритет* и определяет значение конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="a3359-160">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="a3359-161">Конфигурация подпараметров</span><span class="sxs-lookup"><span data-stu-id="a3359-161">Suboptions configuration</span></span>

<span data-ttu-id="a3359-162">Конфигурация подпараметров демонстрируется в примере &num;3 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="a3359-163">В приложениях должны создаваться классы приложений, относящиеся к определенным группам сценариев (классам).</span><span class="sxs-lookup"><span data-stu-id="a3359-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="a3359-164">Части приложения, которым требуются значения конфигурации, должны иметь доступ только к используемым ими значениям конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="a3359-165">При привязке параметров к конфигурации каждое свойство, относящееся к типу параметров, привязывается к ключу конфигурации в формате `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="a3359-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="a3359-166">Например, свойство `MyOptions.Option1` привязывается к ключу `Option1`, который считывается из свойства `option1` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="a3359-167">В приведенном ниже коде в контейнер службы добавляется третья служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a3359-168">Она привязывает `MySubOptions` к разделу `subsection` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="a3359-169">Методу расширения `GetSection` требуется пакет NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="a3359-169">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="a3359-170">`Microsoft.Extensions.Options.ConfigurationExtensions` неявным образом упоминается в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3359-170">`Microsoft.Extensions.Options.ConfigurationExtensions` is implicitly referenced in ASP.NET Core apps.</span></span>

<span data-ttu-id="a3359-171">В файле *appsettings.json* образца определяется член `subsection` с ключами для `suboption1` и `suboption2`.</span><span class="sxs-lookup"><span data-stu-id="a3359-171">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="a3359-172">Класс `MySubOptions` определяет свойства `SubOption1` и `SubOption2` для хранения значений параметров (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-172">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="a3359-173">Метод `OnGet` страничной модели возвращает строку со значениями параметров (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-173">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="a3359-174">Когда приложение выполняется, метод `OnGet` возвращает строку со значениями класса подпараметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-174">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="a3359-175">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления</span><span class="sxs-lookup"><span data-stu-id="a3359-175">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="a3359-176">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления, демонстрируются в примере &num;4 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-176">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="a3359-177">Параметры могут передаваться в модель представления или путем внедрения <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> непосредственно в представление (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-177">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="a3359-178">В примере приложения показано, как внедрить `IOptionsMonitor<MyOptions>` с директивой `@inject`.</span><span class="sxs-lookup"><span data-stu-id="a3359-178">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="a3359-179">Когда запускается приложение, значения параметров появляются на отображаемой странице:</span><span class="sxs-lookup"><span data-stu-id="a3359-179">When the app is run, the options values are shown in the rendered page:</span></span>

![Значения параметров Option1: value1_from_json и Option2: –1 загружаются из модели и путем внедрения в представление.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="a3359-181">Повторная загрузка данных конфигурации с помощью IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="a3359-181">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="a3359-182">Повторная загрузка данных конфигурации с помощью <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> демонстрируется в примере &num;5 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-182">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="a3359-183">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> поддерживает повторную загрузку параметров с минимальными затратами на обработку.</span><span class="sxs-lookup"><span data-stu-id="a3359-183"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="a3359-184">Параметры вычисляются один раз на каждый запрос при обращении к ним и кэшируются на все время существования запроса.</span><span class="sxs-lookup"><span data-stu-id="a3359-184">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="a3359-185">В приведенном ниже примере демонстрируется создание экземпляра <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> после изменения файла *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="a3359-185">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="a3359-186">Несколько запросов к серверу возвращают константные значения, предоставляемые файлом *appsettings.json*, пока файл не изменится и конфигурация не загрузится повторно.</span><span class="sxs-lookup"><span data-stu-id="a3359-186">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="a3359-187">Ниже показаны начальные значения `option1` и `option2`, загруженные из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-187">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="a3359-188">Измените значения в файле *appsettings.json* на `value1_from_json UPDATED` и `200`.</span><span class="sxs-lookup"><span data-stu-id="a3359-188">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="a3359-189">Сохраните файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-189">Save the *appsettings.json* file.</span></span> <span data-ttu-id="a3359-190">Обновите окно браузера, чтобы увидеть обновленные значения параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-190">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="a3359-191">Поддержка именованных параметров в IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="a3359-191">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="a3359-192">Включение поддержки именованных параметров в <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> демонстрируется в примере &num;6 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-192">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="a3359-193">Поддержка *именованных параметров* позволяет приложению различать конфигурации именованных параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-193">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="a3359-194">В примере приложения именованные параметры должны быть объявлены с помощью элемента [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), который вызывает метод расширения [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="a3359-194">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="a3359-195">Пример приложения обращается к именованным параметрам с помощью метода <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-195">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="a3359-196">При запуске образца приложения возвращаются именованные параметры:</span><span class="sxs-lookup"><span data-stu-id="a3359-196">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="a3359-197">Значения `named_options_1` предоставляются из конфигурации, которая загружается из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-197">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="a3359-198">Значения `named_options_2` предоставляются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a3359-198">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="a3359-199">Делегатом `named_options_2` в `ConfigureServices` для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a3359-199">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="a3359-200">Значение по умолчанию для `Option2` предоставляется классом `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="a3359-200">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="a3359-201">Настройка всех параметров с помощью метода ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="a3359-201">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="a3359-202">Настройте все экземпляры параметров с помощью метода <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-202">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="a3359-203">Приведенный ниже код настраивает `Option1` для всех экземпляров конфигурации с общим значением.</span><span class="sxs-lookup"><span data-stu-id="a3359-203">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="a3359-204">Добавьте следующий код в метод `Startup.ConfigureServices` вручную:</span><span class="sxs-lookup"><span data-stu-id="a3359-204">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="a3359-205">Если запустить образец приложения после добавления, результат будет следующим:</span><span class="sxs-lookup"><span data-stu-id="a3359-205">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="a3359-206">Все параметры являются именованными экземплярами.</span><span class="sxs-lookup"><span data-stu-id="a3359-206">All options are named instances.</span></span> <span data-ttu-id="a3359-207">Существующие экземпляры <xref:Microsoft.Extensions.Options.IConfigureOptions%601> считаются нацеленными на экземпляр `Options.DefaultName`, который имеет значение `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="a3359-207">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="a3359-208">Интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> также реализует интерфейс <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-208"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="a3359-209">Реализация <xref:Microsoft.Extensions.Options.IOptionsFactory%601> по умолчанию содержит логику для надлежащего использования каждого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="a3359-209">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="a3359-210">Именованный параметр `null` предназначен для всех именованных экземпляров, а не для какого-то определенного (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> и <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> следуют этому соглашению).</span><span class="sxs-lookup"><span data-stu-id="a3359-210">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="a3359-211">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="a3359-211">OptionsBuilder API</span></span>

<span data-ttu-id="a3359-212"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> используется для настройки экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a3359-212"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="a3359-213">`OptionsBuilder` упрощает создание именованных параметров, так как он является единственным параметром для первоначального вызова `AddOptions<TOptions>(string optionsName)` и не должен появляться во всех последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="a3359-213">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="a3359-214">Проверка параметров и перегрузки `ConfigureOptions`, принимающие зависимости службы, доступны только через `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a3359-214">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="a3359-215">Использование служб внедрения зависимостей для настройки параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-215">Use DI services to configure options</span></span>

<span data-ttu-id="a3359-216">Вы можете получить доступ к другим службам, доступным в результате внедрения зависимостей при настройке параметров, двумя способами.</span><span class="sxs-lookup"><span data-stu-id="a3359-216">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="a3359-217">Передайте делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) в [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="a3359-217">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="a3359-218">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) предоставляет перегрузки метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), которые позволяют использовать до пяти служб для настройки параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-218">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="a3359-219">Создайте собственный тип, реализующий <xref:Microsoft.Extensions.Options.IConfigureOptions%601> или <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, и зарегистрируйте этот тип как службу.</span><span class="sxs-lookup"><span data-stu-id="a3359-219">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="a3359-220">Рекомендуем передать делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), так как создать службу сложнее.</span><span class="sxs-lookup"><span data-stu-id="a3359-220">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="a3359-221">Создание собственного типа эквивалентно тому, что делает платформа при использовании метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="a3359-221">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="a3359-222">При вызове метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) регистрируется временный универсальный интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, имеющий конструктор, который принимает указанные универсальные типы службы.</span><span class="sxs-lookup"><span data-stu-id="a3359-222">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="a3359-223">Проверка параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-223">Options validation</span></span>

<span data-ttu-id="a3359-224">Эта функция позволяет проверить параметры при их настройке.</span><span class="sxs-lookup"><span data-stu-id="a3359-224">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="a3359-225">Вызовите `Validate` с использованием метода проверки, который возвращает `true`, если параметры являются допустимыми, и `false`, если они являются недопустимыми:</span><span class="sxs-lookup"><span data-stu-id="a3359-225">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="a3359-226">В предыдущем примере для именованного экземпляра параметров задается `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="a3359-226">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="a3359-227">Экземпляр параметров по умолчанию — `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="a3359-227">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="a3359-228">Проверка выполняется при создании экземпляра параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-228">Validation runs when the options instance is created.</span></span> <span data-ttu-id="a3359-229">Экземпляр параметров гарантированно проходит проверку при первом обращении.</span><span class="sxs-lookup"><span data-stu-id="a3359-229">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3359-230">Проверка параметров не защищает от изменений, вносимых после исходной настройки и проверки параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-230">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="a3359-231">Метод `Validate` принимает `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="a3359-231">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="a3359-232">Чтобы полностью настроить проверку, реализуйте `IValidateOptions<TOptions>`, чтобы:</span><span class="sxs-lookup"><span data-stu-id="a3359-232">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="a3359-233">выполнить проверку нескольких типов параметров — `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`;</span><span class="sxs-lookup"><span data-stu-id="a3359-233">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="a3359-234">выполнить проверку, зависящую от другого типа параметра — `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`.</span><span class="sxs-lookup"><span data-stu-id="a3359-234">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="a3359-235">`IValidateOptions` проверяет:</span><span class="sxs-lookup"><span data-stu-id="a3359-235">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="a3359-236">определенный именованный экземпляр параметров;</span><span class="sxs-lookup"><span data-stu-id="a3359-236">A specific named options instance.</span></span>
* <span data-ttu-id="a3359-237">все параметры, при которых `name` имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="a3359-237">All options when `name` is `null`.</span></span>

<span data-ttu-id="a3359-238">Возвращает `ValidateOptionsResult` из реализации интерфейса:</span><span class="sxs-lookup"><span data-stu-id="a3359-238">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="a3359-239">Проверка на основе заметок к данным доступна в пакете [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) с помощью вызова метода <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> в `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="a3359-239">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="a3359-240">`Microsoft.Extensions.Options.DataAnnotations` неявным образом упоминается в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3359-240">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

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

<span data-ttu-id="a3359-241">Безотложная проверка (быстрое прекращение при запуске) находится на этапе рассмотрения для включения в будущие выпуски.</span><span class="sxs-lookup"><span data-stu-id="a3359-241">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="a3359-242">Пост-конфигурация параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-242">Options post-configuration</span></span>

<span data-ttu-id="a3359-243">Задайте пост-конфигурацию с помощью <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-243">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="a3359-244">Пост-конфигурация применяется после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-244">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a3359-245">Для последующей настройки именованных параметров доступен метод <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-245"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a3359-246">Для последующей настройки всех экземпляров конфигурации служит метод <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-246">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="a3359-247">Доступ к параметрам во время запуска</span><span class="sxs-lookup"><span data-stu-id="a3359-247">Accessing options during startup</span></span>

<span data-ttu-id="a3359-248"><xref:Microsoft.Extensions.Options.IOptions%601> и <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> можно использовать в методе `Startup.Configure`, так как службы создаются до выполнения метода `Configure`.</span><span class="sxs-lookup"><span data-stu-id="a3359-248"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="a3359-249">Не используйте <xref:Microsoft.Extensions.Options.IOptions%601> или <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a3359-249">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a3359-250">Из-за очередности регистрации служб состояние параметров может быть несогласованным.</span><span class="sxs-lookup"><span data-stu-id="a3359-250">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="a3359-251">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="a3359-251">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="a3359-252">Когда [параметры конфигурации](xref:fundamentals/configuration/index) изолируются по сценарию в отдельных классах, в приложениях соблюдаются два важных принципа программной инженерии.</span><span class="sxs-lookup"><span data-stu-id="a3359-252">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="a3359-253">[Принцип изоляции интерфейсов или инкапсуляция](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation). Сценарии (классы), которые зависят от параметров конфигурации, зависят только от используемых ими параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-253">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="a3359-254">[Разделение ответственности](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns). Параметры для разных частей приложения не зависят друг от друга и не связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="a3359-254">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="a3359-255">В параметрах также предусмотрен механизм для проверки данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-255">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="a3359-256">Дополнительные сведения см. в разделе [Проверка параметров](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="a3359-256">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="a3359-257">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a3359-257">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3359-258">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a3359-258">Prerequisites</span></span>

<span data-ttu-id="a3359-259">Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет в пакет [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="a3359-259">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="a3359-260">Интерфейсы параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-260">Options interfaces</span></span>

<span data-ttu-id="a3359-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> используется для извлечения параметров и управления уведомлениями о параметрах для экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a3359-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="a3359-262"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> поддерживается в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="a3359-262"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="a3359-263">уведомления об изменениях;</span><span class="sxs-lookup"><span data-stu-id="a3359-263">Change notifications</span></span>
* <span data-ttu-id="a3359-264">[именованные параметры](#named-options-support-with-iconfigurenamedoptions);</span><span class="sxs-lookup"><span data-stu-id="a3359-264">[Named options](#named-options-support-with-iconfigurenamedoptions)</span></span>
* <span data-ttu-id="a3359-265">[перезагружаемые конфигурации](#reload-configuration-data-with-ioptionssnapshot);</span><span class="sxs-lookup"><span data-stu-id="a3359-265">[Reloadable configuration](#reload-configuration-data-with-ioptionssnapshot)</span></span>
* <span data-ttu-id="a3359-266">объявление определенных параметров недействительными (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>).</span><span class="sxs-lookup"><span data-stu-id="a3359-266">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="a3359-267">Сценарии [пост-конфигурации](#options-post-configuration) позволяют задать или изменить параметры после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-267">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="a3359-268">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsFactory%601> отвечает за создание экземпляров параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-268"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="a3359-269">Он имеет единственный метод <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-269">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="a3359-270">При реализации по умолчанию принимаются все зарегистрированные интерфейсы <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>. Также сначала выполняются все основные настройки, а затем действия после конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-270">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="a3359-271">Она различает интерфейсы <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> и <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и вызывает только соответствующий интерфейс.</span><span class="sxs-lookup"><span data-stu-id="a3359-271">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="a3359-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> используется интерфейсом <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для записи экземпляров `TOptions` в кэш.</span><span class="sxs-lookup"><span data-stu-id="a3359-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="a3359-273"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> делает экземпляры параметров в мониторе недействительными, что приводит к повторному вычислению значений (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="a3359-273">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="a3359-274">Значения можно также вводить вручную с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-274">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="a3359-275">Метод <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> используется, если необходимо повторно создать все именованные экземпляры по требованию.</span><span class="sxs-lookup"><span data-stu-id="a3359-275">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="a3359-276"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> полезно использовать в сценариях, когда параметры должны заново вычисляться при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="a3359-276"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="a3359-277">Дополнительные сведения см. в разделе [Повторная загрузка данных конфигурации с помощью IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="a3359-277">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="a3359-278"><xref:Microsoft.Extensions.Options.IOptions%601> можно использовать, чтобы обеспечить поддержку определенных параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-278"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="a3359-279">Но <xref:Microsoft.Extensions.Options.IOptions%601> не поддерживается в описанных выше сценариях <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-279">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="a3359-280">Вы можете продолжать использовать <xref:Microsoft.Extensions.Options.IOptions%601> в существующих платформах и библиотеках, которые уже используют интерфейс <xref:Microsoft.Extensions.Options.IOptions%601>. В таком случае вам не потребуются сценарии, предоставляемые <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-280">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="a3359-281">Конфигурация общих параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-281">General options configuration</span></span>

<span data-ttu-id="a3359-282">Конфигурация общих параметров демонстрируется в примере &num;1 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-282">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="a3359-283">Класс параметров должен быть неабстрактным с открытым конструктором без параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-283">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="a3359-284">Приведенный ниже класс (`MyOptions`) имеет два свойства: `Option1` и `Option2`.</span><span class="sxs-lookup"><span data-stu-id="a3359-284">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="a3359-285">Задавать значения по умолчанию необязательно, однако конструктор класса в этом примере задает значение по умолчанию для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a3359-285">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="a3359-286">Значение по умолчанию для `Option2` устанавливается путем прямой инициализации (*Models/MyOptions.cs*).</span><span class="sxs-lookup"><span data-stu-id="a3359-286">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="a3359-287">Класс `MyOptions` добавляется в контейнер службы с помощью интерфейса <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> и привязывается к конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-287">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="a3359-288">В следующей страничной модели применяется [внедрение зависимостей конструктора](xref:mvc/controllers/dependency-injection) с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для доступа к параметрам (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-288">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="a3359-289">В файле *appsettings.json* образца задаются значения для свойств `option1` и `option2`:</span><span class="sxs-lookup"><span data-stu-id="a3359-289">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="a3359-290">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-290">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="a3359-291">При использовании настраиваемого компонента <xref:System.Configuration.ConfigurationBuilder> для загрузки конфигурации параметров из файла настроек необходимо убедиться, что базовый путь задан правильно:</span><span class="sxs-lookup"><span data-stu-id="a3359-291">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="a3359-292">При загрузке конфигурации параметров из файла настроек с помощью <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> явно задавать базовый путь не требуется.</span><span class="sxs-lookup"><span data-stu-id="a3359-292">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="a3359-293">Настройка простых параметров с помощью делегата</span><span class="sxs-lookup"><span data-stu-id="a3359-293">Configure simple options with a delegate</span></span>

<span data-ttu-id="a3359-294">Настройка простых параметров с помощью делегата демонстрируется в примере &num;2 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-294">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="a3359-295">Используйте делегат для задания значений параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-295">Use a delegate to set options values.</span></span> <span data-ttu-id="a3359-296">В образце приложения используется класс `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-296">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="a3359-297">В приведенном ниже коде в контейнер службы добавляется еще одна служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-297">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a3359-298">Она использует делегат для настройки привязки к `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="a3359-298">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="a3359-299">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a3359-299">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="a3359-300">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-300">You can add multiple configuration providers.</span></span> <span data-ttu-id="a3359-301">Поставщики конфигурации доступны в пакетах NuGet и применяются в порядке регистрации.</span><span class="sxs-lookup"><span data-stu-id="a3359-301">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="a3359-302">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a3359-302">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="a3359-303">При каждом вызове <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601> добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="a3359-303">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="a3359-304">В предыдущем примере значения `Option1` и `Option2` задаются в файле *appsettings.json*, однако значения `Option1` и `Option2` переопределяются настроенным делегатом.</span><span class="sxs-lookup"><span data-stu-id="a3359-304">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="a3359-305">Если включено более одной службы конфигурации, последний источник конфигурации *имеет приоритет* и определяет значение конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-305">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="a3359-306">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-306">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="a3359-307">Конфигурация подпараметров</span><span class="sxs-lookup"><span data-stu-id="a3359-307">Suboptions configuration</span></span>

<span data-ttu-id="a3359-308">Конфигурация подпараметров демонстрируется в примере &num;3 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-308">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="a3359-309">В приложениях должны создаваться классы приложений, относящиеся к определенным группам сценариев (классам).</span><span class="sxs-lookup"><span data-stu-id="a3359-309">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="a3359-310">Части приложения, которым требуются значения конфигурации, должны иметь доступ только к используемым ими значениям конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-310">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="a3359-311">При привязке параметров к конфигурации каждое свойство, относящееся к типу параметров, привязывается к ключу конфигурации в формате `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="a3359-311">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="a3359-312">Например, свойство `MyOptions.Option1` привязывается к ключу `Option1`, который считывается из свойства `option1` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-312">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="a3359-313">В приведенном ниже коде в контейнер службы добавляется третья служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-313">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a3359-314">Она привязывает `MySubOptions` к разделу `subsection` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-314">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="a3359-315">Методу расширения `GetSection` требуется пакет NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="a3359-315">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="a3359-316">Если приложение использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии), пакет включается автоматически.</span><span class="sxs-lookup"><span data-stu-id="a3359-316">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="a3359-317">В файле *appsettings.json* образца определяется член `subsection` с ключами для `suboption1` и `suboption2`.</span><span class="sxs-lookup"><span data-stu-id="a3359-317">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="a3359-318">Класс `MySubOptions` определяет свойства `SubOption1` и `SubOption2` для хранения значений параметров (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-318">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="a3359-319">Метод `OnGet` страничной модели возвращает строку со значениями параметров (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-319">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="a3359-320">Когда приложение выполняется, метод `OnGet` возвращает строку со значениями класса подпараметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-320">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="a3359-321">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления</span><span class="sxs-lookup"><span data-stu-id="a3359-321">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="a3359-322">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления, демонстрируются в примере &num;4 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-322">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="a3359-323">Параметры могут передаваться в модель представления или путем внедрения <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> непосредственно в представление (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-323">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="a3359-324">В примере приложения показано, как внедрить `IOptionsMonitor<MyOptions>` с директивой `@inject`.</span><span class="sxs-lookup"><span data-stu-id="a3359-324">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="a3359-325">Когда запускается приложение, значения параметров появляются на отображаемой странице:</span><span class="sxs-lookup"><span data-stu-id="a3359-325">When the app is run, the options values are shown in the rendered page:</span></span>

![Значения параметров Option1: value1_from_json и Option2: –1 загружаются из модели и путем внедрения в представление.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="a3359-327">Повторная загрузка данных конфигурации с помощью IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="a3359-327">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="a3359-328">Повторная загрузка данных конфигурации с помощью <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> демонстрируется в примере &num;5 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-328">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="a3359-329">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> поддерживает повторную загрузку параметров с минимальными затратами на обработку.</span><span class="sxs-lookup"><span data-stu-id="a3359-329"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="a3359-330">Параметры вычисляются один раз на каждый запрос при обращении к ним и кэшируются на все время существования запроса.</span><span class="sxs-lookup"><span data-stu-id="a3359-330">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="a3359-331">В приведенном ниже примере демонстрируется создание экземпляра <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> после изменения файла *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="a3359-331">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="a3359-332">Несколько запросов к серверу возвращают константные значения, предоставляемые файлом *appsettings.json*, пока файл не изменится и конфигурация не загрузится повторно.</span><span class="sxs-lookup"><span data-stu-id="a3359-332">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="a3359-333">Ниже показаны начальные значения `option1` и `option2`, загруженные из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-333">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="a3359-334">Измените значения в файле *appsettings.json* на `value1_from_json UPDATED` и `200`.</span><span class="sxs-lookup"><span data-stu-id="a3359-334">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="a3359-335">Сохраните файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-335">Save the *appsettings.json* file.</span></span> <span data-ttu-id="a3359-336">Обновите окно браузера, чтобы увидеть обновленные значения параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-336">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="a3359-337">Поддержка именованных параметров в IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="a3359-337">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="a3359-338">Включение поддержки именованных параметров в <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> демонстрируется в примере &num;6 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-338">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="a3359-339">Поддержка *именованных параметров* позволяет приложению различать конфигурации именованных параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-339">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="a3359-340">В примере приложения именованные параметры должны быть объявлены с помощью элемента [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), который вызывает метод расширения [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="a3359-340">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="a3359-341">Пример приложения обращается к именованным параметрам с помощью метода <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-341">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="a3359-342">При запуске образца приложения возвращаются именованные параметры:</span><span class="sxs-lookup"><span data-stu-id="a3359-342">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="a3359-343">Значения `named_options_1` предоставляются из конфигурации, которая загружается из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-343">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="a3359-344">Значения `named_options_2` предоставляются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a3359-344">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="a3359-345">Делегатом `named_options_2` в `ConfigureServices` для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a3359-345">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="a3359-346">Значение по умолчанию для `Option2` предоставляется классом `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="a3359-346">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="a3359-347">Настройка всех параметров с помощью метода ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="a3359-347">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="a3359-348">Настройте все экземпляры параметров с помощью метода <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-348">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="a3359-349">Приведенный ниже код настраивает `Option1` для всех экземпляров конфигурации с общим значением.</span><span class="sxs-lookup"><span data-stu-id="a3359-349">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="a3359-350">Добавьте следующий код в метод `Startup.ConfigureServices` вручную:</span><span class="sxs-lookup"><span data-stu-id="a3359-350">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="a3359-351">Если запустить образец приложения после добавления, результат будет следующим:</span><span class="sxs-lookup"><span data-stu-id="a3359-351">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="a3359-352">Все параметры являются именованными экземплярами.</span><span class="sxs-lookup"><span data-stu-id="a3359-352">All options are named instances.</span></span> <span data-ttu-id="a3359-353">Существующие экземпляры <xref:Microsoft.Extensions.Options.IConfigureOptions%601> считаются нацеленными на экземпляр `Options.DefaultName`, который имеет значение `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="a3359-353">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="a3359-354">Интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> также реализует интерфейс <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-354"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="a3359-355">Реализация <xref:Microsoft.Extensions.Options.IOptionsFactory%601> по умолчанию содержит логику для надлежащего использования каждого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="a3359-355">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="a3359-356">Именованный параметр `null` предназначен для всех именованных экземпляров, а не для какого-то определенного (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> и <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> следуют этому соглашению).</span><span class="sxs-lookup"><span data-stu-id="a3359-356">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="a3359-357">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="a3359-357">OptionsBuilder API</span></span>

<span data-ttu-id="a3359-358"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> используется для настройки экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a3359-358"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="a3359-359">`OptionsBuilder` упрощает создание именованных параметров, так как он является единственным параметром для первоначального вызова `AddOptions<TOptions>(string optionsName)` и не должен появляться во всех последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="a3359-359">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="a3359-360">Проверка параметров и перегрузки `ConfigureOptions`, принимающие зависимости службы, доступны только через `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a3359-360">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="a3359-361">Использование служб внедрения зависимостей для настройки параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-361">Use DI services to configure options</span></span>

<span data-ttu-id="a3359-362">Вы можете получить доступ к другим службам, доступным в результате внедрения зависимостей при настройке параметров, двумя способами.</span><span class="sxs-lookup"><span data-stu-id="a3359-362">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="a3359-363">Передайте делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) в [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="a3359-363">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="a3359-364">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) предоставляет перегрузки метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), которые позволяют использовать до пяти служб для настройки параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-364">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="a3359-365">Создайте собственный тип, реализующий <xref:Microsoft.Extensions.Options.IConfigureOptions%601> или <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, и зарегистрируйте этот тип как службу.</span><span class="sxs-lookup"><span data-stu-id="a3359-365">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="a3359-366">Рекомендуем передать делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), так как создать службу сложнее.</span><span class="sxs-lookup"><span data-stu-id="a3359-366">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="a3359-367">Создание собственного типа эквивалентно тому, что делает платформа при использовании метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="a3359-367">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="a3359-368">При вызове метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) регистрируется временный универсальный интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, имеющий конструктор, который принимает указанные универсальные типы службы.</span><span class="sxs-lookup"><span data-stu-id="a3359-368">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="a3359-369">Проверка параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-369">Options validation</span></span>

<span data-ttu-id="a3359-370">Эта функция позволяет проверить параметры при их настройке.</span><span class="sxs-lookup"><span data-stu-id="a3359-370">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="a3359-371">Вызовите `Validate` с использованием метода проверки, который возвращает `true`, если параметры являются допустимыми, и `false`, если они являются недопустимыми:</span><span class="sxs-lookup"><span data-stu-id="a3359-371">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="a3359-372">В предыдущем примере для именованного экземпляра параметров задается `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="a3359-372">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="a3359-373">Экземпляр параметров по умолчанию — `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="a3359-373">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="a3359-374">Проверка выполняется при создании экземпляра параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-374">Validation runs when the options instance is created.</span></span> <span data-ttu-id="a3359-375">Экземпляр параметров гарантированно проходит проверку при первом обращении.</span><span class="sxs-lookup"><span data-stu-id="a3359-375">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3359-376">Проверка параметров не защищает от изменений, вносимых после исходной настройки и проверки параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-376">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="a3359-377">Метод `Validate` принимает `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="a3359-377">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="a3359-378">Чтобы полностью настроить проверку, реализуйте `IValidateOptions<TOptions>`, чтобы:</span><span class="sxs-lookup"><span data-stu-id="a3359-378">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="a3359-379">выполнить проверку нескольких типов параметров — `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`;</span><span class="sxs-lookup"><span data-stu-id="a3359-379">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="a3359-380">выполнить проверку, зависящую от другого типа параметра — `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`.</span><span class="sxs-lookup"><span data-stu-id="a3359-380">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="a3359-381">`IValidateOptions` проверяет:</span><span class="sxs-lookup"><span data-stu-id="a3359-381">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="a3359-382">определенный именованный экземпляр параметров;</span><span class="sxs-lookup"><span data-stu-id="a3359-382">A specific named options instance.</span></span>
* <span data-ttu-id="a3359-383">все параметры, при которых `name` имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="a3359-383">All options when `name` is `null`.</span></span>

<span data-ttu-id="a3359-384">Возвращает `ValidateOptionsResult` из реализации интерфейса:</span><span class="sxs-lookup"><span data-stu-id="a3359-384">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="a3359-385">Проверка на основе заметок к данным доступна в пакете [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) с помощью вызова метода <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> в `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="a3359-385">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="a3359-386">`Microsoft.Extensions.Options.DataAnnotations` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a3359-386">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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

<span data-ttu-id="a3359-387">Безотложная проверка (быстрое прекращение при запуске) находится на этапе рассмотрения для включения в будущие выпуски.</span><span class="sxs-lookup"><span data-stu-id="a3359-387">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="a3359-388">Пост-конфигурация параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-388">Options post-configuration</span></span>

<span data-ttu-id="a3359-389">Задайте пост-конфигурацию с помощью <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-389">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="a3359-390">Пост-конфигурация применяется после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-390">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a3359-391">Для последующей настройки именованных параметров доступен метод <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-391"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a3359-392">Для последующей настройки всех экземпляров конфигурации служит метод <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-392">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="a3359-393">Доступ к параметрам во время запуска</span><span class="sxs-lookup"><span data-stu-id="a3359-393">Accessing options during startup</span></span>

<span data-ttu-id="a3359-394"><xref:Microsoft.Extensions.Options.IOptions%601> и <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> можно использовать в методе `Startup.Configure`, так как службы создаются до выполнения метода `Configure`.</span><span class="sxs-lookup"><span data-stu-id="a3359-394"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="a3359-395">Не используйте <xref:Microsoft.Extensions.Options.IOptions%601> или <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a3359-395">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a3359-396">Из-за очередности регистрации служб состояние параметров может быть несогласованным.</span><span class="sxs-lookup"><span data-stu-id="a3359-396">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="a3359-397">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="a3359-397">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="a3359-398">Когда [параметры конфигурации](xref:fundamentals/configuration/index) изолируются по сценарию в отдельных классах, в приложениях соблюдаются два важных принципа программной инженерии.</span><span class="sxs-lookup"><span data-stu-id="a3359-398">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="a3359-399">[Принцип изоляции интерфейсов или инкапсуляция](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation). Сценарии (классы), которые зависят от параметров конфигурации, зависят только от используемых ими параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-399">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="a3359-400">[Разделение ответственности](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns). Параметры для разных частей приложения не зависят друг от друга и не связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="a3359-400">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="a3359-401">В параметрах также предусмотрен механизм для проверки данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-401">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="a3359-402">Дополнительные сведения см. в разделе [Проверка параметров](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="a3359-402">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="a3359-403">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a3359-403">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3359-404">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a3359-404">Prerequisites</span></span>

<span data-ttu-id="a3359-405">Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет в пакет [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="a3359-405">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="a3359-406">Интерфейсы параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-406">Options interfaces</span></span>

<span data-ttu-id="a3359-407"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> используется для извлечения параметров и управления уведомлениями о параметрах для экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a3359-407"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="a3359-408"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> поддерживается в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="a3359-408"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="a3359-409">уведомления об изменениях;</span><span class="sxs-lookup"><span data-stu-id="a3359-409">Change notifications</span></span>
* <span data-ttu-id="a3359-410">[именованные параметры](#named-options-support-with-iconfigurenamedoptions);</span><span class="sxs-lookup"><span data-stu-id="a3359-410">[Named options](#named-options-support-with-iconfigurenamedoptions)</span></span>
* <span data-ttu-id="a3359-411">[перезагружаемые конфигурации](#reload-configuration-data-with-ioptionssnapshot);</span><span class="sxs-lookup"><span data-stu-id="a3359-411">[Reloadable configuration](#reload-configuration-data-with-ioptionssnapshot)</span></span>
* <span data-ttu-id="a3359-412">объявление определенных параметров недействительными (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>).</span><span class="sxs-lookup"><span data-stu-id="a3359-412">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="a3359-413">Сценарии [пост-конфигурации](#options-post-configuration) позволяют задать или изменить параметры после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-413">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="a3359-414">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsFactory%601> отвечает за создание экземпляров параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-414"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="a3359-415">Он имеет единственный метод <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-415">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="a3359-416">При реализации по умолчанию принимаются все зарегистрированные интерфейсы <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>. Также сначала выполняются все основные настройки, а затем действия после конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-416">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="a3359-417">Она различает интерфейсы <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> и <xref:Microsoft.Extensions.Options.IConfigureOptions%601> и вызывает только соответствующий интерфейс.</span><span class="sxs-lookup"><span data-stu-id="a3359-417">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="a3359-418"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> используется интерфейсом <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для записи экземпляров `TOptions` в кэш.</span><span class="sxs-lookup"><span data-stu-id="a3359-418"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="a3359-419"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> делает экземпляры параметров в мониторе недействительными, что приводит к повторному вычислению значений (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="a3359-419">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="a3359-420">Значения можно также вводить вручную с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-420">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="a3359-421">Метод <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> используется, если необходимо повторно создать все именованные экземпляры по требованию.</span><span class="sxs-lookup"><span data-stu-id="a3359-421">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="a3359-422"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> полезно использовать в сценариях, когда параметры должны заново вычисляться при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="a3359-422"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="a3359-423">Дополнительные сведения см. в разделе [Повторная загрузка данных конфигурации с помощью IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="a3359-423">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="a3359-424"><xref:Microsoft.Extensions.Options.IOptions%601> можно использовать, чтобы обеспечить поддержку определенных параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-424"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="a3359-425">Но <xref:Microsoft.Extensions.Options.IOptions%601> не поддерживается в описанных выше сценариях <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-425">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="a3359-426">Вы можете продолжать использовать <xref:Microsoft.Extensions.Options.IOptions%601> в существующих платформах и библиотеках, которые уже используют интерфейс <xref:Microsoft.Extensions.Options.IOptions%601>. В таком случае вам не потребуются сценарии, предоставляемые <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-426">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="a3359-427">Конфигурация общих параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-427">General options configuration</span></span>

<span data-ttu-id="a3359-428">Конфигурация общих параметров демонстрируется в примере &num;1 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-428">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="a3359-429">Класс параметров должен быть неабстрактным с открытым конструктором без параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-429">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="a3359-430">Приведенный ниже класс (`MyOptions`) имеет два свойства: `Option1` и `Option2`.</span><span class="sxs-lookup"><span data-stu-id="a3359-430">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="a3359-431">Задавать значения по умолчанию необязательно, однако конструктор класса в этом примере задает значение по умолчанию для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a3359-431">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="a3359-432">Значение по умолчанию для `Option2` устанавливается путем прямой инициализации (*Models/MyOptions.cs*).</span><span class="sxs-lookup"><span data-stu-id="a3359-432">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="a3359-433">Класс `MyOptions` добавляется в контейнер службы с помощью интерфейса <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> и привязывается к конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-433">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="a3359-434">В следующей страничной модели применяется [внедрение зависимостей конструктора](xref:mvc/controllers/dependency-injection) с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> для доступа к параметрам (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-434">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="a3359-435">В файле *appsettings.json* образца задаются значения для свойств `option1` и `option2`:</span><span class="sxs-lookup"><span data-stu-id="a3359-435">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="a3359-436">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-436">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="a3359-437">При использовании настраиваемого компонента <xref:System.Configuration.ConfigurationBuilder> для загрузки конфигурации параметров из файла настроек необходимо убедиться, что базовый путь задан правильно:</span><span class="sxs-lookup"><span data-stu-id="a3359-437">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="a3359-438">При загрузке конфигурации параметров из файла настроек с помощью <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> явно задавать базовый путь не требуется.</span><span class="sxs-lookup"><span data-stu-id="a3359-438">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="a3359-439">Настройка простых параметров с помощью делегата</span><span class="sxs-lookup"><span data-stu-id="a3359-439">Configure simple options with a delegate</span></span>

<span data-ttu-id="a3359-440">Настройка простых параметров с помощью делегата демонстрируется в примере &num;2 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-440">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="a3359-441">Используйте делегат для задания значений параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-441">Use a delegate to set options values.</span></span> <span data-ttu-id="a3359-442">В образце приложения используется класс `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-442">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="a3359-443">В приведенном ниже коде в контейнер службы добавляется еще одна служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-443">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a3359-444">Она использует делегат для настройки привязки к `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="a3359-444">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="a3359-445">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a3359-445">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="a3359-446">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-446">You can add multiple configuration providers.</span></span> <span data-ttu-id="a3359-447">Поставщики конфигурации доступны в пакетах NuGet и применяются в порядке регистрации.</span><span class="sxs-lookup"><span data-stu-id="a3359-447">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="a3359-448">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a3359-448">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="a3359-449">При каждом вызове <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601> добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="a3359-449">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="a3359-450">В предыдущем примере значения `Option1` и `Option2` задаются в файле *appsettings.json*, однако значения `Option1` и `Option2` переопределяются настроенным делегатом.</span><span class="sxs-lookup"><span data-stu-id="a3359-450">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="a3359-451">Если включено более одной службы конфигурации, последний источник конфигурации *имеет приоритет* и определяет значение конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-451">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="a3359-452">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-452">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="a3359-453">Конфигурация подпараметров</span><span class="sxs-lookup"><span data-stu-id="a3359-453">Suboptions configuration</span></span>

<span data-ttu-id="a3359-454">Конфигурация подпараметров демонстрируется в примере &num;3 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-454">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="a3359-455">В приложениях должны создаваться классы приложений, относящиеся к определенным группам сценариев (классам).</span><span class="sxs-lookup"><span data-stu-id="a3359-455">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="a3359-456">Части приложения, которым требуются значения конфигурации, должны иметь доступ только к используемым ими значениям конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a3359-456">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="a3359-457">При привязке параметров к конфигурации каждое свойство, относящееся к типу параметров, привязывается к ключу конфигурации в формате `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="a3359-457">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="a3359-458">Например, свойство `MyOptions.Option1` привязывается к ключу `Option1`, который считывается из свойства `option1` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-458">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="a3359-459">В приведенном ниже коде в контейнер службы добавляется третья служба <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-459">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a3359-460">Она привязывает `MySubOptions` к разделу `subsection` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-460">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="a3359-461">Методу расширения `GetSection` требуется пакет NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="a3359-461">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="a3359-462">Если приложение использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии), пакет включается автоматически.</span><span class="sxs-lookup"><span data-stu-id="a3359-462">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="a3359-463">В файле *appsettings.json* образца определяется член `subsection` с ключами для `suboption1` и `suboption2`.</span><span class="sxs-lookup"><span data-stu-id="a3359-463">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="a3359-464">Класс `MySubOptions` определяет свойства `SubOption1` и `SubOption2` для хранения значений параметров (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-464">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="a3359-465">Метод `OnGet` страничной модели возвращает строку со значениями параметров (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-465">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="a3359-466">Когда приложение выполняется, метод `OnGet` возвращает строку со значениями класса подпараметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-466">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="a3359-467">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления</span><span class="sxs-lookup"><span data-stu-id="a3359-467">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="a3359-468">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления, демонстрируются в примере &num;4 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-468">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="a3359-469">Параметры могут передаваться в модель представления или путем внедрения <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> непосредственно в представление (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-469">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="a3359-470">В примере приложения показано, как внедрить `IOptionsMonitor<MyOptions>` с директивой `@inject`.</span><span class="sxs-lookup"><span data-stu-id="a3359-470">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="a3359-471">Когда запускается приложение, значения параметров появляются на отображаемой странице:</span><span class="sxs-lookup"><span data-stu-id="a3359-471">When the app is run, the options values are shown in the rendered page:</span></span>

![Значения параметров Option1: value1_from_json и Option2: –1 загружаются из модели и путем внедрения в представление.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="a3359-473">Повторная загрузка данных конфигурации с помощью IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="a3359-473">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="a3359-474">Повторная загрузка данных конфигурации с помощью <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> демонстрируется в примере &num;5 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-474">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="a3359-475">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> поддерживает повторную загрузку параметров с минимальными затратами на обработку.</span><span class="sxs-lookup"><span data-stu-id="a3359-475"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="a3359-476">Параметры вычисляются один раз на каждый запрос при обращении к ним и кэшируются на все время существования запроса.</span><span class="sxs-lookup"><span data-stu-id="a3359-476">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="a3359-477">В приведенном ниже примере демонстрируется создание экземпляра <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> после изменения файла *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="a3359-477">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="a3359-478">Несколько запросов к серверу возвращают константные значения, предоставляемые файлом *appsettings.json*, пока файл не изменится и конфигурация не загрузится повторно.</span><span class="sxs-lookup"><span data-stu-id="a3359-478">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="a3359-479">Ниже показаны начальные значения `option1` и `option2`, загруженные из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-479">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="a3359-480">Измените значения в файле *appsettings.json* на `value1_from_json UPDATED` и `200`.</span><span class="sxs-lookup"><span data-stu-id="a3359-480">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="a3359-481">Сохраните файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-481">Save the *appsettings.json* file.</span></span> <span data-ttu-id="a3359-482">Обновите окно браузера, чтобы увидеть обновленные значения параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-482">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="a3359-483">Поддержка именованных параметров в IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="a3359-483">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="a3359-484">Включение поддержки именованных параметров в <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> демонстрируется в примере &num;6 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="a3359-484">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="a3359-485">Поддержка *именованных параметров* позволяет приложению различать конфигурации именованных параметров.</span><span class="sxs-lookup"><span data-stu-id="a3359-485">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="a3359-486">В примере приложения именованные параметры должны быть объявлены с помощью элемента [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), который вызывает метод расширения [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="a3359-486">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="a3359-487">Пример приложения обращается к именованным параметрам с помощью метода <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a3359-487">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="a3359-488">При запуске образца приложения возвращаются именованные параметры:</span><span class="sxs-lookup"><span data-stu-id="a3359-488">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="a3359-489">Значения `named_options_1` предоставляются из конфигурации, которая загружается из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a3359-489">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="a3359-490">Значения `named_options_2` предоставляются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a3359-490">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="a3359-491">Делегатом `named_options_2` в `ConfigureServices` для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a3359-491">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="a3359-492">Значение по умолчанию для `Option2` предоставляется классом `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="a3359-492">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="a3359-493">Настройка всех параметров с помощью метода ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="a3359-493">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="a3359-494">Настройте все экземпляры параметров с помощью метода <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-494">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="a3359-495">Приведенный ниже код настраивает `Option1` для всех экземпляров конфигурации с общим значением.</span><span class="sxs-lookup"><span data-stu-id="a3359-495">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="a3359-496">Добавьте следующий код в метод `Startup.ConfigureServices` вручную:</span><span class="sxs-lookup"><span data-stu-id="a3359-496">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="a3359-497">Если запустить образец приложения после добавления, результат будет следующим:</span><span class="sxs-lookup"><span data-stu-id="a3359-497">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="a3359-498">Все параметры являются именованными экземплярами.</span><span class="sxs-lookup"><span data-stu-id="a3359-498">All options are named instances.</span></span> <span data-ttu-id="a3359-499">Существующие экземпляры <xref:Microsoft.Extensions.Options.IConfigureOptions%601> считаются нацеленными на экземпляр `Options.DefaultName`, который имеет значение `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="a3359-499">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="a3359-500">Интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> также реализует интерфейс <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-500"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="a3359-501">Реализация <xref:Microsoft.Extensions.Options.IOptionsFactory%601> по умолчанию содержит логику для надлежащего использования каждого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="a3359-501">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="a3359-502">Именованный параметр `null` предназначен для всех именованных экземпляров, а не для какого-то определенного (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> и <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> следуют этому соглашению).</span><span class="sxs-lookup"><span data-stu-id="a3359-502">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="a3359-503">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="a3359-503">OptionsBuilder API</span></span>

<span data-ttu-id="a3359-504"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> используется для настройки экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a3359-504"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="a3359-505">`OptionsBuilder` упрощает создание именованных параметров, так как он является единственным параметром для первоначального вызова `AddOptions<TOptions>(string optionsName)` и не должен появляться во всех последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="a3359-505">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="a3359-506">Проверка параметров и перегрузки `ConfigureOptions`, принимающие зависимости службы, доступны только через `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a3359-506">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="a3359-507">Использование служб внедрения зависимостей для настройки параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-507">Use DI services to configure options</span></span>

<span data-ttu-id="a3359-508">Вы можете получить доступ к другим службам, доступным в результате внедрения зависимостей при настройке параметров, двумя способами.</span><span class="sxs-lookup"><span data-stu-id="a3359-508">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="a3359-509">Передайте делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) в [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="a3359-509">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="a3359-510">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) предоставляет перегрузки метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), которые позволяют использовать до пяти служб для настройки параметров:</span><span class="sxs-lookup"><span data-stu-id="a3359-510">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="a3359-511">Создайте собственный тип, реализующий <xref:Microsoft.Extensions.Options.IConfigureOptions%601> или <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, и зарегистрируйте этот тип как службу.</span><span class="sxs-lookup"><span data-stu-id="a3359-511">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="a3359-512">Рекомендуем передать делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), так как создать службу сложнее.</span><span class="sxs-lookup"><span data-stu-id="a3359-512">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="a3359-513">Создание собственного типа эквивалентно тому, что делает платформа при использовании метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="a3359-513">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="a3359-514">При вызове метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) регистрируется временный универсальный интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, имеющий конструктор, который принимает указанные универсальные типы службы.</span><span class="sxs-lookup"><span data-stu-id="a3359-514">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="a3359-515">Пост-конфигурация параметров</span><span class="sxs-lookup"><span data-stu-id="a3359-515">Options post-configuration</span></span>

<span data-ttu-id="a3359-516">Задайте пост-конфигурацию с помощью <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-516">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="a3359-517">Пост-конфигурация применяется после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a3359-517">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a3359-518">Для последующей настройки именованных параметров доступен метод <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-518"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a3359-519">Для последующей настройки всех экземпляров конфигурации служит метод <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="a3359-519">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="a3359-520">Доступ к параметрам во время запуска</span><span class="sxs-lookup"><span data-stu-id="a3359-520">Accessing options during startup</span></span>

<span data-ttu-id="a3359-521"><xref:Microsoft.Extensions.Options.IOptions%601> и <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> можно использовать в методе `Startup.Configure`, так как службы создаются до выполнения метода `Configure`.</span><span class="sxs-lookup"><span data-stu-id="a3359-521"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="a3359-522">Не используйте <xref:Microsoft.Extensions.Options.IOptions%601> или <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a3359-522">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a3359-523">Из-за очередности регистрации служб состояние параметров может быть несогласованным.</span><span class="sxs-lookup"><span data-stu-id="a3359-523">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a3359-524">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a3359-524">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
