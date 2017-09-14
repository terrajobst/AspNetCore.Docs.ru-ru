---
title: "Основы ASP.NET Core"
author: rick-anderson
description: "В этой статье приводится общий обзор основных понятий, которые нужно знать для сборки приложений ASP.NET Core."
keywords: "ASP.NET Core, основы, обзор"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99fbe0e02be27a0fbbb7ff65bc15713aab58c003
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="aspnet-core-fundamentals-overview"></a>ASP.NET Core, основы, обзор

Приложение ASP.NET Core — это консольное приложение, создающее веб-сервер в своем методе `Main`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

Метод `Main` вызывает `WebHost.CreateDefaultBuilder`, создающий узел веб-приложения по шаблону конструктора. Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`). В приведенном выше примере веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) выделяется автоматически. Веб-узел ASP.NET Core попытается запуститься на сервере IIS, если он будет доступен. Другие веб-серверы, такие как [HTTP.sys](xref:fundamentals/servers/httpsys), можно использовать, вызывая соответствующий метод расширения. `UseStartup` подробно описывается в следующем разделе.

`IWebHostBuilder`, тип возвращаемого значения вызова `WebHost.CreateDefaultBuilder`, предоставляет множество вспомогательных методов. В число таких методов входят, например, `UseHttpSys` для размещения приложения на веб-сервере HTTP.sys и `UseContentRoot` для указания корневого каталога содержимого. Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

Метод `Main` применяет `WebHostBuilder`, создающий узел веб-приложения по шаблону конструктора. Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`). В приведенном выше примере используется веб-сервер [Kestrel](xref:fundamentals/servers/kestrel). Другие веб-серверы, такие как [WebListener](xref:fundamentals/servers/weblistener), можно использовать, вызывая соответствующий метод расширения. `UseStartup` подробно описывается в следующем разделе.

`WebHostBuilder` предоставляет множество вспомогательных методов, включая `UseIISIntegration` для размещения в IIS и IIS Express и `UseContentRoot` для указания корневого каталога содержимого. Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.

---

## <a name="startup"></a>Запуск

Метод `UseStartup` в `WebHostBuilder` определяет класс `Startup` для вашего приложения:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

В классе `Startup` определяется конвейер обработки запросов и настраиваются все необходимые для приложения службы. Класс `Startup` должен быть открытым и содержать следующие методы.

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* `ConfigureServices` определяет [службы](#services), используемые вашим приложением (такие как MVC ASP.NET Core, Entity Framework Core, Identity и т. д.).

* `Configure` определяет [ПО промежуточного слоя](xref:fundamentals/middleware) в конвейере обработки запросов.

Дополнительные сведения см. в разделе [Запуск приложения](xref:fundamentals/startup).

## <a name="services"></a>Службы

Служба — это компонент, предназначенный для общего использования в приложении. Доступ к службе предоставляется путем [внедрения зависимостей](xref:fundamentals/dependency-injection). ASP.NET Core включает встроенный контейнер для инверсии управления (IoC), который поддерживает [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) по умолчанию. Встроенный контейнер можно заменить на контейнер по вашему выбору. Помимо формирования слабых взаимосвязей внедрение зависимостей обеспечивает доступ к службам в рамках всего приложения. Например, в любой части вашего приложения доступна служба [входа](xref:fundamentals/logging).

Дополнительные сведения см. в разделе [Введение зависимостей](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>ПО промежуточного слоя

В ASP.NET Core конвейер запросов создается с помощью [ПО промежуточного слоя](xref:fundamentals/middleware). ПО промежуточного слоя ASP.NET Core выполняет асинхронную логику в `HttpContext`, а затем либо вызывает следующее ПО промежуточного слоя в последовательности, либо завершает запрос напрямую. Компонент ПО промежуточного слоя с именем "XYZ" добавляется путем вызова метода расширения `UseXYZ` в методе `Configure`.

ASP.NET Core содержит большой набор встроенных ПО промежуточного слоя:

* [Статические файлы](xref:fundamentals/static-files)

* [Маршрутизация](xref:fundamentals/routing)

* [Проверка подлинности](xref:security/authentication/index)

В ASP.NET Core можно использовать любое ПО промежуточного слоя на базе [OWIN](http://owin.org), а также писать собственные, пользовательские ПО промежуточного слоя.

Дополнительные сведения см. в статьях [ПО промежуточного слоя](xref:fundamentals/middleware) и [Открытый веб-интерфейс .NET (OWIN)](xref:fundamentals/owin).

## <a name="servers"></a>Серверы

В модели размещения ASP.NET Core запросы не прослушиваются напрямую. Переадресация запросов в приложение происходит через реализацию сервера HTTP. Переадресованный запрос упаковывается как набор объектов функций, доступ к которым можно получить через интерфейсы. Приложение внедряет этот набор в `HttpContext`. ASP.NET Core включает управляемый кроссплатформенный веб-сервер под названием [Kestrel](xref:fundamentals/servers/kestrel). Обычно Kestrel выполняется за рабочим веб-сервером, таким как [IIS](https://iis.net) или [nginx](http://nginx.org).

Дополнительные сведения см. в статьях [Серверы](xref:fundamentals/servers/index) и [Размещения](xref:fundamentals/hosting).

## <a name="content-root"></a>Корневой каталог содержимого

Корневой каталог содержимого — это базовый путь к любому содержимому, которое используется приложением, включая представления, [страницы Razor](xref:mvc/razor-pages/index) и статические активы. По умолчанию корневой каталог содержимого совпадает с базовой папкой, в которой хранится исполняемый файл приложения. Другое расположение для корневого каталога содержимого определяет параметр `WebHostBuilder`.

## <a name="web-root"></a>Корневой веб-узел

Корневой веб-узел приложения — это каталог в проекте, содержащий открытые статические ресурсы, такие как CSS, JavaScript и файлы изображений. По умолчанию ПО промежуточного слоя статических файлов обслуживает только файлы из корневого веб-каталога и его подпапок. Дополнительные сведения см. в статье о [работе со статическими файлами](xref:fundamentals/static-files). По умолчанию путь к корневому веб-узлу указывает на папку */wwwroot*; другое местонахождение можно указать с помощью параметра `WebHostBuilder`.

## <a name="configuration"></a>Конфигурация

В ASP.NET Core для обработки простых пар имен и значений используется новая модель конфигурации. Новая модель конфигурации опирается не на `System.Configuration` или *web.config*, а на упорядоченный набор поставщиков конфигурации. Встроенные поставщики конфигурации поддерживают различные форматы файлов (XML, JSON, INI) и переменных среды для выполнения конфигурации на основе среды. Кроме того, можно писать и собственные, пользовательские поставщики конфигурации.

Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration).

## <a name="environments"></a>Среды

Среды разработка и рабочая среда представляют собой основные составляющие в ASP.NET Core и могут задаваться с использованием соответствующих переменных.

Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).

## <a name="net-core-vs-net-framework-runtime"></a>Выбор между .NET Core и .NET Framework

Приложения ASP.NET Core могут предназначаться для среды .NET Core или .NET Framework. Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="additional-information"></a>Дополнительные сведения

См. также следующие статьи.

- [Обработка ошибок](xref:fundamentals/error-handling)
- [Поставщики файлов](xref:fundamentals/file-providers)
- [Глобализация и локализация](xref:fundamentals/localization)
- [Ведение журнала](xref:fundamentals/logging)
- [Управление состоянием приложения](xref:fundamentals/app-state)