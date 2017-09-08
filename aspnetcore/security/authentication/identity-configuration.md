---
title: "Настройка удостоверения"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a><span data-ttu-id="6ff90-102">Настройка удостоверения</span><span class="sxs-lookup"><span data-stu-id="6ff90-102">Configure Identity</span></span>

<span data-ttu-id="6ff90-103">Удостоверение ASP.NET Core имеет некоторые виды поведения по умолчанию, которые можно легко переопределить в классе при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="6ff90-103">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="6ff90-104">Политики паролей</span><span class="sxs-lookup"><span data-stu-id="6ff90-104">Passwords policy</span></span>

<span data-ttu-id="6ff90-105">По умолчанию удостоверение требует, что пароли содержать прописные буквы, строчные буквы и цифры.</span><span class="sxs-lookup"><span data-stu-id="6ff90-105">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="6ff90-106">Существуют также некоторые другие ограничения.</span><span class="sxs-lookup"><span data-stu-id="6ff90-106">There are also some other restrictions.</span></span> <span data-ttu-id="6ff90-107">Если вы хотите упростить ограничения для пароля, можно сделать это в класс запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="6ff90-107">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

<span data-ttu-id="6ff90-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span><span class="sxs-lookup"><span data-stu-id="6ff90-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="6ff90-109">Параметры файлов cookie приложения</span><span class="sxs-lookup"><span data-stu-id="6ff90-109">Application's cookie settings</span></span>

<span data-ttu-id="6ff90-110">Политика паролей, как и все параметры файла cookie приложения может изменяться в класс startup.</span><span class="sxs-lookup"><span data-stu-id="6ff90-110">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

<span data-ttu-id="6ff90-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span><span class="sxs-lookup"><span data-stu-id="6ff90-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span></span>

## <a name="users-lockout"></a><span data-ttu-id="6ff90-112">Блокировки пользователя</span><span class="sxs-lookup"><span data-stu-id="6ff90-112">User's lockout</span></span>

<span data-ttu-id="6ff90-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span><span class="sxs-lookup"><span data-stu-id="6ff90-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span></span>
