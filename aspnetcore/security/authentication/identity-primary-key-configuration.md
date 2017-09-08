---
title: "Настройте тип данных удостоверения первичные ключи"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="a3791-102">Настройте тип данных удостоверения первичные ключи</span><span class="sxs-lookup"><span data-stu-id="a3791-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="a3791-103">Удостоверение ASP.NET Core позволяет легко настроить тип данных для первичных ключей.</span><span class="sxs-lookup"><span data-stu-id="a3791-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="a3791-104">По умолчанию использует удостоверение строковый тип данных, но очень быстро, можно переопределить это поведение.</span><span class="sxs-lookup"><span data-stu-id="a3791-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="a3791-105">Практическое руководство</span><span class="sxs-lookup"><span data-stu-id="a3791-105">How to</span></span>

1.  <span data-ttu-id="a3791-106">Первым шагом является реализация модели удостоверения и переопределить тип string с нужного типа данных.</span><span class="sxs-lookup"><span data-stu-id="a3791-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    <span data-ttu-id="a3791-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span><span class="sxs-lookup"><span data-stu-id="a3791-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span></span>

    <span data-ttu-id="a3791-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span><span class="sxs-lookup"><span data-stu-id="a3791-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span></span>
    
2.  <span data-ttu-id="a3791-109">Реализовать контекст базы данных удостоверений с помощью моделей и типа данных для первичных ключей</span><span class="sxs-lookup"><span data-stu-id="a3791-109">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    <span data-ttu-id="a3791-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span><span class="sxs-lookup"><span data-stu-id="a3791-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span></span>
    
3.  <span data-ttu-id="a3791-111">Использование моделей и типа данных, необходимые для первичных ключей при объявлении удостоверения службы в классе при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="a3791-111">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    <span data-ttu-id="a3791-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span><span class="sxs-lookup"><span data-stu-id="a3791-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span></span>
