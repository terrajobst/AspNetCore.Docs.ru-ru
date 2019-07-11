---
title: Политика схем в ASP.NET Core
author: rick-anderson
description: Схемы политики проверки подлинности облегчают имеют схему проверки подлинности единого логического
ms.author: riande
ms.date: 02/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: be03f349455c673b0739935ad20e596325c8cb74
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815288"
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

В следующем примере включается динамический выбор схем для каждого запроса. То есть как сочетать файлы cookie и API проверки подлинности:

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
