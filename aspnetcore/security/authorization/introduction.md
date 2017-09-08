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
ms.openlocfilehash: 040525505a982fc1be1901effb9186a8fe1cdbdf
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="introduction"></a>Вступление

<a name=security-authorization-introduction></a>

Авторизация — это процесс, определяющее, какое может выполнять пользователь. Например пользователь с правами администратора может создать библиотеку документов, добавьте документы, редактирования документов и их удаления. Пользователь без прав администратора работы с библиотекой только авторизованные читать документы.

Авторизация — ортогональных и независимо от проверки подлинности — это процесс проверка личности пользователя. Проверка подлинности может создать один или несколько идентификаторов для текущего пользователя.

## <a name="authorization-types"></a>Типы проверки подлинности

Авторизации ASP.NET Core предоставляет декларативный простой [роли](roles.md#security-authorization-role-based) и [широкие возможности на основе политики](policies.md#security-authorization-policies-based) модели. Авторизации представляется в формате требований и обработчики оценки утверждения пользователя соответствие требованиям. В простых политик или политик, что оценка удостоверение пользователя и свойства ресурса, который пользователь пытается получить доступ к может основываться принудительных проверок.

## <a name="namespaces"></a>Пространства имен

Компоненты авторизации, включая `AuthorizeAttribute` и `AllowAnonymousAttribute` атрибуты найдены в `Microsoft.AspNetCore.Authorization` пространства имен.
