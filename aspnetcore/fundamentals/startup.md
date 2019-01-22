---
title: Запуск приложения в ASP.NET Core
author: tdykstra
description: Сведения о том, как класс Startup в ASP.NET Core настраивает службы и конвейер запросов приложения.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/startup
ms.openlocfilehash: 685b496943642b349321a36a7200d6d51ecf4d6e
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396237"
---
# <a name="app-startup-in-aspnet-core"></a>Запуск приложения в ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Люк Лэтем](https://github.com/guardrex) (Luke Latham) и [Стив Смит](https://ardalis.com) (Steve Smith)

Класс `Startup` настраивает службы и конвейер запросов приложения.

## <a name="the-startup-class"></a>Класс Startup

Приложения ASP.NET Core используют класс `Startup`, который по соглашению называется `Startup`. Класс `Startup`:

* При необходимости содержит метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> для настройки *служб* приложения. Служба — многократно используемый компонент, обеспечивающий функциональность приложения. Службы настраиваются &mdash; или *регистрируются*&mdash;в `ConfigureServices` и используются в приложениях с помощью [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection) или <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.
* Содержит метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> для создания конвейера обработки запросов приложения.

`ConfigureServices` и `Configure` вызываются средой выполнения при запуске приложения:

[!code-csharp[](startup/sample_snapshot/Startup1.cs?highlight=4,10)]

Класс `Startup` указывается в приложении при создании [узла](xref:fundamentals/host/index) приложения. Узел приложения создается при вызове `Build` в построителе узлов в классе `Program`. Класс `Startup` обычно указывается путем вызова метода [WebHostBuilderExtensions.UseStartup\<TStartup >](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) в построителе узлов.

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=10)]

Узел предоставляет службы, которые доступны конструктору классов `Startup`. Приложение добавляет дополнительные службы через `ConfigureServices`. Как службы узла, так и службы приложения доступны в `Configure` и во всем приложении.

Типичным применением [внедрения зависимостей](xref:fundamentals/dependency-injection) в класс `Startup` является внедрение:

* <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> для настройки служб средой;
* <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> для чтения конфигурации;
* <xref:Microsoft.Extensions.Logging.ILoggerFactory> для создания средства ведения журнала в `Startup.ConfigureServices`.

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

Альтернативой внедрению `IHostingEnvironment` является использование подхода на основе соглашений. Когда приложение определяет отдельные классы `Startup` для различных сред (например, `StartupDevelopment`), подходящий класс `Startup` выбирается во время выполнения. Класс, у которого суффикс имени соответствует текущей среде, получает приоритет. Если приложение выполняется в среде разработки и включает в себя оба класса — `Startup` и `StartupDevelopment`, используется класс `StartupDevelopment`. Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments#environment-based-startup-class-and-methods).

