---
title: Внедрение зависимостей в обработчиках требование в ASP.NET Core
author: rick-anderson
description: Узнайте, как внедрить обработчики требование авторизации в приложение ASP.NET Core с помощью внедрения зависимости.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 4de7f0e49ade459968f8c30fbad76ce96a65815f
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="98ee8-103">Внедрение зависимостей в обработчиках требование в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98ee8-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="98ee8-104">[Необходимо зарегистрировать обработчики авторизации](xref:security/authorization/policies#handler-registration) в коллекцию службы во время настройки (с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="98ee8-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="98ee8-105">Предположим, что у вас есть репозитория правил, которые вы хотите оценить внутри обработчика авторизации и репозитория зарегистрирован в службе сбора.</span><span class="sxs-lookup"><span data-stu-id="98ee8-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="98ee8-106">Авторизация будет решения и вставляют, на ваш конструктор.</span><span class="sxs-lookup"><span data-stu-id="98ee8-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="98ee8-107">Например, если вы хотели использовать ASP. NET ведение журналов инфраструктуры, необходимо ввести `ILoggerFactory` в обработчике.</span><span class="sxs-lookup"><span data-stu-id="98ee8-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="98ee8-108">Такой обработчик может иметь вид:</span><span class="sxs-lookup"><span data-stu-id="98ee8-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="98ee8-109">Зарегистрировать обработчик с `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="98ee8-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="98ee8-110">Экземпляр обработчика будет создаваться при запуске приложения, а также будет DI внедрить зарегистрированного `ILoggerFactory` в конструктор.</span><span class="sxs-lookup"><span data-stu-id="98ee8-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="98ee8-111">Обработчики, использующие Entity Framework не должен быть зарегистрирован как одноэлементных кортежей.</span><span class="sxs-lookup"><span data-stu-id="98ee8-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
