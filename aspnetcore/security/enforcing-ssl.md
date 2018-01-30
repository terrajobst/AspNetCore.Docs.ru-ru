---
title: "Применение протокола SSL в приложении ASP.NET Core"
author: rick-anderson
description: "Показано, как протокол SSL в ASP.NET Core веб-приложения"
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2e0a2f4732e574c80ceef8fd21a530a11aef254c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
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

Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности. Применение `[RequireHttps]` атрибутов для всех контроллеров не считается защищено настолько, насколько требования HTTPS глобально. Не может гарантировать новых контроллеров добавлен в приложение необходимо применить `[RequireHttps]` атрибута.
