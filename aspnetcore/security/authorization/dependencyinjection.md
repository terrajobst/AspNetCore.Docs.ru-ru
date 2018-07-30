---
title: Внедрение зависимостей в обработчики требований в ASP.NET Core
author: rick-anderson
description: Узнайте, как внедрить обработчики требований авторизации в приложении ASP.NET Core с помощью внедрения зависимости.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342118"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="932cc-103">Внедрение зависимостей в обработчики требований в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="932cc-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="932cc-104">[Необходимо зарегистрировать обработчики авторизации](xref:security/authorization/policies#handler-registration) в коллекции службы во время настройки (с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="932cc-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="932cc-105">Предположим, что у вас есть репозиторий правил, которые вы хотите оценить внутри обработчика авторизации, и этот репозиторий был зарегистрирован в коллекции службы.</span><span class="sxs-lookup"><span data-stu-id="932cc-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="932cc-106">Авторизация будет устранить и вставим ее в конструктор.</span><span class="sxs-lookup"><span data-stu-id="932cc-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="932cc-107">Например, если вы хотите использовать ASP. NET ведение журналов инфраструктуры, нужно внедрить `ILoggerFactory` в ваш обработчик.</span><span class="sxs-lookup"><span data-stu-id="932cc-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="932cc-108">Такой обработчик может выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="932cc-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="932cc-109">Зарегистрировать обработчик с `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="932cc-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="932cc-110">Экземпляр обработчика будет создаваться при запуске приложения, а также будет DI внедрить зарегистрированный `ILoggerFactory` в конструктор.</span><span class="sxs-lookup"><span data-stu-id="932cc-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="932cc-111">Обработчики, использующие Entity Framework не должен быть зарегистрирован как одноэлементных экземпляров.</span><span class="sxs-lookup"><span data-stu-id="932cc-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
