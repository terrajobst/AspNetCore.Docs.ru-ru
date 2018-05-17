---
title: Обнаружение изменений с помощью токенов изменений в ASP.NET Core
author: guardrex
description: Сведения об использовании токенов изменений для отслеживания изменений.
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 3055eec91adc412b596d4cc73e8523e18ff63331
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a><span data-ttu-id="6b3fc-103">Обнаружение изменений с помощью токенов изменений в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b3fc-103">Detect changes with change tokens in ASP.NET Core</span></span>

<span data-ttu-id="6b3fc-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="6b3fc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6b3fc-105">*Токен изменений* является низкоуровневым стандартным блоком общего назначения, используемым для отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-105">A *change token* is a general-purpose, low-level building block used to track changes.</span></span>

<span data-ttu-id="6b3fc-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6b3fc-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ichangetoken-interface"></a><span data-ttu-id="6b3fc-107">Интерфейс IChangeToken</span><span class="sxs-lookup"><span data-stu-id="6b3fc-107">IChangeToken interface</span></span>

<span data-ttu-id="6b3fc-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) распространяет уведомления о том, что произошло изменение.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-108">[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) propagates notifications that a change has occurred.</span></span> <span data-ttu-id="6b3fc-109">`IChangeToken` находится в пространстве имен [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-109">`IChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="6b3fc-110">Для приложений, которые не используют метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), сошлитесь на пакет NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-110">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="6b3fc-111">`IChangeToken` имеет два свойства:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-111">`IChangeToken` has two properties:</span></span>

* <span data-ttu-id="6b3fc-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) указывает, выполняет ли токен обратные вызовы с упреждением.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-112">[ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) indicate if the token proactively raises callbacks.</span></span> <span data-ttu-id="6b3fc-113">Если для `ActiveChangedCallbacks` задано значение `false`, обратный вызов не выполняется, а приложению нужно опросить `HasChanged` на предмет изменений.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-113">If `ActiveChangedCallbacks` is set to `false`, a callback is never called, and the app must poll `HasChanged` for changes.</span></span> <span data-ttu-id="6b3fc-114">Кроме того, отмена токена может никогда не произойти в случае отсутствия изменений или отключения либо удаления базового прослушивателя изменений.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-114">It's also possible for a token to never be cancelled if no changes occur or the underlying change listener is disposed or disabled.</span></span>
* <span data-ttu-id="6b3fc-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) возвращает значение, указывающее, произошло ли изменение.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-115">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) gets a value that indicates if a change has occurred.</span></span>

<span data-ttu-id="6b3fc-116">Интерфейс имеет один метод — [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), который регистрирует обратный вызов, выполняемый при изменении токена.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-116">The interface has one method, [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), which registers a callback that's invoked when the token has changed.</span></span> <span data-ttu-id="6b3fc-117">Перед выполнением обратного вызова нужно задать `HasChanged`.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-117">`HasChanged` must be set before the callback is invoked.</span></span>

## <a name="changetoken-class"></a><span data-ttu-id="6b3fc-118">Класс ChangeToken</span><span class="sxs-lookup"><span data-stu-id="6b3fc-118">ChangeToken class</span></span>

<span data-ttu-id="6b3fc-119">`ChangeToken` — это статический класс, который распространяет уведомления о том, что произошло изменение.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-119">`ChangeToken` is a static class used to propagate notifications that a change has occurred.</span></span> <span data-ttu-id="6b3fc-120">`ChangeToken` находится в пространстве имен [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-120">`ChangeToken` resides in the [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) namespace.</span></span> <span data-ttu-id="6b3fc-121">Для приложений, которые не используют метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), сошлитесь на пакет NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-121">For apps that don't use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, reference the [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet package in the project file.</span></span>

<span data-ttu-id="6b3fc-122">Метод `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) регистрирует `Action`, вызываемый при изменении токена:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-122">The `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) method registers an `Action` to call whenever the token changes:</span></span>
* <span data-ttu-id="6b3fc-123">`Func<IChangeToken>` создает токен.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-123">`Func<IChangeToken>` produces the token.</span></span>
* <span data-ttu-id="6b3fc-124">`Action` вызывается при изменении токена.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-124">`Action` is called when the token changes.</span></span>

