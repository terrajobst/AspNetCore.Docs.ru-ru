---
title: "Применение HTTPS в приложении ASP.NET Core"
author: rick-anderson
description: "Показано, как требуется HTTPS/TLS в ASP.NET Core веб-приложения."
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 636077ea21581716308384ebf8d47c1e417a256a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>Применение HTTPS в приложении ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом документе показано, как:

- Требуйте использования протокола HTTPS для всех запросов.
- Перенаправление всех запросов HTTP на HTTPS.

> [!WARNING]
> Сделать **не** использовать `RequireHttpsAttribute` на веб-API, получать конфиденциальные сведения. `RequireHttpsAttribute`использует коды состояния HTTP для перенаправления обозревателей с HTTP на HTTPS. Клиенты API не может понять или подчиняются перенаправлений с HTTP на HTTPS. Такие клиенты могут отправлять сведения по протоколу HTTP. Веб-API должен:
>
>* Прослушивает HTTP.
>* Закрывает соединение с кодом состояния 400 (неправильный запрос) и обслуживает запрос.

## <a name="require-https"></a>Требовать использования протокола HTTPS

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) используется для обязательного использования протокола HTTPS. `[RequireHttpsAttribute]`можно дополнить контроллеров или методы или могут быть применены глобально. Чтобы применить атрибут глобально, добавьте следующий код в `ConfigureServices` в `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Предыдущий выделенный код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются. Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).

Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности. Применение `[RequireHttps]` атрибут ко всем страницам контроллеры и Razor не считается защищено настолько, насколько требования HTTPS глобально. Не может гарантировать `[RequireHttps]` атрибут при добавлении новых контроллеров и страниц Razor.