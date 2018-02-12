---
title: "Запуск приложения в ASP.NET Core"
author: ardalis
description: "Сведения о том, как класс Startup в ASP.NET Core настраивает службы и конвейер запросов приложения."
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/startup
ms.openlocfilehash: cf9e6a25f5b9cc8395c803a11c15622349620a07
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="application-startup-in-aspnet-core"></a>Запуск приложения в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com) (Steve Smith), [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Люк Лэтем](https://github.com/guardrex) (Luke Latham)

Класс `Startup` настраивает службы и конвейер запросов приложения.

## <a name="the-startup-class"></a>Класс Startup

Приложения ASP.NET Core используют класс `Startup`, который по соглашению называется `Startup`. Класс `Startup`:

* При необходимости может включать метод [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) для настройки служб приложения.
* Должен включать метод [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) для создания конвейера обработки запросов приложения.

`ConfigureServices` и `Configure` вызываются средой выполнения при запуске приложения:

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Укажите класс `Startup` с методом [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_):

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Конструктор классов `Startup` принимает зависимости, определенные узлом. Типичным применением [внедрения зависимостей](xref:fundamentals/dependency-injection) в класс `Startup` является внедрение:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) для настройки служб средой.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для настройки приложения во время запуска.

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Альтернативой внедрению `IHostingEnvironment` является использование подхода на основе соглашений. Приложение может определять отдельные классы `Startup` для различных сред (например, `StartupDevelopment`), при этом подходящий класс запуска выбирается во время выполнения. Класс, у которого суффикс имени соответствует текущей среде, получает приоритет. Если приложение выполняется в среде разработки и включает в себя оба класса — `Startup` и `StartupDevelopment`, используется класс `StartupDevelopment`. Дополнительные сведения см. в разделе [Работа с несколькими средами](xref:fundamentals/environments#startup-conventions).

Дополнительные сведения о `WebHostBuilder` см. в разделе [Размещение](xref:fundamentals/hosting). Сведения об обработке ошибок во время запуска см. в разделе [Обработка исключений при запуске](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Метод ConfigureServices

Метод [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices):

* Необязательный.
* Вызывается веб-узлом перед методом `Configure` для настройки служб приложения.
* По соглашению используется для задания [параметров конфигурации](xref:fundamentals/configuration/index).

Добавление служб в контейнер службы делает их доступными в приложении и методе `Configure`. Службы разрешаются посредством [внедрения зависимостей](xref:fundamentals/dependency-injection) или из [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Веб-узел может настраивать некоторые службы перед вызовом методов `Startup`. Дополнительные сведения см. в разделе [Размещение](xref:fundamentals/hosting). 

Для функций, нуждающихся в значительной настройке, существуют методы расширения `Add[Service]` в [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Обычное веб-приложение регистрирует службы для Entity Framework, удостоверения и MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Службы, доступные в классе Startup

Веб-узел предоставляет несколько служб, которые доступны конструктору классов `Startup`. Приложение добавляет дополнительные службы через `ConfigureServices`. Как службы узла, так и службы приложения доступны в `Configure` и во всем приложении.

## <a name="the-configure-method"></a>Метод конфигурации

Метод [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) используется для указания того, как приложение реагирует на HTTP-запросы. Конвейер запросов настраивается путем добавления компонентов [ПО промежуточного слоя](xref:fundamentals/middleware) в экземпляр [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` доступен для метода `Configure`, но он не зарегистрирован в контейнере службы. Размещение создает `IApplicationBuilder` и передает его непосредственно в `Configure` ([источник ссылки](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[Шаблоны ASP.NET Core](/dotnet/core/tools/dotnet-new) настраивают конвейер, добавляя поддержку страницы исключений разработчика, [BrowserLink](http://vswebessentials.com/features/browserlink), страниц ошибок, статических файлов и ASP.NET MVC:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Каждый метод расширения `Use` добавляет компонент ПО промежуточного слоя в конвейер запросов. Например, метод расширения `UseMvc` добавляет в конвейер [ПО промежуточного слоя для маршрутизации](xref:fundamentals/routing) и настраивает [MVC](xref:mvc/overview) в качестве обработчика по умолчанию.

В сигнатуре метода также могут быть указаны дополнительные службы, такие как `IHostingEnvironment` и `ILoggerFactory`. Когда дополнительные службы указаны, они внедряются, если доступны.

Дополнительные сведения об использовании `IApplicationBuilder` см. в разделе [ПО промежуточного слоя](xref:fundamentals/middleware).

## <a name="convenience-methods"></a>Удобный метод

Удобные методы [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) и [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) можно использовать вместо указания класса `Startup`. Несколько вызовов `ConfigureServices` добавляются друг к другу. Несколько вызовов `Configure` используют вызов последнего метода.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a>Фильтры запуска

Используйте [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) для настройки ПО промежуточного слоя в начале или конце конвейера ПО промежуточного слоя [Configure](#the-configure-method) приложения. `IStartupFilter` удобно использовать, чтобы обеспечить запуск ПО промежуточного слоя до или после ПО промежуточного слоя, добавляемого библиотеками в начале или в конце конвейера обработки запросов приложения.

`IStartupFilter` реализует отдельный метод [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), который принимает и возвращает `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) определяет класс для настройки конвейера запросов приложения. Дополнительные сведения см. в разделе [Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).

Каждый `IStartupFilter` реализует один или несколько компонентов ПО промежуточного слоя в конвейере запросов. Фильтры вызываются в том порядке, в котором они были добавлены в контейнер службы. Фильтры могут добавлять ПО промежуточного слоя до или после передачи управления следующему фильтру, поэтому они добавляются в начало или конец конвейера приложения.

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)) показывает, как зарегистрировать ПО промежуточного слоя с помощью `IStartupFilter`. Этот пример включает ПО промежуточного слоя, которое задает значение параметров из параметра строки запроса:

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` настраивается в классе `RequestSetOptionsStartupFilter`:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` регистрируется в контейнере службы в `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Если указан параметр строки запроса для `option`, ПО промежуточного слоя обрабатывает присвоение значения до того, как ПО промежуточного слоя MVC отображает отклик:

![Окно обозревателя с отображаемой страницей индекса. Значение параметра отображается как "From Middleware", так как запрашивается страница с помощью параметра строки запроса и задано значение параметра "From Middleware".](startup/_static/index.png)

Порядок выполнения ПО промежуточного слоя определяется порядком регистраций `IStartupFilter`:

* Несколько реализаций `IStartupFilter` могут взаимодействовать с одними и теми же объектами. Если важен порядок, упорядочите регистрации службы `IStartupFilter` в соответствии с требуемым порядком выполнения ПО промежуточного слоя.
* Библиотеки могут добавлять ПО промежуточного слоя с одной или несколькими реализациями `IStartupFilter`, которые выполняются до или после другого ПО промежуточного слоя приложения, зарегистрированного с помощью `IStartupFilter`. Чтобы вызвать ПО промежуточного слоя `IStartupFilter` до ПО промежуточного слоя, добавляемого библиотекой `IStartupFilter`, расположите регистрацию службы до добавления библиотеки в контейнер службы. Чтобы вызвать его после этого момента, расположите регистрацию службы после добавления библиотеки.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение](xref:fundamentals/hosting)
* [Работа с несколькими средами](xref:fundamentals/environments)
* [ПО промежуточного слоя](xref:fundamentals/middleware)
* [Ведение журнала](xref:fundamentals/logging/index)
* [Конфигурация](xref:fundamentals/configuration/index)
* [Класс StartupLoader: метод FindStartupType (источник ссылки)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