<span data-ttu-id="6b3fc-125">`ChangeToken` имеет перегрузку [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_), которая принимает дополнительный параметр `TState`, передаваемый в объект-получатель токена `Action`.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-125">`ChangeToken` has an [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) overload that takes an additional `TState` parameter that's passed into the token consumer `Action`.</span></span>

<span data-ttu-id="6b3fc-126">`OnChange` возвращает [IDisposable](/dotnet/api/system.idisposable).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-126">`OnChange` returns an [IDisposable](/dotnet/api/system.idisposable).</span></span> <span data-ttu-id="6b3fc-127">При вызове [Dispose](/dotnet/api/system.idisposable.dispose) токен прекращает прослушивать дальнейшие изменения и освобождает свои ресурсы.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-127">Calling [Dispose](/dotnet/api/system.idisposable.dispose) stops the token from listening for further changes and releases the token's resources.</span></span>

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a><span data-ttu-id="6b3fc-128">Примеры использования токенов изменений в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b3fc-128">Example uses of change tokens in ASP.NET Core</span></span>

<span data-ttu-id="6b3fc-129">Токены изменений используются в ключевых областях ASP.NET Core для отслеживания изменений в объектах:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-129">Change tokens are used in prominent areas of ASP.NET Core monitoring changes to objects:</span></span>

