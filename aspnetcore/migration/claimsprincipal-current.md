---
title: Миграция из ClaimsPrincipal. Current
author: mjrousos
description: Узнайте, как выполнить миграцию с ClaimsPrincipal. Current, чтобы получить удостоверение текущего пользователя, прошедшего проверку подлинности, и утверждения в ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: f7472f5b851d3869da3d26b881e276ce4ca004fb
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115968"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Миграция из ClaimsPrincipal. Current

В проектах ASP.NET 4. x было распространено использование [ClaimsPrincipal. Current](/dotnet/api/system.security.claims.claimsprincipal.current) для получения удостоверения и утверждений текущего пользователя, прошедшего проверку подлинности. В ASP.NET Core это свойство больше не задано. Код, который был в зависимости от него, необходимо обновить, чтобы получить удостоверение текущего пользователя, прошедшего проверку подлинности, с помощью различных средств.

## <a name="context-specific-data-instead-of-static-data"></a>Данные, зависящие от контекста, а не статические данные

При использовании ASP.NET Core значения `ClaimsPrincipal.Current` и `Thread.CurrentPrincipal` не устанавливаются. Эти свойства представляют статическое состояние, которое ASP.NET Core обычно избежать. Вместо этого ASP.NET Core архитектура предназначена для получения зависимостей (таких как удостоверение текущего пользователя) из коллекций служб, зависящих от контекста (с помощью модели [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI)). Более того, `Thread.CurrentPrincipal` является статическим потоком, поэтому он может не сохранять изменения в некоторых асинхронных сценариях (и `ClaimsPrincipal.Current` просто вызывает `Thread.CurrentPrincipal` по умолчанию).

Чтобы понять типы проблем, которые статические члены потока могут привести к асинхронным сценариям, рассмотрим следующий фрагмент кода:

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

Предыдущий образец кода задает `Thread.CurrentPrincipal` и проверяет его значение до и после ожидания асинхронного вызова. `Thread.CurrentPrincipal` зависит от *потока* , в котором он установлен, и метод, скорее всего, возобновляет выполнение в другом потоке после ожидания. Следовательно, `Thread.CurrentPrincipal` находится в момент первой проверки, но имеет значение NULL после вызова `await Task.Yield()`.

Получение удостоверения текущего пользователя из коллекции служб DI для приложения является более тестируемым, так как можно легко внедрять удостоверения тестов.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Получение текущего пользователя в ASP.NET Core приложении

Существует несколько вариантов извлечения `ClaimsPrincipal` текущего пользователя, прошедшего проверку подлинности ASP.NET Core вместо `ClaimsPrincipal.Current`.

* **ControllerBase. User**. Контроллеры MVC могут обращаться к текущему аутентифицированному пользователю с помощью его свойства [пользователя](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) .
* **HttpContext. User**. Компоненты с доступом к текущему `HttpContext` (например, по промежуточного слоя) могут получать `ClaimsPrincipal` текущего пользователя из [HttpContext. User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Передано из вызывающей стороны**. Библиотеки без доступа к текущей `HttpContext` часто вызываются из контроллеров или компонентов промежуточного слоя и могут иметь удостоверение текущего пользователя, переданное в качестве аргумента.
* **Ихттпконтекстакцессор**. Проект, переносимый в ASP.NET Core, может быть слишком большим для простого передачи удостоверения текущего пользователя во все необходимые расположения. В таких случаях в качестве обходного пути можно использовать [ихттпконтекстакцессор](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) . `IHttpContextAccessor` может получить доступ к текущей `HttpContext` (если она существует). Если используется DI, см. раздел <xref:fundamentals/httpcontext>. Краткосрочное решение для получения удостоверения текущего пользователя в коде, которое еще не было обновлено для работы с архитектурой на основе ASP.NET Core, будет выглядеть так:

  * Сделайте `IHttpContextAccessor` доступным в контейнере DI, вызвав [аддхттпконтекстакцессор](https://github.com/aspnet/Hosting/issues/793) в `Startup.ConfigureServices`.
  * Получите экземпляр `IHttpContextAccessor` во время запуска и сохраните его в статической переменной. Экземпляр становится доступным для кода, который ранее получал текущего пользователя из статического свойства.
  * Получение `ClaimsPrincipal` текущего пользователя с помощью `HttpContextAccessor.HttpContext?.User`. Если этот код используется вне контекста HTTP-запроса, `HttpContext` имеет значение null.

Последний вариант, использующий экземпляр `IHttpContextAccessor`, хранящийся в статической переменной, противоречит принципам ASP.NET Core (которые добавляют внедренные зависимости в статические зависимости). Запланируйте в конечном итоге извлечение `IHttpContextAccessor` экземпляров из внедрения зависимостей. Статический вспомогательный метод может быть полезным мостом, но при переносе больших существующих приложений ASP.NET, которые ранее использовали `ClaimsPrincipal.Current`.
