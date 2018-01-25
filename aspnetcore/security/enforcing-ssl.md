---
title: "Применение протокола SSL в приложении ASP.NET Core"
author: rick-anderson
description: "Показано, как протокол SSL в ASP.NET Core веб-приложения"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: f248e9c0463cf4a46a447a9c896b3276a50f5f08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="af707-103">Применение протокола SSL в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af707-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="af707-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="af707-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="af707-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="af707-105">This document shows how to:</span></span>

- <span data-ttu-id="af707-106">Требуйте шифрование SSL для всех запросов (только для запросов HTTPS).</span><span class="sxs-lookup"><span data-stu-id="af707-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="af707-107">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af707-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="af707-108">Обязательное использование SSL</span><span class="sxs-lookup"><span data-stu-id="af707-108">Require SSL</span></span>

<span data-ttu-id="af707-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется протокол SSL.</span><span class="sxs-lookup"><span data-stu-id="af707-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="af707-110">Вы можете декорировать контроллеров или методов с этим атрибутом, или можно применить глобально, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="af707-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="af707-111">Добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="af707-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="af707-112">Выделенный код выше требует использования всех запросов `HTTPS`, игнорироваться HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="af707-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="af707-113">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="af707-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="af707-114">В разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="af707-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="af707-115">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="af707-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="af707-116">Применение `[RequireHttps]` атрибутов для всех контроллеров не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="af707-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="af707-117">Не может гарантировать новых контроллеров добавлен в приложение необходимо применить `[RequireHttps]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="af707-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
