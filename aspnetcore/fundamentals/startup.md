---
title: "Запуск приложения в ASP.NET Core"
author: ardalis
description: "Узнайте, как класс Startup в ASP.NET Core настраивает службы и конвейер обработки запросов приложения."
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: dd2eb3d3996bc0bf277c8d5e772c8568ef9f147e
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2017
---
# <a name="application-startup-in-aspnet-core"></a>Запуск приложения в ASP.NET Core

По [Стив Смит](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), и [Latham Люк](https://github.com/guardrex)

Класс `Startup` настраивает службы и конвейер обработки запросов приложения. 

## <a name="the-startup-class"></a>Класс Startup

Приложения ASP.NET Core используют класс `Startup`, который называется `Startup` по соглашению. Класс `Startup`:

* При необходимости может содержать метод [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) для настройки служб приложения. 
* Должен обязательно включать метод [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) для создания конвейера обработки запросов приложения. 

`ConfigureServices`и `Configure` вызываются средой выполнения при запуске приложения: 

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Укажите класс `Startup` в методе [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_):

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Конструктор класса `Startup` принимает зависимости, определенные средой размещения. Обычно [внедрение зависимостей](xref:fundamentals/dependency-injection) в классе `Startup` представляет собой передачу параметра [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) для настройки служб средой: 

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Альтернативой внедрения `IHostingStartup` является использовании подхода на основе соглашения. Приложение может определить отдельные `Startup` классы для различных сред (например, `StartupDevelopment`), и соответствующий класс выбирается во время выполнения. Класс, чей суффикс имени соответствует текущей среде имеет приоритет. Если приложение выполняется в среде разработки, и включает в себя классы `Startup` и `StartupDevelopment`, то будет использоваться `StartupDevelopment`. Дополнительные сведения см. [работа с несколькими средами](xref:fundamentals/environments#startup-conventions).

Дополнительные сведения о `WebHostBuilder` см. в разделе [Размещение](xref:fundamentals/hosting). Сведения об обработке ошибок во время запуска см. в разделе [Обработка исключений при запуске](xref:fundamentals/error-handling#startup-exception-handling). 

## <a name="the-configureservices-method"></a>Метод ConfigureServices

Метод [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices): 

* Необязательный.
* Вызывается веб-узлом перед методом `Configure` для настройки служб приложения. 
* Должен устанавливать [параметры конфигурации](xref:fundamentals/configuration/index) в соответствии с соглашением. 

Добавление служб в контейнер служб делает их доступными в приложении и в методе `Configure`. Службы разрешаются посредством [внедрения зависимостей](xref:fundamentals/dependency-injection) или [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices). 	

Веб-узел может настраивать некоторые службы до того, как будут вызваны методы класса `Startup`. Подробные сведения доступны в разделе [Размещения](xref:fundamentals/hosting). 

Для функций, требующих значительной настройки, существует метод расширения `Add[Service]` в [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Типичное веб-приложение регистрирует службы для платформ Entity Framework, Identity и MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Службы, доступные в Startup

Веб-узел предоставляет несколько служб, которые доступны для конструктора класса `Startup`. Приложение добавляет дополнительные службы через `ConfigureServices`. Затем службы узла и приложения доступны в `Configure` и в приложении.

## <a name="the-configure-method"></a>Метод Configure

Метод [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) используется для указания того, как приложение реагирует на запросы HTTP. Конвейер запросов настраивается путем добавления [по промежуточного слоя](xref:fundamentals/middleware) в экземпляр [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder`доступен для `Configure` метода, но он не зарегистрирован в контейнере службы. Размещение создает `IApplicationBuilder` и передает его непосредственно к `Configure` ([ссылки на источник](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[Шаблоны ASP.NET Core](/dotnet/core/tools/dotnet-new) имеют настройки конвейера с поддержкой страниц исключения для разработки, [BrowserLink](http://vswebessentials.com/features/browserlink), страниц ошибок, статических файлов и ASP.NET MVC: 

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Каждый метод расширения `Use` добавляет компонент ПО промежуточного слоя в конвейере. Например, метод расширения `UseMvc` добавляет [ПО промежуточного слоя для маршрутизации](xref:fundamentals/routing) в конвейер обработки запросов и настраивает [MVC](xref:mvc/overview) как обработчик по умолчанию. 

Дополнительные службы, такие как `IHostingEnvironment` и `ILoggerFactory`, также можно указать в сигнатуре метода. При этом дополнительные службы добавляются в случае, если они доступны. 

Дополнительные сведения об использовании `IApplicationBuilder` смотрите в разделе [ПО промежуточного слоя](xref:fundamentals/middleware). 

## <a name="convenience-methods"></a>Удобные методы

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) и [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) это удобные методы, которые можно использовать вместо указания класса `Startup`. Несколько вызовов `ConfigureServices` прибавляются друг к другу. Но при нескольких вызовах `Configure` будет использоваться только последний вызов метода.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a>Фильтры Startup 

Используйте [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) для настройки ПО промежуточного слоя в начале или конце метода [Configure](#the-configure-method). `IStartupFilter` полезен для обеспечения работы ПО промежуточного слоя до или после добавления библиотек, в начале или в конце конвейера обработки запросов приложения. 

`IStartupFilter`реализует один метод [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), который получает и возвращает `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) определяет класс, для настройки конвейера запросов приложения. Дополнительные сведения см. в разделе [Создание конвейера по промежуточного слоя с IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).

Каждый `IStartupFilter` реализует определенный набор [ПО промежуточного слоя](xref:fundamentals/middleware) в конвейере обработки запросов. Фильтры вызываются в порядке, в котором они были добавлены в контейнер службы. Фильтры могут добавлять ПО промежуточного слоя до или после передачи управления классу следующего фильтра, поэтому они добавляются в начало или конец конвейера приложения. 

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([загрузка](xref:tutorials/index#how-to-download-a-sample)) показывает, как зарегистрировать ПО промежуточного слоя с `IStartupFilter`. Пример приложения включает ПО промежуточного слоя, которое устанавливает значение параметров с помощью запроса строки параметров: 

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` настраивается в классе `RequestSetOptionsStartupFilter`:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` регистрируется в контейнере службы в `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Если параметр строки запроса для `option` предоставлен, то ПО промежуточного слоя обрабатывает присвоения значений до того, как ПО промежуточного слоя MVC отображает ответ: 

![Окно обозревателя, показывающее отображаемой страницы индекса. Значение параметра подготавливается к просмотру как на основе «Из промежуточного слоя» в запросе страницы с параметром строки запроса и значение параметра равным «Из промежуточного слоя».](startup/_static/index.png)

Порядок выполнения ПО промежуточного слоя устанавливается в порядке регистрации в `IStartupFilter`: 

* Несколько реализаций `IStartupFilter` может взаимодействовать с одним объектом. Если порядок важен, то порядок регистрации в службе `IStartupFilter` должен соответствовать порядку в котором должно выполняться по промежуточного слоя.
* Библиотеки могут добавлять по промежуточного слоя с одной или несколькими реализациями `IStartupFilter`, которые выполняются до или после того, как другое по промежуточного слоя будет зарегистрировано в `IStartupFilter`. Для вызова по промежуточного слоя `IStartupFilter`, до того, как будет добавлено по промежуточного слоя `IStartupFilter` библиотекой, поместите регистрации службы перед библиотекой которая добавляет его в контейнер службы. Чтобы вызвать позже, поместите регистрацию службы после добавления библиотеки.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение](xref:fundamentals/hosting)
* [Работа с несколькими средами](xref:fundamentals/environments)
* [ПО промежуточного слоя](xref:fundamentals/middleware)
* [Ведение журнала](xref:fundamentals/logging/index)
* [Конфигурация](xref:fundamentals/configuration/index)
* [Класс StartupLoader: метод FindStartupType (источник ссылки)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
