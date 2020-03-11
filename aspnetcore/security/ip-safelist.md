---
title: Адрес списка надежных IP-адресов клиента для ASP.NET Core
author: damienbod
description: Узнайте, как создавать по промежуточного слоя или фильтры действий для проверки удаленных IP-адресов по списку утвержденных IP-адресов.
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: d25c375f7e659168ab8cc9d8e11753cb7dfde831
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652054"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="0b032-103">Адрес списка надежных IP-адресов клиента для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b032-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="0b032-104">[(Damien Бауден](https://twitter.com/damien_bod) и [Tom Dykstra)](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0b032-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="0b032-105">В этой статье показаны три способа реализации safelist IP-адрес (также известный как утвержденный список) в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b032-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="0b032-106">Вы можете использовать:</span><span class="sxs-lookup"><span data-stu-id="0b032-106">You can use:</span></span>

* <span data-ttu-id="0b032-107">По промежуточного слоя для проверки удаленного IP-адреса каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="0b032-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="0b032-108">Фильтры действий для проверки удаленного IP-адреса запросов для конкретных контроллеров или методов действий.</span><span class="sxs-lookup"><span data-stu-id="0b032-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="0b032-109">Razor Pages фильтры для проверки удаленного IP-адреса запросов на страницах Razor.</span><span class="sxs-lookup"><span data-stu-id="0b032-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="0b032-110">В каждом случае строка, содержащая утвержденные IP-адреса клиентов, сохраняется в параметре приложения.</span><span class="sxs-lookup"><span data-stu-id="0b032-110">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="0b032-111">По промежуточного слоя или фильтра анализирует строку в виде списка и проверяет, входит ли удаленный IP-адрес в список.</span><span class="sxs-lookup"><span data-stu-id="0b032-111">The middleware or filter parses the string into a list and checks if the remote IP is in the list.</span></span> <span data-ttu-id="0b032-112">В противном случае возвращается код состояния HTTP 403 запрещено.</span><span class="sxs-lookup"><span data-stu-id="0b032-112">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="0b032-113">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0b032-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="0b032-114">Списка надежных отправителей</span><span class="sxs-lookup"><span data-stu-id="0b032-114">The safelist</span></span>

<span data-ttu-id="0b032-115">Список настраивается в файле *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="0b032-115">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="0b032-116">Это список, разделенный точкой с запятой, который может содержать адреса IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="0b032-116">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="0b032-117">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="0b032-117">Middleware</span></span>

<span data-ttu-id="0b032-118">Метод `Configure` добавляет по промежуточного слоя и передает ему строку списка надежных отправителей в параметре конструктора.</span><span class="sxs-lookup"><span data-stu-id="0b032-118">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="0b032-119">По промежуточного слоя анализирует строку в массив и ищет удаленный IP-адрес в массиве.</span><span class="sxs-lookup"><span data-stu-id="0b032-119">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="0b032-120">Если удаленный IP-адрес не найден, по промежуточного слоя возвращает HTTP 401 запрещено.</span><span class="sxs-lookup"><span data-stu-id="0b032-120">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="0b032-121">Этот процесс проверки обходится для HTTP-запросов GET.</span><span class="sxs-lookup"><span data-stu-id="0b032-121">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="0b032-122">Фильтр действий</span><span class="sxs-lookup"><span data-stu-id="0b032-122">Action filter</span></span>

<span data-ttu-id="0b032-123">Если вы хотите, чтобы он был только для конкретных контроллеров или методов действий, используйте фильтр действий.</span><span class="sxs-lookup"><span data-stu-id="0b032-123">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="0b032-124">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="0b032-124">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckFilter.cs)]

<span data-ttu-id="0b032-125">Фильтр действий добавляется в контейнер служб.</span><span class="sxs-lookup"><span data-stu-id="0b032-125">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="0b032-126">Затем фильтр можно использовать на контроллере или в методе действия.</span><span class="sxs-lookup"><span data-stu-id="0b032-126">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="0b032-127">В примере приложения фильтр применяется к методу `Get`.</span><span class="sxs-lookup"><span data-stu-id="0b032-127">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="0b032-128">Поэтому при тестировании приложения путем отправки `Get` запроса API атрибут проверяет IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="0b032-128">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="0b032-129">При тестировании путем вызова API с любым другим методом HTTP по промежуточного слоя проверяет IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="0b032-129">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="0b032-130">Фильтр Razor Pages</span><span class="sxs-lookup"><span data-stu-id="0b032-130">Razor Pages filter</span></span> 

<span data-ttu-id="0b032-131">Если требуется использование списка надежных отправителей для Razor Pages приложения, используйте фильтр Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0b032-131">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="0b032-132">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="0b032-132">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckPageFilter.cs)]

<span data-ttu-id="0b032-133">Этот фильтр включается путем добавления его в коллекцию фильтров MVC.</span><span class="sxs-lookup"><span data-stu-id="0b032-133">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="0b032-134">При запуске приложения и запросе страницы Razor фильтр Razor Pages проверяет IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="0b032-134">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b032-135">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="0b032-135">Next steps</span></span>

<span data-ttu-id="0b032-136">Дополнительные [сведения о ASP.NET Core по промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="0b032-136">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
