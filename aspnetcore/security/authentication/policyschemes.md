---
title: Схемы политики в ASP.NET Core
author: rick-anderson
description: Схемы политики аутентификации упрощают создание одной логической схемы проверки подлинности
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: f02d8e5cac20a9b60c5eddbd28253efacf682ea1
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880713"
---
# <a name="policy-schemes-in-aspnet-core"></a>Схемы политики в ASP.NET Core

Схемы политики аутентификации упрощают создание одной логической схемы проверки подлинности, потенциально использующей несколько подходов. Например, схема политики может использовать проверку подлинности Google для решения проблем и проверку подлинности файлов cookie для всех остальных. Схемы политики проверки подлинности делают следующее:

* Простое перенаправление любого действия проверки подлинности в другую схему.
* Пересылать динамически на основе запроса.

Все схемы проверки подлинности, использующие производные <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> и связанные с [аусентикатионхандлер\<TOptions >](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):

* Являются автоматическими схемами политик в ASP.NET Core 2,1 и более поздних версий.
* Можно включить с помощью настройки параметров схемы.

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>Примеры

В следующем примере показана схема более высокого уровня, которая сочетает схемы более низкого уровня. Проверка подлинности Google используется для решения проблем, а проверка подлинности файлов cookie используется для всех остальных элементов:

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

В следующем примере включается динамическое выделение схем на основе каждого запроса. То есть, как смешивать файлы cookie и проверку подлинности API:

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
