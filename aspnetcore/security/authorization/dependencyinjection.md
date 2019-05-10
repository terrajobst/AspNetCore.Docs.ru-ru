---
title: Внедрение зависимостей в обработчики требований в ASP.NET Core
author: rick-anderson
description: Узнайте, как внедрить обработчики требований авторизации в приложении ASP.NET Core с помощью внедрения зависимости.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896371"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Внедрение зависимостей в обработчики требований в ASP.NET Core

<a name="security-authorization-di"></a>

[Необходимо зарегистрировать обработчики авторизации](xref:security/authorization/policies#handler-registration) в коллекции службы во время настройки (с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection)).

Предположим, что у вас есть репозиторий правил, которые вы хотите оценить внутри обработчика авторизации, и этот репозиторий был зарегистрирован в коллекции службы. Авторизация будет устранить и вставим ее в конструктор.

Например, если вы хотите использовать ASP. NET ведение журналов инфраструктуры, нужно внедрить `ILoggerFactory` в ваш обработчик. Такой обработчик может выглядеть так:

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

Зарегистрировать обработчик с `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Экземпляр обработчика будет создаваться при запуске приложения, а также будет DI внедрить зарегистрированный `ILoggerFactory` в конструктор.

> [!NOTE]
> Обработчики, использующие Entity Framework не должен быть зарегистрирован как одноэлементных экземпляров.
