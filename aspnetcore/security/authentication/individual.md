---
title: Статьи, на основе ASP.NET Core проектов, созданных с помощью отдельных учетных записей пользователей
author: rick-anderson
description: Определения статей, основанных на ASP.NET Core проекты, созданные с помощью отдельных учетных записей пользователей.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 699def0133f53b922477ac294f70db41998945ef
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729554"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="ac925-103">Статьи, на основе ASP.NET Core проектов, созданных с помощью отдельных учетных записей пользователей</span><span class="sxs-lookup"><span data-stu-id="ac925-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="ac925-104">Удостоверение ASP.NET Core включается в шаблонах проектов в Visual Studio с помощью параметра «Отдельных учетных записей пользователей».</span><span class="sxs-lookup"><span data-stu-id="ac925-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="ac925-105">Проверка подлинности шаблоны доступны в .NET Core CLI с `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="ac925-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="ac925-106">Следующие статьи показано, как использовать код, созданный в шаблонах ASP.NET Core, используйте отдельные учетные записи:</span><span class="sxs-lookup"><span data-stu-id="ac925-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="ac925-107">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="ac925-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="ac925-108">Подтверждение учетной записи и восстановление пароля в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ac925-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ac925-109">Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации</span><span class="sxs-lookup"><span data-stu-id="ac925-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
