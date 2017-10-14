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
# <a name="introduction"></a><span data-ttu-id="f3d12-103">Вступление</span><span class="sxs-lookup"><span data-stu-id="f3d12-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="f3d12-104">Авторизация — это процесс, определяющее, какое может выполнять пользователь.</span><span class="sxs-lookup"><span data-stu-id="f3d12-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="f3d12-105">Например пользователь с правами администратора может создать библиотеку документов, добавьте документы, редактирования документов и их удаления.</span><span class="sxs-lookup"><span data-stu-id="f3d12-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="f3d12-106">Пользователь без прав администратора работы с библиотекой только авторизованные читать документы.</span><span class="sxs-lookup"><span data-stu-id="f3d12-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="f3d12-107">Авторизация — ортогональных и независимо от проверки подлинности — это процесс проверка личности пользователя.</span><span class="sxs-lookup"><span data-stu-id="f3d12-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="f3d12-108">Проверка подлинности может создать один или несколько идентификаторов для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="f3d12-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="f3d12-109">Типы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="f3d12-109">Authorization Types</span></span>

<span data-ttu-id="f3d12-110">Авторизации ASP.NET Core предоставляет декларативный простой [роли](roles.md#security-authorization-role-based) и [широкие возможности на основе политики](policies.md#security-authorization-policies-based) модели.</span><span class="sxs-lookup"><span data-stu-id="f3d12-110">ASP.NET Core authorization provides a simple declarative [role](roles.md#security-authorization-role-based) and a [rich policy based](policies.md#security-authorization-policies-based) model.</span></span> <span data-ttu-id="f3d12-111">Авторизации представляется в формате требований и обработчики оценки утверждения пользователя соответствие требованиям.</span><span class="sxs-lookup"><span data-stu-id="f3d12-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="f3d12-112">В простых политик или политик, что оценка удостоверение пользователя и свойства ресурса, который пользователь пытается получить доступ к может основываться принудительных проверок.</span><span class="sxs-lookup"><span data-stu-id="f3d12-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="f3d12-113">Пространства имен</span><span class="sxs-lookup"><span data-stu-id="f3d12-113">Namespaces</span></span>

<span data-ttu-id="f3d12-114">Компоненты авторизации, включая `AuthorizeAttribute` и `AllowAnonymousAttribute` атрибуты найдены в `Microsoft.AspNetCore.Authorization` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="f3d12-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
