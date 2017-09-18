---
title: "Настройка удостоверения ASP.NET Core"
author: AdrienTorris
description: "Значения по умолчанию ASP.NET Core Identity понять и настройки различных свойств Identity для использования пользовательских значений."
keywords: "Проверку подлинности ASP.NET Core, удостоверение,"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 629fcc2941b2d2fda9604a3eac04be3d0f5294b2
ms.sourcegitcommit: ddefc78270bd9b5ae0b1bd8de6c45f6977e7dceb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2017
---
# <a name="configure-identity"></a><span data-ttu-id="dfe76-104">Настройка удостоверения</span><span class="sxs-lookup"><span data-stu-id="dfe76-104">Configure Identity</span></span>

<span data-ttu-id="dfe76-105">Удостоверение ASP.NET Core имеет некоторые виды поведения по умолчанию, которые можно легко переопределить в классе при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="dfe76-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="dfe76-106">Политики паролей</span><span class="sxs-lookup"><span data-stu-id="dfe76-106">Passwords policy</span></span>

<span data-ttu-id="dfe76-107">По умолчанию удостоверение требует, что пароли содержать прописные буквы, строчные буквы и цифры.</span><span class="sxs-lookup"><span data-stu-id="dfe76-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="dfe76-108">Существуют также некоторые другие ограничения.</span><span class="sxs-lookup"><span data-stu-id="dfe76-108">There are also some other restrictions.</span></span> <span data-ttu-id="dfe76-109">Если вы хотите упростить ограничения для пароля, можно сделать это в класс запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="dfe76-109">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a><span data-ttu-id="dfe76-110">Параметры файлов cookie приложения</span><span class="sxs-lookup"><span data-stu-id="dfe76-110">Application's cookie settings</span></span>

<span data-ttu-id="dfe76-111">Политика паролей, как и все параметры файла cookie приложения может изменяться в класс startup.</span><span class="sxs-lookup"><span data-stu-id="dfe76-111">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a><span data-ttu-id="dfe76-112">Блокировки пользователя</span><span class="sxs-lookup"><span data-stu-id="dfe76-112">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
