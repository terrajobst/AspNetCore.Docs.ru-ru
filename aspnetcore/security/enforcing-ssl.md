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
ms.openlocfilehash: 3b72cddb7a240ad6d6e1427796e9bb4f7003a3f7
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="e8d95-103">Применение протокола SSL в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8d95-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="e8d95-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="e8d95-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e8d95-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="e8d95-105">This document shows how to:</span></span>

- <span data-ttu-id="e8d95-106">Требуйте шифрование SSL для всех запросов (только для запросов HTTPS).</span><span class="sxs-lookup"><span data-stu-id="e8d95-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="e8d95-107">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8d95-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="e8d95-108">Обязательное использование SSL</span><span class="sxs-lookup"><span data-stu-id="e8d95-108">Require SSL</span></span>

<span data-ttu-id="e8d95-109">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется протокол SSL.</span><span class="sxs-lookup"><span data-stu-id="e8d95-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="e8d95-110">Вы можете декорировать контроллеров или методов с этим атрибутом, или можно применить глобально, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="e8d95-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="e8d95-111">Добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e8d95-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="e8d95-112">Выделенный код выше требует использования всех запросов `HTTPS`, игнорироваться HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="e8d95-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="e8d95-113">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="e8d95-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="e8d95-114">В разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="e8d95-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="e8d95-115">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="e8d95-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="e8d95-116">Применение `[RequireHttps]` атрибутов для всех контроллеров не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="e8d95-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="e8d95-117">Не может гарантировать новых контроллеров добавлен в приложение необходимо применить `[RequireHttps]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="e8d95-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
