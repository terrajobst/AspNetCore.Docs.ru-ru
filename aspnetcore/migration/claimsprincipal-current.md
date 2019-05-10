---
title: Перенос из ClaimsPrincipal.Current
author: mjrousos
description: Узнайте, как избежать ClaimsPrincipal.Current для получения удостоверения текущего прошедшего проверку подлинности пользователя и утверждения в ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: 526cc3cf3a58a656e2a1b162cfaccacc7694dc51
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894791"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Перенос из ClaimsPrincipal.Current

В проектах ASP.NET 4.x, было часто используется [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) для получения текущего с проверкой подлинности удостоверения пользователя и утверждения. В ASP.NET Core это свойство больше не устанавливаются. Код, который был в зависимости от его необходимо обновить, чтобы получить удостоверение текущего пользователя вошедшего в систему через различные средства.

## <a name="context-specific-data-instead-of-static-data"></a>Контекстные данные, а не статических данных

При использовании ASP.NET Core, значения `ClaimsPrincipal.Current` и `Thread.CurrentPrincipal` не установлены. Оба эти свойства представляют статическое состояние, что ASP.NET Core обычно позволяет избежать. Вместо этого архитектура ASP.NET Core — получение зависимостей (например, удостоверение текущего пользователя) из коллекции контекстные службы (с помощью его [внедрения зависимостей](xref:fundamentals/dependency-injection) модели (DI)). Более, `Thread.CurrentPrincipal` статичен, поток, поэтому не может сохраняться изменения в некоторые асинхронные сценарии (и `ClaimsPrincipal.Current` просто вызывает `Thread.CurrentPrincipal` по умолчанию).

Чтобы понять виды проблем поток статических членов может привести к в асинхронных сценариях, используйте следующий фрагмент кода:

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

Указанный выше код задает образец `Thread.CurrentPrincipal` и проверяет его значение до и после ожидает асинхронный вызов. `Thread.CurrentPrincipal` относится только к *поток* на котором оно задано, и метод вероятнее всего возобновить выполнение в другом потоке после оператора await. Следовательно `Thread.CurrentPrincipal` присутствует, если она сначала проверяется, но имеет значение null после вызова `await Task.Yield()`.

Получение удостоверение текущего пользователя из приложения внедрения Зависимостей службы коллекции слишком более тестируем, так как удостоверения теста можно легко внедрить.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Получить текущего пользователя в приложении ASP.NET Core

Существует несколько вариантов для извлечения текущего прошедшего проверку подлинности пользователя `ClaimsPrincipal` в ASP.NET Core вместо `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Контроллеры MVC можно получить доступ к текущего пользователя, прошедшего проверку подлинности с их [пользователя](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) свойство.
* **HttpContext.User**. Компоненты с доступом к текущему `HttpContext` (например, по промежуточного слоя) можно получить текущий пользователь `ClaimsPrincipal` из [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Переданный из вызывающего объекта**. Библиотеки без доступа к текущему `HttpContext` часто вызываются из контроллеров или компоненты промежуточного слоя и может иметь удостоверение текущего пользователя, передается в качестве аргумента.
* **IHttpContextAccessor**. Проект, переносе в ASP.NET Core может быть слишком большим, легко передавать удостоверение текущего пользователя все необходимые расположения. В таких случаях [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) можно использовать в качестве обходного решения. `IHttpContextAccessor` может получить доступ к текущим `HttpContext` (если он существует). Это временное решение, чтобы получение удостоверение текущего пользователя в коде, который еще не были обновлены для работы с ASP.NET Core DI-управляемая архитектура будет следующим:

  * Сделать `IHttpContextAccessor` в контейнер с внедрением Зависимостей путем вызова [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) в `Startup.ConfigureServices`.
  * Получить экземпляр `IHttpContextAccessor` во время запуска и сохранить ее в статической переменной. Экземпляр становится доступным для кода, который ранее получение текущего пользователя из статического свойства.
  * Получить текущего пользователя `ClaimsPrincipal` с помощью `HttpContextAccessor.HttpContext?.User`. Если этот код используется вне контекста HTTP-запроса, `HttpContext` имеет значение null.

Последний параметр, с помощью `IHttpContextAccessor` экземпляр хранится в статической переменной, противоположные принципам ASP.NET Core (предпочитая внедренного зависимости статических зависимостей). Планируете со временем получения `IHttpContextAccessor` вместо экземпляров из внедрения зависимостей. Статические вспомогательные может быть полезно моста, однако при переносе большого существующих приложений ASP.NET, которые были ранее с помощью `ClaimsPrincipal.Current`.
