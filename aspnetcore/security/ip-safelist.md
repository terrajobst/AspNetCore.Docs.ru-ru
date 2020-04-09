---
title: Клиент IP-безопасное место для ASP.NET Core
author: damienbod
description: Узнайте, как написать промежуточное программное обеспечение или фильтры действий для проверки удаленных IP-адресов со списком утвержденных IP-адресов.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
uid: security/ip-safelist
ms.openlocfilehash: 2db879a6918245cbacff8b1a5dc15786ffab6a34
ms.sourcegitcommit: 196e4a36df5be5b04fedcff484a4261f8046ec57
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2020
ms.locfileid: "80471800"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="53c5f-103">Клиент IP-безопасное место для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53c5f-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="53c5f-104">[Дэмиен Боуден](https://twitter.com/damien_bod) и [Том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="53c5f-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="53c5f-105">В этой статье показаны три способа реализации списка safelist IP-адресов (также известный как список разрешений) в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="53c5f-105">This article shows three ways to implement an IP address safelist (also known as an allow list) in an ASP.NET Core app.</span></span> <span data-ttu-id="53c5f-106">Сопроводительное приложение образца демонстрирует все три подхода.</span><span class="sxs-lookup"><span data-stu-id="53c5f-106">An accompanying sample app demonstrates all three approaches.</span></span> <span data-ttu-id="53c5f-107">Вы можете использовать:</span><span class="sxs-lookup"><span data-stu-id="53c5f-107">You can use:</span></span>

* <span data-ttu-id="53c5f-108">Middleware, чтобы проверить удаленный IP-адрес каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="53c5f-108">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="53c5f-109">Фильтры действий MVC для проверки удаленного IP-адреса запросов на конкретные контроллеры или методы действий.</span><span class="sxs-lookup"><span data-stu-id="53c5f-109">MVC action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="53c5f-110">Razor Pages фильтры для проверки удаленного IP-адреса запросов на страницы Razor.</span><span class="sxs-lookup"><span data-stu-id="53c5f-110">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="53c5f-111">В каждом случае строка, содержащая одобренные IP-адреса клиента, хранится в настройках приложения.</span><span class="sxs-lookup"><span data-stu-id="53c5f-111">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="53c5f-112">Промежуток или фильтр:</span><span class="sxs-lookup"><span data-stu-id="53c5f-112">The middleware or filter:</span></span>

* <span data-ttu-id="53c5f-113">Разбирает строку в массив.</span><span class="sxs-lookup"><span data-stu-id="53c5f-113">Parses the string into an array.</span></span> 
* <span data-ttu-id="53c5f-114">Проверка наличия удаленного IP-адреса в массиве.</span><span class="sxs-lookup"><span data-stu-id="53c5f-114">Checks if the remote IP address exists in the array.</span></span>

<span data-ttu-id="53c5f-115">Доступ разрешен, если массив содержит IP-адрес.</span><span class="sxs-lookup"><span data-stu-id="53c5f-115">Access is allowed if the array contains the IP address.</span></span> <span data-ttu-id="53c5f-116">В противном случае возвращается код статуса HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="53c5f-116">Otherwise, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="53c5f-117">[Просмотр или загрузка образца кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples) [(как скачать)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="53c5f-117">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="ip-address-safelist"></a><span data-ttu-id="53c5f-118">Список безопасности IP-адресов</span><span class="sxs-lookup"><span data-stu-id="53c5f-118">IP address safelist</span></span>

<span data-ttu-id="53c5f-119">В примере приложения, IP-адрес safelist является:</span><span class="sxs-lookup"><span data-stu-id="53c5f-119">In the sample app, the IP address safelist is:</span></span>

* <span data-ttu-id="53c5f-120">Определяется `AdminSafeList` свойством в файле *appsettings.json.*</span><span class="sxs-lookup"><span data-stu-id="53c5f-120">Defined by the `AdminSafeList` property in the *appsettings.json* file.</span></span>
* <span data-ttu-id="53c5f-121">Строка, различенная за поломки, которая может содержать как [варианты 4 Протокола Интернета (IPv4),](https://wikipedia.org/wiki/IPv4) так и [адреса Протокола Интернета 6 (IPv6).](https://wikipedia.org/wiki/IPv6)</span><span class="sxs-lookup"><span data-stu-id="53c5f-121">A semicolon-delimited string that may contain both [Internet Protocol version 4 (IPv4)](https://wikipedia.org/wiki/IPv4) and [Internet Protocol version 6 (IPv6)](https://wikipedia.org/wiki/IPv6) addresses.</span></span>

[!code-json[](ip-safelist/samples/3.x/ClientIpAspNetCore/appsettings.json?range=1-3&highlight=2)]

<span data-ttu-id="53c5f-122">В предыдущем примере допускаются адреса `127.0.0.1` `192.168.1.5` IPv4 и и адрес `::1` iPv6 `0:0:0:0:0:0:0:1`loopback (сжатый формат для ) .</span><span class="sxs-lookup"><span data-stu-id="53c5f-122">In the preceding example, the IPv4 addresses of `127.0.0.1` and `192.168.1.5` and the IPv6 loopback address of `::1` (compressed format for `0:0:0:0:0:0:0:1`) are allowed.</span></span>

## <a name="middleware"></a><span data-ttu-id="53c5f-123">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="53c5f-123">Middleware</span></span>

<span data-ttu-id="53c5f-124">Метод `Startup.Configure` добавляет пользовательский `AdminSafeListMiddleware` тип промежуточного посуды в конвейер запроса приложения.</span><span class="sxs-lookup"><span data-stu-id="53c5f-124">The `Startup.Configure` method adds the custom `AdminSafeListMiddleware` middleware type to the app's request pipeline.</span></span> <span data-ttu-id="53c5f-125">Безопасный список извлекается с помощью поставщика конфигурации .NET Core и передается в качестве параметра конструктора.</span><span class="sxs-lookup"><span data-stu-id="53c5f-125">The safelist is retrieved with the .NET Core configuration provider and is passed as a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureAddMiddleware)]

<span data-ttu-id="53c5f-126">Промежуточное программное обеспечение разбирает строку в массив и ищет удаленный IP-адрес в массиве.</span><span class="sxs-lookup"><span data-stu-id="53c5f-126">The middleware parses the string into an array and searches for the remote IP address in the array.</span></span> <span data-ttu-id="53c5f-127">Если удаленный IP-адрес не найден, промежуточное программное обеспечение возвращает HTTP 403 Запрещено.</span><span class="sxs-lookup"><span data-stu-id="53c5f-127">If the remote IP address isn't found, the middleware returns HTTP 403 Forbidden.</span></span> <span data-ttu-id="53c5f-128">Этот процесс проверки обойдя запросы HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="53c5f-128">This validation process is bypassed for HTTP GET requests.</span></span>

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Middlewares/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="53c5f-129">Фильтр действия</span><span class="sxs-lookup"><span data-stu-id="53c5f-129">Action filter</span></span>

<span data-ttu-id="53c5f-130">Если требуется контроль доступа на основе безопасного доступа для определенных контроллеров MVC или методов действий, используйте фильтр действия.</span><span class="sxs-lookup"><span data-stu-id="53c5f-130">If you want safelist-driven access control for specific MVC controllers or action methods, use an action filter.</span></span> <span data-ttu-id="53c5f-131">Пример:</span><span class="sxs-lookup"><span data-stu-id="53c5f-131">For example:</span></span>

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckActionFilter.cs?name=snippet_ClassOnly)]

<span data-ttu-id="53c5f-132">В `Startup.ConfigureServices`введите фильтр действия в коллекцию фильтров MVC.</span><span class="sxs-lookup"><span data-stu-id="53c5f-132">In `Startup.ConfigureServices`, add the action filter to the MVC filters collection.</span></span> <span data-ttu-id="53c5f-133">В следующем примере `ClientIpCheckActionFilter` добавляется фильтр действия.</span><span class="sxs-lookup"><span data-stu-id="53c5f-133">In the following example, a `ClientIpCheckActionFilter` action filter is added.</span></span> <span data-ttu-id="53c5f-134">Безопасный список и экземпляр регистратора консолей передаются в качестве параметров конструктора.</span><span class="sxs-lookup"><span data-stu-id="53c5f-134">A safelist and a console logger instance are passed as constructor parameters.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

<span data-ttu-id="53c5f-135">Фильтр действия может быть применен к контроллеру или методу действия с атрибутом [«ServiceFilter»:](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute)</span><span class="sxs-lookup"><span data-stu-id="53c5f-135">The action filter can then be applied to a controller or action method with the [[ServiceFilter]](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute) attribute:</span></span>

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_ActionFilter&highlight=1)]

