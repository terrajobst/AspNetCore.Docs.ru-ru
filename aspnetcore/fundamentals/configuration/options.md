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
ms.openlocfilehash: aab96b5313a8632950e51f5586612c1d0d3d176e
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2018
---
# <a name="options-pattern-in-aspnet-core"></a>Параметры шаблона в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Шаблон параметров использует классы параметров для представления групп связанных настроек. Когда параметры конфигурации, изолируются компонент в классы отдельных параметров, приложение устанавливает двух принципов важные программного обеспечения:

* [Принцип разделения интерфейс (ISP)](http://deviq.com/interface-segregation-principle/): функции (классы), зависящих от параметров конфигурации зависят от только параметры конфигурации, которые они используют.
* [Разделение областей ответственности](http://deviq.com/separation-of-concerns/): параметры для разных частей приложения, не зависящие от них или связанные друг с другом.

[Просмотреть или загрузить образец кода](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) проще выполнить с помощью образца приложения в этой статье.

## <a name="basic-options-configuration"></a>Основные параметры конфигурации

Основные параметры конфигурации, представленной в качестве примера &num;1 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Класс параметров должен быть абстрактным открытый конструктор без параметров. Следующий класс `MyOptions`, имеет два свойства `Option1` и `Option2`. Установка значений по умолчанию является необязательным, но конструктор класса, в следующем примере задает значение по умолчанию `Option1`. `Option2`значение по умолчанию, задать путем инициализации свойства непосредственно (*Models/MyOptions.cs*):

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` Класс добавляется в контейнер службы с [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) и привязан к конфигурации:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

Следующие страницы использует модель [внедрения зависимостей конструктор](xref:fundamentals/dependency-injection#what-is-dependency-injection) с [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) для доступа к параметрам (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Образец *appsettings.json* файл задает значения для `option1` и `option2`:

[!code-json[Main](options/sample/appsettings.json)]

При запуске приложения модель страницы `OnGet` метод возвращает строку, содержащую класс значения параметров:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Настройте параметры простой делегат

Демонстрируется настройка простыми параметрами с помощью делегата в качестве примера &num;2 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Использование делегата для задания значений параметров. В образце приложения используется `MyOptionsWithDelegateConfig` класса (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

В следующем коде секунды `IConfigureOptions<TOptions>` служба добавляется в контейнер службы. Она использует делегат для настройки привязки с `MyOptionsWithDelegateConfig`:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Можно добавить несколько поставщиков конфигурации. Поставщики конфигурации, доступные в пакетах NuGet. Время, они применены, они регистрируются.

Каждый вызов [Настройка&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) добавляет `IConfigureOptions<TOptions>` службу в контейнер службы. В предыдущем примере значения `Option1` и `Option2` указываются в *appsettings.json*, но значения `Option1` и `Option2` переопределяются настроенных делегата.

При включении более одной конфигурации службы указано последнее источник конфигурации *побеждает* и задает значение конфигурации. При запуске приложения модель страницы `OnGet` метод возвращает строку, содержащую класс значения параметров:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Изменение конфигурации

Демонстрируется изменение конфигурации в качестве примера &num;3 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Приложения следует создать классы параметров, относящихся к конкретному компоненту группы (классы) в приложении. Частей приложения, которые требуют значения конфигурации только должны иметь доступ к значения конфигурации, которые в них используются.

При привязке параметров конфигурации, каждое свойство в тип параметров привязывается к раздел конфигурации в формате `property[:sub-property:]`. Например `MyOptions.Option1` свойства, связанного с ключом `Option1`, который считывается из `option1` свойство в *appsettings.json*.

В следующем коде третий `IConfigureOptions<TOptions>` служба добавляется в контейнер службы. Привязывает `MySubOptions` к разделу `subsection` из *appsettings.json* файла:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection` Требует метод расширения [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) пакет NuGet. Если приложение использует [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, пакет автоматически включается.

Образец *appsettings.json* файл определяет `subsection` элемент с разделов для `suboption1` и `suboption2`:

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions` Класс определяет свойства, `SubOption1` и `SubOption2`, для хранения значения суб-параметр (*Models/MySubOptions.cs*):

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

Модель страницы `OnGet` метод возвращает строку со значениями параметра вложенных (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

При запуске приложения `OnGet` метод возвращает строку, содержащую параметр вложенный класс значения:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Параметры, представленные с модели представления или представления прямого внедрения

Параметры, предоставляемый модели представления или с прямого представления путем внедрения кода демонстрируется пример &num;4 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Параметры могут передаваться в модель представления или путем вставки `IOptions<TOptions>` непосредственно в представление (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Прямые вставки внедрить `IOptions<MyOptions>` с `@inject` директиву:

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

При запуске приложения в отображаемой странице показаны значения параметров:

![Вариант 1 значения параметров: value1_from_json и вариант 2: -1 загружаются из модели и внедрение в представлении.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Перезагрузить данные конфигурации с IOptionsSnapshot

Повторная загрузка данных конфигурации с `IOptionsSnapshot` показано в следующем примере &num;5 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Требуется ASP.NET Core 1.1 или более поздней версии.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) поддерживает повторная загрузка параметров с минимальными лишнюю работу. В ASP.NET Core 1.1 `IOptionsSnapshot` представляет собой моментальный снимок [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) и обновления автоматически каждый раз, когда монитор запускает изменения, основанные на изменение источника данных. В ASP.NET Core 2.0 и более поздних версиях параметры вычисляются один раз на каждый запрос, когда доступ к и кэшируется на время существования запроса.

В следующем примере показано, как новый `IOptionsSnapshot` создается после *appsettings.json* изменений (*Pages/Index.cshtml.cs*). Несколько запросов к серверу возвращать значения констант *appsettings.json* файл, пока не был изменен и перезагрузок конфигурации.

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

На следующем рисунке показана первоначального `option1` и `option2` значения загружены из *appsettings.json* файла:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Измените значения в *appsettings.json* файл `value1_from_json UPDATED` и `200`. Сохранить *appsettings.json* файла. Обновите браузер, чтобы увидеть, что значения параметров будут обновлены:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Поддержка параметров с IConfigureNamedOptions именованного

Поддержка параметров с именем [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) демонстрируется пример &num;6 в [пример приложения](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Требуется ASP.NET Core 2.0 или более поздней версии.*

*Именованные параметры* поддержки позволяет приложению различать именованные параметры конфигурации. В примере приложения именованные параметры должны быть объявлены с [ConfigureNamedOptions&lt;TOptions&gt;. Настройка](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) метод:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

Пример приложения, обращающийся к именованные параметры с [IOptionsSnapshot&lt;TOptions&gt;. Получить](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Выполнение образца приложения, возвращаются именованные параметры.

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1`Предоставляет значения из конфигурации, из которого загружаются *appsettings.json* файла. `named_options_2`Предоставляет значения по:

* `named_options_2` Делегата в `ConfigureServices` для `Option1`.
* Значение по умолчанию для `Option2` , предоставляемые `MyOptions` класса.

Настройте все экземпляры именованных параметров с [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) метод. В следующем коде настраивается `Option1` для всех именованных экземпляров конфигурации с общим значением. Добавьте следующий код вручную на `Configure` метод:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Выполнение образца приложения после добавления кода выдает следующий результат:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> В ASP.NET Core 2.0 и более поздних версиях все параметры являются именованными экземплярами. Существующие `IConfigureOption` экземпляры рассматриваются как предназначенные для `Options.DefaultName` экземпляра, то есть `string.Empty`. `IConfigureNamedOptions`также реализует `IConfigureOptions`. Реализация по умолчанию [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([ссылки на источник](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) содержит логику для использования каждого соответствующим образом. `null` Именованный параметр используется для всех экземпляров с именами вместо указанный именованный экземпляр ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) и [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) использующих данное соглашение о).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*Требуется ASP.NET Core 2.0 или более поздней версии.*

Задать postconfiguration с [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). Запускается после окончания postconfiguration [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) происходит конфигурации:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) для последующей настройки именованные параметры:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Используйте [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) для последующей настройки всех именованных экземпляров конфигурации:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Параметры фабрики, мониторинга и кэша

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) используется для получения уведомлений при `TOptions` экземпляров изменений. `IOptionsMonitor`поддерживает reloadable "Параметры", уведомления об изменениях, и `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) создание новых параметрах экземпляров отвечает (ядро ASP.NET 2.0 или более поздней версии). Он имеет один [создать](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) метод. Реализация по умолчанию принимает все зарегистрированные `IConfigureOptions` и `IPostConfigureOptions` и запускает все сначала настраивается, за которым следует после настраивает. Позволяют разделить `IConfigureNamedOptions` и `IConfigureOptions` и вызывает только соответствующий интерфейс.

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ядро ASP.NET 2.0 или более поздней версии) используется `IOptionsMonitor` кэш `TOptions` экземпляров. `IOptionsMonitorCache` Делает недействительными параметры экземпляров в мониторе, чтобы значение будет пересчитано ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Значения могут быть вручную обеспечены также [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). [Снимите](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) метод используется, когда все именованные экземпляры может быть повторно создан по требованию.

## <a name="accessing-options-during-startup"></a>Доступ к параметры во время запуска

`IOptions`можно использовать в `Configure`, поскольку службы построены перед `Configure` выполнения метода. Если поставщик служб построен `ConfigureServices` для доступа к параметрам, он не будет содержать параметры конфигурации, предоставляемых после построения поставщика услуг. Таким образом в состояние несогласованные параметры могут существовать из-за порядок регистрации службы.

Поскольку параметры обычно загружаются из конфигурации, конфигурации можно использовать при запуске в обоих `Configure` и `ConfigureServices`. Примеры использования конфигурации во время запуска см. в разделе [запуска приложения](xref:fundamentals/startup) раздела.

## <a name="see-also"></a>См. также

* [Конфигурация](xref:fundamentals/configuration/index)
