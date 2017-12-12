---
title: "Запуск приложения в ASP.NET Core"
author: ardalis
description: "Узнайте, как класс Startup в ASP.NET Core настраивает службы и конвейер обработки запросов приложения."
keywords: "ASP.NET Core, запуска, настроить метод, метод ConfigureServices"
ms.author: tdykstra
manager: wpickett
ms.date: 02/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 83b2647df8beec1feae33400224dacf9823be9b4
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="application-startup-in-aspnet-core"></a>Запуск приложения в ASP.NET Core

По [Стив Смит](https://ardalis.com/) и [Tom Dykstra](https://github.com/tdykstra/)

`Startup` Класс настраивает службы и конвейер обработки запросов приложения.

## <a name="the-startup-class"></a>Класс запуска

Приложения ASP.NET Core требуется `Startup` класса, который называется `Startup` по соглашению. Укажите имя класса запуска в `Main` программы [WebHostBuilderExtensions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions) [ `UseStartup<TStartup>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) метод. В разделе [размещения](xref:fundamentals/hosting) для получения дополнительных сведений о `WebHostBuilder`, выполняемая перед `Startup`.

Можно определить отдельные `Startup` классы для различных сред и соответствующие выбрана одна во время выполнения. При указании `startupAssembly` в [конфигурации WebHost](https://docs.microsoft.com/aspnet/core/fundamentals/hosting?tabs=aspnetcore2x#configuring-a-host) или параметры, размещение будет загрузить эту сборку для запуска и выполните поиск `Startup` или `Startup[Environment]` типа. Класс которого совпадения суффикс имени текущей среде будет иметь приоритет, поэтому, если приложение выполняется в *разработки* среды и включает в себя `Startup` и `StartupDevelopment` класса `StartupDevelopment` класс будет использовать. В разделе [FindStartupType](https://github.com/aspnet/Hosting/blob/rel/1.1.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs) в `StartupLoader` и [работа с несколькими средами](environments.md#startup-conventions).

Кроме того, можно определить фиксированный `Startup` класс, который будет использоваться независимо от среды, путем вызова `UseStartup<TStartup>`. Этот способ является рекомендуемым.

`Startup` Конструктор класса могут принимать зависимости, которые обеспечиваются через [внедрения зависимостей](xref:fundamentals/dependency-injection). Общий подход заключается в использовании `IHostingEnvironment` Настройка [конфигурации](xref:fundamentals/configuration/index) источников.

`Startup` Класс должен включать `Configure` метода и при необходимости можно включить `ConfigureServices` метода, которые вызываются при запуске приложения. Класс может также включать [конкретных версий этих методов](xref:fundamentals/environments#startup-conventions). `ConfigureServices`, если он имеется, вызывается перед `Configure`.

Дополнительные сведения о [обработки исключений во время запуска приложения](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Метод ConfigureServices

[ConfigureServices](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.startupbase#Microsoft_AspNetCore_Hosting_StartupBase_ConfigureServices_Microsoft_Extensions_DependencyInjection_IServiceCollection_) является необязательным; но, если вызывается до `Configure` метод с веб-узла. Веб-узла могут настраивать некоторые службы перед ``Startup`` методы вызываются (см. [размещение](xref:fundamentals/hosting)). По соглашению [параметры конфигурации](xref:fundamentals/configuration/index) устанавливаются в этом методе.

Для функций, требующих значительной установки существует `Add[Service]` методы расширения в [IServiceCollection](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.iservicecollection). В этом примере с помощью шаблона веб-сайта по умолчанию настраивает приложение для использования служб для Entity Framework, удостоверения и MVC:

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

Добавление служб в контейнер службы делает их доступными в приложении через [внедрения зависимостей](xref:fundamentals/dependency-injection).

## <a name="services-available-in-startup"></a>Службы, доступные при запуске

Внедрение зависимостей ASP.NET Core предоставляет службы во время запуска приложения. Можно запросить этих служб, включая соответствующий интерфейс в качестве параметра на ваш `Startup` конструктор класса или его `Configure` метод. `ConfigureServices` Метод принимает только `IServiceCollection` параметра (но любой зарегистрированную службу может извлекаться из этой коллекции, так что дополнительные параметры не требуются).

Ниже перечислены некоторые службы, обычно запрашиваемые `Startup` методов:

* В конструкторе: `IHostingEnvironment`,`ILogger<Startup>`
* В `ConfigureServices`:`IServiceCollection`
* В `Configure`: `IApplicationBuilder`, `IHostingEnvironment`,`ILoggerFactory`

Все службы, добавленные ``WebHostBuilder`` ``ConfigureServices`` может запрашиваться метод ``Startup`` конструктор класса или его ``Configure`` метод. Используйте `WebHostBuilder` для предоставления любых служб, которые требуются во время `Startup` методы.

## <a name="the-configure-method"></a>Метод конфигурации

`Configure` Метод используется для указания того, как приложение ASP.NET отвечает на HTTP-запросы. Конвейер запросов настраивается путем добавления [по промежуточного слоя](middleware.md) компонентов `IApplicationBuilder` экземпляра, предоставляемая внедрения зависимостей.

В следующем примере с помощью шаблона веб-сайта по умолчанию используются несколько методов расширения для настройки конвейера с поддержкой [BrowserLink](http://vswebessentials.com/features/browserlink), страницы ошибок, статические файлы, ASP.NET MVC и удостоверений.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Каждый `Use` добавляет метод расширения [по промежуточного слоя](xref:fundamentals/middleware) в конвейер обработки запросов. Например `UseMvc` добавляет метод расширения [маршрутизации](routing.md) по промежуточного слоя в конвейере и настраивает [MVC](xref:mvc/overview) как обработчик по умолчанию.

Дополнительные сведения об использовании `IApplicationBuilder`, в разделе [по промежуточного слоя](xref:fundamentals/middleware).

Дополнительные службы, такие как `IHostingEnvironment` и `ILoggerFactory` также может быть указан в сигнатуре метода в таком случае эти службы будут [введенного](dependency-injection.md) если они доступны. 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Работа с несколькими средами](xref:fundamentals/environments)
* [ПО промежуточного слоя](xref:fundamentals/middleware)
* [Ведение журнала](xref:fundamentals/logging/index)
* [Конфигурация](xref:fundamentals/configuration/index)