<span data-ttu-id="53c5f-136">В примере приложения фильтр действия применяется к `Get` методу действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="53c5f-136">In the sample app, the action filter is applied to the controller's `Get` action method.</span></span> <span data-ttu-id="53c5f-137">При тестировании приложения путем отправки:</span><span class="sxs-lookup"><span data-stu-id="53c5f-137">When you test the app by sending:</span></span>

* <span data-ttu-id="53c5f-138">Запрос HTTP GET, `[ServiceFilter]` атрибут проверяет IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="53c5f-138">An HTTP GET request, the `[ServiceFilter]` attribute validates the client IP address.</span></span> <span data-ttu-id="53c5f-139">Если доступ к `Get` методу действия разрешен, то изменение следующего вывода консоли производится фильтром действия и методом действия:</span><span class="sxs-lookup"><span data-stu-id="53c5f-139">If access is allowed to the `Get` action method, a variation of the following console output is produced by the action filter and action method:</span></span>

    ```
    dbug: ClientIpSafelistComponents.Filters.ClientIpCheckActionFilter[0]
          Remote IpAddress: ::1
    dbug: ClientIpAspNetCore.Controllers.ValuesController[0]
          successful HTTP GET    
    ```

* <span data-ttu-id="53c5f-140">Глагол запроса HTTP, `AdminSafeListMiddleware` кроме GET, посредник проверяет IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="53c5f-140">An HTTP request verb other than GET, the `AdminSafeListMiddleware` middleware validates the client IP address.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="53c5f-141">Фильтр Страницбрина</span><span class="sxs-lookup"><span data-stu-id="53c5f-141">Razor Pages filter</span></span>

