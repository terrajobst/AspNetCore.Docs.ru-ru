---
title: Шаблон параметров в ASP.NET Core
author: guardrex
description: Узнайте, как использовать шаблон параметров для представления групп связанных параметров в приложениях ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 8d219658b7116ad7e1f2a73678ef76ea1ce69595
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209191"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="bcf7a-103">Шаблон параметров в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bcf7a-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="bcf7a-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="bcf7a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="bcf7a-105">Для версии 1.1 этого раздела скачайте статью [Шаблон параметров в ASP.NET Core (версия 1.1, PDF-файл)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-105">For the 1.1 version of this topic, download [Options pattern in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="bcf7a-106">Шаблон параметров использует классы для представления групп связанных настроек.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-106">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="bcf7a-107">Когда [параметры конфигурации](xref:fundamentals/configuration/index) изолируются по сценарию в отдельных классах, в приложениях соблюдаются два важных принципа программной инженерии.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-107">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="bcf7a-108">[Принцип изоляции интерфейсов или инкапсуляция](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation). Сценарии (классы), которые зависят от параметров конфигурации, зависят только от используемых ими параметров конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-108">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="bcf7a-109">[Разделение ответственности](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns). Параметры для разных частей приложения не зависят друг от друга и не связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-109">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="bcf7a-110">В параметрах также предусмотрен механизм для проверки данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-110">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="bcf7a-111">Дополнительные сведения см. в разделе [Проверка параметров](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-111">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="bcf7a-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bcf7a-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bcf7a-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="bcf7a-113">Prerequisites</span></span>

<span data-ttu-id="bcf7a-114">Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет в пакет [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-114">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="bcf7a-115">Интерфейсы параметров</span><span class="sxs-lookup"><span data-stu-id="bcf7a-115">Options interfaces</span></span>

<span data-ttu-id="bcf7a-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> используется для извлечения параметров и управления уведомлениями о параметрах для экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="bcf7a-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> поддерживается в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> supports the following scenarios:</span></span>

* <span data-ttu-id="bcf7a-118">уведомления об изменениях;</span><span class="sxs-lookup"><span data-stu-id="bcf7a-118">Change notifications</span></span>
* <span data-ttu-id="bcf7a-119">[именованные параметры](#named-options-support-with-iconfigurenamedoptions);</span><span class="sxs-lookup"><span data-stu-id="bcf7a-119">[Named options](#named-options-support-with-iconfigurenamedoptions)</span></span>
* <span data-ttu-id="bcf7a-120">[перезагружаемые конфигурации](#reload-configuration-data-with-ioptionssnapshot);</span><span class="sxs-lookup"><span data-stu-id="bcf7a-120">[Reloadable configuration](#reload-configuration-data-with-ioptionssnapshot)</span></span>
* <span data-ttu-id="bcf7a-121">объявление определенных параметров недействительными (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-121">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)</span></span>

<span data-ttu-id="bcf7a-122">Сценарии [пост-конфигурации](#options-post-configuration) позволяют задать или изменить параметры после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-122">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions`1> configuration occurs.</span></span>

<span data-ttu-id="bcf7a-123">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsFactory`1> отвечает за создание экземпляров параметров.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-123"><xref:Microsoft.Extensions.Options.IOptionsFactory`1> is responsible for creating new options instances.</span></span> <span data-ttu-id="bcf7a-124">Он имеет единственный метод <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-124">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="bcf7a-125">При реализации по умолчанию принимаются все зарегистрированные интерфейсы <xref:Microsoft.Extensions.Options.IConfigureOptions`1> и <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>. Также сначала выполняются все основные настройки, а затем действия после конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-125">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions`1> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="bcf7a-126">Она различает интерфейсы <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> и <xref:Microsoft.Extensions.Options.IConfigureOptions`1> и вызывает только соответствующий интерфейс.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-126">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> and <xref:Microsoft.Extensions.Options.IConfigureOptions`1> and only calls the appropriate interface.</span></span>

<span data-ttu-id="bcf7a-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> используется интерфейсом <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> для записи экземпляров `TOptions` в кэш.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> to cache `TOptions` instances.</span></span> <span data-ttu-id="bcf7a-128"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> делает экземпляры параметров в мониторе недействительными, что приводит к повторному вычислению значений (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-128">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="bcf7a-129">Значения можно также вводить вручную с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-129">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="bcf7a-130">Метод <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> используется, если необходимо повторно создать все именованные экземпляры по требованию.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-130">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="bcf7a-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> полезно использовать в сценариях, когда параметры должны заново вычисляться при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="bcf7a-132">Дополнительные сведения см. в разделе [Повторная загрузка данных конфигурации с помощью IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-132">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="bcf7a-133"><xref:Microsoft.Extensions.Options.IOptions`1> можно использовать, чтобы обеспечить поддержку определенных параметров.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-133"><xref:Microsoft.Extensions.Options.IOptions`1> can be used to support options.</span></span> <span data-ttu-id="bcf7a-134">Но <xref:Microsoft.Extensions.Options.IOptions`1> не поддерживается в описанных выше сценариях <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-134">However, <xref:Microsoft.Extensions.Options.IOptions`1> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span></span> <span data-ttu-id="bcf7a-135">Вы можете продолжать использовать <xref:Microsoft.Extensions.Options.IOptions`1> в существующих платформах и библиотеках, которые уже используют интерфейс <xref:Microsoft.Extensions.Options.IOptions`1>. В таком случае вам не потребуются сценарии, предоставляемые <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-135">You may continue to use <xref:Microsoft.Extensions.Options.IOptions`1> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions`1> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="bcf7a-136">Конфигурация общих параметров</span><span class="sxs-lookup"><span data-stu-id="bcf7a-136">General options configuration</span></span>

<span data-ttu-id="bcf7a-137">Конфигурация общих параметров демонстрируется в примере &num;1 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-137">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="bcf7a-138">Класс параметров должен быть неабстрактным с открытым конструктором без параметров.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-138">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="bcf7a-139">Приведенный ниже класс (`MyOptions`) имеет два свойства: `Option1` и `Option2`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-139">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="bcf7a-140">Задавать значения по умолчанию необязательно, однако конструктор класса в этом примере задает значение по умолчанию для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-140">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="bcf7a-141">Значение по умолчанию для `Option2` устанавливается путем прямой инициализации (*Models/MyOptions.cs*).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-141">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="bcf7a-142">Класс `MyOptions` добавляется в контейнер службы с помощью интерфейса <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> и привязывается к конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-142">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="bcf7a-143">В следующей страничной модели применяется [внедрение зависимостей конструктора](xref:mvc/controllers/dependency-injection) с помощью <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> для доступа к параметрам (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="bcf7a-143">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="bcf7a-144">В файле *appsettings.json* образца задаются значения для свойств `option1` и `option2`:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-144">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="bcf7a-145">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-145">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="bcf7a-146">При использовании настраиваемого компонента <xref:System.Configuration.ConfigurationBuilder> для загрузки конфигурации параметров из файла настроек необходимо убедиться, что базовый путь задан правильно:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-146">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="bcf7a-147">При загрузке конфигурации параметров из файла настроек с помощью <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> явно задавать базовый путь не требуется.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-147">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="bcf7a-148">Настройка простых параметров с помощью делегата</span><span class="sxs-lookup"><span data-stu-id="bcf7a-148">Configure simple options with a delegate</span></span>

<span data-ttu-id="bcf7a-149">Настройка простых параметров с помощью делегата демонстрируется в примере &num;2 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-149">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="bcf7a-150">Используйте делегат для задания значений параметров.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-150">Use a delegate to set options values.</span></span> <span data-ttu-id="bcf7a-151">В образце приложения используется класс `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="bcf7a-151">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="bcf7a-152">В приведенном ниже коде в контейнер службы добавляется еще одна служба <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-152">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service is added to the service container.</span></span> <span data-ttu-id="bcf7a-153">Она использует делегат для настройки привязки к `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-153">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="bcf7a-154">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-154">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="bcf7a-155">Можно добавить несколько поставщиков конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-155">You can add multiple configuration providers.</span></span> <span data-ttu-id="bcf7a-156">Поставщики конфигурации доступны в пакетах NuGet и применяются в порядке регистрации.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-156">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="bcf7a-157">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-157">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="bcf7a-158">При каждом вызове <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> служба <xref:Microsoft.Extensions.Options.IConfigureOptions`1> добавляется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-158">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service to the service container.</span></span> <span data-ttu-id="bcf7a-159">В предыдущем примере значения `Option1` и `Option2` задаются в файле *appsettings.json*, однако значения `Option1` и `Option2` переопределяются настроенным делегатом.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-159">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="bcf7a-160">Если включено более одной службы конфигурации, последний источник конфигурации *имеет приоритет* и определяет значение конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-160">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="bcf7a-161">Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-161">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="bcf7a-162">Конфигурация подпараметров</span><span class="sxs-lookup"><span data-stu-id="bcf7a-162">Suboptions configuration</span></span>

<span data-ttu-id="bcf7a-163">Конфигурация подпараметров демонстрируется в примере &num;3 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-163">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="bcf7a-164">В приложениях должны создаваться классы приложений, относящиеся к определенным группам сценариев (классам).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-164">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="bcf7a-165">Части приложения, которым требуются значения конфигурации, должны иметь доступ только к используемым ими значениям конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-165">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="bcf7a-166">При привязке параметров к конфигурации каждое свойство, относящееся к типу параметров, привязывается к ключу конфигурации в формате `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-166">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="bcf7a-167">Например, свойство `MyOptions.Option1` привязывается к ключу `Option1`, который считывается из свойства `option1` в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-167">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="bcf7a-168">В приведенном ниже коде в контейнер службы добавляется третья служба <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-168">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service is added to the service container.</span></span> <span data-ttu-id="bcf7a-169">Она привязывает `MySubOptions` к разделу `subsection` файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-169">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="bcf7a-170">Методу расширения `GetSection` требуется пакет NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-170">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="bcf7a-171">Если приложение использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии), пакет включается автоматически.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-171">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="bcf7a-172">В файле *appsettings.json* образца определяется член `subsection` с ключами для `suboption1` и `suboption2`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-172">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="bcf7a-173">Класс `MySubOptions` определяет свойства `SubOption1` и `SubOption2` для хранения значений параметров (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="bcf7a-173">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="bcf7a-174">Метод `OnGet` страничной модели возвращает строку со значениями параметров (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="bcf7a-174">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="bcf7a-175">Когда приложение выполняется, метод `OnGet` возвращает строку со значениями класса подпараметров:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-175">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="bcf7a-176">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления</span><span class="sxs-lookup"><span data-stu-id="bcf7a-176">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="bcf7a-177">Параметры, предоставляемые моделью представления или посредством прямого внедрения представления, демонстрируются в примере &num;4 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-177">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="bcf7a-178">Параметры могут передаваться в модель представления или путем внедрения <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> непосредственно в представление (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="bcf7a-178">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="bcf7a-179">В примере приложения показано, как внедрить `IOptionsMonitor<MyOptions>` с директивой `@inject`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-179">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="bcf7a-180">Когда запускается приложение, значения параметров появляются на отображаемой странице:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-180">When the app is run, the options values are shown in the rendered page:</span></span>

![Значения параметров Option1: value1_from_json и Option2: –1 загружаются из модели и путем внедрения в представление.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="bcf7a-182">Повторная загрузка данных конфигурации с помощью IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="bcf7a-182">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="bcf7a-183">Повторная загрузка данных конфигурации с помощью <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> демонстрируется в примере &num;5 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-183">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="bcf7a-184">Интерфейс <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> поддерживает повторную загрузку параметров с минимальными затратами на обработку.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-184"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="bcf7a-185">Параметры вычисляются один раз на каждый запрос при обращении к ним и кэшируются на все время существования запроса.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-185">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="bcf7a-186">В приведенном ниже примере демонстрируется создание экземпляра <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> после изменения файла *appsettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-186">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="bcf7a-187">Несколько запросов к серверу возвращают константные значения, предоставляемые файлом *appsettings.json*, пока файл не изменится и конфигурация не загрузится повторно.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-187">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="bcf7a-188">Ниже показаны начальные значения `option1` и `option2`, загруженные из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-188">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="bcf7a-189">Измените значения в файле *appsettings.json* на `value1_from_json UPDATED` и `200`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-189">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="bcf7a-190">Сохраните файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-190">Save the *appsettings.json* file.</span></span> <span data-ttu-id="bcf7a-191">Обновите окно браузера, чтобы увидеть обновленные значения параметров:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-191">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="bcf7a-192">Поддержка именованных параметров в IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="bcf7a-192">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="bcf7a-193">Включение поддержки именованных параметров в <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> демонстрируется в примере &num;6 в примере приложения.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-193">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="bcf7a-194">Поддержка *именованных параметров* позволяет приложению различать конфигурации именованных параметров.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-194">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="bcf7a-195">В примере приложения именованные параметры должны быть объявлены с помощью элемента [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), который вызывает метод расширения [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="bcf7a-195">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="bcf7a-196">Пример приложения обращается к именованным параметрам с помощью метода <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="bcf7a-196">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="bcf7a-197">При запуске образца приложения возвращаются именованные параметры:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-197">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="bcf7a-198">Значения `named_options_1` предоставляются из конфигурации, которая загружается из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-198">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="bcf7a-199">Значения `named_options_2` предоставляются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-199">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="bcf7a-200">Делегатом `named_options_2` в `ConfigureServices` для `Option1`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-200">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="bcf7a-201">Значение по умолчанию для `Option2` предоставляется классом `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-201">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="bcf7a-202">Настройка всех параметров с помощью метода ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="bcf7a-202">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="bcf7a-203">Настройте все экземпляры параметров с помощью метода <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-203">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="bcf7a-204">Приведенный ниже код настраивает `Option1` для всех экземпляров конфигурации с общим значением.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-204">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="bcf7a-205">Добавьте следующий код в метод `Startup.ConfigureServices` вручную:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-205">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="bcf7a-206">Если запустить образец приложения после добавления, результат будет следующим:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-206">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="bcf7a-207">Все параметры являются именованными экземплярами.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-207">All options are named instances.</span></span> <span data-ttu-id="bcf7a-208">Существующие экземпляры <xref:Microsoft.Extensions.Options.IConfigureOptions`1> считаются нацеленными на экземпляр `Options.DefaultName`, который имеет значение `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-208">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions`1> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="bcf7a-209">Интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> также реализует интерфейс <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-209"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span></span> <span data-ttu-id="bcf7a-210">Реализация <xref:Microsoft.Extensions.Options.IOptionsFactory`1> по умолчанию содержит логику для надлежащего использования каждого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-210">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory`1> has logic to use each appropriately.</span></span> <span data-ttu-id="bcf7a-211">Именованный параметр `null` предназначен для всех именованных экземпляров, а не для какого-то определенного (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> и <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> следуют этому соглашению).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-211">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="bcf7a-212">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="bcf7a-212">OptionsBuilder API</span></span>

<span data-ttu-id="bcf7a-213"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> используется для настройки экземпляров `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-213"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="bcf7a-214">`OptionsBuilder` упрощает создание именованных параметров, так как он является единственным параметром для первоначального вызова `AddOptions<TOptions>(string optionsName)` и не должен появляться во всех последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-214">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="bcf7a-215">Проверка параметров и перегрузки `ConfigureOptions`, принимающие зависимости службы, доступны только через `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-215">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="bcf7a-216">Использование служб внедрения зависимостей для настройки параметров</span><span class="sxs-lookup"><span data-stu-id="bcf7a-216">Use DI services to configure options</span></span>

<span data-ttu-id="bcf7a-217">Вы можете получить доступ к другим службам, доступным в результате внедрения зависимостей при настройке параметров, двумя способами.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-217">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="bcf7a-218">Передайте делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) в [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-218">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="bcf7a-219">[OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) предоставляет перегрузки метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), которые позволяют использовать до пяти служб для настройки параметров:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-219">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="bcf7a-220">Создайте собственный тип, реализующий <xref:Microsoft.Extensions.Options.IConfigureOptions`1> или <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, и зарегистрируйте этот тип как службу.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-220">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions`1> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> and register the type as a service.</span></span>

<span data-ttu-id="bcf7a-221">Рекомендуем передать делегат конфигурации методу [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), так как создать службу сложнее.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-221">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="bcf7a-222">Создание собственного типа эквивалентно тому, что делает платформа при использовании метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-222">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="bcf7a-223">При вызове метода [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) регистрируется временный универсальный интерфейс <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, имеющий конструктор, который принимает указанные универсальные типы службы.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-223">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, which has a constructor that accepts the generic service types specified.</span></span> 

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="bcf7a-224">Проверка параметров</span><span class="sxs-lookup"><span data-stu-id="bcf7a-224">Options validation</span></span>

<span data-ttu-id="bcf7a-225">Эта функция позволяет проверить параметры при их настройке.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-225">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="bcf7a-226">Вызовите `Validate` с использованием метода проверки, который возвращает `true`, если параметры являются допустимыми, и `false`, если они являются недопустимыми:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-226">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="bcf7a-227">В предыдущем примере для именованного экземпляра параметров задается `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-227">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="bcf7a-228">Экземпляр параметров по умолчанию — `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-228">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="bcf7a-229">Проверка выполняется при создании экземпляра параметров.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-229">Validation runs when the options instance is created.</span></span> <span data-ttu-id="bcf7a-230">Экземпляр параметров гарантированно проходит проверку при первом обращении.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-230">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bcf7a-231">Проверка параметров не защищает от изменений, вносимых после исходной настройки и проверки параметров.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-231">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="bcf7a-232">Метод `Validate` принимает `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-232">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="bcf7a-233">Чтобы полностью настроить проверку, реализуйте `IValidateOptions<TOptions>`, чтобы:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-233">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="bcf7a-234">выполнить проверку нескольких типов параметров — `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`;</span><span class="sxs-lookup"><span data-stu-id="bcf7a-234">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="bcf7a-235">выполнить проверку, зависящую от другого типа параметра — `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-235">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="bcf7a-236">`IValidateOptions` проверяет:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-236">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="bcf7a-237">определенный именованный экземпляр параметров;</span><span class="sxs-lookup"><span data-stu-id="bcf7a-237">A specific named options instance.</span></span>
* <span data-ttu-id="bcf7a-238">все параметры, при которых `name` имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-238">All options when `name` is `null`.</span></span>

<span data-ttu-id="bcf7a-239">Возвращает `ValidateOptionsResult` из реализации интерфейса:</span><span class="sxs-lookup"><span data-stu-id="bcf7a-239">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="bcf7a-240">Проверка на основе заметок к данным доступна в пакете [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) с помощью вызова метода `ValidateDataAnnotations` в `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-240">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the `ValidateDataAnnotations` method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="bcf7a-241">`Microsoft.Extensions.Options.DataAnnotations` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="bcf7a-241">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 or later).</span></span>

```csharp
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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="bcf7a-242">Безотложная проверка (быстрое прекращение при запуске) находится на этапе рассмотрения для включения в будущие выпуски.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-242">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

::: moniker-end

## <a name="options-post-configuration"></a><span data-ttu-id="bcf7a-243">Пост-конфигурация параметров</span><span class="sxs-lookup"><span data-stu-id="bcf7a-243">Options post-configuration</span></span>

<span data-ttu-id="bcf7a-244">Задайте пост-конфигурацию с помощью <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-244">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>.</span></span> <span data-ttu-id="bcf7a-245">Пост-конфигурация применяется после выполнения всех действий настройки с помощью <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-245">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions`1> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bcf7a-246">Для последующей настройки именованных параметров доступен метод <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-246"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="bcf7a-247">Для последующей настройки всех экземпляров конфигурации служит метод <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-247">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="bcf7a-248">Доступ к параметрам во время запуска</span><span class="sxs-lookup"><span data-stu-id="bcf7a-248">Accessing options during startup</span></span>

<span data-ttu-id="bcf7a-249"><xref:Microsoft.Extensions.Options.IOptions`1> и <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> можно использовать в методе `Startup.Configure`, так как службы создаются до выполнения метода `Configure`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-249"><xref:Microsoft.Extensions.Options.IOptions`1> and <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="bcf7a-250">Не используйте <xref:Microsoft.Extensions.Options.IOptions`1> или <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-250">Don't use <xref:Microsoft.Extensions.Options.IOptions`1> or <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bcf7a-251">Из-за очередности регистрации служб состояние параметров может быть несогласованным.</span><span class="sxs-lookup"><span data-stu-id="bcf7a-251">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bcf7a-252">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bcf7a-252">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
