---
title: Миграция с ClaimsPrincipal.Current
author: mjrousos
description: Дополнительные сведения о миграции от ClaimsPrincipal.Current для получения удостоверения текущего прошедшего проверку подлинности пользователя и утверждения в ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/claimsprincipal-current
ms.openlocfilehash: ea43d17e76380baf57cd9debbc508e8812cfa4a6
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851536"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Миграция с ClaimsPrincipal.Current

В проектах ASP.NET был обычно используется [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) извлечь текущий с проверкой подлинности удостоверения пользователя и заявки. Это свойство больше не устанавливаются в ASP.NET Core. Код, который, зависящие от нее должен быть обновлен для получения удостоверения текущего прошедшего проверку подлинности пользователя через различными способами.

## <a name="context-specific-data-instead-of-static-data"></a>Контекстные данные вместо статических данных

При использовании ASP.NET Core, оба значения `ClaimsPrincipal.Current` и `Thread.CurrentPrincipal` не заданы. Оба эти свойства представляют статических состояние, которое обычно избегает ASP.NET Core. Вместо этого архитектура ASP.NET Core состоит в извлечении зависимости (например, удостоверение текущего пользователя) из коллекции контекстно зависимые службы (с помощью его [внедрения зависимостей](xref:fundamentals/dependency-injection) модели (DI)). Более, `Thread.CurrentPrincipal` статичен, поток, поэтому не может сохраняться изменения в некоторых сценариях асинхронной (и `ClaimsPrincipal.Current` просто вызывает `Thread.CurrentPrincipal` по умолчанию).

Для понимания виды проблем поток статических членов может привести к в асинхронные сценарии, рассмотрим следующий фрагмент кода:

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

Предыдущий образец кода устанавливает `Thread.CurrentPrincipal` и проверяет его значение до и после Ожидание асинхронного вызова. `Thread.CurrentPrincipal` относится только к *поток* на котором установлено, и метод может возобновить выполнение в другом потоке после оператора await. Следовательно `Thread.CurrentPrincipal` участия, если он сначала проверяется, но имеет значение null после вызова `await Task.Yield()`.

Получение удостоверение текущего пользователя из приложения DI службы коллекции является более тестируемых слишком, поскольку тест удостоверения легко могут быть добавлены.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Получение текущего пользователя в приложении ASP.NET Core

Существует несколько вариантов для извлечения текущего прошедшего проверку подлинности пользователя `ClaimsPrincipal` в ASP.NET Core вместо `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Контроллеров MVC доступны текущего пользователя, прошедшего проверку подлинности с их [пользователя](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) свойство.
* **HttpContext.User**. Компоненты с доступом к текущему `HttpContext` (например, по промежуточного слоя) можно получить текущий пользователь `ClaimsPrincipal` из [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Переданный вызывающего**. Библиотеки не имеющие доступа к текущим `HttpContext` часто вызывается из контроллеров или компонентов по промежуточного слоя и может быть передана в качестве аргумента идентификатора текущего пользователя.
* **IHttpContextAccessor**. Проект ASP.NET, переносимые в ASP.NET Core может быть слишком большим, чтобы легко передать все требуемые позиции удостоверение текущего пользователя. В таких случаях [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) можно использовать в качестве решения этой проблемы. `IHttpContextAccessor` может получить доступ к текущим `HttpContext` (если он существует). Краткосрочное решение можно получить удостоверение текущего пользователя в коде, который еще не была обновлена для работы с архитектурой управляемых DI ASP.NET Core будет следующим:

  * Сделать `IHttpContextAccessor` доступного в контейнере DI путем вызова [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) в `Startup.ConfigureServices`.
  * Получить экземпляр `IHttpContextAccessor` во время загрузки и сохранения его в статической переменной. Экземпляр становится доступным для кода, который ранее получение текущего пользователя из статического свойства.
  * Получение текущего пользователя `ClaimsPrincipal` с помощью `HttpContextAccessor.HttpContext?.User`. Если этот код используется вне контекста HTTP-запроса, `HttpContext` имеет значение null.

Конечный параметр, с помощью `IHttpContextAccessor`, противоречит принципы ASP.NET Core (предпочитая подставляемого зависимости статических зависимости). Запланируйте окончательно удалить зависимость от статического `IHttpContextAccessor` вспомогательный. Может быть полезным моста, при переносе большого существующего приложения ASP.NET, которые были ранее с помощью `ClaimsPrincipal.Current`.
