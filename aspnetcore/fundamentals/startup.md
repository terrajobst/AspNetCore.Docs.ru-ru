---
title: Запуск приложения в ASP.NET Core
author: ardalis
description: Сведения о том, как класс Startup в ASP.NET Core настраивает службы и конвейер запросов приложения.
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 2212344cb3c651714e8c520b096ab0c4eaf5a180
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206460"
---
# <a name="app-startup-in-aspnet-core"></a>Запуск приложения в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com) (Steve Smith), [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Люк Лэтем](https://github.com/guardrex) (Luke Latham)

Класс `Startup` настраивает службы и конвейер запросов приложения.

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([описание скачивания](xref:index#how-to-download-a-sample)).

## <a name="the-startup-class"></a>Класс Startup

Приложения ASP.NET Core используют класс `Startup`, который по соглашению называется `Startup`. Класс `Startup`:

* При необходимости может включать метод [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) для настройки служб приложения.
* Должен включать метод [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) для создания конвейера обработки запросов приложения.

`ConfigureServices` и `Configure` вызываются средой выполнения при запуске приложения:

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

Укажите класс `Startup` в методе [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_):

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

Веб-узел предоставляет несколько служб, которые доступны конструктору классов `Startup`. Приложение добавляет дополнительные службы через `ConfigureServices`. Как службы узла, так и службы приложения доступны в `Configure` и во всем приложении.

Типичным применением [внедрения зависимостей](xref:fundamentals/dependency-injection) в класс `Startup` является внедрение:

* [IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) для настройки служб средой.
* [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для чтения конфигурации.
* [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) для создания средства ведения журнала в `Startup.ConfigureServices`.

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

Альтернативой внедрению `IHostingEnvironment` является использование подхода на основе соглашений. Когда приложение определяет отдельные классы `Startup` для различных сред (например, `StartupDevelopment`), подходящий класс `Startup` выбирается во время выполнения. Класс, у которого суффикс имени соответствует текущей среде, получает приоритет. Если приложение выполняется в среде разработки и включает в себя оба класса — `Startup` и `StartupDevelopment`, используется класс `StartupDevelopment`. Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Дополнительные сведения о `WebHostBuilder` см. в разделе [Размещение](xref:fundamentals/host/index). Сведения об обработке ошибок во время запуска см. в разделе [Обработка исключений при запуске](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Метод ConfigureServices

Метод [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices):

* Optional
* Вызывается веб-узлом перед методом `Configure` для настройки служб приложения.
* По соглашению используется для задания [параметров конфигурации](xref:fundamentals/configuration/index).

По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`. Пример см. в разделе [Настройка служб удостоверений](xref:security/authentication/identity#pw).

Добавление служб в контейнер служб делает их доступными в приложении и в методе `Configure`. Службы разрешаются посредством [внедрения зависимостей](xref:fundamentals/dependency-injection) или из [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).

Веб-узел может настраивать некоторые службы перед вызовом методов `Startup`. Дополнительные сведения см. в разделе о [размещении в ASP.NET Core](xref:fundamentals/host/index).

Для функций, нуждающихся в значительной настройке, существуют методы расширения `Add[Service]` в [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection). Обычное приложение ASP.NET Core регистрирует службы для Entity Framework, удостоверения и MVC:

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="the-configure-method"></a>Метод Configure 

Метод [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) используется для указания того, как приложение реагирует на HTTP-запросы. Конвейер запросов настраивается путем добавления компонентов [ПО промежуточного слоя](xref:fundamentals/middleware/index) в экземпляр [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder). `IApplicationBuilder` доступен для метода `Configure`, но он не зарегистрирован в контейнере службы. При размещении создается `IApplicationBuilder` и передается непосредственно в `Configure`.

[Шаблоны ASP.NET Core](/dotnet/core/tools/dotnet-new) настраивают конвейер, добавляя поддержку страницы исключений разработчика, [BrowserLink](http://vswebessentials.com/features/browserlink), страниц ошибок, статических файлов и ASP.NET Core MVC:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

Каждый метод расширения `Use` добавляет компонент ПО промежуточного слоя в конвейер запросов. Например, метод расширения `UseMvc` добавляет в конвейер запросов [ПО промежуточного слоя для маршрутизации](xref:fundamentals/routing) и настраивает [MVC](xref:mvc/overview) в качестве обработчика по умолчанию.

Каждый компонент ПО промежуточного слоя в конвейере запросов отвечает за вызов следующего компонента в конвейере или замыкает цепочку, если это необходимо. Если замыкание в цепочке ПО промежуточного слоя не происходит, у каждого ПО промежуточного слоя есть еще один шанс обработать запрос перед отправкой клиенту.

В сигнатуре метода также могут быть указаны дополнительные службы, такие как `IHostingEnvironment` и `ILoggerFactory`. Когда дополнительные службы указаны, они внедряются, если доступны.

Дополнительные сведения об использовании `IApplicationBuilder` и порядке обработки ПО промежуточного слоя см. в разделе [ПО промежуточного слоя](xref:fundamentals/middleware/index).

## <a name="convenience-methods"></a>Удобный метод

Удобные методы [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) и [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) можно использовать вместо указания класса `Startup`. Несколько вызовов `ConfigureServices` добавляются друг к другу. Несколько вызовов `Configure` используют вызов последнего метода.

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="extend-startup-with-startup-filters"></a>Расширение класса Startup с использованием фильтров запуска

Используйте [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) для настройки ПО промежуточного слоя в начале или конце конвейера ПО промежуточного слоя [Configure](#the-configure-method) приложения. `IStartupFilter` удобно использовать, чтобы обеспечить запуск ПО промежуточного слоя до или после ПО промежуточного слоя, добавляемого библиотеками в начале или в конце конвейера обработки запросов приложения.

`IStartupFilter` реализует отдельный метод [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), который принимает и возвращает `Action<IApplicationBuilder>`. [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) определяет класс для настройки конвейера запросов приложения. Дополнительные сведения см. в разделе [Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Каждый `IStartupFilter` реализует один или несколько компонентов ПО промежуточного слоя в конвейере запросов. Фильтры вызываются в том порядке, в котором они были добавлены в контейнер службы. Фильтры могут добавлять ПО промежуточного слоя до или после передачи управления следующему фильтру, поэтому они добавляются в начало или конец конвейера приложения.

В примере приложения показано, как зарегистрировать ПО промежуточного слоя с помощью `IStartupFilter`. Этот пример включает ПО промежуточного слоя, которое задает значение параметров из параметра строки запроса:

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` настраивается в классе `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` регистрируется в контейнере службы в [IWebHostBuilder.ConfigureServices](xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.ConfigureServices*) для демонстрации того, как фильтр запуска дополняет `Startup` со стороны класса `Startup`:

[!code-csharp[](startup/sample/Program.cs?name=snippet1&highlight=4-5)]

Если указан параметр строки запроса для `option`, ПО промежуточного слоя обрабатывает присвоение значения до того, как ПО промежуточного слоя MVC отображает отклик:

![Окно обозревателя с отображаемой страницей индекса. Значение параметра отображается как "From Middleware", так как запрашивается страница с помощью параметра строки запроса и задано значение параметра "From Middleware".](startup/_static/index.png)

Порядок выполнения ПО промежуточного слоя определяется порядком регистраций `IStartupFilter`:

* Несколько реализаций `IStartupFilter` могут взаимодействовать с одними и теми же объектами. Если важен порядок, упорядочите регистрации службы `IStartupFilter` в соответствии с требуемым порядком выполнения ПО промежуточного слоя.
* Библиотеки могут добавлять ПО промежуточного слоя с одной или несколькими реализациями `IStartupFilter`, которые выполняются до или после другого ПО промежуточного слоя приложения, зарегистрированного с помощью `IStartupFilter`. Чтобы вызвать ПО промежуточного слоя `IStartupFilter` до ПО промежуточного слоя, добавляемого библиотекой `IStartupFilter`, расположите регистрацию службы до добавления библиотеки в контейнер службы. Чтобы вызвать его после этого момента, расположите регистрацию службы после добавления библиотеки.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Добавление конфигурации из внешней сборки при запуске

Реализация [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) позволяет при запуске добавлять в приложение улучшения из внешней сборки вне класса `Startup` приложения. Дополнительные сведения см. в разделе [Усовершенствование приложения из внешней сборки](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/host/index>
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