* <span data-ttu-id="6b3fc-130">Чтобы отслеживать изменения в файлах, метод [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) интерфейса [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) создает `IChangeToken` для указанных файлов или папки.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-130">For monitoring changes to files, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)'s [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method creates an `IChangeToken` for the specified files or folder to watch.</span></span>
* <span data-ttu-id="6b3fc-131">Токены `IChangeToken` можно добавлять в записи кэша, чтобы активировать вытеснения кэша при изменении.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-131">`IChangeToken` tokens can be added to cache entries to trigger cache evictions on change.</span></span>
* <span data-ttu-id="6b3fc-132">Для изменений `TOptions` используемая по умолчанию реализация [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) интерфейса [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) имеет перегрузку, которая принимает один или несколько экземпляров [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-132">For `TOptions` changes, the default [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) implementation of [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) has an overload that accepts one or more [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1) instances.</span></span> <span data-ttu-id="6b3fc-133">Каждый экземпляр возвращает `IChangeToken`, чтобы зарегистрировать обратный вызов уведомления об изменении для отслеживания изменений параметров.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-133">Each instance returns an `IChangeToken` to register a change notification callback for tracking options changes.</span></span>

## <a name="monitoring-for-configuration-changes"></a><span data-ttu-id="6b3fc-134">Отслеживание изменений конфигурации</span><span class="sxs-lookup"><span data-stu-id="6b3fc-134">Monitoring for configuration changes</span></span>

<span data-ttu-id="6b3fc-135">По умолчанию шаблоны ASP.NET Core используют [файлы конфигурации JSON](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json* и *appsettings.Production.json*) для загрузки параметров конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-135">By default, ASP.NET Core templates use [JSON configuration files](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json*, and *appsettings.Production.json*) to load app configuration settings.</span></span>

<span data-ttu-id="6b3fc-136">Эти файлы настраиваются с помощью метода расширения [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) в [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder), который принимает параметр `reloadOnChange` (ASP.NET Core 1.1 и более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-136">These files are configured using the [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) extension method on [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) that accepts a `reloadOnChange` parameter (ASP.NET Core 1.1 and later).</span></span> <span data-ttu-id="6b3fc-137">`reloadOnChange` указывает, нужно ли перезагружать конфигурацию при изменении файла.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-137">`reloadOnChange` indicates if configuration should be reloaded on file changes.</span></span> <span data-ttu-id="6b3fc-138">Просмотрите этот параметр в удобном методе [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span><span class="sxs-lookup"><span data-stu-id="6b3fc-138">See this setting in the [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) convenience method [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):</span></span>

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

<span data-ttu-id="6b3fc-139">Конфигурация на основе файла представлена [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-139">File-based configuration is represented by [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource).</span></span> <span data-ttu-id="6b3fc-140">`FileConfigurationSource` использует [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) для наблюдения за файлами.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-140">`FileConfigurationSource` uses [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) to monitor files.</span></span>

<span data-ttu-id="6b3fc-141">По умолчанию `IFileMonitor` предоставляется [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), который использует [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) для отслеживания изменений в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-141">By default, the `IFileMonitor` is provided by a [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), which uses [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) to monitor for configuration file changes.</span></span>

<span data-ttu-id="6b3fc-142">Этот пример приложения демонстрирует две реализации для отслеживания изменений конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-142">The sample app demonstrates two implementations for monitoring configuration changes.</span></span> <span data-ttu-id="6b3fc-143">Если изменяется файл *appsettings.json* или его версия среды, каждая реализация выполняет пользовательский код.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-143">If either the *appsettings.json* file changes or the Environment version of the file changes, each implementation executes custom code.</span></span> <span data-ttu-id="6b3fc-144">Этот пример приложения записывает сообщение в консоль.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-144">The sample app writes a message to the console.</span></span>

<span data-ttu-id="6b3fc-145">`FileSystemWatcher` файла конфигурации может активировать несколько обратных вызовов токена для одного изменения файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-145">A configuration file's `FileSystemWatcher` can trigger multiple token callbacks for a single configuration file change.</span></span> <span data-ttu-id="6b3fc-146">Реализация в примере защищает от этой проблемы, проверяя хэши для файлов конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-146">The sample's implementation guards against this problem by checking file hashes on the configuration files.</span></span> <span data-ttu-id="6b3fc-147">Проверка хэшей файлов позволяет убедиться, что изменился по меньшей мере один из файлов конфигурации, прежде чем запускать пользовательский код.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-147">Checking file hashes ensures that at least one of the configuration files has changed before running the custom code.</span></span> <span data-ttu-id="6b3fc-148">Этот пример использует хэширование файлов SHA1 (*Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="6b3fc-148">The sample uses SHA1 file hashing (*Utilities/Utilities.cs*):</span></span>

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   <span data-ttu-id="6b3fc-149">Повторная попытка реализуется посредством экспоненциальной задержки.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-149">A retry is implemented with an exponential back-off.</span></span> <span data-ttu-id="6b3fc-150">Повторная попытка нужна, так как может возникать блокировка файлов, препятствующая вычислению нового хэша для одного из файлов.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-150">The re-try is present because file locking may occur that temporarily prevents computing a new hash on one of the files.</span></span>

### <a name="simple-startup-change-token"></a><span data-ttu-id="6b3fc-151">Токен изменений для простого запуска</span><span class="sxs-lookup"><span data-stu-id="6b3fc-151">Simple startup change token</span></span>

<span data-ttu-id="6b3fc-152">Зарегистрируйте обратный вызов `Action` объекта-получателя токена для уведомлений об изменениях в токене перезагрузки конфигурации (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="6b3fc-152">Register a token consumer `Action` callback for change notifications to the configuration reload token (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="6b3fc-153">`config.GetReloadToken()` предоставляет токен.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-153">`config.GetReloadToken()` provides the token.</span></span> <span data-ttu-id="6b3fc-154">Обратный вызов является методом `InvokeChanged`:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-154">The callback is the `InvokeChanged` method:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="6b3fc-155">`state` обратного вызова используется для передачи `IHostingEnvironment`.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-155">The `state` of the callback is used to pass in the `IHostingEnvironment`.</span></span> <span data-ttu-id="6b3fc-156">Это удобно для определения правильного JSON-файла конфигурации *appsettings*, который требуется отслеживать, *appsettings.&lt;среда&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-156">This is useful to determine the correct *appsettings* configuration JSON file to monitor, *appsettings.&lt;Environment&gt;.json*.</span></span> <span data-ttu-id="6b3fc-157">Хэши файлов используются для предотвращения многократного выполнения оператора `WriteConsole` из-за нескольких обратных вызовов токена при всего одном изменении файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-157">File hashes are used to prevent the `WriteConsole` statement from running multiple times due to multiple token callbacks when the configuration file has only changed once.</span></span>

<span data-ttu-id="6b3fc-158">Эта система выполняется, пока запущено приложение, и не может быть отключена пользователем.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-158">This system runs as long as the app is running and can't be disabled by the user.</span></span>

### <a name="monitoring-configuration-changes-as-a-service"></a><span data-ttu-id="6b3fc-159">Отслеживание изменений конфигурации как служба</span><span class="sxs-lookup"><span data-stu-id="6b3fc-159">Monitoring configuration changes as a service</span></span>

<span data-ttu-id="6b3fc-160">Этот пример реализует следующее:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-160">The sample implements:</span></span>

* <span data-ttu-id="6b3fc-161">Базовое отслеживание токена запуска.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-161">Basic startup token monitoring.</span></span>
* <span data-ttu-id="6b3fc-162">Отслеживание как служба.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-162">Monitoring as a service.</span></span>
* <span data-ttu-id="6b3fc-163">Механизм для включения и отключения отслеживания.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-163">A mechanism to enable and disable monitoring.</span></span>

<span data-ttu-id="6b3fc-164">Этот пример задает интерфейс `IConfigurationMonitor` (*Extensions/ConfigurationMonitor.cs*):</span><span class="sxs-lookup"><span data-stu-id="6b3fc-164">The sample establishes an `IConfigurationMonitor` interface (*Extensions/ConfigurationMonitor.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

<span data-ttu-id="6b3fc-165">Конструктор реализованного класса `ConfigurationMonitor` регистрирует обратный вызов для уведомлений об изменениях:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-165">The constructor of the implemented class, `ConfigurationMonitor`, registers a callback for change notifications:</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

<span data-ttu-id="6b3fc-166">`config.GetReloadToken()` предоставляет токен.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-166">`config.GetReloadToken()` supplies the token.</span></span> <span data-ttu-id="6b3fc-167">`InvokeChanged` является методом обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-167">`InvokeChanged` is the callback method.</span></span> <span data-ttu-id="6b3fc-168">`state` в этом экземпляре является строкой, описывающей состояние отслеживания.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-168">The `state` in this instance is a string that describes the monitoring state.</span></span> <span data-ttu-id="6b3fc-169">Используются два свойства:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-169">Two properties are used:</span></span>

* <span data-ttu-id="6b3fc-170">`MonitoringEnabled` указывает, нужно ли обратному вызову выполнять свой пользовательский код.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-170">`MonitoringEnabled` indicates if the callback should run its custom code.</span></span>
* <span data-ttu-id="6b3fc-171">`CurrentState` описывает текущее состояние отслеживания для использования в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-171">`CurrentState` describes the current monitoring state for use in the UI.</span></span>

<span data-ttu-id="6b3fc-172">Метод `InvokeChanged` похож на описанный ранее подход, за исключением того, что он:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-172">The `InvokeChanged` method is similar to the earlier approach, except that it:</span></span>

* <span data-ttu-id="6b3fc-173">не выполняет свой код, если только `MonitoringEnabled` не имеет значение `true`;</span><span class="sxs-lookup"><span data-stu-id="6b3fc-173">Doesn't run its code unless `MonitoringEnabled` is `true`.</span></span>
* <span data-ttu-id="6b3fc-174">задает для строки свойства `CurrentState` описательное сообщение, регистрирующее время выполнения кода;</span><span class="sxs-lookup"><span data-stu-id="6b3fc-174">Sets the `CurrentState` property string to a descriptive message that records the time that the code ran.</span></span>
* <span data-ttu-id="6b3fc-175">записывает текущее значение `state` в своих выходных данных `WriteConsole`.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-175">Notes the current `state` in its `WriteConsole` output.</span></span>

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

<span data-ttu-id="6b3fc-176">Экземпляр `ConfigurationMonitor` регистрируется как служба в `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-176">An instance `ConfigurationMonitor` is registered as a service in `ConfigureServices` of *Startup.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="6b3fc-177">Страница индекса позволяет пользователю управлять отслеживанием конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-177">The Index page offers the user control over configuration monitoring.</span></span> <span data-ttu-id="6b3fc-178">Экземпляр `IConfigurationMonitor` внедряется в `IndexModel`:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-178">The instance of `IConfigurationMonitor` is injected into the `IndexModel`:</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="6b3fc-179">Кнопка включает и отключает отслеживание:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-179">A button enables and disables monitoring:</span></span>

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="6b3fc-180">При активации `OnPostStartMonitoring` отслеживание включается, а текущее состояние сбрасывается.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-180">When `OnPostStartMonitoring` is triggered, monitoring is enabled, and the current state is cleared.</span></span> <span data-ttu-id="6b3fc-181">При активации `OnPostStopMonitoring` отслеживание отключается, а состояние указывает на отсутствие отслеживания.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-181">When `OnPostStopMonitoring` is triggered, monitoring is disabled, and the state is set to reflect that monitoring isn't occurring.</span></span>

## <a name="monitoring-cached-file-changes"></a><span data-ttu-id="6b3fc-182">Отслеживание изменений кэшированных файлов</span><span class="sxs-lookup"><span data-stu-id="6b3fc-182">Monitoring cached file changes</span></span>

<span data-ttu-id="6b3fc-183">Содержимое файла можно кэшировать в памяти с помощью [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-183">File content can be cached in-memory using [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="6b3fc-184">Кэширование в памяти описано в разделе [Кэш в памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-184">In-memory caching is described in the [Cache in-memory](xref:performance/caching/memory) topic.</span></span> <span data-ttu-id="6b3fc-185">*Устаревшие* (просроченные) данные возвращаются из кэша при изменении источника исходных данных без каких-либо дополнительных действий, таких как описанная ниже реализация.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-185">Without taking additional steps, such as the implementation described below, *stale* (outdated) data is returned from a cache if the source data changes.</span></span>

<span data-ttu-id="6b3fc-186">Если не учитывать состояние кэшированного исходного файла при продлении [скользящего срока действия](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration), это приведет к устаревшим данным кэша.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-186">Not taking into account the status of a cached source file when renewing a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period leads to stale cache data.</span></span> <span data-ttu-id="6b3fc-187">Каждый запрос к данным продляет скользящий срок действия, но этот файл никогда не загружается в кэш.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-187">Each request for the data renews the sliding expiration period, but the file is never reloaded into the cache.</span></span> <span data-ttu-id="6b3fc-188">Любые функции приложения, использующие кэшированное содержимое, могут получить устаревшее содержимое.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-188">Any app features that use the file's cached content are subject to possibly receiving stale content.</span></span>

<span data-ttu-id="6b3fc-189">Использование токенов изменений в сценарий кэширования файлов предотвращает появление устаревшего содержимого файлов в кэше.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-189">Using change tokens in a file caching scenario prevents stale file content in the cache.</span></span> <span data-ttu-id="6b3fc-190">Этот пример демонстрирует реализацию данного подхода.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-190">The sample app demonstrates an implementation of the approach.</span></span>

<span data-ttu-id="6b3fc-191">`GetFileContent` в примере используется для:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-191">The sample uses `GetFileContent` to:</span></span>

* <span data-ttu-id="6b3fc-192">возврата содержимого файла;</span><span class="sxs-lookup"><span data-stu-id="6b3fc-192">Return file content.</span></span>
* <span data-ttu-id="6b3fc-193">реализации алгоритма повторных попыток с экспоненциальной задержкой, чтобы охватить ситуации, когда блокировка файла временно препятствует его чтению.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-193">Implement a retry algorithm with exponential back-off to cover cases where a file lock is temporarily preventing a file from being read.</span></span>

<span data-ttu-id="6b3fc-194">*Utilities/Utilities.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-194">*Utilities/Utilities.cs*:</span></span>

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

<span data-ttu-id="6b3fc-195">Для обработки поиска кэшированных файлов создается `FileService`.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-195">A `FileService` is created to handle cached file lookups.</span></span> <span data-ttu-id="6b3fc-196">Направленный в службу вызов метода `GetFileContent` пытается получить содержимое файла из кэша в памяти и вернуть его вызывающему объекту (*Services/FileService.cs*).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-196">The `GetFileContent` method call of the service attempts to obtain file content from the in-memory cache and return it to the caller (*Services/FileService.cs*).</span></span>

<span data-ttu-id="6b3fc-197">Если кэшированное содержимое не удается найти по ключу кэша, выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="6b3fc-197">If cached content isn't found using the cache key, the following actions are taken:</span></span>

1. <span data-ttu-id="6b3fc-198">Содержимое файла получается с помощью `GetFileContent`.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-198">The file content is obtained using `GetFileContent`.</span></span>
1. <span data-ttu-id="6b3fc-199">Токен изменений извлекается из файла поставщика с помощью [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-199">A change token is obtained from the file provider with [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch).</span></span> <span data-ttu-id="6b3fc-200">При изменении файла активируется обратный вызов токена.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-200">The token's callback is triggered when the file is modified.</span></span>
1. <span data-ttu-id="6b3fc-201">Содержимое файла кэшируется с использованием [скользящего срока действия](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-201">The file content is cached with a [sliding expiration](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) period.</span></span> <span data-ttu-id="6b3fc-202">Токен изменений подключается к [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) для исключения записи кэша, если файл изменяется во время его кэширования.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-202">The change token is attached with [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) to evict the cache entry if the file changes while it's cached.</span></span>

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

<span data-ttu-id="6b3fc-203">`FileService` регистрируется в контейнере службы вместе со службой кэширования памяти (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="6b3fc-203">The `FileService` is registered in the service container along with the memory caching service (*Startup.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

<span data-ttu-id="6b3fc-204">Страничная модель загружает содержимое файла с помощью службы (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="6b3fc-204">The page model loads the file's content using the service (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a><span data-ttu-id="6b3fc-205">Класс CompositeChangeToken</span><span class="sxs-lookup"><span data-stu-id="6b3fc-205">CompositeChangeToken class</span></span>

<span data-ttu-id="6b3fc-206">Чтобы представить один или несколько экземпляров `IChangeToken` в одном объекте, используйте класс [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken).</span><span class="sxs-lookup"><span data-stu-id="6b3fc-206">For representing one or more `IChangeToken` instances in a single object, use the [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) class.</span></span>

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

<span data-ttu-id="6b3fc-207">`HasChanged` для составного токена указывает `true`, если `HasChanged` любой из представленных токенов имеет значение `true`.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-207">`HasChanged` on the composite token reports `true` if any represented token `HasChanged` is `true`.</span></span> <span data-ttu-id="6b3fc-208">`ActiveChangeCallbacks` для составного токена указывает `true`, если `ActiveChangeCallbacks` любой из представленных токенов имеет значение `true`.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-208">`ActiveChangeCallbacks` on the composite token reports `true` if any represented token `ActiveChangeCallbacks` is `true`.</span></span> <span data-ttu-id="6b3fc-209">Если возникает несколько одновременных событий изменения, обратный вызов составного изменения выполняется всего один раз.</span><span class="sxs-lookup"><span data-stu-id="6b3fc-209">If multiple concurrent change events occur, the composite change callback is invoked exactly one time.</span></span>

## <a name="see-also"></a><span data-ttu-id="6b3fc-210">См. также</span><span class="sxs-lookup"><span data-stu-id="6b3fc-210">See also</span></span>

* [<span data-ttu-id="6b3fc-211">Кэш в памяти</span><span class="sxs-lookup"><span data-stu-id="6b3fc-211">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="6b3fc-212">Работа с распределенным кэшем</span><span class="sxs-lookup"><span data-stu-id="6b3fc-212">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="6b3fc-213">Обнаружение изменений с помощью маркеров изменений</span><span class="sxs-lookup"><span data-stu-id="6b3fc-213">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="6b3fc-214">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="6b3fc-214">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="6b3fc-215">ПО промежуточного слоя для кэширования ответов</span><span class="sxs-lookup"><span data-stu-id="6b3fc-215">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="6b3fc-216">Вспомогательная функция тега кэша</span><span class="sxs-lookup"><span data-stu-id="6b3fc-216">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="6b3fc-217">Вспомогательная функция тега распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="6b3fc-217">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
