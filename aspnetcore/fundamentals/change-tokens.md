---
title: Обнаружение изменений с помощью токенов изменений в ASP.NET Core
author: guardrex
description: Сведения об использовании токенов изменений для отслеживания изменений.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/07/2019
uid: fundamentals/change-tokens
ms.openlocfilehash: bb30d7a4c7dc82200821c60a49c314b246562111
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007206"
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="4d340-103">Обнаружение изменений с помощью токенов изменений в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d340-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="4d340-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="4d340-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4d340-105">*Токен изменений* — это низкоуровневый стандартный блок общего назначения, используемый для отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="4d340-105">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="4d340-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4d340-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="4d340-107">Интерфейс IChangeToken</span><span class="sxs-lookup"><span data-stu-id="4d340-107">IChangeToken interface</span></span>

<span data-ttu-id="4d340-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> распространяет уведомления о том, что произошло изменение.</span><span class="sxs-lookup"><span data-stu-id="4d340-108"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="4d340-109">`IChangeToken` находится в пространстве имен <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="4d340-109">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="4d340-110">Пакет [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet явно предоставляется приложениям ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d340-110">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="4d340-111">`IChangeToken` имеет два свойства:</span><span class="sxs-lookup"><span data-stu-id="4d340-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="4d340-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> указывает, выполняет ли токен обратные вызовы с упреждением.</span><span class="sxs-lookup"><span data-stu-id="4d340-112"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="4d340-113">Если для `ActiveChangedCallbacks` задано значение `false`, обратный вызов не выполняется, а приложению нужно опросить `HasChanged` на предмет изменений.</span><span class="sxs-lookup"><span data-stu-id="4d340-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="4d340-114">Кроме того, отмена токена может никогда не произойти в случае отсутствия изменений или отключения либо удаления базового прослушивателя изменений.</span><span class="sxs-lookup"><span data-stu-id="4d340-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="4d340-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> получает значение, указывающее, произошло ли изменение.</span><span class="sxs-lookup"><span data-stu-id="4d340-115"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="4d340-116">Интерфейс `IChangeToken` включает метод [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*), который регистрирует обратный вызов, выполняемый при изменении токена.</span><span class="sxs-lookup"><span data-stu-id="4d340-116">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="4d340-117">Перед выполнением обратного вызова нужно задать `HasChanged`.</span><span class="sxs-lookup"><span data-stu-id="4d340-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="4d340-118">Класс ChangeToken</span><span class="sxs-lookup"><span data-stu-id="4d340-118">ChangeToken class</span></span>

