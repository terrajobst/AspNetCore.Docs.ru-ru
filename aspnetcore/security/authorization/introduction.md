---
title: Общие сведения об авторизации в ASP.NET Core
author: rick-anderson
description: Изучение основ авторизации и принцип действия авторизации в приложениях ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Общие сведения об авторизации в ASP.NET Core

<a name="security-authorization-introduction"></a>

Авторизация — это процесс, определяющее, какое может выполнять пользователь. Например пользователь с правами администратора может создать библиотеку документов, добавьте документы, редактирования документов и их удаления. Пользователь без прав администратора работы с библиотекой только авторизованные читать документы.

Авторизация — ортогональных и независимо от проверки подлинности. Однако авторизации требуется механизм проверки подлинности. Проверка подлинности — это процесс проверка личности пользователя. Проверка подлинности может создать один или несколько идентификаторов для текущего пользователя.

## <a name="authorization-types"></a>Типы проверки подлинности

Авторизации ASP.NET Core предоставляет простой, декларативный [роли](xref:security/authorization/roles) и форматированного [на основе политики](xref:security/authorization/policies) модели. Авторизации представляется в формате требований и обработчики оценки утверждения пользователя соответствие требованиям. В простых политик или политик, что оценка удостоверение пользователя и свойства ресурса, который пользователь пытается получить доступ к может основываться принудительных проверок.

## <a name="namespaces"></a>Пространства имен

Компоненты авторизации, включая `AuthorizeAttribute` и `AllowAnonymousAttribute` атрибуты, найдены в `Microsoft.AspNetCore.Authorization` пространства имен.

Обратитесь к документации на [простой авторизации](xref:security/authorization/simple).
