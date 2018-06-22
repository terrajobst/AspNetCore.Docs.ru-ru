---
title: Статьи, на основе ASP.NET Core проектов, созданных с помощью отдельных учетных записей пользователей
author: rick-anderson
description: Определения статей, основанных на ASP.NET Core проекты, созданные с помощью отдельных учетных записей пользователей.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f7fb9e8cd1b5c4cc3283ddd7606a0bbd30f554d5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274431"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Статьи, на основе ASP.NET Core проектов, созданных с помощью отдельных учетных записей пользователей

Удостоверение ASP.NET Core включается в шаблонах проектов в Visual Studio с помощью параметра «Отдельных учетных записей пользователей».

Проверка подлинности шаблоны доступны в .NET Core CLI с `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Следующие статьи показано, как использовать код, созданный в шаблонах ASP.NET Core, используйте отдельные учетные записи:

* [Двухфакторная проверка подлинности с помощью SMS](xref:security/authentication/2fa)
* [Подтверждение учетной записи и восстановление пароля в ASP.NET Core](xref:security/authentication/accconfirm)
* [Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации](xref:security/authorization/secure-data)