<span data-ttu-id="4d340-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> — это статический класс, который распространяет уведомления о том, что произошло изменение.</span><span class="sxs-lookup"><span data-stu-id="4d340-119"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="4d340-120">`ChangeToken` находится в пространстве имен <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="4d340-120">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="4d340-121">Пакет [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet явно предоставляется приложениям ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d340-121">The [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package is implicitly provided to the ASP.NET Core apps.</span></span>

<span data-ttu-id="4d340-122">Метод [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) регистрирует объект `Action`, вызываемый при изменении токена:</span><span class="sxs-lookup"><span data-stu-id="4d340-122">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="4d340-123">`Func<IChangeToken>` создает токен.</span><span class="sxs-lookup"><span data-stu-id="4d340-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="4d340-124">`Action` вызывается при изменении токена.</span><span class="sxs-lookup"><span data-stu-id="4d340-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="4d340-125">Перегрузка [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) принимает дополнительный параметр `TState`, передаваемый в объект-получатель токена `Action`.</span><span class="sxs-lookup"><span data-stu-id="4d340-125">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="4d340-126">`OnChange` возвращает <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="4d340-126">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="4d340-127">При вызове <xref:System.IDisposable.Dispose*> токен прекращает прослушивать дальнейшие изменения и освобождает свои ресурсы.</span><span class="sxs-lookup"><span data-stu-id="4d340-127">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="4d340-128">Примеры использования токенов изменений в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d340-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="4d340-129">Токены изменений используются в ключевых областях ASP.NET Core для отслеживания изменений в объектах:</span><span class="sxs-lookup"><span data-stu-id="4d340-129">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="4d340-130">Чтобы отслеживать изменения в файлах, метод <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> интерфейса <xref:Microsoft.Extensions.FileProviders.IFileProvider> создает `IChangeToken` для указанных файлов или папки.</span><span class="sxs-lookup"><span data-stu-id="4d340-130">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="4d340-131">Токены `IChangeToken` можно добавлять в записи кэша, чтобы активировать вытеснения кэша при изменении.</span><span class="sxs-lookup"><span data-stu-id="4d340-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="4d340-132">Для изменений `TOptions` используемая по умолчанию реализация <xref:Microsoft.Extensions.Options.OptionsMonitor`1> интерфейса <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> имеет перегрузку, которая принимает один или несколько экземпляров <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>.</span><span class="sxs-lookup"><span data-stu-id="4d340-132">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="4d340-133">Каждый экземпляр возвращает `IChangeToken`, чтобы зарегистрировать обратный вызов уведомления об изменении для отслеживания изменений параметров.</span><span class="sxs-lookup"><span data-stu-id="4d340-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="4d340-134">Отслеживание изменений конфигурации</span><span class="sxs-lookup"><span data-stu-id="4d340-134">Monitor for configuration changes</span></span>

<span data-ttu-id="4d340-135">По умолчанию шаблоны ASP.NET Core используют [файлы конфигурации JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json* и *appsettings.Production.json*) для загрузки параметров конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="4d340-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="4d340-136">Эти файлы настраиваются с помощью метода расширения [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) в <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, который принимает параметр `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="4d340-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="4d340-137">`reloadOnChange` указывает, нужно ли перезагружать конфигурацию при изменении файла.</span><span class="sxs-lookup"><span data-stu-id="4d340-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="4d340-138">Этот параметр отображается в удобном методе <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> класса <xref:Microsoft.Extensions.Hosting.Host>:</span><span class="sxs-lookup"><span data-stu-id="4d340-138">This setting appears in the <xref:Microsoft.Extensions.Hosting.Host> convenience method <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="4d340-139">Конфигурация на основе файла представлена <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="4d340-139">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="4d340-140">`FileConfigurationSource` использует <xref:Microsoft.Extensions.FileProviders.IFileProvider> для отслеживания файлов.</span><span class="sxs-lookup"><span data-stu-id="4d340-140">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="4d340-141">По умолчанию `IFileMonitor` предоставляется объектом <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, который использует <xref:System.IO.FileSystemWatcher> для отслеживания изменений в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-141">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="4d340-142">Этот пример приложения демонстрирует две реализации для отслеживания изменений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="4d340-143">Если какой-либо из файлов *appsettings* был изменен, каждая реализация отслеживания файлов выполняет пользовательский код &mdash; пример приложения записывает сообщение на консоль.</span><span class="sxs-lookup"><span data-stu-id="4d340-143">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="4d340-144">`FileSystemWatcher` файла конфигурации может активировать несколько обратных вызовов токена для одного изменения файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-144">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="4d340-145">Чтобы пользовательский код выполнялся один раз при активации нескольких обратных вызовов токена, реализация в примере проверяет хэши файлов.</span><span class="sxs-lookup"><span data-stu-id="4d340-145">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="4d340-146">В примере используется хэширование файлов SHA1.</span><span class="sxs-lookup"><span data-stu-id="4d340-146">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="4d340-147">Повторная попытка реализуется посредством экспоненциальной задержки.</span><span class="sxs-lookup"><span data-stu-id="4d340-147">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="4d340-148">Повторная попытка нужна, так как может возникать блокировка файлов, препятствующая вычислению нового хэша в файле.</span><span class="sxs-lookup"><span data-stu-id="4d340-148">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="4d340-149">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d340-149">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="4d340-150">Токен изменений для простого запуска</span><span class="sxs-lookup"><span data-stu-id="4d340-150">Simple startup change token</span></span>

<span data-ttu-id="4d340-151">Зарегистрируйте обратный вызов `Action` объекта-получателя токена для уведомлений об изменениях в токене перезагрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-151">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="4d340-152">В `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="4d340-152">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="4d340-153">`config.GetReloadToken()` предоставляет токен.</span><span class="sxs-lookup"><span data-stu-id="4d340-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="4d340-154">Обратный вызов является методом `InvokeChanged`:</span><span class="sxs-lookup"><span data-stu-id="4d340-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="4d340-155">`state` обратного вызова используется для передачи `IWebHostEnvironment`. Это удобно для определения правильного JSON-файла конфигурации *appsettings*, который требуется отслеживать (например, *appsettings.Development.json* при работе в среде разработки).</span><span class="sxs-lookup"><span data-stu-id="4d340-155">The `state` of the callback is used to pass in the `IWebHostEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="4d340-156">Хэши файлов используются для предотвращения многократного выполнения оператора `WriteConsole` из-за нескольких обратных вызовов токена при всего одном изменении файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-156">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="4d340-157">Эта система выполняется, пока запущено приложение, и не может быть отключена пользователем.</span><span class="sxs-lookup"><span data-stu-id="4d340-157">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="4d340-158">Отслеживание изменений конфигурации как служба</span><span class="sxs-lookup"><span data-stu-id="4d340-158">Monitor configuration changes as a service</span></span>

<span data-ttu-id="4d340-159">Этот пример реализует следующее:</span><span class="sxs-lookup"><span data-stu-id="4d340-159">The sample implements:</span></span>

* <span data-ttu-id="4d340-160">Базовое отслеживание токена запуска.</span><span class="sxs-lookup"><span data-stu-id="4d340-160">Basic startup token monitoring.</span></span>
* <span data-ttu-id="4d340-161">Отслеживание как служба.</span><span class="sxs-lookup"><span data-stu-id="4d340-161">Monitoring as a service.</span></span>
* <span data-ttu-id="4d340-162">Механизм для включения и отключения отслеживания.</span><span class="sxs-lookup"><span data-stu-id="4d340-162">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="4d340-163">Этот пример задает интерфейс `IConfigurationMonitor`.</span><span class="sxs-lookup"><span data-stu-id="4d340-163">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="4d340-164">*Extensions/ConfigurationMonitor.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d340-164">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="4d340-165">Конструктор реализованного класса `ConfigurationMonitor` регистрирует обратный вызов для уведомлений об изменениях:</span><span class="sxs-lookup"><span data-stu-id="4d340-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="4d340-166">`config.GetReloadToken()` предоставляет токен.</span><span class="sxs-lookup"><span data-stu-id="4d340-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="4d340-167">`InvokeChanged` является методом обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="4d340-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="4d340-168">`state` в этом экземпляре является ссылкой на экземпляр `IConfigurationMonitor`, используемый для доступа к состоянию мониторинга.</span><span class="sxs-lookup"><span data-stu-id="4d340-168">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="4d340-169">Используются два свойства:</span><span class="sxs-lookup"><span data-stu-id="4d340-169">Two properties are used:</span></span>

* <span data-ttu-id="4d340-170">`MonitoringEnabled` указывает, нужно ли обратному вызову выполнять свой пользовательский код.</span><span class="sxs-lookup"><span data-stu-id="4d340-170">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="4d340-171">`CurrentState` описывает текущее состояние отслеживания для использования в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="4d340-171">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="4d340-172">Метод `InvokeChanged` похож на описанный ранее подход, за исключением того, что он:</span><span class="sxs-lookup"><span data-stu-id="4d340-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="4d340-173">не выполняет свой код, если только `MonitoringEnabled` не имеет значение `true`;</span><span class="sxs-lookup"><span data-stu-id="4d340-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="4d340-174">выводит текущее значение `state` в своих выходных данных `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="4d340-174">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="4d340-175">Экземпляр `ConfigurationMonitor` регистрируется как служба в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4d340-175">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="4d340-176">Страница индекса позволяет пользователю управлять отслеживанием конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-176">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="4d340-177">Экземпляр `IConfigurationMonitor` внедряется в `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="4d340-177">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="4d340-178">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d340-178">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="4d340-179">Монитор конфигурации (`_monitor`) позволяет включить или отключить мониторинг и задать текущее состояние для обратной связи пользовательского интерфейса:</span><span class="sxs-lookup"><span data-stu-id="4d340-179">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="4d340-180">При активации `OnPostStartMonitoring` отслеживание включается, а текущее состояние сбрасывается.</span><span class="sxs-lookup"><span data-stu-id="4d340-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="4d340-181">При активации `OnPostStopMonitoring` отслеживание отключается, а состояние указывает на отсутствие отслеживания.</span><span class="sxs-lookup"><span data-stu-id="4d340-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="4d340-182">Кнопки пользовательского интерфейса для включения и отключения отслеживания.</span><span class="sxs-lookup"><span data-stu-id="4d340-182">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="4d340-183">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4d340-183">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="4d340-184">Отслеживание изменений кэшированных файлов</span><span class="sxs-lookup"><span data-stu-id="4d340-184">Monitor cached file changes</span></span>

<span data-ttu-id="4d340-185">Содержимое файла можно кэшировать в памяти с помощью <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="4d340-185">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="4d340-186">Кэширование в памяти описано в разделе [Кэш в памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="4d340-186">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="4d340-187">*Устаревшие* (просроченные) данные возвращаются из кэша при изменении источника исходных данных без каких-либо дополнительных действий, таких как описанная ниже реализация.</span><span class="sxs-lookup"><span data-stu-id="4d340-187">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="4d340-188">Например, если не учитывать состояние кэшированного исходного файла при продлении [скользящего срока действия](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration), это приведет к появлению в кэше устаревших данных файлов.</span><span class="sxs-lookup"><span data-stu-id="4d340-188">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="4d340-189">Каждый запрос к данным продляет скользящий срок действия, но этот файл никогда не загружается в кэш.</span><span class="sxs-lookup"><span data-stu-id="4d340-189">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="4d340-190">Любые функции приложения, использующие кэшированное содержимое, могут получить устаревшее содержимое.</span><span class="sxs-lookup"><span data-stu-id="4d340-190">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="4d340-191">Использование токенов изменений в сценарии кэширования файлов предотвращает появление устаревшего содержимого файлов в кэше.</span><span class="sxs-lookup"><span data-stu-id="4d340-191">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="4d340-192">Этот пример демонстрирует реализацию данного подхода.</span><span class="sxs-lookup"><span data-stu-id="4d340-192">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="4d340-193">`GetFileContent` в примере используется для:</span><span class="sxs-lookup"><span data-stu-id="4d340-193">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="4d340-194">возврата содержимого файла;</span><span class="sxs-lookup"><span data-stu-id="4d340-194">Return file content.</span></span>
* <span data-ttu-id="4d340-195">реализации алгоритма повторных попыток с экспоненциальной задержкой, чтобы охватить ситуации, когда блокировка файла временно препятствует его считыванию.</span><span class="sxs-lookup"><span data-stu-id="4d340-195">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="4d340-196">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d340-196">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="4d340-197">Для обработки поиска кэшированных файлов создается `FileService`.</span><span class="sxs-lookup"><span data-stu-id="4d340-197">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="4d340-198">Направленный в службу вызов метода `GetFileContent` пытается получить содержимое файла из кэша в памяти и вернуть его вызывающему объекту (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="4d340-198">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="4d340-199">Если кэшированное содержимое не удается найти по ключу кэша, выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="4d340-199">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="4d340-200">Содержимое файла получается с помощью `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="4d340-200">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="4d340-201">Токен изменений извлекается из файла поставщика с помощью [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="4d340-201">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="4d340-202">При изменении файла активируется обратный вызов токена.</span><span class="sxs-lookup"><span data-stu-id="4d340-202">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="4d340-203">Содержимое файла кэшируется с использованием [скользящего срока действия](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration).</span><span class="sxs-lookup"><span data-stu-id="4d340-203">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="4d340-204">Токен изменений подключается к [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) для исключения записи кэша, если файл изменяется во время его кэширования.</span><span class="sxs-lookup"><span data-stu-id="4d340-204">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="4d340-205">В следующем примере файлы хранятся в [корневом каталоге содержимого](xref:fundamentals/index#content-root) приложения.</span><span class="sxs-lookup"><span data-stu-id="4d340-205">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="4d340-206">`IWebHostEnvironment.ContentRootFileProvider` используется для получения <xref:Microsoft.Extensions.FileProviders.IFileProvider> с указанием на приложение `IWebHostEnvironment.ContentRootPath`.</span><span class="sxs-lookup"><span data-stu-id="4d340-206">`IWebHostEnvironment.ContentRootFileProvider` is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's `IWebHostEnvironment.ContentRootPath`.</span></span> <span data-ttu-id="4d340-207">Для получения `filePath` используется [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="4d340-207">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="4d340-208">`FileService` регистрируется в контейнере службы вместе со службой кэширования памяти.</span><span class="sxs-lookup"><span data-stu-id="4d340-208">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="4d340-209">В `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4d340-209">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="4d340-210">Страничная модель загружает содержимое файла с помощью службы.</span><span class="sxs-lookup"><span data-stu-id="4d340-210">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="4d340-211">На странице индексов используется метод `OnGet` (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="4d340-211">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/3.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="4d340-212">Класс CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="4d340-212">CompositeChangeToken class</span></span>

<span data-ttu-id="4d340-213">Чтобы представить один или несколько экземпляров `IChangeToken` в одном объекте, используйте класс <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="4d340-213">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

<span data-ttu-id="4d340-214">`HasChanged` для составного токена указывает `true`, если `HasChanged` любой из представленных токенов имеет значение `true`.</span><span class="sxs-lookup"><span data-stu-id="4d340-214">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="4d340-215">`ActiveChangeCallbacks` для составного токена указывает `true`, если `ActiveChangeCallbacks` любой из представленных токенов имеет значение `true`.</span><span class="sxs-lookup"><span data-stu-id="4d340-215">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="4d340-216">Если возникает несколько одновременных событий изменения, обратный вызов составного изменения выполняется один раз.</span><span class="sxs-lookup"><span data-stu-id="4d340-216">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="4d340-217">*Токен изменений* — это низкоуровневый стандартный блок общего назначения, используемый для отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="4d340-217">A *change token* is a general-purpose, low-level building block used to track state changes.</span></span>

<span data-ttu-id="4d340-218">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4d340-218">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/change-tokens/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="4d340-219">Интерфейс IChangeToken</span><span class="sxs-lookup"><span data-stu-id="4d340-219">IChangeToken interface</span></span>

<span data-ttu-id="4d340-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> распространяет уведомления о том, что произошло изменение.</span><span class="sxs-lookup"><span data-stu-id="4d340-220"><xref:Microsoft.Extensions.Primitives.IChangeToken> propagates notifications that a change has occurred.</span></span> <span data-ttu-id="4d340-221">`IChangeToken` находится в пространстве имен <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="4d340-221">`IChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="4d340-222">Для приложений, которые не используют метапакет [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), создайте ссылку на пакет NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/).</span><span class="sxs-lookup"><span data-stu-id="4d340-222">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="4d340-223">`IChangeToken` имеет два свойства:</span><span class="sxs-lookup"><span data-stu-id="4d340-223">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="4d340-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> указывает, выполняет ли токен обратные вызовы с упреждением.</span><span class="sxs-lookup"><span data-stu-id="4d340-224"><xref:Microsoft.Extensions.Primitives.IChangeToken.ActiveChangeCallbacks> indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="4d340-225">Если для `ActiveChangedCallbacks` задано значение `false`, обратный вызов не выполняется, а приложению нужно опросить `HasChanged` на предмет изменений.</span><span class="sxs-lookup"><span data-stu-id="4d340-225">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="4d340-226">Кроме того, отмена токена может никогда не произойти в случае отсутствия изменений или отключения либо удаления базового прослушивателя изменений.</span><span class="sxs-lookup"><span data-stu-id="4d340-226">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="4d340-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> получает значение, указывающее, произошло ли изменение.</span><span class="sxs-lookup"><span data-stu-id="4d340-227"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> receives a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="4d340-228">Интерфейс `IChangeToken` включает метод [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*), который регистрирует обратный вызов, выполняемый при изменении токена.</span><span class="sxs-lookup"><span data-stu-id="4d340-228">The `IChangeToken` interface includes the [RegisterChangeCallback(Action\<Object>, Object)](xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*) method, which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="4d340-229">Перед выполнением обратного вызова нужно задать `HasChanged`.</span><span class="sxs-lookup"><span data-stu-id="4d340-229">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="4d340-230">Класс ChangeToken</span><span class="sxs-lookup"><span data-stu-id="4d340-230">ChangeToken class</span></span>

<span data-ttu-id="4d340-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> — это статический класс, который распространяет уведомления о том, что произошло изменение.</span><span class="sxs-lookup"><span data-stu-id="4d340-231"><xref:Microsoft.Extensions.Primitives.ChangeToken> is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="4d340-232">`ChangeToken` находится в пространстве имен <xref:Microsoft.Extensions.Primitives?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="4d340-232">`ChangeToken` resides in the <xref:Microsoft.Extensions.Primitives?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="4d340-233">Для приложений, которые не используют метапакет [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), создайте ссылку на пакет NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/).</span><span class="sxs-lookup"><span data-stu-id="4d340-233">For apps that don't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference for the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package.</span></span>

<span data-ttu-id="4d340-234">Метод [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) регистрирует объект `Action`, вызываемый при изменении токена:</span><span class="sxs-lookup"><span data-stu-id="4d340-234">The [ChangeToken.OnChange(Func\<IChangeToken>, Action)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) method registers an `Action` to call whenever the token changes:</span></span>

* <span data-ttu-id="4d340-235">`Func<IChangeToken>` создает токен.</span><span class="sxs-lookup"><span data-stu-id="4d340-235">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="4d340-236">`Action` вызывается при изменении токена.</span><span class="sxs-lookup"><span data-stu-id="4d340-236">`Action` is called when the token changes.</span></span>

<span data-ttu-id="4d340-237">Перегрузка [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) принимает дополнительный параметр `TState`, передаваемый в объект-получатель токена `Action`.</span><span class="sxs-lookup"><span data-stu-id="4d340-237">The [ChangeToken.OnChange\<TState>(Func\<IChangeToken>, Action\<TState>, TState)](xref:Microsoft.Extensions.Primitives.ChangeToken.OnChange*) overload takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="4d340-238">`OnChange` возвращает <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="4d340-238">`OnChange` returns an <xref:System.IDisposable>.</span></span> <span data-ttu-id="4d340-239">При вызове <xref:System.IDisposable.Dispose*> токен прекращает прослушивать дальнейшие изменения и освобождает свои ресурсы.</span><span class="sxs-lookup"><span data-stu-id="4d340-239">Calling <xref:System.IDisposable.Dispose*> stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="4d340-240">Примеры использования токенов изменений в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d340-240">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="4d340-241">Токены изменений используются в ключевых областях ASP.NET Core для отслеживания изменений в объектах:</span><span class="sxs-lookup"><span data-stu-id="4d340-241">Change tokens are used in prominent areas of ASP.NET Core to monitor for changes to objects:</span></span>

* <span data-ttu-id="4d340-242">Чтобы отслеживать изменения в файлах, метод <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> интерфейса <xref:Microsoft.Extensions.FileProviders.IFileProvider> создает `IChangeToken` для указанных файлов или папки.</span><span class="sxs-lookup"><span data-stu-id="4d340-242">For monitoring changes to files, <xref:Microsoft.Extensions.FileProviders.IFileProvider>'s <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*> method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="4d340-243">Токены `IChangeToken` можно добавлять в записи кэша, чтобы активировать вытеснения кэша при изменении.</span><span class="sxs-lookup"><span data-stu-id="4d340-243">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="4d340-244">Для изменений `TOptions` используемая по умолчанию реализация <xref:Microsoft.Extensions.Options.OptionsMonitor`1> интерфейса <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> имеет перегрузку, которая принимает один или несколько экземпляров <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1>.</span><span class="sxs-lookup"><span data-stu-id="4d340-244">For `TOptions` changes, the default <xref:Microsoft.Extensions.Options.OptionsMonitor`1> implementation of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> has an overload that accepts one or more <xref:Microsoft.Extensions.Options.IOptionsChangeTokenSource`1> instances.</span></span> <span data-ttu-id="4d340-245">Каждый экземпляр возвращает `IChangeToken`, чтобы зарегистрировать обратный вызов уведомления об изменении для отслеживания изменений параметров.</span><span class="sxs-lookup"><span data-stu-id="4d340-245">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitor-for-configuration-changes"></a><span data-ttu-id="4d340-246">Отслеживание изменений конфигурации</span><span class="sxs-lookup"><span data-stu-id="4d340-246">Monitor for configuration changes</span></span>

<span data-ttu-id="4d340-247">По умолчанию шаблоны ASP.NET Core используют [файлы конфигурации JSON](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json* и *appsettings.Production.json*) для загрузки параметров конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="4d340-247">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration-provider) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="4d340-248">Эти файлы настраиваются с помощью метода расширения [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) в <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, который принимает параметр `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="4d340-248">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*) extension method on <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> that accepts a `reloadOnChange` parameter.</span></span> <span data-ttu-id="4d340-249">`reloadOnChange` указывает, нужно ли перезагружать конфигурацию при изменении файла.</span><span class="sxs-lookup"><span data-stu-id="4d340-249">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="4d340-250">Этот параметр отображается в удобном методе <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> класса <xref:Microsoft.AspNetCore.WebHost>:</span><span class="sxs-lookup"><span data-stu-id="4d340-250">This setting appears in the <xref:Microsoft.AspNetCore.WebHost> convenience method <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
          reloadOnChange: true);
```

<span data-ttu-id="4d340-251">Конфигурация на основе файла представлена <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="4d340-251">File-based configuration is represented by <xref:Microsoft.Extensions.Configuration.FileConfigurationSource>.</span></span> <span data-ttu-id="4d340-252">`FileConfigurationSource` использует <xref:Microsoft.Extensions.FileProviders.IFileProvider> для отслеживания файлов.</span><span class="sxs-lookup"><span data-stu-id="4d340-252">`FileConfigurationSource` uses <xref:Microsoft.Extensions.FileProviders.IFileProvider> to monitor files.</span></span>

<span data-ttu-id="4d340-253">По умолчанию `IFileMonitor` предоставляется объектом <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, который использует <xref:System.IO.FileSystemWatcher> для отслеживания изменений в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-253">By default, the `IFileMonitor` is provided by a <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider>, which uses <xref:System.IO.FileSystemWatcher> to monitor for configuration file changes.</span></span>

<span data-ttu-id="4d340-254">Этот пример приложения демонстрирует две реализации для отслеживания изменений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-254">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="4d340-255">Если какой-либо из файлов *appsettings* был изменен, каждая реализация отслеживания файлов выполняет пользовательский код &mdash; пример приложения записывает сообщение на консоль.</span><span class="sxs-lookup"><span data-stu-id="4d340-255">If any of the *appsettings* files change, both of the file monitoring implementations execute custom code&mdash;the sample app writes a message to the console.</span></span>

<span data-ttu-id="4d340-256">`FileSystemWatcher` файла конфигурации может активировать несколько обратных вызовов токена для одного изменения файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-256">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="4d340-257">Чтобы пользовательский код выполнялся один раз при активации нескольких обратных вызовов токена, реализация в примере проверяет хэши файлов.</span><span class="sxs-lookup"><span data-stu-id="4d340-257">To ensure that the custom code is only run once when multiple token callbacks are triggered, the sample's implementation checks file hashes.</span></span> <span data-ttu-id="4d340-258">В примере используется хэширование файлов SHA1.</span><span class="sxs-lookup"><span data-stu-id="4d340-258">The sample uses SHA1 file hashing.</span></span> <span data-ttu-id="4d340-259">Повторная попытка реализуется посредством экспоненциальной задержки.</span><span class="sxs-lookup"><span data-stu-id="4d340-259">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="4d340-260">Повторная попытка нужна, так как может возникать блокировка файлов, препятствующая вычислению нового хэша в файле.</span><span class="sxs-lookup"><span data-stu-id="4d340-260">The retry is present because file locking may occur that temporarily prevents computing a new hash on a file.</span></span>

<span data-ttu-id="4d340-261">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d340-261">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet1)]

### <a name="simple-startup-change-token"></a><span data-ttu-id="4d340-262">Токен изменений для простого запуска</span><span class="sxs-lookup"><span data-stu-id="4d340-262">Simple startup change token</span></span>

<span data-ttu-id="4d340-263">Зарегистрируйте обратный вызов `Action` объекта-получателя токена для уведомлений об изменениях в токене перезагрузки конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-263">Register a token consumer `Action` callback for change notifications to the configuration reload token.</span></span>

<span data-ttu-id="4d340-264">В `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="4d340-264">In `Startup.Configure`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="4d340-265">`config.GetReloadToken()` предоставляет токен.</span><span class="sxs-lookup"><span data-stu-id="4d340-265">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="4d340-266">Обратный вызов является методом `InvokeChanged`:</span><span class="sxs-lookup"><span data-stu-id="4d340-266">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="4d340-267">`state` обратного вызова используется для передачи `IHostingEnvironment`. Это удобно для определения правильного JSON-файла конфигурации *appsettings*, который требуется отслеживать (например, *appsettings.Development.json* при работе в среде разработки).</span><span class="sxs-lookup"><span data-stu-id="4d340-267">The `state` of the callback is used to pass in the `IHostingEnvironment`, which is useful for specifying the correct *appsettings* configuration file to monitor (for example, *appsettings.Development.json* when in the Development environment).</span></span> <span data-ttu-id="4d340-268">Хэши файлов используются для предотвращения многократного выполнения оператора `WriteConsole` из-за нескольких обратных вызовов токена при всего одном изменении файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-268">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="4d340-269">Эта система выполняется, пока запущено приложение, и не может быть отключена пользователем.</span><span class="sxs-lookup"><span data-stu-id="4d340-269">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitor-configuration-changes-as-a-service"></a><span data-ttu-id="4d340-270">Отслеживание изменений конфигурации как служба</span><span class="sxs-lookup"><span data-stu-id="4d340-270">Monitor configuration changes as a service</span></span>

<span data-ttu-id="4d340-271">Этот пример реализует следующее:</span><span class="sxs-lookup"><span data-stu-id="4d340-271">The sample implements:</span></span>

* <span data-ttu-id="4d340-272">Базовое отслеживание токена запуска.</span><span class="sxs-lookup"><span data-stu-id="4d340-272">Basic startup token monitoring.</span></span>
* <span data-ttu-id="4d340-273">Отслеживание как служба.</span><span class="sxs-lookup"><span data-stu-id="4d340-273">Monitoring as a service.</span></span>
* <span data-ttu-id="4d340-274">Механизм для включения и отключения отслеживания.</span><span class="sxs-lookup"><span data-stu-id="4d340-274">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="4d340-275">Этот пример задает интерфейс `IConfigurationMonitor`.</span><span class="sxs-lookup"><span data-stu-id="4d340-275">The sample establishes an `IConfigurationMonitor` interface.</span></span>

<span data-ttu-id="4d340-276">*Extensions/ConfigurationMonitor.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d340-276">*Extensions/ConfigurationMonitor.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="4d340-277">Конструктор реализованного класса `ConfigurationMonitor` регистрирует обратный вызов для уведомлений об изменениях:</span><span class="sxs-lookup"><span data-stu-id="4d340-277">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="4d340-278">`config.GetReloadToken()` предоставляет токен.</span><span class="sxs-lookup"><span data-stu-id="4d340-278">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="4d340-279">`InvokeChanged` является методом обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="4d340-279">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="4d340-280">`state` в этом экземпляре является ссылкой на экземпляр `IConfigurationMonitor`, используемый для доступа к состоянию мониторинга.</span><span class="sxs-lookup"><span data-stu-id="4d340-280">The `state` in this instance is a reference to the `IConfigurationMonitor` instance that's used to access the monitoring state.</span></span> <span data-ttu-id="4d340-281">Используются два свойства:</span><span class="sxs-lookup"><span data-stu-id="4d340-281">Two properties are used:</span></span>

* <span data-ttu-id="4d340-282">`MonitoringEnabled` указывает, нужно ли обратному вызову выполнять свой пользовательский код.</span><span class="sxs-lookup"><span data-stu-id="4d340-282">`MonitoringEnabled` &ndash; Indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="4d340-283">`CurrentState` описывает текущее состояние отслеживания для использования в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="4d340-283">`CurrentState` &ndash; Describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="4d340-284">Метод `InvokeChanged` похож на описанный ранее подход, за исключением того, что он:</span><span class="sxs-lookup"><span data-stu-id="4d340-284">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="4d340-285">не выполняет свой код, если только `MonitoringEnabled` не имеет значение `true`;</span><span class="sxs-lookup"><span data-stu-id="4d340-285">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="4d340-286">выводит текущее значение `state` в своих выходных данных `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="4d340-286">Outputs the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="4d340-287">Экземпляр `ConfigurationMonitor` регистрируется как служба в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4d340-287">An instance `ConfigurationMonitor` is registered as a service in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="4d340-288">Страница индекса позволяет пользователю управлять отслеживанием конфигурации.</span><span class="sxs-lookup"><span data-stu-id="4d340-288">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="4d340-289">Экземпляр `IConfigurationMonitor` внедряется в `IndexModel`.</span><span class="sxs-lookup"><span data-stu-id="4d340-289">The instance of `IConfigurationMonitor` is injected into the `IndexModel`.</span></span>

<span data-ttu-id="4d340-290">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d340-290">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="4d340-291">Монитор конфигурации (`_monitor`) позволяет включить или отключить мониторинг и задать текущее состояние для обратной связи пользовательского интерфейса:</span><span class="sxs-lookup"><span data-stu-id="4d340-291">The configuration monitor (`_monitor`) is used to enable or disable monitoring and set the current state for UI feedback:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="4d340-292">При активации `OnPostStartMonitoring` отслеживание включается, а текущее состояние сбрасывается.</span><span class="sxs-lookup"><span data-stu-id="4d340-292">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="4d340-293">При активации `OnPostStopMonitoring` отслеживание отключается, а состояние указывает на отсутствие отслеживания.</span><span class="sxs-lookup"><span data-stu-id="4d340-293">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

<span data-ttu-id="4d340-294">Кнопки пользовательского интерфейса для включения и отключения отслеживания.</span><span class="sxs-lookup"><span data-stu-id="4d340-294">Buttons in the UI enable and disable monitoring.</span></span>

<span data-ttu-id="4d340-295">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="4d340-295">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml?name=snippet_Buttons)]

