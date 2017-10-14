---
title: "Внедрение зависимостей в обработчиках требование"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 308951a45ee6576f096e1cdc792208b89e476e61
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="c65d4-103">Внедрение зависимостей в обработчиках требование</span><span class="sxs-lookup"><span data-stu-id="c65d4-103">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="c65d4-104">[Необходимо зарегистрировать обработчики авторизации](policies.md#security-authorization-policies-based-handler-registration) в коллекцию службы во время настройки (с помощью [внедрения зависимостей](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="c65d4-104">[Authorization handlers must be registered](policies.md#security-authorization-policies-based-handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="c65d4-105">Предположим, что у вас есть репозитория правил, которые вы хотите оценить внутри обработчика авторизации и репозитория зарегистрирован в службе сбора.</span><span class="sxs-lookup"><span data-stu-id="c65d4-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span>  <span data-ttu-id="c65d4-106">Авторизация будет решения и вставляют, на ваш конструктор.</span><span class="sxs-lookup"><span data-stu-id="c65d4-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="c65d4-107">Например, если вы хотели использовать ASP. NET ведение журналов инфраструктуры, необходимо ввести `ILoggerFactory` в обработчике.</span><span class="sxs-lookup"><span data-stu-id="c65d4-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="c65d4-108">Такой обработчик может иметь вид:</span><span class="sxs-lookup"><span data-stu-id="c65d4-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="c65d4-109">Зарегистрировать обработчик с `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="c65d4-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
   ```

<span data-ttu-id="c65d4-110">Экземпляр обработчика будет создаваться при запуске приложения, а также будет DI внедрить зарегистрированного `ILoggerFactory` в конструктор.</span><span class="sxs-lookup"><span data-stu-id="c65d4-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="c65d4-111">Обработчики, использующие Entity Framework не должен быть зарегистрирован как одноэлементных кортежей.</span><span class="sxs-lookup"><span data-stu-id="c65d4-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
