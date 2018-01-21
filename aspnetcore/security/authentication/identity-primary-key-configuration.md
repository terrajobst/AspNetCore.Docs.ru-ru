---
title: "Настройте тип данных первичного ключа удостоверения"
author: AdrienTorris
description: "В этой статье описаны шаги по настройке в нужный тип данных используется для ASP.NET Core Identity первичного ключа."
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: da46aa90a434a978a55467da982d746eb2d959ee
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a>Настройка типа данных первичного ключа ASP.NET Core Identity

ASP.NET Core удостоверений можно настроить тип данных, используемый для представления первичного ключа. Использует удостоверение `string` тип данных по умолчанию. Это поведение можно переопределить.

## <a name="customize-the-primary-key-data-type"></a>Настроить тип данных первичного ключа

1. Создать пользовательскую реализацию [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) класса. Представляет тип, используемый для создания пользовательских объектов. В следующем примере значение по умолчанию `string` заменяется типом `Guid`.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. Создать пользовательскую реализацию [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) класса. Представляет тип, используемый для создания объектов роли. В следующем примере значение по умолчанию `string` заменяется типом `Guid`.
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. Создайте класс контекста пользовательской базе данных. Он наследует от класса контекста базы данных Entity Framework, используемый для удостоверения. `TUser` И `TRole` аргументы ссылаться на пользовательские классы пользователя и роли, созданные на предыдущем шаге, соответственно. `Guid` Для первичного ключа определен тип данных.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. Зарегистрируйте класс контекста пользовательской базе данных, при добавлении удостоверения службы в классе при запуске приложения.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    `AddEntityFrameworkStores` Метод не принимает `TKey` аргумент, как это требовалось в ASP.NET Core 1.x. Тип данных первичный ключ определяется путем анализа `DbContext` объекта.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    `AddEntityFrameworkStores` Метод принимает `TKey` аргумент, указывающий тип данных первичный ключ.
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>Тестирование изменений

После завершения изменений конфигурации свойства, представляющего первичный ключ отражает новый тип данных. В следующем примере показано обращение к свойству в контроллер MVC.

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
