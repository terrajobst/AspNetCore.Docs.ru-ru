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
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="fbbde-104">Применение протокола SSL в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbbde-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="fbbde-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="fbbde-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fbbde-106">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="fbbde-106">This document shows how to:</span></span>

- <span data-ttu-id="fbbde-107">Требуйте шифрование SSL для всех запросов (только для запросов HTTPS).</span><span class="sxs-lookup"><span data-stu-id="fbbde-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="fbbde-108">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fbbde-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="fbbde-109">Обязательное использование SSL</span><span class="sxs-lookup"><span data-stu-id="fbbde-109">Require SSL</span></span>

<span data-ttu-id="fbbde-110">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется протокол SSL.</span><span class="sxs-lookup"><span data-stu-id="fbbde-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="fbbde-111">Вы можете декорировать контроллеров или методов с этим атрибутом, или можно применить глобально, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="fbbde-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="fbbde-112">Добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="fbbde-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="fbbde-113">Выделенный код выше требует использования всех запросов `HTTPS`, игнорироваться HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="fbbde-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="fbbde-114">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="fbbde-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="fbbde-115">В разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="fbbde-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="fbbde-116">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="fbbde-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="fbbde-117">Применение `[RequireHttps]` атрибут все контроллер не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="fbbde-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="fbbde-118">Не может гарантировать новых контроллеров добавлен в приложение необходимо применить `[RequireHttps]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="fbbde-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
