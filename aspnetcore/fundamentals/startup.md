---
title: Запуск приложения в ASP.NET Core
author: rick-anderson
description: Сведения о том, как класс Startup в ASP.NET Core настраивает службы и конвейер запросов приложения.
ms.author: riande
ms.custom: mvc
ms.date: 8/7/2019
uid: fundamentals/startup
ms.openlocfilehash: 9407de4ee91ba43b2c95fa98f0cf479bf8539cab
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310500"
---
# <a name="app-startup-in-aspnet-core"></a>Запуск приложения в ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Люк Лэтэм](https://github.com/guardrex) (Luke Latham) и [Стив Смит](https://ardalis.com) (Steve Smith)

Класс `Startup` настраивает службы и конвейер запросов приложения.

## <a name="the-startup-class"></a>Класс Startup

Приложения ASP.NET Core используют класс `Startup`, который по соглашению называется `Startup`. Класс `Startup`:

* При необходимости содержит метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> для настройки *служб* приложения. Служба — многократно используемый компонент, обеспечивающий функциональность приложения. Службы настраиваются &mdash; или *регистрируются*&mdash;в `ConfigureServices` и используются в приложениях с помощью [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection) или <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.
* Содержит метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> для создания конвейера обработки запросов приложения.

`ConfigureServices` и `Configure` вызываются средой выполнения ASP.NET Core при запуске приложения:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

Предыдущий пример предназначен для [Razor Pages](xref:razor-pages/index); версия для MVC похожа.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup1.cs)]

::: moniker-end

Класс `Startup` указывается при создании [узла](xref:fundamentals/index#host) приложения. Класс `Startup` обычно указывается путем вызова метода [`WebHostBuilderExtensions.UseStartup<TStartup>`](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) в построителе узлов.

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/Program3.cs?name=snippet_Program&highlight=12)]

Узел предоставляет службы, которые доступны конструктору классов `Startup`. Приложение добавляет дополнительные службы через `ConfigureServices`. Как службы узла, так и службы приложения доступны в `Configure` и во всем приложении.

При использовании <xref:Microsoft.Extensions.Hosting.IHostBuilder> в конструктор `Startup` могут внедряться только следующие типы служб:

* `IWebHostEnvironment`
* `IHostEnvironment`
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartUp2.cs?name=snippet)]

Большинство служб недоступны, пока не будет вызван метод `Configure`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Узел предоставляет службы, которые доступны конструктору классов `Startup`. Приложение добавляет дополнительные службы через `ConfigureServices`. Как службы узла, так и службы приложения доступны в `Configure` и во всем приложении.

Типичным применением [внедрения зависимостей](xref:fundamentals/dependency-injection) в класс `Startup` является внедрение:

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> для настройки служб средой;
* <xref:Microsoft.Extensions.Configuration.IConfiguration> для чтения конфигурации;
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> для создания средства ведения журнала в `Startup.ConfigureServices`.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

Большинство служб недоступны, пока не будет вызван метод `Configure`.

::: moniker-end

### <a name="multiple-startup"></a>Несколько запусков

Когда приложение определяет отдельные классы `Startup` для различных сред (например, `StartupDevelopment`), подходящий класс `Startup` выбирается во время выполнения. Класс, у которого суффикс имени соответствует текущей среде, получает приоритет. Если приложение выполняется в среде разработки и включает в себя оба класса — `Startup` и `StartupDevelopment`, используется класс `StartupDevelopment`. Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Дополнительные сведения об узле см. в разделе [Узел](xref:fundamentals/index#host). Сведения об обработке ошибок во время запуска см. в разделе [Обработка исключений при запуске](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Метод ConfigureServices

Метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>:

* Необязательный параметр.
* Вызывается узлом перед методом `Configure` для настройки служб приложения.
* По соглашению используется для задания [параметров конфигурации](xref:fundamentals/configuration/index).

Узел может настраивать некоторые службы перед вызовом методов `Startup`. Дополнительную информацию см. в разделе [Узел](xref:fundamentals/index#host).

Для функций, нуждающихся в значительной настройке, существуют методы расширения `Add{Service}` в <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>. Например, **Add**DbContext, **Add**DefaultIdentity, **Add**EntityFrameworkStores и **Add**RazorPages:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/StartupIdentity.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup3.cs)]

::: moniker-end

Добавление служб в контейнер служб делает их доступными в приложении и в методе `Configure`. Службы разрешаются посредством [внедрения зависимостей](xref:fundamentals/dependency-injection) или из <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.

::: moniker range="< aspnetcore-3.0"

Подробнее о `SetCompatibilityVersion` см. в сведениях о [SetCompatibilityVersion](xref:mvc/compatibility-version).

::: moniker-end

## <a name="the-configure-method"></a>Метод Configure

Метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> используется для указания того, как приложение реагирует на HTTP-запросы. Конвейер запросов настраивается путем добавления компонентов [ПО промежуточного слоя](xref:fundamentals/middleware/index) в экземпляр <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>. `IApplicationBuilder` доступен для метода `Configure`, но он не зарегистрирован в контейнере службы. При размещении создается `IApplicationBuilder` и передается непосредственно в `Configure`.

[Шаблоны ASP.NET Core](/dotnet/core/tools/dotnet-new) настраивают конвейер с поддержкой следующих компонентов и функций:

