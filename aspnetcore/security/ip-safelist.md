---
title: Объединение списков надежных IP-адрес клиента для ASP.NET Core
author: damienbod
description: Узнайте, как по промежуточного слоя или действия фильтров для проверки IP-адресов на основе списка утвержденных IP-адреса записи.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: ca05989efabea3a71c6912e98055a6746e0f5966
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223937"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="5b313-103">Объединение списков надежных IP-адрес клиента для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b313-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="5b313-104">По [Дэмиен Боуден](https://twitter.com/damien_bod) и [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5b313-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="5b313-105">В этой статье показаны три способа реализации safelist IP-адрес (также известный как утвержденный список) в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b313-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="5b313-106">Можно использовать:</span><span class="sxs-lookup"><span data-stu-id="5b313-106">You can use:</span></span>

* <span data-ttu-id="5b313-107">По промежуточного слоя для проверки удаленного IP-адрес каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="5b313-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="5b313-108">Фильтры действий для проверки IP-адрес удаленного запросы для определенных контроллеров или методы действий.</span><span class="sxs-lookup"><span data-stu-id="5b313-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="5b313-109">Фильтры страниц Razor для проверки IP-адрес удаленного запросов для страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="5b313-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="5b313-110">В каждом случае строка, содержащая утвержденных клиентских IP-адресов хранится в параметр приложения.</span><span class="sxs-lookup"><span data-stu-id="5b313-110">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="5b313-111">В по промежуточного слоя или фильтр анализирует строку в список и проверяет, является ли удаленный IP-адрес в списке.</span><span class="sxs-lookup"><span data-stu-id="5b313-111">The middleware or filter parses the string into a list and checks if the remote IP is in the list.</span></span> <span data-ttu-id="5b313-112">В противном случае возвращается код состояния HTTP 403-запрещено.</span><span class="sxs-lookup"><span data-stu-id="5b313-112">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="5b313-113">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b313-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="5b313-114">Объединение списков надежных</span><span class="sxs-lookup"><span data-stu-id="5b313-114">The safelist</span></span>

<span data-ttu-id="5b313-115">Список настраивается в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="5b313-115">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="5b313-116">Оно является списком разделенных точкой с запятой и может содержать адреса IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="5b313-116">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="5b313-117">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="5b313-117">Middleware</span></span>

<span data-ttu-id="5b313-118">`Configure` Метод добавляет по промежуточного слоя и передает ему строку safelist в качестве параметра конструктора.</span><span class="sxs-lookup"><span data-stu-id="5b313-118">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="5b313-119">По промежуточного слоя выполняет синтаксический анализ строки в массив и ищет удаленный IP-адрес в массиве.</span><span class="sxs-lookup"><span data-stu-id="5b313-119">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="5b313-120">Если удаленный IP-адрес не найден, по промежуточного слоя возвращает HTTP 401 запрещено.</span><span class="sxs-lookup"><span data-stu-id="5b313-120">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="5b313-121">Этот процесс проверки пропускается для запросов HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="5b313-121">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="5b313-122">Фильтр действий</span><span class="sxs-lookup"><span data-stu-id="5b313-122">Action filter</span></span>

<span data-ttu-id="5b313-123">Объединение списков надежных только для определенных контроллеров или методы действий, используйте фильтр действий.</span><span class="sxs-lookup"><span data-stu-id="5b313-123">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="5b313-124">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="5b313-124">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="5b313-125">Фильтр действий добавляется в контейнер служб.</span><span class="sxs-lookup"><span data-stu-id="5b313-125">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="5b313-126">Фильтр можно использовать на контроллеру или методу действия.</span><span class="sxs-lookup"><span data-stu-id="5b313-126">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="5b313-127">В примере приложения, фильтр применяется к `Get` метод.</span><span class="sxs-lookup"><span data-stu-id="5b313-127">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="5b313-128">Так что при тестировании приложения, отправляя `Get` API запроса, атрибут проверки IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="5b313-128">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="5b313-129">Если вы тестируете путем вызова API с любой другой метод HTTP, по промежуточного слоя проверки IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="5b313-129">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="5b313-130">Фильтр страниц Razor</span><span class="sxs-lookup"><span data-stu-id="5b313-130">Razor Pages filter</span></span> 

<span data-ttu-id="5b313-131">Если требуется safelist в приложение Razor Pages, используйте фильтр Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="5b313-131">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="5b313-132">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="5b313-132">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="5b313-133">Этот фильтр включается путем добавления его в коллекцию фильтров MVC.</span><span class="sxs-lookup"><span data-stu-id="5b313-133">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="5b313-134">Когда вы запустите приложение и запрос страницы Razor, Razor Pages фильтр проверки IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="5b313-134">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b313-135">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5b313-135">Next steps</span></span>

<span data-ttu-id="5b313-136">[Дополнительные сведения о по промежуточного слоя ASP.NET Core](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="5b313-136">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
