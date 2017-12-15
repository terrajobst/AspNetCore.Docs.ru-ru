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
ms.openlocfilehash: 8adb96c7261a2e7b1556f0daddcf6f135862b53a
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2017
---
# <a name="application-startup-in-aspnet-core"></a>Запуск приложения в ASP.NET Core

По [Стив Смит](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), и [Latham Люк](https://github.com/guardrex)

`Startup` Класс настраивает службы и конвейер обработки запросов приложения.

## <a name="the-startup-class"></a>Класс запуска

Использование приложения ASP.NET Core `Startup` класса, который называется `Startup` по соглашению. `Startup` Класса:

* При необходимости можно включить [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) метод для настройки приложения службы.
* Необходимо включить [Настройка](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) способ создания конвейера обработки запросов приложения.

`ConfigureServices`и `Configure` вызывается средой выполнения, при запуске приложения:

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

Укажите `Startup` класса [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) метод:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

`Startup` Конструктор классов принимает зависимости, определенные средой размещения. Обычно [внедрения зависимостей](xref:fundamentals/dependency-injection) в `Startup` класс является вставка [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) для настройки служб средой:

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

Альтернативой вводится `IHostingStartup` заключается в использовании подход на основе соглашения. Приложение можно определить отдельные `Startup` классы для различных сред (например, `StartupDevelopment`), а класс соответствующие запуска выбирается во время выполнения. Класс, чьи суффикс имени соответствует текущей среде приоритет. Если приложение выполняется в среде разработки, а также включает в себя `Startup` класса и `StartupDevelopment` класса `StartupDevelopment` используется класс. Дополнительные сведения см. [работа с несколькими средами](xref:fundamentals/environments#startup-conventions).

Дополнительные сведения о `WebHostBuilder`, в разделе [размещения](xref:fundamentals/hosting) раздела. Сведения об обработке ошибок во время запуска см. в разделе [обработка исключений при запуске](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Метод ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) является метод:

* Необязательно.
* Вызывается перед узлом web `Configure` метод для настройки приложения службы.
* Где [параметры конфигурации](xref:fundamentals/configuration/index) устанавливаются в соответствии с соглашением.

Добавление служб в контейнер службы становятся доступными в приложении и в `Configure` метод. Службы, разрешаются посредством [внедрения зависимостей](xref:fundamentals/dependency-injection) или [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Веб-узла могут настраивать некоторые службы перед `Startup` вызываются методы. Подробные сведения доступны в [размещения](xref:fundamentals/hosting) раздела. 

Для функций, требующих значительной установки, существуют `Add[Service]` методы расширения в [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Обычно веб-приложение регистрирует службы для платформы Entity Framework, удостоверения и MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a>Службы, доступные при запуске

Веб-узел предоставляет несколько служб, которые доступны для `Startup` конструктора класса. Приложение добавляет дополнительные службы через `ConfigureServices`. Затем службы узла и приложения доступны в `Configure` и в приложении.

## <a name="the-configure-method"></a>Метод конфигурации

[Настройка](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) метод используется для указания того, как приложение реагирует на запросы HTTP. Конвейер запросов настраивается путем добавления [по промежуточного слоя](xref:fundamentals/middleware) компоненты для [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) экземпляра. `IApplicationBuilder`доступен для `Configure` метода, но он не зарегистрирован в контейнере службы. Размещение создает `IApplicationBuilder` и передает его непосредственно к `Configure` ([ссылки на источник](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).

[Шаблоны ASP.NET Core](/dotnet/core/tools/dotnet-new) настройки конвейера с поддержкой страницей исключение разработчика [BrowserLink](http://vswebessentials.com/features/browserlink), страницы ошибок, статические файлы и ASP.NET MVC:

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Каждый `Use` метод расширения добавляет компонент по промежуточного слоя в конвейере. Например `UseMvc` добавляет метод расширения [маршрутизации по промежуточного слоя](xref:fundamentals/routing) в конвейер обработки запросов и настраивает [MVC](xref:mvc/overview) как обработчик по умолчанию.

Дополнительные службы, такие как `IHostingEnvironment` и `ILoggerFactory`, также можно указать в подписи метода. При указании дополнительных служб добавляются в том случае, если они доступны.

Дополнительные сведения об использовании `IApplicationBuilder`, в разделе [по промежуточного слоя](xref:fundamentals/middleware).

## <a name="convenience-methods"></a>Удобные методы

[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) и [Настройка](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) удобных методов, которые можно использовать вместо указания `Startup` класса. Несколько вызовов `ConfigureServices` append друг с другом. Несколько вызовов `Configure` использовать последнего вызова метода.

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=16,20)]

## <a name="startup-filters"></a>При запуске фильтры

Используйте [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) для настройки по промежуточного слоя в начале или конце приложения [Настройка](#the-configure-method) конвейера по промежуточного слоя. `IStartupFilter`полезно для обеспечения работы по промежуточного слоя до или после добавления библиотек, в начале или в конце конвейера обработки запросов приложения по промежуточного слоя.

`IStartupFilter`реализует один метод [Настройка](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), который получает и возвращает `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) определяет класс, для настройки конвейера запросов приложения. Дополнительные сведения см. в разделе [Создание конвейера по промежуточного слоя с IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).

Каждый `IStartupFilter` реализует один или несколько middlewares в конвейер обработки запросов. Фильтры, вызываются в порядке, в котором они были добавлены в контейнер службы. Добавить фильтры по промежуточного слоя перед или после передачи управления классу следующей фильтрации, таким образом они добавьте в начало или конец конвейера приложения.

[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) показано, как зарегистрировать по промежуточного слоя с `IStartupFilter`. Пример приложения включает по промежуточного слоя, которое задает значение параметров из параметра строки запроса:

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` Настраивается в `RequestSetOptionsStartupFilter` класса:

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` Регистрируется в контейнере службы в `ConfigureServices`:

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

Если параметр строки запроса для `option` не предоставлен, по промежуточного слоя обрабатывает присвоения значения перед по промежуточного слоя MVC отображает ответ:

![Окно обозревателя, показывающее отображаемой страницы индекса. Значение параметра подготавливается к просмотру как на основе «Из промежуточного слоя» в запросе страницы с параметром строки запроса и значение параметра равным «Из промежуточного слоя».](startup/_static/index.png)

Имеет значение порядок выполнения по промежуточного слоя в порядке от `IStartupFilter` регистрации:

* Несколько `IStartupFilter` реализации может взаимодействовать с тех же самых объектов. Если порядок важен, порядок их `IStartupFilter` службы регистрации в соответствии с порядком их middlewares должен выполняться.
* Библиотеки могут добавлять по промежуточного слоя с одним или несколькими `IStartupFilter` реализации, которые выполняются до или после другого по промежуточного слоя приложение зарегистрировано в `IStartupFilter`. Для вызова `IStartupFilter` по промежуточного слоя, прежде чем по промежуточного слоя, добавленные в библиотеке `IStartupFilter`, поместите регистрации службы перед библиотеки добавляется в контейнер службы. Чтобы вызвать впоследствии, поместите регистрации службы после добавления библиотеки.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение](xref:fundamentals/hosting)
* [Работа с несколькими средами](xref:fundamentals/environments)
* [ПО промежуточного слоя](xref:fundamentals/middleware)
* [Ведение журнала](xref:fundamentals/logging/index)
* [Конфигурация](xref:fundamentals/configuration/index)
* [Класс StartupLoader: метод FindStartupType (Справочник по источник)](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116))
