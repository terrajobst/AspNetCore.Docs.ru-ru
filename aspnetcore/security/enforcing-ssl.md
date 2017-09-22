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
ms.openlocfilehash: e8e7d4a69fd681534fb313ff113805bfd6a6d44e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="9b0cd-104">Применение протокола SSL в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b0cd-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="9b0cd-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="9b0cd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9b0cd-106">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="9b0cd-106">This document shows how to:</span></span>

- <span data-ttu-id="9b0cd-107">Требуйте шифрование SSL для всех запросов (только для запросов HTTPS).</span><span class="sxs-lookup"><span data-stu-id="9b0cd-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="9b0cd-108">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9b0cd-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="9b0cd-109">Обязательное использование SSL</span><span class="sxs-lookup"><span data-stu-id="9b0cd-109">Require SSL</span></span>

<span data-ttu-id="9b0cd-110">[RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется протокол SSL.</span><span class="sxs-lookup"><span data-stu-id="9b0cd-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="9b0cd-111">Вы можете декорировать контроллеров или методов с этим атрибутом, или можно применить глобально, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="9b0cd-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="9b0cd-112">Добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="9b0cd-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="9b0cd-113">Выделенный код выше требует использования всех запросов `HTTPS`, игнорироваться HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="9b0cd-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="9b0cd-114">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="9b0cd-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="9b0cd-115">В разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="9b0cd-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="9b0cd-116">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="9b0cd-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="9b0cd-117">Применение `[RequireHttps]` атрибут все контроллер не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="9b0cd-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="9b0cd-118">Не может гарантировать новых контроллеров добавлен в приложение необходимо применить `[RequireHttps]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="9b0cd-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>

## <a name="set-up-iis-express-for-sslhttps"></a><span data-ttu-id="9b0cd-119">Настроить сервер IIS Express для SSL или HTTPS</span><span class="sxs-lookup"><span data-stu-id="9b0cd-119">Set up IIS Express for SSL/HTTPS</span></span>

<span data-ttu-id="9b0cd-120">В разделе [Настройка HTTPS для разработчиков на ASP.NET Core](xref:security/https#iisxpress).</span><span class="sxs-lookup"><span data-stu-id="9b0cd-120">See [Setting up HTTPS for development in ASP.NET Core](xref:security/https#iisxpress).</span></span>
