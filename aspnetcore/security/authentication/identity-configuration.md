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
