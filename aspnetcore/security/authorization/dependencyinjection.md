---
title: Внедрение зависимостей в обработчиках требование в ASP.NET Core
author: rick-anderson
description: Узнайте, как внедрить обработчики требование авторизации в приложение ASP.NET Core с помощью внедрения зависимости.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: c6bb2589c6fef9f4586e6f4ddbb574866e6c48ab
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273726"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Внедрение зависимостей в обработчиках требование в ASP.NET Core

<a name="security-authorization-di"></a>

[Необходимо зарегистрировать обработчики авторизации](xref:security/authorization/policies#handler-registration) в коллекцию службы во время настройки (с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).

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
