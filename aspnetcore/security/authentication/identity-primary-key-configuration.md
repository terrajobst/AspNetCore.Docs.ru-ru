---
title: "Настройте тип данных удостоверения первичные ключи"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="864ea-102">Настройте тип данных удостоверения первичные ключи</span><span class="sxs-lookup"><span data-stu-id="864ea-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="864ea-103">Удостоверение ASP.NET Core позволяет легко настроить тип данных для первичных ключей.</span><span class="sxs-lookup"><span data-stu-id="864ea-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="864ea-104">По умолчанию использует удостоверение строковый тип данных, но очень быстро, можно переопределить это поведение.</span><span class="sxs-lookup"><span data-stu-id="864ea-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="864ea-105">Практическое руководство</span><span class="sxs-lookup"><span data-stu-id="864ea-105">How to</span></span>

1.  <span data-ttu-id="864ea-106">Первым шагом является реализация модели удостоверения и переопределить тип string с нужного типа данных.</span><span class="sxs-lookup"><span data-stu-id="864ea-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  <span data-ttu-id="864ea-107">Реализовать контекст базы данных удостоверений с помощью моделей и типа данных для первичных ключей</span><span class="sxs-lookup"><span data-stu-id="864ea-107">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  <span data-ttu-id="864ea-108">Использование моделей и типа данных, необходимые для первичных ключей при объявлении удостоверения службы в классе при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="864ea-108">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
