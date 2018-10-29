---
title: Основы ASP.NET Core
author: rick-anderson
description: Познакомьтесь с основными понятиями для создания приложений ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/index
ms.openlocfilehash: 56344315acc59003248ffaf1e61455b94a93a545
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090723"
---
# <a name="aspnet-core-fundamentals"></a>Основы ASP.NET Core

Приложение ASP.NET Core — это консольное приложение, создающее веб-сервер в своем методе `Program.Main`. Метод `Main` является *управляемой точкой входа* приложения:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

Узел .NET Core:

* загружает среду выполнения [.NET Core](https://github.com/dotnet/coreclr);
* использует первый аргумент командной строки в качестве пути к управляемому двоичному файлу, который содержит точку входа (`Main`) и начинает выполнение кода.

Метод `Main` вызывает [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), создающий веб-узел по [шаблону конструктора](https://wikipedia.org/wiki/Builder_pattern). Конструктор содержит методы, определяющие веб-сервер (например, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), а также класс запуска (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). В приведенном выше примере веб-сервер [Kestrel](xref:fundamentals/servers/kestrel) выделяется автоматически. Веб-хост ASP.NET Core попытается запуститься в службах IIS, если они доступны. Другие веб-серверы, такие как [HTTP.sys](xref:fundamentals/servers/httpsys), можно использовать, вызывая соответствующий метод расширения. `UseStartup` подробно описывается в следующем разделе.

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, тип возвращаемого значения вызова `WebHost.CreateDefaultBuilder`, предоставляет множество вспомогательных методов. В число таких методов входят, например, `UseHttpSys` для размещения приложения в HTTP.sys и <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> для указания корневого каталога содержимого. Методы <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> и <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> собирают объект <xref:Microsoft.AspNetCore.Hosting.IWebHost>, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

Узел .NET Core:

* загружает среду выполнения [.NET Core](https://github.com/dotnet/coreclr);
* использует первый аргумент командной строки в качестве пути к управляемому двоичному файлу, который содержит точку входа (`Main`) и начинает выполнение кода.

Метод `Main` применяет <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, создающий узел веб-приложения по [шаблону конструктора](https://wikipedia.org/wiki/Builder_pattern). Конструктор содержит методы, определяющие веб-сервер (например, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>), а также класс запуска (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>). В приведенном выше примере используется веб-сервер [Kestrel](xref:fundamentals/servers/kestrel). Другие веб-серверы, такие как [WebListener](xref:fundamentals/servers/weblistener), можно использовать, вызывая соответствующий метод расширения. `UseStartup` подробно описывается в следующем разделе.

В `WebHostBuilder` есть множество вспомогательных методов, включая <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> для размещения в службах IIS и IIS Express и <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> для указания корневого каталога содержимого. Методы <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> и <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> собирают объект <xref:Microsoft.AspNetCore.Hosting.IWebHost>, в котором будет размещаться приложение, и переходят к ожиданию HTTP-запросов.

::: moniker-end

## <a name="startup"></a>Запуск

Метод `UseStartup` в `WebHostBuilder` определяет класс `Startup` для вашего приложения:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

В классе `Startup` определяется конвейер обработки запросов и настраиваются все необходимые для приложения службы. Класс `Startup` должен быть открытым и содержать следующие методы.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> определяет [службы](#dependency-injection-services), используемые вашим приложением (такие как ASP.NET Core MVC, Entity Framework Core и Identity). <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> определяет [ПО промежуточного слоя](xref:fundamentals/middleware/index), вызываемое в конвейере обработки запросов.

Дополнительные сведения см. в разделе <xref:fundamentals/startup>.

## <a name="content-root"></a>Корневой каталог содержимого

Корневой каталог содержимого — это базовый путь к любому содержимому, которое используется приложением, включая представления MVC, [Razor Pages](xref:razor-pages/index) и статические активы. По умолчанию корневой каталог содержимого совпадает с базовым путем исполняемого файла приложения.

## <a name="web-root-webroot"></a>Корневой каталог (webroot)

Корневой каталог приложения — это каталог в проекте, содержащий открытые и статические ресурсы, такие как CSS, JavaScript и файлы изображений. По умолчанию корневым каталогом является *wwwroot*.

Для указания на корневой каталог файлов Razor (*.cshtml*) используется символ тильды и косой черты `~/`. Пути, начинающиеся с `~/`, называются виртуальными путями.

## <a name="dependency-injection-services"></a>Введение зависимостей (службы)

*Служба* — это компонент, предназначенный для общего использования в приложении. Доступ к службе предоставляется путем [внедрения зависимостей](xref:fundamentals/dependency-injection). ASP.NET Core включает встроенный контейнер для инверсии управления (IoC), который поддерживает [внедрение конструктора](xref:mvc/controllers/dependency-injection#constructor-injection) по умолчанию. При необходимости вы можете заменить контейнер по умолчанию. Помимо формирования [слабых взаимосвязей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), внедрение зависимостей обеспечивает доступ к службам (например, [ведение журнала](xref:fundamentals/logging/index)) в рамках всего приложения.

Дополнительные сведения см. в разделе <xref:fundamentals/dependency-injection>.

## <a name="middleware"></a>ПО промежуточного слоя

В ASP.NET Core конвейер запросов создается с помощью [ПО промежуточного слоя](xref:fundamentals/middleware/index). ПО промежуточного слоя ASP.NET Core выполняет асинхронные операции в `HttpContext`, а затем либо вызывает следующее ПО промежуточного слоя в конвейере, либо завершает запрос.

По соглашению компонент ПО промежуточного слоя с именем "XYZ" добавляется в конвейер путем вызова метода расширения `UseXYZ` в методе `Configure`.

ASP.NET Core включает целый ряд встроенного ПО промежуточного слоя и позволяет писать собственные пользовательские ПО промежуточного слоя. [Открытый веб-интерфейс для .NET (OWIN)](xref:fundamentals/owin), позволяющий ослабить связь веб-приложений с веб-серверами, поддерживается в приложениях ASP.NET Core.

Дополнительные сведения см. в разделах <xref:fundamentals/middleware/index> и <xref:fundamentals/owin>.

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>Инициирование HTTP-запросов

Можно использовать <xref:System.Net.Http.IHttpClientFactory> для доступа к экземплярам <xref:System.Net.Http.HttpClient> для HTTP-запросов.

Дополнительные сведения см. в разделе <xref:fundamentals/http-requests>.

::: moniker-end

## <a name="environments"></a>Среды

Среды, например *среда разработки* и *рабочая среда*, представляют собой ключевые компоненты ASP.NET Core и могут задаваться с использованием соответствующих переменных среды, файлов параметров и аргумента командной строки.

Дополнительные сведения см. в разделе <xref:fundamentals/environments>.

## <a name="hosting"></a>Размещение

Приложения ASP.NET Core настраивают и запускают *хост*, который отвечает за запуск приложений и управление жизненным циклом.

Дополнительные сведения см. в разделе <xref:fundamentals/host/index>.

## <a name="servers"></a>Серверы

Модель размещения ASP.NET Core не прослушивает запросы напрямую. Модель размещения полагается на реализацию HTTP-сервера для переадресации запроса в приложение. Переадресованный запрос упаковывается как набор объектов функций, доступ к которым можно получить через интерфейсы. ASP.NET Core включает управляемый кроссплатформенный веб-сервер под названием [Kestrel](xref:fundamentals/servers/kestrel). Kestrel обычно выполняется позади рабочего веб-сервера, например [IIS](https://www.iis.net/) или [Nginx](http://nginx.org), в обратной конфигурации прокси-сервера. Kestrel также можно использовать в роли общедоступного пограничного сервера с прямым доступом к Интернету в ASP.NET Core 2.0 или более поздней версии.

Дополнительные сведения см. в разделе <xref:fundamentals/servers/index>.

## <a name="configuration"></a>Конфигурация

ASP.NET Core использует модель конфигурации на основе пар "имя-значение". Модель конфигурации не основана на <xref:System.Configuration> или *web.config*. Конфигурация получает параметры от упорядоченного набора поставщиков конфигурации. Встроенные поставщики конфигурации поддерживают различные форматы файлов (XML, JSON, INI), переменных среды и аргументов командной строки. Кроме того, можно писать и собственные, пользовательские поставщики конфигурации.

Дополнительные сведения см. в разделе <xref:fundamentals/configuration/index>.

## <a name="logging"></a>Ведение журнала

ASP.NET Core поддерживает API ведения журнала, который работает с разными поставщиками. Встроенные поставщики поддерживают отправку журналов в одно расположение или несколько. Можно также использовать сторонние платформы ведения журнала.

Дополнительные сведения см. в разделе <xref:fundamentals/logging/index>.

## <a name="error-handling"></a>Обработка ошибок

ASP.NET Core включает встроенные сценарии для обработки ошибок в приложениях, включая страницу исключений разработчика, настраиваемые страницы ошибок, страницы статических кодов состояний и обработку исключений при запуске.

Дополнительные сведения см. в разделе <xref:fundamentals/error-handling>.

## <a name="routing"></a>Маршрутизация

В ASP.NET Core присутствуют сценарии для маршрутизации запросов приложений обработчикам маршрутов.

Дополнительные сведения см. в разделе <xref:fundamentals/routing>.

## <a name="file-providers"></a>Поставщики файлов

ASP.NET Core ограничивает доступ к файловой системе с помощью поставщиков файлов, которые предоставляют общий интерфейс для работы с файлами на разных платформах.

Дополнительные сведения см. в разделе <xref:fundamentals/file-providers>.

## <a name="static-files"></a>Статические файлы

ПО промежуточного слоя для статических файлов обрабатывает такие статические файлы, как HTML, CSS, файлы изображений и JavaScript.

Дополнительные сведения см. в разделе <xref:fundamentals/static-files>.

## <a name="session-and-app-state"></a>Состояние сеанса и приложения

ASP.NET Core предлагает несколько способов сохранения состояния сеанса и приложения во время работы пользователя в веб-приложении.

Дополнительные сведения см. в разделе <xref:fundamentals/app-state>.

## <a name="globalization-and-localization"></a>Глобализация и локализация

Создание многоязычного веб-сайта на основе ASP.NET Core позволяет расширить аудиторию. ASP.NET Core предоставляет службы и ПО промежуточного слоя для локализации содержимого на разные языки и для разных региональных параметров.

Дополнительные сведения см. в разделе <xref:fundamentals/localization>.

## <a name="request-features"></a>Параметры запроса

Сведения о реализации веб-сервера, связанные с HTTP-запросами и ответами, определяются в интерфейсах. Эти интерфейсы используются реализациями сервера и ПО промежуточного слоя для создания и изменения конвейера размещения приложений.

Дополнительные сведения см. в разделе <xref:fundamentals/request-features>.

## <a name="background-tasks"></a>Фоновые задачи

Фоновые задачи реализуются как *размещенные службы*. Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.

Дополнительные сведения см. в разделе <xref:fundamentals/host/hosted-services>.

## <a name="access-httpcontext"></a>Доступ к HttpContext

Доступ к `HttpContext` предоставляется автоматически при обработке запросов с Razor Pages и MVC. Если к `HttpContext` нет доступа, вы можете получить доступ к `HttpContext` через интерфейс <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> и реализацию по умолчанию <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.

Дополнительные сведения см. в разделе <xref:fundamentals/httpcontext>.

## <a name="websockets"></a>Протокол WebSocket

[WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям. Он используется для таких приложений, как чаты, биржевые тикеры, игры, а также везде, где в веб-приложении нужны функции реального времени. ASP.NET Core поддерживает сценарии WebSocket.

Дополнительные сведения см. в разделе <xref:fundamentals/websockets>.

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a>Метапакет Microsoft.AspNetCore.App

Метапакет [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) упрощает управление пакетами.

Дополнительные сведения см. в разделе <xref:fundamentals/metapackage-app>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a>Метапакет Microsoft.AspNetCore.All

Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:

* все пакеты, поддерживаемые командой ASP.NET Core;
* все пакеты, поддерживаемые Entity Framework Core;
* внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core.

Дополнительные сведения см. в разделе <xref:fundamentals/metapackage>.

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a>Выбор между .NET Core и .NET Framework

Приложения ASP.NET Core могут задавать среду выполнения .NET Core или .NET Framework как целевую.

Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).

## <a name="choose-between-aspnet-core-and-aspnet"></a>Выбор между ASP.NET Core и ASP.NET

Дополнительные сведения о выборе между ASP.NET Core и ASP.NET см. в разделе <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.
