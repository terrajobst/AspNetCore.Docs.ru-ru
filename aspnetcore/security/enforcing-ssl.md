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
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="210fd-104">Применение протокола SSL в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="210fd-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="210fd-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="210fd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="210fd-106">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="210fd-106">This document shows how to:</span></span>

- <span data-ttu-id="210fd-107">Требуйте шифрование SSL для всех запросов (только для запросов HTTPS).</span><span class="sxs-lookup"><span data-stu-id="210fd-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="210fd-108">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="210fd-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="210fd-109">Обязательное использование SSL</span><span class="sxs-lookup"><span data-stu-id="210fd-109">Require SSL</span></span>

<span data-ttu-id="210fd-110">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется протокол SSL.</span><span class="sxs-lookup"><span data-stu-id="210fd-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="210fd-111">Вы можете декорировать контроллеров или методов с этим атрибутом, или можно применить глобально, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="210fd-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="210fd-112">Добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="210fd-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="210fd-113">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span><span class="sxs-lookup"><span data-stu-id="210fd-113">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]</span></span>

<span data-ttu-id="210fd-114">Выделенный код выше требует использования всех запросов `HTTPS`, игнорироваться HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="210fd-114">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="210fd-115">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="210fd-115">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="210fd-116">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span><span class="sxs-lookup"><span data-stu-id="210fd-116">[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]</span></span>

<span data-ttu-id="210fd-117">В разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="210fd-117">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="210fd-118">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="210fd-118">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="210fd-119">Применение `[RequireHttps]` атрибут все контроллер не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="210fd-119">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="210fd-120">Не может гарантировать новых контроллеров добавлен в приложение необходимо применить `[RequireHttps]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="210fd-120">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>

## <a name="set-up-iis-express-for-sslhttps"></a><span data-ttu-id="210fd-121">Настроить сервер IIS Express для SSL или HTTPS</span><span class="sxs-lookup"><span data-stu-id="210fd-121">Set up IIS Express for SSL/HTTPS</span></span>

<span data-ttu-id="210fd-122">В разделе [Настройка HTTPS для разработчиков на ASP.NET Core](xref:security/https#iisxpress).</span><span class="sxs-lookup"><span data-stu-id="210fd-122">See [Setting up HTTPS for development in ASP.NET Core](xref:security/https#iisxpress).</span></span>
