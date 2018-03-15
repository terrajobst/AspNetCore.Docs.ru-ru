---
title: "Настроить тип данных первичного ключа удостоверения в ASP.NET Core"
author: AdrienTorris
description: "Дополнительные сведения о действиях по настройке в нужный тип данных используется для ASP.NET Core Identity первичного ключа."
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 02482b81faa64b01765a90c2c6ffe9cf92b1a7e7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a>Настроить тип данных первичного ключа удостоверения в ASP.NET Core

ASP.NET Core удостоверений можно настроить тип данных, используемый для представления первичного ключа. Использует удостоверение `string` тип данных по умолчанию. Это поведение можно переопределить.

## <a name="customize-the-primary-key-data-type"></a>Настроить тип данных первичного ключа

1. Создать пользовательскую реализацию [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) класса. Представляет тип, используемый для создания пользовательских объектов. В следующем примере значение по умолчанию `string` заменяется типом `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. Создать пользовательскую реализацию [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) класса. Представляет тип, используемый для создания объектов роли. В следующем примере значение по умолчанию `string` заменяется типом `Guid`.
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. Создайте класс контекста пользовательской базе данных. Он наследует от класса контекста базы данных Entity Framework, используемый для удостоверения. `TUser` И `TRole` аргументы ссылаться на пользовательские классы пользователя и роли, созданные на предыдущем шаге, соответственно. `Guid` Для первичного ключа определен тип данных.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. Зарегистрируйте класс контекста пользовательской базе данных, при добавлении удостоверения службы в классе при запуске приложения.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    `AddEntityFrameworkStores` Метод не принимает `TKey` аргумент, как это требовалось в ASP.NET Core 1.x. Тип данных первичный ключ определяется путем анализа `DbContext` объекта.
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    `AddEntityFrameworkStores` Метод принимает `TKey` аргумент, указывающий тип данных первичный ключ.
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>Тестирование изменений

После завершения изменений конфигурации свойства, представляющего первичный ключ отражает новый тип данных. В следующем примере показано обращение к свойству в контроллер MVC.

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