## <a name="monitor-cached-file-changes"></a><span data-ttu-id="4d340-296">Отслеживание изменений кэшированных файлов</span><span class="sxs-lookup"><span data-stu-id="4d340-296">Monitor cached file changes</span></span>

<span data-ttu-id="4d340-297">Содержимое файла можно кэшировать в памяти с помощью <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span><span class="sxs-lookup"><span data-stu-id="4d340-297">File content can be cached in-memory using <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="4d340-298">Кэширование в памяти описано в разделе [Кэш в памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="4d340-298">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="4d340-299">*Устаревшие* (просроченные) данные возвращаются из кэша при изменении источника исходных данных без каких-либо дополнительных действий, таких как описанная ниже реализация.</span><span class="sxs-lookup"><span data-stu-id="4d340-299">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="4d340-300">Например, если не учитывать состояние кэшированного исходного файла при продлении [скользящего срока действия](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration), это приведет к появлению в кэше устаревших данных файлов.</span><span class="sxs-lookup"><span data-stu-id="4d340-300">For example, not taking into account the status of a cached source file when renewing a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period leads to stale cached file data.</span></span> <span data-ttu-id="4d340-301">Каждый запрос к данным продляет скользящий срок действия, но этот файл никогда не загружается в кэш.</span><span class="sxs-lookup"><span data-stu-id="4d340-301">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="4d340-302">Любые функции приложения, использующие кэшированное содержимое, могут получить устаревшее содержимое.</span><span class="sxs-lookup"><span data-stu-id="4d340-302">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="4d340-303">Использование токенов изменений в сценарии кэширования файлов предотвращает появление устаревшего содержимого файлов в кэше.</span><span class="sxs-lookup"><span data-stu-id="4d340-303">Using change tokens in a file caching scenario prevents the presence of stale file content in the cache.</span></span> <span data-ttu-id="4d340-304">Этот пример демонстрирует реализацию данного подхода.</span><span class="sxs-lookup"><span data-stu-id="4d340-304">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="4d340-305">`GetFileContent` в примере используется для:</span><span class="sxs-lookup"><span data-stu-id="4d340-305">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="4d340-306">возврата содержимого файла;</span><span class="sxs-lookup"><span data-stu-id="4d340-306">Return file content.</span></span>
* <span data-ttu-id="4d340-307">реализации алгоритма повторных попыток с экспоненциальной задержкой, чтобы охватить ситуации, когда блокировка файла временно препятствует его считыванию.</span><span class="sxs-lookup"><span data-stu-id="4d340-307">Implement a retry algorithm with exponential back-off to cover cases where a file lock temporarily prevents reading a file.</span></span>

<span data-ttu-id="4d340-308">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d340-308">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="4d340-309">Для обработки поиска кэшированных файлов создается `FileService`.</span><span class="sxs-lookup"><span data-stu-id="4d340-309">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="4d340-310">Направленный в службу вызов метода `GetFileContent` пытается получить содержимое файла из кэша в памяти и вернуть его вызывающему объекту (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="4d340-310">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="4d340-311">Если кэшированное содержимое не удается найти по ключу кэша, выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="4d340-311">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="4d340-312">Содержимое файла получается с помощью `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="4d340-312">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="4d340-313">Токен изменений извлекается из файла поставщика с помощью [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span><span class="sxs-lookup"><span data-stu-id="4d340-313">A change token is obtained from the file provider with [IFileProviders.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*).</span></span> <span data-ttu-id="4d340-314">При изменении файла активируется обратный вызов токена.</span><span class="sxs-lookup"><span data-stu-id="4d340-314">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="4d340-315">Содержимое файла кэшируется с использованием [скользящего срока действия](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration).</span><span class="sxs-lookup"><span data-stu-id="4d340-315">The file content is cached with a [sliding expiration](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.SlidingExpiration) period.</span></span> <span data-ttu-id="4d340-316">Токен изменений подключается к [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) для исключения записи кэша, если файл изменяется во время его кэширования.</span><span class="sxs-lookup"><span data-stu-id="4d340-316">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.AddExpirationToken*) to evict the cache entry if the file changes while it's cached.</span></span>

<span data-ttu-id="4d340-317">В следующем примере файлы хранятся в [корневом каталоге содержимого](xref:fundamentals/index#content-root) приложения.</span><span class="sxs-lookup"><span data-stu-id="4d340-317">In the following example, files are stored in the app's [content root](xref:fundamentals/index#content-root).</span></span> <span data-ttu-id="4d340-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) используется для получения объекта <xref:Microsoft.Extensions.FileProviders.IFileProvider>, который указывает на <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath> приложения.</span><span class="sxs-lookup"><span data-stu-id="4d340-318">[IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootFileProvider) is used to obtain an <xref:Microsoft.Extensions.FileProviders.IFileProvider> pointing at the app's <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>.</span></span> <span data-ttu-id="4d340-319">Для получения `filePath` используется [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span><span class="sxs-lookup"><span data-stu-id="4d340-319">The `filePath` is obtained with [IFileInfo.PhysicalPath](xref:Microsoft.Extensions.FileProviders.IFileInfo.PhysicalPath).</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="4d340-320">`FileService` регистрируется в контейнере службы вместе со службой кэширования памяти.</span><span class="sxs-lookup"><span data-stu-id="4d340-320">The `FileService` is registered in the service container along with the memory caching service.</span></span>

<span data-ttu-id="4d340-321">В `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4d340-321">In `Startup.ConfigureServices`:</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="4d340-322">Страничная модель загружает содержимое файла с помощью службы.</span><span class="sxs-lookup"><span data-stu-id="4d340-322">The page model loads the file's content using the service.</span></span>

<span data-ttu-id="4d340-323">На странице индексов используется метод `OnGet` (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="4d340-323">In the Index page's `OnGet` method (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/samples/2.x/SampleApp/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="4d340-324">Класс CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="4d340-324">CompositeChangeToken class</span></span>

<span data-ttu-id="4d340-325">Чтобы представить один или несколько экземпляров `IChangeToken` в одном объекте, используйте класс <xref:Microsoft.Extensions.Primitives.CompositeChangeToken>.</span><span class="sxs-lookup"><span data-stu-id="4d340-325">For representing one or more `IChangeToken` instances in a single object, use the <xref:Microsoft.Extensions.Primitives.CompositeChangeToken> class.</span></span>

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        {
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

<span data-ttu-id="4d340-326">`HasChanged` для составного токена указывает `true`, если `HasChanged` любой из представленных токенов имеет значение `true`.</span><span class="sxs-lookup"><span data-stu-id="4d340-326">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="4d340-327">`ActiveChangeCallbacks` для составного токена указывает `true`, если `ActiveChangeCallbacks` любой из представленных токенов имеет значение `true`.</span><span class="sxs-lookup"><span data-stu-id="4d340-327">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="4d340-328">Если возникает несколько одновременных событий изменения, обратный вызов составного изменения выполняется один раз.</span><span class="sxs-lookup"><span data-stu-id="4d340-328">If multiple concurrent change events occur, the composite change callback is invoked one time.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="4d340-329">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4d340-329">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
