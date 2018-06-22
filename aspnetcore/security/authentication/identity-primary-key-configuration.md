---
title: Настроить тип данных первичного ключа удостоверения в ASP.NET Core
author: AdrienTorris
description: Дополнительные сведения о действиях по настройке в нужный тип данных используется для ASP.NET Core Identity первичного ключа.
ms.author: scaddie
ms.date: 09/28/2017
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: cfec91e1194556385bb884ee44cf79c1fcbbcb56
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274360"
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a><span data-ttu-id="7a5c1-103">Настроить тип данных первичного ключа удостоверения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a5c1-103">Configure Identity primary key data type in ASP.NET Core</span></span>

<span data-ttu-id="7a5c1-104">ASP.NET Core удостоверений можно настроить тип данных, используемый для представления первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="7a5c1-105">Использует удостоверение `string` тип данных по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="7a5c1-106">Это поведение можно переопределить.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="7a5c1-107">Настроить тип данных первичного ключа</span><span class="sxs-lookup"><span data-stu-id="7a5c1-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="7a5c1-108">Создать пользовательскую реализацию [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) класса.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-108">Create a custom implementation of the [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="7a5c1-109">Представляет тип, используемый для создания пользовательских объектов.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="7a5c1-110">В следующем примере значение по умолчанию `string` заменяется типом `Guid`.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. <span data-ttu-id="7a5c1-111">Создать пользовательскую реализацию [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) класса.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-111">Create a custom implementation of the [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="7a5c1-112">Представляет тип, используемый для создания объектов роли.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="7a5c1-113">В следующем примере значение по умолчанию `string` заменяется типом `Guid`.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. <span data-ttu-id="7a5c1-114">Создайте класс контекста пользовательской базе данных.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-114">Create a custom database context class.</span></span> <span data-ttu-id="7a5c1-115">Он наследует от класса контекста базы данных Entity Framework, используемый для удостоверения.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="7a5c1-116">`TUser` И `TRole` аргументы ссылаться на пользовательские классы пользователя и роли, созданные на предыдущем шаге, соответственно.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="7a5c1-117">`Guid` Для первичного ключа определен тип данных.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. <span data-ttu-id="7a5c1-118">Зарегистрируйте класс контекста пользовательской базе данных, при добавлении удостоверения службы в классе при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7a5c1-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7a5c1-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   <span data-ttu-id="7a5c1-120">`AddEntityFrameworkStores` Метод не принимает `TKey` аргумент, как это требовалось в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="7a5c1-121">Тип данных первичный ключ определяется путем анализа `DbContext` объекта.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7a5c1-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7a5c1-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   <span data-ttu-id="7a5c1-123">`AddEntityFrameworkStores` Метод принимает `TKey` аргумент, указывающий тип данных первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   ---

## <a name="test-the-changes"></a><span data-ttu-id="7a5c1-124">Тестирование изменений</span><span class="sxs-lookup"><span data-stu-id="7a5c1-124">Test the changes</span></span>

<span data-ttu-id="7a5c1-125">После завершения изменений конфигурации свойства, представляющего первичный ключ отражает новый тип данных.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="7a5c1-126">В следующем примере показано обращение к свойству в контроллер MVC.</span><span class="sxs-lookup"><span data-stu-id="7a5c1-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
