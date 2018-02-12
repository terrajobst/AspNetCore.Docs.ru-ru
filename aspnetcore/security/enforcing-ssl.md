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
# <a name="enforcing-https-in-an-aspnet-core-app"></a><span data-ttu-id="018af-103">Применение HTTPS в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="018af-103">Enforcing HTTPS in an ASP.NET Core app</span></span>

<span data-ttu-id="018af-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="018af-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="018af-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="018af-105">This document shows how to:</span></span>

- <span data-ttu-id="018af-106">Требуйте использования протокола HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="018af-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="018af-107">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="018af-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="018af-108">Сделать **не** использовать `RequireHttpsAttribute` на веб-API, получать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="018af-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="018af-109">`RequireHttpsAttribute`использует коды состояния HTTP для перенаправления обозревателей с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="018af-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="018af-110">Клиенты API не может понять или подчиняются перенаправлений с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="018af-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="018af-111">Такие клиенты могут отправлять сведения по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="018af-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="018af-112">Веб-API должен:</span><span class="sxs-lookup"><span data-stu-id="018af-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="018af-113">Прослушивает HTTP.</span><span class="sxs-lookup"><span data-stu-id="018af-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="018af-114">Закрывает соединение с кодом состояния 400 (неправильный запрос) и обслуживает запрос.</span><span class="sxs-lookup"><span data-stu-id="018af-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="018af-115">Требовать использования протокола HTTPS</span><span class="sxs-lookup"><span data-stu-id="018af-115">Require HTTPS</span></span>

<span data-ttu-id="018af-116">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) используется для обязательного использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="018af-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="018af-117">`[RequireHttpsAttribute]`можно дополнить контроллеров или методы или могут быть применены глобально.</span><span class="sxs-lookup"><span data-stu-id="018af-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="018af-118">Чтобы применить атрибут глобально, добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="018af-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="018af-119">Предыдущий выделенный код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются.</span><span class="sxs-lookup"><span data-stu-id="018af-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="018af-120">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="018af-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="018af-121">Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="018af-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="018af-122">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="018af-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="018af-123">Применение `[RequireHttps]` атрибут ко всем страницам контроллеры и Razor не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="018af-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="018af-124">Не может гарантировать `[RequireHttps]` атрибут при добавлении новых контроллеров и страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="018af-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>