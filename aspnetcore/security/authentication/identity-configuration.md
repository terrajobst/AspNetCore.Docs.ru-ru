---
title: "Настройка удостоверения"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a>Настройка удостоверения

Удостоверение ASP.NET Core имеет некоторые виды поведения по умолчанию, которые можно легко переопределить в классе при запуске приложения.

## <a name="passwords-policy"></a>Политики паролей

По умолчанию удостоверение требует, что пароли содержать прописные буквы, строчные буквы и цифры. Существуют также некоторые другие ограничения. Если вы хотите упростить ограничения для пароля, можно сделать это в класс запуска приложения.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a>Параметры файлов cookie приложения

Политика паролей, как и все параметры файла cookie приложения может изменяться в класс startup.

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a>Блокировки пользователя

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
