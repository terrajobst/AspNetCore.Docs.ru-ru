---
title: "Вступление"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: fa6dcbc0627181cd1aca0926fa008f3db907742f
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="introduction"></a>Вступление

<a name="security-authorization-introduction"></a>

Авторизация — это процесс, определяющее, какое может выполнять пользователь. Например пользователь с правами администратора может создать библиотеку документов, добавьте документы, редактирования документов и их удаления. Пользователь без прав администратора работы с библиотекой только авторизованные читать документы.

Авторизация — ортогональных и независимо от проверки подлинности — это процесс проверка личности пользователя. Проверка подлинности может создать один или несколько идентификаторов для текущего пользователя.

## <a name="authorization-types"></a>Типы проверки подлинности

Авторизации ASP.NET Core предоставляет декларативный простой [роли](roles.md#security-authorization-role-based) и [широкие возможности на основе политики](policies.md#security-authorization-policies-based) модели. Авторизации представляется в формате требований и обработчики оценки утверждения пользователя соответствие требованиям. В простых политик или политик, что оценка удостоверение пользователя и свойства ресурса, который пользователь пытается получить доступ к может основываться принудительных проверок.

## <a name="namespaces"></a>Пространства имен

Компоненты авторизации, включая `AuthorizeAttribute` и `AllowAnonymousAttribute` атрибуты найдены в `Microsoft.AspNetCore.Authorization` пространства имен.
