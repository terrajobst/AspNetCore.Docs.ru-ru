---
title: "Настройте тип данных удостоверения первичные ключи"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>Настройте тип данных удостоверения первичные ключи

Удостоверение ASP.NET Core позволяет легко настроить тип данных для первичных ключей. По умолчанию использует удостоверение строковый тип данных, но очень быстро, можно переопределить это поведение.

## <a name="how-to"></a>Практическое руководство

1.  Первым шагом является реализация модели удостоверения и переопределить тип string с нужного типа данных.

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  Реализовать контекст базы данных удостоверений с помощью моделей и типа данных для первичных ключей

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  Использование моделей и типа данных, необходимые для первичных ключей при объявлении удостоверения службы в классе при запуске приложения

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
