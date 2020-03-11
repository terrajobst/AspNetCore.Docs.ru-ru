---
title: Внедрение зависимостей в обработчики требований в ASP.NET Core
author: rick-anderson
description: Узнайте, как внедрять обработчики требований авторизации в приложение ASP.NET Core с помощью внедрения зависимостей.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654364"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="0c3c4-103">Внедрение зависимостей в обработчики требований в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c3c4-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="0c3c4-104">[Обработчики авторизации должны быть зарегистрированы](xref:security/authorization/policies#handler-registration) в коллекции служб во время настройки (с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="0c3c4-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="0c3c4-105">Предположим, у вас есть репозиторий правил, которые необходимо вычислить в обработчике авторизации, и этот репозиторий был зарегистрирован в коллекции служб.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="0c3c4-106">Авторизация будет разрешаться и внедряться в конструктор.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="0c3c4-107">Например, если вы хотите использовать технологию ASP. СЕТЕВая Инфраструктура ведения журнала, которую необходимо внедрить `ILoggerFactory` в обработчик.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="0c3c4-108">Такой обработчик может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0c3c4-108">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="0c3c4-109">Обработчик можно зарегистрировать с помощью `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="0c3c4-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="0c3c4-110">Экземпляр обработчика будет создан при запуске приложения, а DI будет внедрять зарегистрированный `ILoggerFactory` в конструктор.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="0c3c4-111">Обработчики, использующие Entity Framework, не должны регистрироваться как singleton.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