* [Страница со сведениями об исключении для разработчика](xref:fundamentals/error-handling#developer-exception-page)
* [обработчиков исключений](xref:fundamentals/error-handling#exception-handler-page);
* [HTTP Strict Transport Security (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [перенаправления HTTPS](xref:security/enforcing-ssl);
* [Статические файлы](xref:fundamentals/static-files)
* ASP.NET Core [MVC](xref:mvc/overview) и [Razor Pages](xref:razor-pages/index).

::: moniker range="< aspnetcore-3.0"

* [общего регламента по защите данных (GDPR)](xref:security/gdpr);

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Startup.cs?name=snippet)]

Предыдущий пример предназначен для [Razor Pages](xref:razor-pages/index); версия для MVC похожа.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

::: moniker-end

Каждый метод расширения `Use` добавляет один или несколько компонентов ПО промежуточного слоя в конвейер запросов. Например, <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*> настраивает [ПО промежуточного слоя](xref:fundamentals/middleware/index) для обслуживания [статических файлов](xref:fundamentals/static-files).

Каждый компонент ПО промежуточного слоя в конвейере запросов отвечает за вызов следующего компонента в конвейере или замыкает цепочку, если это необходимо.

::: moniker range=">= aspnetcore-3.0"

В сигнатуре метода `Configure` могут быть указаны дополнительные службы, такие как `IWebHostEnvironment`, `ILoggerFactory` или любые службы, определенные в `ConfigureServices`. Эти службы внедряются, если доступны.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

В сигнатуре метода `Configure` могут быть указаны дополнительные службы, такие как `IHostingEnvironment`, `ILoggerFactory` или любые службы, определенные в `ConfigureServices`. Эти службы внедряются, если доступны.

::: moniker-end

Дополнительные сведения об использовании `IApplicationBuilder` и порядке обработки ПО промежуточного слоя см. в статье <xref:fundamentals/middleware/index>.

<a name="convenience-methods"></a>

## <a name="configure-services-without-startup"></a>Настройка служб без запуска

Для настройки служб и конвейера обработки запросов в построителе узлов вместо класса `Startup` можно использовать удобные методы `ConfigureServices` и `Configure`. Несколько вызовов `ConfigureServices` добавляются друг к другу. При наличии нескольких вызовов метода `Configure` используется последний вызов `Configure`.

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program1.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=16,20)]

::: moniker-end

## <a name="extend-startup-with-startup-filters"></a>Расширение класса Startup с использованием фильтров запуска

Используйте <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> для настройки ПО промежуточного слоя в начале или конце конвейера ПО промежуточного слоя [Configure](#the-configure-method) приложения. `IStartupFilter` используется для создания конвейера методов `Configure`. [IStartupFilter.Configure](xref:Microsoft.AspNetCore.Hosting.IStartupFilter.Configure*) может обеспечивать запуск ПО промежуточного слоя до или после ПО промежуточного слоя, добавляемого библиотеками.

`IStartupFilter` реализует метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, который принимает и возвращает `Action<IApplicationBuilder>`. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> определяет класс для настройки конвейера запросов приложения. Дополнительные сведения см. в разделе [Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Каждый интерфейс `IStartupFilter` может добавлять один или несколько компонентов ПО промежуточного слоя в конвейер запросов. Фильтры вызываются в том порядке, в котором они были добавлены в контейнер службы. Фильтры могут добавлять ПО промежуточного слоя до или после передачи управления следующему фильтру, поэтому они добавляются в начало или конец конвейера приложения.

В следующем примере показана регистрация ПО промежуточного слоя с помощью `IStartupFilter`. ПО промежуточного слоя `RequestSetOptionsMiddleware` задает значения параметров из параметра строки запроса:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsMiddleware.cs?name=snippet1)]

`RequestSetOptionsMiddleware` настраивается в классе `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/3.0_samples/StartupFilterSample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` настраивается в классе `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

::: moniker-end

`IStartupFilter` регистрируется в контейнере службы в <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](startup/3.0_samples/StartupFilterSample/Program.cs?name=snippet&highlight=19-20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

::: moniker-end

Если указан параметр строки запроса для `option`, ПО промежуточного слоя обрабатывает присвоение значения до того, как ПО промежуточного слоя ASP.NET Core отображает ответ.

Порядок выполнения ПО промежуточного слоя определяется порядком регистраций `IStartupFilter`:

* Несколько реализаций `IStartupFilter` могут взаимодействовать с одними и теми же объектами. Если важен порядок, упорядочите регистрации службы `IStartupFilter` в соответствии с требуемым порядком выполнения ПО промежуточного слоя.
* Библиотеки могут добавлять ПО промежуточного слоя с одной или несколькими реализациями `IStartupFilter`, которые выполняются до или после другого ПО промежуточного слоя приложения, зарегистрированного с помощью `IStartupFilter`. Чтобы вызвать ПО промежуточного слоя `IStartupFilter` до ПО промежуточного слоя, добавляемого библиотекой `IStartupFilter`, сделайте следующее:

  * расположите регистрацию службы до добавления библиотеки в контейнер службы.
  * Чтобы вызвать его после этого момента, расположите регистрацию службы после добавления библиотеки.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Добавление конфигурации из внешней сборки при запуске

Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`. Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Узел](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
