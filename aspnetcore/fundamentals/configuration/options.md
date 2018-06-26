---
title: Шаблон параметров в ASP.NET Core
author: guardrex
description: Узнайте, как использовать шаблон параметров для представления групп связанных параметров в приложениях ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: 11f3e0b0cc1356db4c5fb9a2ce948099ed9f85b5
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252390"
---
# <a name="options-pattern-in-aspnet-core"></a>Шаблон параметров в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Шаблон параметров использует классы для представления групп связанных настроек. Когда параметры конфигурации изолируются по функции в отдельных классах, в приложениях соблюдаются два важных принципа программной инженерии.

* [Принцип изоляции интерфейсов](http://deviq.com/interface-segregation-principle/). Функции (классы), которые зависят от параметров конфигурации, зависят только от используемых ими параметров конфигурации.
* [Разделение ответственности](http://deviq.com/separation-of-concerns/). Параметры для разных частей приложения не зависят друг от друга и не связаны друг с другом.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample)). Информацию в этой статье будет проще воспринимать, если пользоваться примером приложения.

## <a name="basic-options-configuration"></a>Конфигурация основных параметров

Конфигурация основных параметров демонстрируется в примере № 1 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Класс параметров должен быть неабстрактным с открытым конструктором без параметров. Приведенный ниже класс (`MyOptions`) имеет два свойства: `Option1` и `Option2`. Задавать значения по умолчанию необязательно, однако конструктор класса в этом примере задает значение по умолчанию для `Option1`. Значение по умолчанию для `Option2` устанавливается путем прямой инициализации (*Models/MyOptions.cs*).

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

Класс `MyOptions` добавляется в контейнер службы с помощью [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) и привязывается к конфигурации:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

Следующая страничная модель использует [внедрение зависимостей конструктора](xref:fundamentals/dependency-injection#what-is-dependency-injection) с помощью [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) для доступа к параметрам (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

В файле *appsettings.json* образца задаются значения для свойств `option1` и `option2`:

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Настройка простых параметров с помощью делегата

Настройка простых параметров с помощью делегата демонстрируется в примере № 2 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Используйте делегат для задания значений параметров. В образце приложения используется класс `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

В приведенном ниже коде в контейнер службы добавляется еще одна служба `IConfigureOptions<TOptions>`. Она использует делегат для настройки привязки к `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Можно добавить несколько поставщиков конфигурации. Поставщики конфигурации доступны в пакетах NuGet. Они применяются в порядке регистрации.

Каждый вызов [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) добавляет службу `IConfigureOptions<TOptions>` в контейнер службы. В предыдущем примере значения `Option1` и `Option2` задаются в файле *appsettings.json*, однако значения `Option1` и `Option2` переопределяются настроенным делегатом.

Если включено более одной службы конфигурации, последний источник конфигурации *имеет приоритет* и определяет значение конфигурации. Когда приложение выполняется, метод `OnGet` страничной модели возвращает строку со значениями класса параметров:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Конфигурация подпараметров

Конфигурация подпараметров демонстрируется в примере № 3 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

В приложениях должны создаваться классы приложений, относящиеся к определенным группам функциональных возможностей (классам). Части приложения, которым требуются значения конфигурации, должны иметь доступ только к используемым ими значениям конфигурации.

При привязке параметров к конфигурации каждое свойство, относящееся к типу параметров, привязывается к ключу конфигурации в формате `property[:sub-property:]`. Например, свойство `MyOptions.Option1` привязывается к ключу `Option1`, который считывается из свойства `option1` в файле *appsettings.json*.

В приведенном ниже коде в контейнер службы добавляется третья служба `IConfigureOptions<TOptions>`. Она привязывает `MySubOptions` к разделу `subsection` файла *appsettings.json*.

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

Методу расширения `GetSection` требуется пакет NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/). Если приложение использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии), пакет включается автоматически.

В файле *appsettings.json* образца определяется член `subsection` с ключами для `suboption1` и `suboption2`.

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

Класс `MySubOptions` определяет свойства `SubOption1` и `SubOption2` для хранения значений подпараметров (*Models/MySubOptions.cs*).

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

Метод `OnGet` страничной модели возвращает строку со значениями параметров (*Pages/Index.cshtml.cs*).

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Когда приложение выполняется, метод `OnGet` возвращает строку со значениями класса подпараметров:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Параметры, предоставляемые моделью представления или посредством прямого внедрения представления

Параметры, предоставляемые моделью представления или посредством прямого внедрения представления, демонстрируются в примере № 4 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Параметры могут передаваться в модель представления или путем внедрения `IOptions<TOptions>` непосредственно в представление (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Чтобы выполнить прямое внедрение, внедрите `IOptions<MyOptions>` с помощью директивы `@inject`:

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

При запуске приложения значения параметров отображаются на отрисовываемой странице:

![Значения параметров Option1: value1_from_json и Option2: –1 загружаются из модели и путем внедрения в представление.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Повторная загрузка данных конфигурации с помощью IOptionsSnapshot

Повторная загрузка данных конфигурации с помощью `IOptionsSnapshot` демонстрируется в примере № 5 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Требуется ASP.NET Core 1.1 или более поздней версии.*

Интерфейс [IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) поддерживает повторную загрузку параметров с минимальными затратами на обработку. В ASP.NET Core 1.1 интерфейс `IOptionsSnapshot` является моментальным снимком [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) и обновляется автоматически каждый раз, когда монитор инициирует изменения при изменении источника данных. В ASP.NET Core 2.0 и более поздних версиях параметры вычисляются один раз на каждый запрос при обращении к ним и кэшируются на все время существования запроса.

В приведенном ниже примере демонстрируется создание экземпляра `IOptionsSnapshot` после изменения файла *appsettings.json* (*Pages/Index.cshtml.cs*). Несколько запросов к серверу возвращают константные значения, предоставляемые файлом *appsettings.json*, пока файл не изменится и конфигурация не загрузится повторно.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Ниже показаны начальные значения `option1` и `option2`, загруженные из файла *appsettings.json*.

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Измените значения в файле *appsettings.json* на `value1_from_json UPDATED` и `200`. Сохраните файл *appsettings.json*. Обновите окно браузера, чтобы увидеть обновленные значения параметров:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Поддержка именованных параметров в IConfigureNamedOptions

Поддержка именованных параметров в [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) демонстрируется в примере № 6 в [образце приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Требуется ASP.NET Core 2.0 или более поздней версии.*

Поддержка *именованных параметров* позволяет приложению различать конфигурации именованных параметров. В примере приложения именованные параметры должны быть объявлены с помощью [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure), что, в свою очередь, вызывает метод расширения [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure):

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

Образец приложения обращается к именованным параметрам с помощью метода [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

При запуске образца приложения возвращаются именованные параметры:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

Значения `named_options_1` предоставляются из конфигурации, которая загружается из файла *appsettings.json*. Значения `named_options_2` предоставляются следующим образом:

* Делегатом `named_options_2` в `ConfigureServices` для `Option1`.
* Значение по умолчанию для `Option2` предоставляется классом `MyOptions`.

Настройте все экземпляры именованных параметров с помощью метода [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall). Приведенный ниже код настраивает `Option1` для всех именованных экземпляров конфигурации с общим значением. Добавьте следующий код в метод `Configure` вручную:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Если запустить образец приложения после добавления, результат будет следующим:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> В ASP.NET Core 2.0 и более поздних версиях все параметры являются именованными экземплярами. Существующие экземпляры `IConfigureOption` считаются нацеленными на экземпляр `Options.DefaultName`, который имеет значение `string.Empty`. Интерфейс `IConfigureNamedOptions` также реализует интерфейс `IConfigureOptions`. Реализация [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) по умолчанию ([источник ссылки](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) содержит логику для надлежащего использования каждого экземпляра. Именованный параметр `null` предназначен для всех именованных экземпляров, а не для какого-то определенного ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) и [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) следуют этому соглашению).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*Требуется ASP.NET Core 2.0 или более поздней версии.*

Задайте пост-конфигурацию с помощью [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). Пост-конфигурация применяется после выполнения всех действий настройки с помощью [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Для последующей настройки именованных параметров доступен метод [PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure):

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Для последующей настройки всех именованных экземпляров конфигурации служит метод [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall):

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Фабрика, мониторинг и кэш параметров

Интерфейс [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) служит для создания уведомлений об изменении экземпляров `TOptions`. `IOptionsMonitor` поддерживает перезагружаемые параметры, уведомления об изменениях и `IPostConfigureOptions`.

Интерфейс [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 или более поздней версии) отвечает за создание экземпляров параметров. Он имеет единственный метод [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create). Реализация по умолчанию принимает все зарегистрированные интерфейсы `IConfigureOptions` и `IPostConfigureOptions` и выполняет сначала все конфигурации, а затем постконфигурации. Она различает интерфейсы `IConfigureNamedOptions` и `IConfigureOptions` и вызывает только соответствующий интерфейс.

Интерфейс [IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 или более поздней версии) используется интерфейсом `IOptionsMonitor` для кэширования экземпляров `TOptions`. `IOptionsMonitorCache` делает экземпляры параметров в мониторе недействительными, что приводит к повторному вычислению значений ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Значения можно также вводить вручную с помощью [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). Метод [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) используется, если необходимо повторно создать все именованные экземпляры по требованию.

## <a name="accessing-options-during-startup"></a>Доступ к параметрам во время запуска

`IOptions` можно использовать в методе `Configure`, так как службы создаются до выполнения метода `Configure`. Если в `ConfigureServices` создается поставщик службы для доступа к параметрам, он не будет содержать конфигурации параметров, предоставленные после его создания. Поэтому из-за очередности регистрации служб состояние параметров может быть несогласованным.

Так как параметры, как правило, загружаются из конфигурации, конфигурация может использоваться при запуске как в `Configure`, так и в `ConfigureServices`. Примеры использования конфигурации во время запуска см. в статье [Запуск приложения](xref:fundamentals/startup).

## <a name="see-also"></a>См. также

* [Конфигурация](xref:fundamentals/configuration/index)