Дополнительные сведения об узле см. в статье <xref:fundamentals/host/index>. Сведения об обработке ошибок во время запуска см. в разделе [Обработка исключений при запуске](xref:fundamentals/error-handling#startup-exception-handling).

## <a name="the-configureservices-method"></a>Метод ConfigureServices

Метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*>:

* Необязательный параметр.
* Вызывается узлом перед методом `Configure` для настройки служб приложения.
* По соглашению используется для задания [параметров конфигурации](xref:fundamentals/configuration/index).

По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`. Пример см. в разделе [Настройка служб удостоверений](xref:security/authentication/identity#pw).

Узел может настраивать некоторые службы перед вызовом методов `Startup`. Для получения дополнительной информации см. <xref:fundamentals/host/index>.

Для функций, нуждающихся в значительной настройке, существуют методы расширения `Add{Service}` в <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>. Обычное приложение ASP.NET Core регистрирует службы для Entity Framework, удостоверения и MVC:

[!code-csharp[](startup/sample_snapshot/Startup3.cs?highlight=4,7,11)]

Добавление служб в контейнер служб делает их доступными в приложении и в методе `Configure`. Службы разрешаются посредством [внедрения зависимостей](xref:fundamentals/dependency-injection) или из <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.

## <a name="the-configure-method"></a>Метод Configure 

Метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> используется для указания того, как приложение реагирует на HTTP-запросы. Конвейер запросов настраивается путем добавления компонентов [ПО промежуточного слоя](xref:fundamentals/middleware/index) в экземпляр <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>. `IApplicationBuilder` доступен для метода `Configure`, но он не зарегистрирован в контейнере службы. При размещении создается `IApplicationBuilder` и передается непосредственно в `Configure`.

[Шаблоны ASP.NET Core](/dotnet/core/tools/dotnet-new) настраивают конвейер с поддержкой следующих компонентов и функций:

* [страниц исключений для разработчика](xref:fundamentals/error-handling#the-developer-exception-page);
* [обработчиков исключений](xref:fundamentals/error-handling#configure-a-custom-exception-handling-page);
* [HTTP Strict Transport Security (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [перенаправления HTTPS](xref:security/enforcing-ssl);
* [Статические файлы](xref:fundamentals/static-files)
* [общего регламента по защите данных (GDPR)](xref:security/gdpr);
* ASP.NET Core [MVC](xref:mvc/overview) и [Razor Pages](xref:razor-pages/index).

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

Каждый метод расширения `Use` добавляет один или несколько компонентов ПО промежуточного слоя в конвейер запросов. Например, метод расширения `UseMvc` добавляет в конвейер запросов [ПО промежуточного слоя для маршрутизации](xref:fundamentals/routing) и настраивает [MVC](xref:mvc/overview) в качестве обработчика по умолчанию.

Каждый компонент ПО промежуточного слоя в конвейере запросов отвечает за вызов следующего компонента в конвейере или замыкает цепочку, если это необходимо. Если замыкание в цепочке ПО промежуточного слоя не происходит, у каждого ПО промежуточного слоя есть еще один шанс обработать запрос перед отправкой клиенту.

В сигнатуре метода `Configure` также могут быть указаны дополнительные службы, такие как `IHostingEnvironment` и `ILoggerFactory`. Когда дополнительные службы указаны, они внедряются, если доступны.

Дополнительные сведения об использовании `IApplicationBuilder` и порядке обработки ПО промежуточного слоя см. в статье <xref:fundamentals/middleware/index>.

## <a name="convenience-methods"></a>Удобный метод

Для настройки служб и конвейера обработки запросов в построителе узлов вместо класса `Startup` можно использовать удобные методы `ConfigureServices` и `Configure`. Несколько вызовов `ConfigureServices` добавляются друг к другу. При наличии нескольких вызовов метода `Configure` используется последний вызов `Configure`.

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=18,22)]

## <a name="extend-startup-with-startup-filters"></a>Расширение класса Startup с использованием фильтров запуска

Используйте <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> для настройки ПО промежуточного слоя в начале или конце конвейера ПО промежуточного слоя [Configure](#the-configure-method) приложения. `IStartupFilter` удобно использовать, чтобы обеспечить запуск ПО промежуточного слоя до или после ПО промежуточного слоя, добавляемого библиотеками в начале или в конце конвейера обработки запросов приложения.

`IStartupFilter` реализует отдельный метод <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, который принимает и возвращает `Action<IApplicationBuilder>`. <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> определяет класс для настройки конвейера запросов приложения. Дополнительные сведения см. в разделе [Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).

Каждый `IStartupFilter` реализует один или несколько компонентов ПО промежуточного слоя в конвейере запросов. Фильтры вызываются в том порядке, в котором они были добавлены в контейнер службы. Фильтры могут добавлять ПО промежуточного слоя до или после передачи управления следующему фильтру, поэтому они добавляются в начало или конец конвейера приложения.

В следующем примере показана регистрация ПО промежуточного слоя с помощью `IStartupFilter`.

ПО промежуточного слоя `RequestSetOptionsMiddleware` задает значения параметров из параметра строки запроса:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

`RequestSetOptionsMiddleware` настраивается в классе `RequestSetOptionsStartupFilter`:

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

`IStartupFilter` регистрируется в контейнере службы в <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> и дополняет `Startup` из-за пределов класса `Startup`:

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

Если указан параметр строки запроса для `option`, ПО промежуточного слоя обрабатывает присвоение значения до того, как ПО промежуточного слоя MVC отображает отклик:

![Окно обозревателя с отображаемой страницей индекса. Значение параметра отображается как "From Middleware", так как запрашивается страница с помощью параметра строки запроса и задано значение параметра "From Middleware".](startup/_static/index.png)

Порядок выполнения ПО промежуточного слоя определяется порядком регистраций `IStartupFilter`:

* Несколько реализаций `IStartupFilter` могут взаимодействовать с одними и теми же объектами. Если важен порядок, упорядочите регистрации службы `IStartupFilter` в соответствии с требуемым порядком выполнения ПО промежуточного слоя.
* Библиотеки могут добавлять ПО промежуточного слоя с одной или несколькими реализациями `IStartupFilter`, которые выполняются до или после другого ПО промежуточного слоя приложения, зарегистрированного с помощью `IStartupFilter`. Чтобы вызвать ПО промежуточного слоя `IStartupFilter` до ПО промежуточного слоя, добавляемого библиотекой `IStartupFilter`, расположите регистрацию службы до добавления библиотеки в контейнер службы. Чтобы вызвать его после этого момента, расположите регистрацию службы после добавления библиотеки.

## <a name="add-configuration-at-startup-from-an-external-assembly"></a>Добавление конфигурации из внешней сборки при запуске

Реализация <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> позволяет при запуске добавлять в приложение улучшения из внешней сборки вне приложения класса `Startup`. Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/host/index>
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
