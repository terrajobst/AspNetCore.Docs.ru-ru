---
title: Политика схем в ASP.NET Core
author: rick-anderson
description: Схемы политики проверки подлинности облегчают имеют схему проверки подлинности единого логического
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: c310b61e14df2b7846e32a602bb75914a5850aff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346780"
---
# <a name="policy-schemes-in-aspnet-core"></a>Политика схем в ASP.NET Core

Схемы проверки подлинности политики упрощают имеют схему проверки подлинности единого логического потенциально могут использовать несколько подходов. Например схему политики может использовать Google для проблем и файл cookie проверки подлинности для всего остального. Схемы политики проверки подлинности сделать.

* Легко пересылать любое действие проверки подлинности в другую схему.
* Вперед, динамически на основе запроса.

Все схемы проверки подлинности, использование производных <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> и связанным [ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):

* — Автоматически политики схемы в ASP.NET Core 2.1 и более поздних версий.
* Можно включить, используя настройки параметров схемы.

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>Примеры

В следующем примере показано выше уровня схемы, сочетающий в себе нижнего уровня схемы. Для задач используется проверка подлинности Google, и файл cookie проверки подлинности используется для всего остального:

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

В следующем примере включается динамический выбор схем для каждого запроса. То есть как сочетать файлы cookie и API проверки подлинности.

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
