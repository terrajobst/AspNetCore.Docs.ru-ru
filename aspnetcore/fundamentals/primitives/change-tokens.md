---
title: "Обнаружения изменений с маркерами изменения в ASP.NET Core"
author: guardrex
description: "Сведения об использовании маркеров изменений для отслеживания изменений."
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: a9479e3d676ed4dc880996a4a77de30d82b84cd5
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Обнаружения изменений с маркерами изменения в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Объект *изменить маркер* является общего назначения, низкоуровневые стандартного блока, используемый для отслеживания изменений.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>Интерфейс IChangeToken

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) распространяет уведомления о том, что произошло изменение. `IChangeToken`находится в [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) пространства имен. Для приложений, которые не используют [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage ссылки [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) пакета NuGet в файле проекта.

`IChangeToken`имеет два свойства:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) указывать, если маркер заранее вызывает обратных вызовов. Если `ActiveChangedCallbacks` равно `false`, никогда не вызывается, обратный вызов, и приложения должен опросить `HasChanged` для изменения. Можно также маркер никогда не будут отменены, если изменения не происходят или базовый прослушиватель изменений удален или отключен.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) возвращает значение, указывающее, если произошло изменение.

Этот интерфейс содержит один метод [RegisterChangeCallback (действие&lt;объекта&gt;, объект)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), который регистрирует обратный вызов, вызываемый при изменении маркер. `HasChanged`необходимо задать до вызова функции обратного вызова.

## <a name="changetoken-class"></a>Класс маркер изменения

`ChangeToken`статический класс, используемый для распространения уведомления о том, что произошло изменение. `ChangeToken`находится в [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) пространства имен. Для приложений, которые не используют [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage ссылки [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) пакета NuGet в файле проекта.

`ChangeToken` [OnChange (Func&lt;IChangeToken&gt;, действия)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) регистры метод `Action` вызывается при каждом изменении токен:
* `Func<IChangeToken>`Создает маркер.
* `Action`вызывается при изменении маркер.

`ChangeToken`имеет [OnChange&lt;температурного состояния&gt;(Func&lt;IChangeToken&gt;, действие&lt;температурного состояния&gt;, температурного состояния)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) перегрузка, которая принимает дополнительные `TState`параметра, передаваемого в токен потребитель `Action`.

`OnChange`Возвращает [IDisposable](/dotnet/api/system.idisposable). Вызов [Dispose](/dotnet/api/system.idisposable.dispose) останавливает маркер от прослушивания для дальнейших изменений и освобождает ресурсы, маркер.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Пример использует токены изменений в ASP.NET Core

Маркеры изменения используются в Показательным областей ASP.NET Core, отслеживание изменений в объекты:

* Для отслеживания изменений в файлы, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) [Контрольные значения](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) метод создает `IChangeToken` для указанных файлов или папку для наблюдения.
* `IChangeToken`Токены можно добавить для записей кэша для запуска вытеснений кэш при изменении.
* Для `TOptions` изменяет значение по умолчанию [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) реализация [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) имеет перегрузку, которая принимает один или несколько [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)экземпляры. Возвращает каждый экземпляр `IChangeToken` для регистрации обратного вызова уведомления изменений для изменения параметров отслеживания.

## <a name="monitoring-for-configuration-changes"></a>Наблюдение за изменения конфигурации

По умолчанию используют шаблоны ASP.NET Core [файлы конфигурации JSON](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *appsettings. Development.JSON*, и *appsettings. Production.JSON*) для загрузки параметров конфигурации приложения.

