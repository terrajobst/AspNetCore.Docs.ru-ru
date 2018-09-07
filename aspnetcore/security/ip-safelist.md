---
title: Объединение списков надежных IP-адрес клиента для ASP.NET Core
author: damienbod
description: Узнайте, как по промежуточного слоя или действия фильтров для проверки IP-адресов на основе списка утвержденных IP-адреса записи.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040117"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="25642-103">Объединение списков надежных IP-адрес клиента для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25642-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="25642-104">По [Дэмиен Боуден](https://twitter.com/damien_bod) и [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="25642-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="25642-105">В этой статье показано два способа реализации safelist IP-адрес (также известный как белый список):</span><span class="sxs-lookup"><span data-stu-id="25642-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="25642-106">С помощью ASP.NET Core по промежуточного слоя для проверки удаленного IP-адрес каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="25642-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="25642-107">Проверить с помощью фильтров действий в ASP.NET Core удаленный IP-адрес запросов для конкретного действия методов.</span><span class="sxs-lookup"><span data-stu-id="25642-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="25642-108">Пример приложения показаны оба подхода.</span><span class="sxs-lookup"><span data-stu-id="25642-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="25642-109">В каждом случае строка, содержащая утвержденных клиентских IP-адресов хранится в параметр приложения.</span><span class="sxs-lookup"><span data-stu-id="25642-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="25642-110">В по промежуточного слоя или фильтр анализирует строку в список и проверяет, является ли удаленный IP-адрес в списке.</span><span class="sxs-lookup"><span data-stu-id="25642-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="25642-111">В противном случае возвращается код состояния HTTP 403-запрещено.</span><span class="sxs-lookup"><span data-stu-id="25642-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="25642-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="25642-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="25642-113">Объединение списков надежных</span><span class="sxs-lookup"><span data-stu-id="25642-113">The safelist</span></span>

<span data-ttu-id="25642-114">Список настраивается в *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="25642-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="25642-115">Оно является списком разделенных точкой с запятой и может содержать адреса IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="25642-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="25642-116">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="25642-116">Middleware</span></span>

<span data-ttu-id="25642-117">`Configure` Метод добавляет по промежуточного слоя и передает ему строку safelist в качестве параметра конструктора.</span><span class="sxs-lookup"><span data-stu-id="25642-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="25642-118">По промежуточного слоя выполняет синтаксический анализ строки в массив и ищет удаленный IP-адрес в массиве.</span><span class="sxs-lookup"><span data-stu-id="25642-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="25642-119">Если удаленный IP-адрес не найден, по промежуточного слоя возвращает HTTP 401 запрещено.</span><span class="sxs-lookup"><span data-stu-id="25642-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="25642-120">Этот процесс проверки пропускается для запросов HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="25642-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="25642-121">Фильтр действий</span><span class="sxs-lookup"><span data-stu-id="25642-121">Action filter</span></span>

<span data-ttu-id="25642-122">Объединение списков надежных только для определенных контроллеров или методы действий, используйте фильтр действий.</span><span class="sxs-lookup"><span data-stu-id="25642-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="25642-123">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="25642-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="25642-124">Фильтр действий добавляется в контейнер служб.</span><span class="sxs-lookup"><span data-stu-id="25642-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="25642-125">Фильтр можно использовать на контроллеру или методу действия.</span><span class="sxs-lookup"><span data-stu-id="25642-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="25642-126">В примере приложения, фильтр применяется к `Get` метод.</span><span class="sxs-lookup"><span data-stu-id="25642-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="25642-127">Так что при тестировании приложения, отправляя `Get` API запроса, атрибут проверки IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="25642-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="25642-128">Если вы тестируете путем вызова API с любой другой метод HTTP, по промежуточного слоя проверки IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="25642-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="25642-129">Фильтр страниц Razor</span><span class="sxs-lookup"><span data-stu-id="25642-129">Razor Pages filter</span></span> 

<span data-ttu-id="25642-130">Если требуется safelist в приложение Razor Pages, используйте фильтр Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="25642-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="25642-131">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="25642-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="25642-132">Этот фильтр включается путем добавления его в коллекцию фильтров MVC.</span><span class="sxs-lookup"><span data-stu-id="25642-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="25642-133">Когда вы запустите приложение и запрос страницы Razor, Razor Pages фильтр проверки IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="25642-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25642-134">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="25642-134">Next steps</span></span>

<span data-ttu-id="25642-135">[Дополнительные сведения о по промежуточного слоя ASP.NET Core](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="25642-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
