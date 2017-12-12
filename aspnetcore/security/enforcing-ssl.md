---
title: "Применение протокола SSL в приложении ASP.NET Core"
author: rick-anderson
description: "Показано, как протокол SSL в ASP.NET Core веб-приложения"
keywords: ASP.NET Core, SSL, HTTPS, RequireHttpsAttribute, IIS Express
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Применение протокола SSL в приложении ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом документе показано, как:

- Требуйте шифрование SSL для всех запросов (только для запросов HTTPS).
- Перенаправление всех запросов HTTP на HTTPS.

## <a name="require-ssl"></a>Обязательное использование SSL

[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется протокол SSL. Вы можете декорировать контроллеров или методов с этим атрибутом, или можно применить глобально, как показано ниже:

Добавьте следующий код в `ConfigureServices` в `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

Выделенный код выше требует использования всех запросов `HTTPS`, игнорироваться HTTP-запросов. Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

В разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting) для получения дополнительной информации.

Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности. Применение `[RequireHttps]` атрибут все контроллер не считается защищено настолько, насколько требования HTTPS глобально. Не может гарантировать новых контроллеров добавлен в приложение необходимо применить `[RequireHttps]` атрибута.
