---
title: "Статьи, на основе проектов, созданных с помощью отдельных учетных записей пользователей"
author: rick-anderson
description: "В этом документе перечислены статей, основанных на проекты, созданные с помощью отдельных учетных записей пользователей."
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 844514f2970b761ec65589765eb21421cd1962a1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>Статьи, на основе проектов, созданных с помощью отдельных учетных записей пользователей

Удостоверение ASP.NET Core включается в шаблонах проектов в Visual Studio с помощью параметра «Отдельных учетных записей пользователей».

Проверка подлинности шаблоны доступны в .NET Core CLI с `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

Следующие статьи показано, как использовать код, созданный в шаблонах ASP.NET Core, используйте отдельные учетные записи:

* [Двухфакторная проверка подлинности с помощью SMS](xref:security/authentication/2fa)
* [Подтверждение учетной записи и восстановление пароля в ASP.NET Core](xref:security/authentication/accconfirm)
* [Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации](xref:security/authorization/secure-data)
