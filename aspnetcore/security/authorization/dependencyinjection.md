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
# <a name="dependency-injection-in-requirement-handlers"></a>Внедрение зависимостей в обработчиках требование

<a name="security-authorization-di"></a>

[Необходимо зарегистрировать обработчики авторизации](policies.md#handler-registration) в коллекцию службы во время настройки (с помощью [внедрения зависимостей](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).

Предположим, что у вас есть репозитория правил, которые вы хотите оценить внутри обработчика авторизации и репозитория зарегистрирован в службе сбора. Авторизация будет решения и вставляют, на ваш конструктор.

Например, если вы хотели использовать ASP. NET ведение журналов инфраструктуры, необходимо ввести `ILoggerFactory` в обработчике. Такой обработчик может иметь вид:

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

Экземпляр обработчика будет создаваться при запуске приложения, а также будет DI внедрить зарегистрированного `ILoggerFactory` в конструктор.

> [!NOTE]
> Обработчики, использующие Entity Framework не должен быть зарегистрирован как одноэлементных кортежей.
