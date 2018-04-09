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
ms.openlocfilehash: 7ba1966dcaf1ce0510f489cfe0ff11501faffa56
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="22224-103">Общие сведения об авторизации в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22224-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="22224-104">Авторизация — это процесс, определяющее, какое может выполнять пользователь.</span><span class="sxs-lookup"><span data-stu-id="22224-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="22224-105">Например пользователь с правами администратора может создать библиотеку документов, добавьте документы, редактирования документов и их удаления.</span><span class="sxs-lookup"><span data-stu-id="22224-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="22224-106">Пользователь без прав администратора работы с библиотекой только авторизованные читать документы.</span><span class="sxs-lookup"><span data-stu-id="22224-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="22224-107">Авторизация — ортогональных и независимо от проверки подлинности — это процесс проверка личности пользователя.</span><span class="sxs-lookup"><span data-stu-id="22224-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="22224-108">Проверка подлинности может создать один или несколько идентификаторов для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="22224-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="22224-109">Типы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="22224-109">Authorization types</span></span>

<span data-ttu-id="22224-110">Авторизации ASP.NET Core предоставляет простой, декларативный [роли](xref:security/authorization/roles) и форматированного [на основе политики](xref:security/authorization/policies) модели.</span><span class="sxs-lookup"><span data-stu-id="22224-110">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="22224-111">Авторизации представляется в формате требований и обработчики оценки утверждения пользователя соответствие требованиям.</span><span class="sxs-lookup"><span data-stu-id="22224-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="22224-112">В простых политик или политик, что оценка удостоверение пользователя и свойства ресурса, который пользователь пытается получить доступ к может основываться принудительных проверок.</span><span class="sxs-lookup"><span data-stu-id="22224-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="22224-113">Пространства имен</span><span class="sxs-lookup"><span data-stu-id="22224-113">Namespaces</span></span>

<span data-ttu-id="22224-114">Компоненты авторизации, включая `AuthorizeAttribute` и `AllowAnonymousAttribute` атрибуты, найдены в `Microsoft.AspNetCore.Authorization` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="22224-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="22224-115">Обратитесь к документации на [простой авторизации](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="22224-115">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