<span data-ttu-id="53c5f-142">Если для приложения Razor Pages требуется контроль доступа, управляемый безопасным, используйте фильтр Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="53c5f-142">If you want safelist-driven access control for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="53c5f-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="53c5f-143">For example:</span></span>

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckPageFilter.cs?name=snippet_ClassOnly)]

<span data-ttu-id="53c5f-144">В, `Startup.ConfigureServices`включить фильтр Razor Pages, добавив его в коллекцию фильтров MVC.</span><span class="sxs-lookup"><span data-stu-id="53c5f-144">In `Startup.ConfigureServices`, enable the Razor Pages filter by adding it to the MVC filters collection.</span></span> <span data-ttu-id="53c5f-145">В следующем примере `ClientIpCheckPageFilter` добавляется фильтр Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="53c5f-145">In the following example, a `ClientIpCheckPageFilter` Razor Pages filter is added.</span></span> <span data-ttu-id="53c5f-146">Безопасный список и экземпляр регистратора консолей передаются в качестве параметров конструктора.</span><span class="sxs-lookup"><span data-stu-id="53c5f-146">A safelist and a console logger instance are passed as constructor parameters.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

<span data-ttu-id="53c5f-147">При запросе страницы *индекса* Razor образца приложения фильтр Razor Pages проверяет IP-адрес клиента.</span><span class="sxs-lookup"><span data-stu-id="53c5f-147">When the sample app's *Index* Razor page is requested, the Razor Pages filter validates the client IP address.</span></span> <span data-ttu-id="53c5f-148">Фильтр производит вариацию следующего вывода консоли:</span><span class="sxs-lookup"><span data-stu-id="53c5f-148">The filter produces a variation of the following console output:</span></span>

```
dbug: ClientIpSafelistComponents.Filters.ClientIpCheckPageFilter[0]
      Remote IpAddress: ::1
```

## <a name="additional-resources"></a><span data-ttu-id="53c5f-149">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="53c5f-149">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="53c5f-150">Фильтры действия</span><span class="sxs-lookup"><span data-stu-id="53c5f-150">Action filters</span></span>](xref:mvc/controllers/filters#action-filters)
* <xref:razor-pages/filter>
