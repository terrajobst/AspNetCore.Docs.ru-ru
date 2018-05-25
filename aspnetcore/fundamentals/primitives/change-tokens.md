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
ms.openlocfilehash: 06751e713fbd579a944333cc3c3b2c0c0ad51eba
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Обнаружение изменений с помощью токенов изменений в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

*Токен изменений* является низкоуровневым стандартным блоком общего назначения, используемым для отслеживания изменений.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Интерфейс IChangeToken

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) распространяет уведомления о том, что произошло изменение. `IChangeToken` находится в пространстве имен [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives). Для приложений, которые не используют метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), сошлитесь на пакет NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) в файле проекта.

`IChangeToken` имеет два свойства:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) указывает, выполняет ли токен обратные вызовы с упреждением. Если для `ActiveChangedCallbacks` задано значение `false`, обратный вызов не выполняется, а приложению нужно опросить `HasChanged` на предмет изменений. Кроме того, отмена токена может никогда не произойти в случае отсутствия изменений или отключения либо удаления базового прослушивателя изменений.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) возвращает значение, указывающее, произошло ли изменение.

Интерфейс имеет один метод — [RegisterChangeCallback(Action&lt;Object&gt;, Object)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), который регистрирует обратный вызов, выполняемый при изменении токена. Перед выполнением обратного вызова нужно задать `HasChanged`.

## <a name="changetoken-class"></a>Класс ChangeToken

`ChangeToken` — это статический класс, который распространяет уведомления о том, что произошло изменение. `ChangeToken` находится в пространстве имен [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives). Для приложений, которые не используют метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), сошлитесь на пакет NuGet [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) в файле проекта.

Метод `ChangeToken` [OnChange(Func&lt;IChangeToken&gt;, Action)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) регистрирует `Action`, вызываемый при изменении токена:
* `Func<IChangeToken>` создает токен.
* `Action` вызывается при изменении токена.

`ChangeToken` имеет перегрузку [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Action&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_), которая принимает дополнительный параметр `TState`, передаваемый в объект-получатель токена `Action`.

`OnChange` возвращает [IDisposable](/dotnet/api/system.idisposable). При вызове [Dispose](/dotnet/api/system.idisposable.dispose) токен прекращает прослушивать дальнейшие изменения и освобождает свои ресурсы.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Примеры использования токенов изменений в ASP.NET Core

Токены изменений используются в ключевых областях ASP.NET Core для отслеживания изменений в объектах:

* Чтобы отслеживать изменения в файлах, метод [Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) интерфейса [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) создает `IChangeToken` для указанных файлов или папки.
* Токены `IChangeToken` можно добавлять в записи кэша, чтобы активировать вытеснения кэша при изменении.
* Для изменений `TOptions` используемая по умолчанию реализация [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) интерфейса [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) имеет перегрузку, которая принимает один или несколько экземпляров [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1). Каждый экземпляр возвращает `IChangeToken`, чтобы зарегистрировать обратный вызов уведомления об изменении для отслеживания изменений параметров.

## <a name="monitoring-for-configuration-changes"></a>Отслеживание изменений конфигурации

По умолчанию шаблоны ASP.NET Core используют [файлы конфигурации JSON](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings.Development.json* и *appsettings.Production.json*) для загрузки параметров конфигурации приложения.

