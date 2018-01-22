---
title: "Внедрение зависимостей в обработчиках требование"
author: rick-anderson
description: "В этом документе описано, как для добавления обработчиков требования авторизации в приложение ASP.NET Core с помощью внедрения зависимости."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 9aaa356fe67a7e2c8177ffa1b886ec6b3dc13ef0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="cc69a-103">Внедрение зависимостей в обработчиках требование</span><span class="sxs-lookup"><span data-stu-id="cc69a-103">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="cc69a-104">[Необходимо зарегистрировать обработчики авторизации](policies.md#handler-registration) в коллекцию службы во время настройки (с помощью [внедрения зависимостей](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="cc69a-104">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="cc69a-105">Предположим, что у вас есть репозитория правил, которые вы хотите оценить внутри обработчика авторизации и репозитория зарегистрирован в службе сбора.</span><span class="sxs-lookup"><span data-stu-id="cc69a-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="cc69a-106">Авторизация будет решения и вставляют, на ваш конструктор.</span><span class="sxs-lookup"><span data-stu-id="cc69a-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="cc69a-107">Например, если вы хотели использовать ASP. NET ведение журналов инфраструктуры, необходимо ввести `ILoggerFactory` в обработчике.</span><span class="sxs-lookup"><span data-stu-id="cc69a-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="cc69a-108">Такой обработчик может иметь вид:</span><span class="sxs-lookup"><span data-stu-id="cc69a-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="cc69a-109">Зарегистрировать обработчик с `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="cc69a-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="cc69a-110">Экземпляр обработчика будет создаваться при запуске приложения, а также будет DI внедрить зарегистрированного `ILoggerFactory` в конструктор.</span><span class="sxs-lookup"><span data-stu-id="cc69a-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="cc69a-111">Обработчики, использующие Entity Framework не должен быть зарегистрирован как одноэлементных кортежей.</span><span class="sxs-lookup"><span data-stu-id="cc69a-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
