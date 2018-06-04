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
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729554"
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
