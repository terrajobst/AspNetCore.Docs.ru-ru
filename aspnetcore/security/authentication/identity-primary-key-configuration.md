---
title: "Настройте тип данных удостоверения первичные ключи"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>Настройте тип данных удостоверения первичные ключи

Удостоверение ASP.NET Core позволяет легко настроить тип данных для первичных ключей. По умолчанию использует удостоверение строковый тип данных, но очень быстро, можно переопределить это поведение.

## <a name="how-to"></a>Практическое руководство

1.  Первым шагом является реализация модели удостоверения и переопределить тип string с нужного типа данных.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  Реализовать контекст базы данных удостоверений с помощью моделей и типа данных для первичных ключей

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  Использование моделей и типа данных, необходимые для первичных ключей при объявлении удостоверения службы в классе при запуске приложения

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