Эти файлы настраиваются с помощью [AddJsonFile (IConfigurationBuilder, String, Boolean, Boolean)](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) метод расширения в [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) , принимающий `reloadOnChange` параметра (ASP.NET Базовая 1.1 и более поздние версии). `reloadOnChange`Указывает, следует перезагружена на изменения в файле конфигурации. Этот параметр в разделе [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) удобный метод [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([ссылки на источник](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Конфигурация на основе файла, представленного [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource`использует [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([ссылки на источник](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) для наблюдения за файлами.

По умолчанию `IFileMonitor` обеспечивается [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([ссылки на источник](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), которая использует [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) для отслеживания для файла конфигурации изменения.

В примере приложения показано две реализации для наблюдения за изменениями конфигурации. Если параметр *appsettings.json* изменения в файле или версия среды файла изменяется, каждая реализация выполняет пользовательский код. Пример приложения записывает сообщение на консоль.

Файл конфигурации `FileSystemWatcher` может активировать несколько маркеров обратные вызовы для изменения файла одной конфигурации. Пример реализации помогает защититься от этой проблемы, проверьте хэшей файлов для файлов конфигурации. Проверка хэшей файлов обеспечивает, по крайней мере один из файлов конфигурации изменилась перед запуском пользовательского кода. В этом образце используется хэширования SHA1 файла (*Utilities/Utilities.cs*):

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Повторная попытка реализуется с помощью экспоненциальной отсрочки. Повторите попытку присутствует, так как блокировка файлов может возникать, предотвращающего вычисление нового хэша на один из файлов.

### <a name="simple-startup-change-token"></a>Маркер изменений простого запуска

Регистрация маркера потребителя `Action` обратный вызов для получения уведомлений об изменениях к маркеру перезагрузку конфигурации (*файла Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()`Возвращает токен. Обратный вызов `InvokeChanged` метод:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

`state` Обратного вызова, используемый для передачи в `IHostingEnvironment`. Это полезно для определения правильного *appsettings* JSON-файл конфигурации для мониторинга, *appsettings.&lt; Среда&gt;.json*. Хэшей файлов используются для предотвращения `WriteConsole` инструкции запуска несколько раз из-за нескольких токенов обратных вызовов при изменении файла конфигурации только один раз.

Эта система выполняется при условии, что приложение работает и не может быть отключено пользователем.

### <a name="monitoring-configuration-changes-as-a-service"></a>Отслеживание изменений конфигурации, как служба

В этом образце реализуется:

* Основные маркера наблюдение за запуском.
* Мониторинг в качестве службы.
* Механизм для включения и отключения наблюдения.

Образец устанавливает `IConfigurationMonitor` интерфейса (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Конструктор реализованный класс `ConfigurationMonitor`, регистрирует обратный вызов уведомления об изменениях:

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()`предоставляет токен. `InvokeChanged`Представляет метод обратного вызова. `state` В этом примере — строка, описывающая состояние мониторинга. Используются два свойства:

* `MonitoringEnabled`Указывает, если обратный вызов необходимо выполнить его пользовательский код.
* `CurrentState`Описывает текущее состояние мониторинга для использования в пользовательском Интерфейсе.

`InvokeChanged` Метод похож на более ранних подход, за исключением того, что он:

* Его код не запускается, если не `MonitoringEnabled` — `true`.
* Наборы `CurrentState` строки свойства описательного сообщения, которое записывает время выполнения кода.
* Заметки о текущего `state` в его `WriteConsole` выходных данных.

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Экземпляр `ConfigurationMonitor` зарегистрирован как служба в `ConfigureServices` из *файла Startup.cs*:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

Страница индекса обеспечивает контроль пользователя мониторинг конфигурации. Экземпляр `IConfigurationMonitor` вставляется в `IndexModel`:

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Кнопка включает и отключает наблюдение за:

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Если `OnPostStartMonitoring` является, мониторинг включен, а также текущее состояние будет очищено. Когда `OnPostStopMonitoring` будет запущен, мониторинг отключен и переходит в состояние, чтобы отразить не выполняется мониторинг.

## <a name="monitoring-cached-file-changes"></a>Наблюдение за изменениями кэшированных файлов

Содержимое файла может быть в кэшированные в памяти с помощью [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Кэширование в памяти описывается в [кэширование в памяти](xref:performance/caching/memory) раздела. Без учета дополнительные действия, как описано ниже, реализация *устаревших* (устаревшие) данные возвращаются из кэша при изменении источника данных.

Без учета состояния кэшированный исходный файл при обновлении [скользящий срок действия](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) периода приводит к устаревшие данные из кэша. Каждый запрос данных обновляет скользящего срока действия, но файл никогда не загружается в кэш. Все функции приложения, использующие кэшированное содержимое файла подвержены возможно получение устаревших содержимого.

При использовании маркеров изменения в сценарий кэширования файлов не устаревших файлов содержимого в кэше. Пример приложения демонстрирует реализацию подхода.

В этом образце используется `GetFileContent` для:

* Возвращает содержимое файла.
* Реализуйте алгоритм повторения с экспоненциальная титульных случаи, где блокировка файла временно препятствует файл выполняется чтение.

*Utilities/Utilities.cs*:

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Объект `FileService` создается для обработки поиск кэшированных файлов. `GetFileContent` Вызова метода службы пытается получить содержимое файла из кэша в памяти и возвращается вызывающему объекту (*Services/FileService.cs*).

Если кэшированное содержимое не будет найден с помощью ключа кэша, выполняются следующие действия.

1. Содержимое файла будет получен с использованием `GetFileContent`.
1. Маркер изменений извлекается из файла поставщика с [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Токен обратного вызова активируется при изменении файла.
1. Содержимое файла кэшируется с [скользящий срок действия](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) период. Маркер изменений, присоединенной к [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) удаления записи кэша, если файл изменяется, пока он кэшируется.

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

`FileService` Регистрируется в контейнере службы вместе с кэширование службы памяти (*файла Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

Модель страницы с загружает содержимое файла с помощью службы (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>Класс CompositeChangeToken

Для представления одного или нескольких `IChangeToken` экземпляры в одном объекте используют [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) класса ([ссылки на источник](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).

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

`HasChanged`для составного маркера отчетов `true` Если любой представить маркер `HasChanged` — `true`. `ActiveChangeCallbacks`для составного маркера отчетов `true` Если любой представить маркер `ActiveChangeCallbacks` — `true`. Если несколько одновременных изменений события происходят, составного изменение обратный вызов выполняется только один раз.

## <a name="see-also"></a>См. также

* [Кэширование в памяти](xref:performance/caching/memory)
* [Работа с распределенного кэша](xref:performance/caching/distributed)
* [Обнаруживать изменения с маркерами изменения](xref:fundamentals/primitives/change-tokens)
* [Кэширование ответов](xref:performance/caching/response)
* [ПО промежуточного слоя для кэширования ответов](xref:performance/caching/middleware)
* [Вспомогательный тег кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Вспомогательный тег распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
