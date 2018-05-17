---
title: Активация ПО промежуточного слоя с помощью контейнера сторонних разработчиков в ASP.NET Core
author: guardrex
description: В этой статье приводятся сведения о том, как использовать строго типизированное ПО промежуточного слоя с активацией на основе фабрики и контейнером сторонних разработчиков в ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: b6fd02d1863efe571bafe1344fb34939883e6cb5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>Активация ПО промежуточного слоя с помощью контейнера сторонних разработчиков в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

В этой статье демонстрируется использование [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) и [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) в качестве точки расширяемости для активации [ПО промежуточного слоя](xref:fundamentals/middleware/index) с помощью контейнера сторонних разработчиков. Вводные сведения о `IMiddlewareFactory` и `IMiddleware` см. в разделе [Активация ПО промежуточного слоя на основе фабрики](xref:fundamentals/middleware/extensibility).

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

В примере приложения показана активация ПО промежуточного слоя путем реализации `IMiddlewareFactory`, `SimpleInjectorMiddlewareFactory`. В этом образце используется контейнер внедрения зависимостей [Simple Injector](https://simpleinjector.org).

Реализация ПО промежуточного слоя в примере записывает значение, предоставленное в параметре строки запроса (`key`). ПО промежуточного слоя использует внедренный контекст базы данных (служба с заданной областью) для записи значения строки запроса в базу данных, выполняющуюся в памяти.

> [!NOTE]
> В примере приложения [Simple Injector](https://github.com/simpleinjector/SimpleInjector) используется исключительно для демонстрации. Использование Simple Injector не означает поддержку данного инструмента. Подходы к активации ПО промежуточного слоя, описанные в документации по Simple Injector и статьях на GitHub, рекомендованы командой Simple Injector. Дополнительные сведения см. в [документации по Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) и [в репозитории Simple Injector в GitHub](https://github.com/simpleinjector/SimpleInjector).

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) предоставляет методы для создания ПО промежуточного слоя.

В примере приложения реализуется фабрика ПО промежуточного слоя для создания экземпляра `SimpleInjectorActivatedMiddleware`. Фабрика ПО промежуточного слоя использует контейнер Simple Injector для разрешения по промежуточного слоя:

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) определяет ПО промежуточного слоя для конвейера запросов приложения.

ПО промежуточного слоя, активированное реализацией `IMiddlewareFactory` (*Middleware/SimpleInjectorActivatedMiddleware.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

Для ПО промежуточного слоя создается расширение (*Middleware/MiddlewareExtensions.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` должен выполнять несколько задач:

* Настройка контейнера Simple Injector.
* Регистрация фабрики и ПО промежуточного слоя.
* Предоставление контекста базы данных приложения из контейнера Simple Injector для страницы Razor.

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

ПО промежуточного слоя регистрируется в конвейере обработки запросов в `Startup.Configure`:

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=12)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [ПО промежуточного слоя](xref:fundamentals/middleware/index)
* [Активация фабричного ПО промежуточного слоя](xref:fundamentals/middleware/extensibility)
* [Репозиторий Simple Injector в GitHub](https://github.com/simpleinjector/SimpleInjector)
* [Документация по Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html)