Эти файлы настраиваются с помощью метода расширения [AddJsonFile(IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) в [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder), который принимает параметр `reloadOnChange` (ASP.NET Core 1.1 и более поздней версии). `reloadOnChange` указывает, нужно ли перезагружать конфигурацию при изменении файла. Просмотрите этот параметр в удобном методе [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Конфигурация на основе файла представлена [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource` использует [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) для наблюдения за файлами.

По умолчанию `IFileMonitor` предоставляется [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider), который использует [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) для отслеживания изменений в файле конфигурации.

Этот пример приложения демонстрирует две реализации для отслеживания изменений конфигурации. Если изменяется файл *appsettings.json* или его версия среды, каждая реализация выполняет пользовательский код. Этот пример приложения записывает сообщение в консоль.

`FileSystemWatcher` файла конфигурации может активировать несколько обратных вызовов токена для одного изменения файла конфигурации. Реализация в примере защищает от этой проблемы, проверяя хэши для файлов конфигурации. Проверка хэшей файлов позволяет убедиться, что изменился по меньшей мере один из файлов конфигурации, прежде чем запускать пользовательский код. Этот пример использует хэширование файлов SHA1 (*Utilities/Utilities.cs*):

   [!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Повторная попытка реализуется посредством экспоненциальной задержки. Повторная попытка нужна, так как может возникать блокировка файлов, препятствующая вычислению нового хэша для одного из файлов.

### <a name="simple-startup-change-token"></a>Токен изменений для простого запуска

Зарегистрируйте обратный вызов `Action` объекта-получателя токена для уведомлений об изменениях в токене перезагрузки конфигурации (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()` предоставляет токен. Обратный вызов является методом `InvokeChanged`:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet3)]

`state` обратного вызова используется для передачи `IHostingEnvironment`. Это удобно для определения правильного JSON-файла конфигурации *appsettings*, который требуется отслеживать, *appsettings.&lt;среда&gt;.json*. Хэши файлов используются для предотвращения многократного выполнения оператора `WriteConsole` из-за нескольких обратных вызовов токена при всего одном изменении файла конфигурации.

Эта система выполняется, пока запущено приложение, и не может быть отключена пользователем.

### <a name="monitoring-configuration-changes-as-a-service"></a>Отслеживание изменений конфигурации как служба

Этот пример реализует следующее:

* Базовое отслеживание токена запуска.
* Отслеживание как служба.
* Механизм для включения и отключения отслеживания.

Этот пример задает интерфейс `IConfigurationMonitor` (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Конструктор реализованного класса `ConfigurationMonitor` регистрирует обратный вызов для уведомлений об изменениях:

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()` предоставляет токен. `InvokeChanged` является методом обратного вызова. `state` в этом экземпляре является ссылкой на экземпляр `IConfigurationMonitor`, используемый для доступа к состоянию мониторинга. Используются два свойства:

* `MonitoringEnabled` указывает, нужно ли обратному вызову выполнять свой пользовательский код.
* `CurrentState` описывает текущее состояние отслеживания для использования в пользовательском интерфейсе.

Метод `InvokeChanged` похож на описанный ранее подход, за исключением того, что он:

* не выполняет свой код, если только `MonitoringEnabled` не имеет значение `true`;
* записывает текущее значение `state` в своих выходных данных `WriteConsole`.

[!code-csharp[](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Экземпляр `ConfigurationMonitor` регистрируется как служба в `ConfigureServices` в файле *Startup.cs*:

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet1)]

Страница индекса позволяет пользователю управлять отслеживанием конфигурации. Экземпляр `IConfigurationMonitor` внедряется в `IndexModel`:

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Кнопка включает и отключает отслеживание:

[!code-cshtml[](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

При активации `OnPostStartMonitoring` отслеживание включается, а текущее состояние сбрасывается. При активации `OnPostStopMonitoring` отслеживание отключается, а состояние указывает на отсутствие отслеживания.

## <a name="monitoring-cached-file-changes"></a>Отслеживание изменений кэшированных файлов

Содержимое файла можно кэшировать в памяти с помощью [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Кэширование в памяти описано в разделе [Кэш в памяти](xref:performance/caching/memory). *Устаревшие* (просроченные) данные возвращаются из кэша при изменении источника исходных данных без каких-либо дополнительных действий, таких как описанная ниже реализация.

Если не учитывать состояние кэшированного исходного файла при продлении [скользящего срока действия](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration), это приведет к устаревшим данным кэша. Каждый запрос к данным продляет скользящий срок действия, но этот файл никогда не загружается в кэш. Любые функции приложения, использующие кэшированное содержимое, могут получить устаревшее содержимое.

Использование токенов изменений в сценарий кэширования файлов предотвращает появление устаревшего содержимого файлов в кэше. Этот пример демонстрирует реализацию данного подхода.

`GetFileContent` в примере используется для:

* возврата содержимого файла;
* реализации алгоритма повторных попыток с экспоненциальной задержкой, чтобы охватить ситуации, когда блокировка файла временно препятствует его чтению.

*Utilities/Utilities.cs*:

[!code-csharp[](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Для обработки поиска кэшированных файлов создается `FileService`. Направленный в службу вызов метода `GetFileContent` пытается получить содержимое файла из кэша в памяти и вернуть его вызывающему объекту (*Services/FileService.cs*).

Если кэшированное содержимое не удается найти по ключу кэша, выполняются следующие действия:

1. Содержимое файла получается с помощью `GetFileContent`.
1. Токен изменений извлекается из файла поставщика с помощью [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). При изменении файла активируется обратный вызов токена.
1. Содержимое файла кэшируется с использованием [скользящего срока действия](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration). Токен изменений подключается к [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) для исключения записи кэша, если файл изменяется во время его кэширования.

[!code-csharp[](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` регистрируется в контейнере службы вместе со службой кэширования памяти (*Startup.cs*):

[!code-csharp[](change-tokens/sample/Startup.cs?name=snippet4)]

Страничная модель загружает содержимое файла с помощью службы (*Pages/Index.cshtml.cs*):

[!code-csharp[](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Класс CompositeChangeToken

Чтобы представить один или несколько экземпляров `IChangeToken` в одном объекте, используйте класс [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken).

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

`HasChanged` для составного токена указывает `true`, если `HasChanged` любой из представленных токенов имеет значение `true`. `ActiveChangeCallbacks` для составного токена указывает `true`, если `ActiveChangeCallbacks` любой из представленных токенов имеет значение `true`. Если возникает несколько одновременных событий изменения, обратный вызов составного изменения выполняется всего один раз.

## <a name="see-also"></a>См. также

* [Кэш в памяти](xref:performance/caching/memory)
* [Работа с распределенным кэшем](xref:performance/caching/distributed)
* [Кэширование ответов](xref:performance/caching/response)
* [ПО промежуточного слоя для кэширования ответов](xref:performance/caching/middleware)
* [Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Вспомогательная функция тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
