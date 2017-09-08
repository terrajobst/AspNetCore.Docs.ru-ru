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
ms.openlocfilehash: 35554939bd574b2826053004ed437bee46154c2b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Применение протокола SSL в приложении ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT)

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

## <a name="set-up-iis-express-for-sslhttps"></a>Настроить сервер IIS Express для SSL или HTTPS

В разделе [Настройка HTTPS для разработчиков на ASP.NET Core](xref:security/https#iisxpress).
