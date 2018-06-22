---
title: Общие сведения об авторизации в ASP.NET Core
author: rick-anderson
description: Изучение основ авторизации и принцип действия авторизации в приложениях ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276871"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="b817c-103">Общие сведения об авторизации в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b817c-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="b817c-104">Авторизация — это процесс, определяющее, какое может выполнять пользователь.</span><span class="sxs-lookup"><span data-stu-id="b817c-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="b817c-105">Например пользователь с правами администратора может создать библиотеку документов, добавьте документы, редактирования документов и их удаления.</span><span class="sxs-lookup"><span data-stu-id="b817c-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="b817c-106">Пользователь без прав администратора работы с библиотекой только авторизованные читать документы.</span><span class="sxs-lookup"><span data-stu-id="b817c-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="b817c-107">Авторизация — ортогональных и независимо от проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b817c-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="b817c-108">Однако авторизации требуется механизм проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b817c-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="b817c-109">Проверка подлинности — это процесс проверка личности пользователя.</span><span class="sxs-lookup"><span data-stu-id="b817c-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="b817c-110">Проверка подлинности может создать один или несколько идентификаторов для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="b817c-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="b817c-111">Типы проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="b817c-111">Authorization types</span></span>

<span data-ttu-id="b817c-112">Авторизации ASP.NET Core предоставляет простой, декларативный [роли](xref:security/authorization/roles) и форматированного [на основе политики](xref:security/authorization/policies) модели.</span><span class="sxs-lookup"><span data-stu-id="b817c-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="b817c-113">Авторизации представляется в формате требований и обработчики оценки утверждения пользователя соответствие требованиям.</span><span class="sxs-lookup"><span data-stu-id="b817c-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="b817c-114">В простых политик или политик, что оценка удостоверение пользователя и свойства ресурса, который пользователь пытается получить доступ к может основываться принудительных проверок.</span><span class="sxs-lookup"><span data-stu-id="b817c-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="b817c-115">Пространства имен</span><span class="sxs-lookup"><span data-stu-id="b817c-115">Namespaces</span></span>

<span data-ttu-id="b817c-116">Компоненты авторизации, включая `AuthorizeAttribute` и `AllowAnonymousAttribute` атрибуты, найдены в `Microsoft.AspNetCore.Authorization` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="b817c-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="b817c-117">Обратитесь к документации на [простой авторизации](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="b817c-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
