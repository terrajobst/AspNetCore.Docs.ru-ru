---
title: "Основы ASP.NET Core"
author: rick-anderson
description: "Познакомьтесь с основными понятиями для создания приложений ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: 7f0e30b3ac7f9cc3a32bd96f45d83ba13505a475
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="aspnet-core-fundamentals"></a>Основы ASP.NET Core

Приложение ASP.NET Core — это консольное приложение, создающее веб-сервер в своем методе `Main`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

Метод `Main` вызывает `WebHost.CreateDefaultBuilder`, создающий узел веб-приложения по шаблону конструктора. Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`). В приведенном выше примере веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) выделяется автоматически. Веб-хост ASP.NET Core попытается запуститься в службах IIS, если они доступны. Другие веб-серверы, такие как [HTTP.sys](xref:fundamentals/servers/httpsys), можно использовать, вызывая соответствующий метод расширения. `UseStartup` подробно описывается в следующем разделе.

`IWebHostBuilder`, тип возвращаемого значения вызова `WebHost.CreateDefaultBuilder`, предоставляет множество вспомогательных методов. В число таких методов входят, например, `UseHttpSys` для размещения приложения в HTTP.sys и `UseContentRoot` для указания корневого каталога содержимого. Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]

Метод `Main` применяет `WebHostBuilder`, создающий узел веб-приложения по шаблону конструктора. Конструктор содержит методы, определяющие веб-сервер (например, `UseKestrel`), а также класс запуска (`UseStartup`). В приведенном выше примере используется веб-сервер [Kestrel](xref:fundamentals/servers/kestrel). Другие веб-серверы, такие как [WebListener](xref:fundamentals/servers/weblistener), можно использовать, вызывая соответствующий метод расширения. `UseStartup` подробно описывается в следующем разделе.

В `WebHostBuilder` есть множество вспомогательных методов, включая `UseIISIntegration` для размещения в службах IIS и IIS Express и `UseContentRoot` для указания корневого каталога содержимого. Методы `Build` и `Run` собирают объект `IWebHost`, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.

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
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

`ConfigureServices` определяет [службы](#dependency-injection-services), используемые вашим приложением (такие как ASP.NET Core MVC, Entity Framework Core и Identity). `Configure` определяет [ПО промежуточного слоя](xref:fundamentals/middleware/index) для конвейера обработки запросов.

Дополнительные сведения см. в разделе [Запуск приложения](xref:fundamentals/startup).

## <a name="content-root"></a>Корневой каталог содержимого

Корневой каталог содержимого — это базовый путь к любому содержимому, которое используется приложением, включая представления, [страницы Razor](xref:mvc/razor-pages/index) и статические активы. По умолчанию корневой каталог содержимого совпадает с базовым путем исполняемого файла приложения.

## <a name="web-root"></a>Корневой веб-узел

Корневой каталог документов — это каталог в проекте, содержащий открытые и статические ресурсы, такие как CSS, JavaScript и файлы изображений.

## <a name="dependency-injection-services"></a>Введение зависимостей (службы)

Служба — это компонент, предназначенный для общего использования в приложении. Доступ к службе предоставляется путем [внедрения зависимостей](xref:fundamentals/dependency-injection). ASP.NET Core включает встроенный контейнер для **l**инверсии **o**управления **C**(IoC), который поддерживает [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) по умолчанию. При необходимости вы можете заменить собственный контейнер по умолчанию. Помимо формирования слабых взаимосвязей, внедрение зависимостей обеспечивает доступ к службам в рамках всего приложения (например, [ведение журнала](xref:fundamentals/logging/index)).

Дополнительные сведения см. в разделе [Введение зависимостей](xref:fundamentals/dependency-injection).

## <a name="middleware"></a>ПО промежуточного слоя

В ASP.NET Core конвейер запросов создается с помощью [ПО промежуточного слоя](xref:fundamentals/middleware/index). ПО промежуточного слоя ASP.NET Core выполняет асинхронную логику в `HttpContext`, а затем либо вызывает следующее ПО промежуточного слоя в последовательности, либо завершает запрос напрямую. Компонент ПО промежуточного слоя с именем "XYZ" добавляется путем вызова метода расширения `UseXYZ` в методе `Configure`.

ASP.NET Core содержит большой набор встроенного ПО промежуточного слоя.

* [Статические файлы](xref:fundamentals/static-files)
* [Маршрутизация](xref:fundamentals/routing)
* [Проверка подлинности](xref:security/authentication/index)
* [ПО промежуточного слоя для сжатия ответов](xref:performance/response-compression)
* [ПО промежуточного слоя для переопределения URL-адресов](xref:fundamentals/url-rewriting)

В приложениях ASP.NET Core можно использовать любое ПО промежуточного слоя на базе [OWIN](http://owin.org), а также писать собственные, пользовательские ПО промежуточного слоя.

Дополнительные сведения см. в статьях [ПО промежуточного слоя](xref:fundamentals/middleware/index) и [Открытый веб-интерфейс .NET (OWIN)](xref:fundamentals/owin).

## <a name="environments"></a>Среды

Окружения для разработки и работы представляют собой ключевые компоненты ASP.NET Core и могут задаваться с использованием соответствующих переменных среды.

Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).

## <a name="configuration"></a>Конфигурация

ASP.NET Core использует модель конфигурации на основе пар "имя-значение". Модель конфигурации не основана на `System.Configuration` или *web.config*. Конфигурация получает параметры от упорядоченного набора поставщиков конфигурации. Встроенные поставщики конфигурации поддерживают различные форматы файлов (XML, JSON, INI) и переменных среды для выполнения конфигурации на основе среды. Кроме того, можно писать и собственные, пользовательские поставщики конфигурации.

Дополнительные сведения см. в разделе [Конфигурация](xref:fundamentals/configuration/index).

## <a name="logging"></a>Ведение журнала

ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками. Встроенные поставщики поддерживают отправку журналов в одно расположение или несколько. Можно также использовать сторонние платформы ведения журнала.

[Ведение журнала](xref:fundamentals/logging/index)

## <a name="error-handling"></a>Обработка ошибок

ASP.NET Core включает встроенные функции для обработки ошибок в приложениях, включая страницу исключений разработчика, настраиваемые страницы ошибок, страницы статических кодов состояний и обработку исключений при запуске.

Дополнительные сведения см. в разделе [Обработка ошибок](xref:fundamentals/error-handling).

## <a name="routing"></a>Маршрутизация

В ASP.NET Core присутствуют функции для маршрутизации запросов приложений обработчикам маршрутов.

Дополнительные сведения см. в разделе [Маршрутизация](xref:fundamentals/routing).

## <a name="file-providers"></a>Поставщики файлов

ASP.NET Core ограничивает доступ к файловой системе с помощью поставщиков файлов, которые предоставляют общий интерфейс для работы с файлами на разных платформах.

Дополнительные сведения см. в разделе [Поставщики файлов](xref:fundamentals/file-providers).

## <a name="static-files"></a>Статические файлы

ПО промежуточного слоя для статических файлов обрабатывает такие статические файлы, как HTML, CSS, файлы изображений и JavaScript.

Дополнительные сведения см. в разделе [Работа со статическими файлами](xref:fundamentals/static-files).

## <a name="hosting"></a>Размещение

Приложения ASP.NET Core настраивают и запускают *хост*, который отвечает за запуск приложений и управление жизненным циклом.

Дополнительные сведения см. в разделе [Размещение](xref:fundamentals/hosting).

## <a name="session-and-application-state"></a>Состояние сеанса и приложения

Состояние сеанса — это функция в ASP.NET Core, которую можно использовать для сохранения пользовательских данных во время просмотра веб-приложения пользователем.

Дополнительные сведения см. в разделе [Состояние сеанса и приложения](xref:fundamentals/app-state).

## <a name="servers"></a>Серверы

Модель размещения ASP.NET Core не прослушивает запросы напрямую. Модель размещения полагается на реализацию HTTP-сервера для переадресации запроса в приложение. Переадресованный запрос упаковывается как набор объектов функций, доступ к которым можно получить через интерфейсы. ASP.NET Core включает управляемый кроссплатформенный веб-сервер под названием [Kestrel](xref:fundamentals/servers/kestrel). Kestrel часто используется как звено, стоящее позади рабочего веб-сервера, такого как [IIS](https://www.iis.net/) или [nginx](http://nginx.org). Kestrel можно запускать как пограничный сервер.

Дополнительные сведения см. в разделе [Серверы](xref:fundamentals/servers/index) и в следующих разделах.

* [Kestrel](xref:fundamentals/servers/kestrel)
* [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [HTTP.sys](xref:fundamentals/servers/httpsys) (ранее — [WebListener](xref:fundamentals/servers/weblistener))

## <a name="globalization-and-localization"></a>Глобализация и локализация

Создание многоязычного веб-сайта на основе ASP.NET Core позволяет расширить аудиторию. ASP.NET Core предоставляет службы и ПО промежуточного слоя для локализации на разные языки и для разных региональных параметров.

Дополнительные сведения см. в разделе [Глобализация и локализация](xref:fundamentals/localization).

## <a name="request-features"></a>Параметры запроса

Сведения о реализации веб-сервера, связанные с HTTP-запросами и ответами, определяются в интерфейсах. Эти интерфейсы используются реализациями сервера и ПО промежуточного слоя для создания и изменения конвейера размещения приложений.

Дополнительные сведения см. в разделе [Параметры запроса](xref:fundamentals/request-features).

## <a name="open-web-interface-for-net-owin"></a>Открытый веб-интерфейс для .NET (OWIN)

ASP.NET Core поддерживает открытый веб-интерфейс для .NET (OWIN). OWIN позволяет ослабить зависимость веб-приложений от веб-сервера.

Дополнительные сведения см. в разделе [Открытый веб-интерфейс для .NET (OWIN)](xref:fundamentals/owin).

## <a name="websockets"></a>Протокол WebSocket

[WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям. Он используется для таких приложений, как чаты, биржевые тикеры, игры, а также везде, где в веб-приложении нужны функции реального времени. ASP.NET Core поддерживает функции WebSocket.

Дополнительные сведения см. в разделе [Протокол WebSocket](xref:fundamentals/websockets).

## <a name="microsoftaspnetcoreall-metapackage"></a>Метапакет Microsoft.AspNetCore.All

Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:

* все пакеты, поддерживаемые командой ASP.NET Core;
* все пакеты, поддерживаемые Entity Framework Core; 
* внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core.

Дополнительные сведения см. в разделе [Метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

## <a name="net-core-vs-net-framework-runtime"></a>Выбор между .NET Core и .NET Framework

Приложения ASP.NET Core могут задавать среду выполнения .NET Core или .NET Framework как целевую.

Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Выбор между ASP.NET Core и ASP.NET

Дополнительные сведения о выборе между ASP.NET Core и ASP.NET см. в разделе [Выбор между ASP.NET Core и ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).
