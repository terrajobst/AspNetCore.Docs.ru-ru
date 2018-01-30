---
title: "Внедрение зависимостей в обработчиках требование"
author: rick-anderson
description: "В этом документе описано, как для добавления обработчиков требования авторизации в приложение ASP.NET Core с помощью внедрения зависимости."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 1b7506b49109264a8c628ea2e39ded9f5ace95d3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
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
