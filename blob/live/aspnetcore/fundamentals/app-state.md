---
title: "Состояние сеанса и приложения в ASP.NET Core"
author: rick-anderson
description: "Методы сохранения приложения и состояние пользователя (сеанс) между запросами."
ms.author: riande
manager: wpickett
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 13b4d759ae574cdf9899ca148f0ffd3d9df6f9ae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="b1f03-103">Общие сведения о состоянии сеанса и приложения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1f03-103">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="b1f03-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Стив Смит](https://ardalis.com/), и [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="b1f03-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="b1f03-105">Протокол HTTP является протоколом без состояния.</span><span class="sxs-lookup"><span data-stu-id="b1f03-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="b1f03-106">Веб-сервер обрабатывает каждый HTTP-запрос как независимый запрос и не сохраняет значения пользователя из предыдущих запросов.</span><span class="sxs-lookup"><span data-stu-id="b1f03-106">A web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="b1f03-107">В этой статье описаны различные способы сохранить состояние приложения и сеанса между запросами.</span><span class="sxs-lookup"><span data-stu-id="b1f03-107">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="b1f03-108">Состояние сеанса</span><span class="sxs-lookup"><span data-stu-id="b1f03-108">Session state</span></span>

<span data-ttu-id="b1f03-109">Состояние сеанса — это функция в ASP.NET Core, которую можно использовать для сохранения пользовательских данных во время просмотра веб-приложения пользователем.</span><span class="sxs-lookup"><span data-stu-id="b1f03-109">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="b1f03-110">Состоящий из словаря или хэш-таблицы на сервере, состояние сеанса сохраняет данные для запросов из браузера.</span><span class="sxs-lookup"><span data-stu-id="b1f03-110">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="b1f03-111">Поддерживаемый кэш данных сеанса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-111">The session data is backed by a cache.</span></span>

<span data-ttu-id="b1f03-112">ASP.NET Core сохраняет состояние сеанса, предоставляя файл cookie, который содержит идентификатор сеанса, который отправляется на сервер при каждом запросе клиента.</span><span class="sxs-lookup"><span data-stu-id="b1f03-112">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="b1f03-113">Сервер использует идентификатор сеанса для выборки данных сеанса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-113">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="b1f03-114">Так как файл cookie сеанса является специфичным для браузера, не могут совместно использовать сеансы браузерами.</span><span class="sxs-lookup"><span data-stu-id="b1f03-114">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="b1f03-115">Файлы cookie сеанса, удаляются только в том случае, при завершении сеанса браузера.</span><span class="sxs-lookup"><span data-stu-id="b1f03-115">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="b1f03-116">Если файл cookie поступает просроченного сеанса, создается новый сеанс, в котором используется один и тот же файл cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-116">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="b1f03-117">Сервер сохраняет сеанс в течение ограниченного времени после выполнения последнего запроса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-117">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="b1f03-118">Можно задать время ожидания сеанса или использовать значение по умолчанию 20 минут.</span><span class="sxs-lookup"><span data-stu-id="b1f03-118">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="b1f03-119">Состояние сеанса идеально подходит для хранения пользовательских данных, связанную с определенным сеансом, но может не быть сохранен без возможности восстановления.</span><span class="sxs-lookup"><span data-stu-id="b1f03-119">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="b1f03-120">Данные удаляются из резервного хранилища либо при вызове `Session.Clear` или после окончания сеанса в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="b1f03-120">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="b1f03-121">Сервер не может определить при закрытии браузера или при удалении файла cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-121">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="b1f03-122">Не храните конфиденциальные данные в сеансе.</span><span class="sxs-lookup"><span data-stu-id="b1f03-122">Do not store sensitive data in session.</span></span> <span data-ttu-id="b1f03-123">Клиент не может закрыть браузер и очистить файл cookie сеанса (и некоторые браузеры сохранения файлов cookie сеанса через windows).</span><span class="sxs-lookup"><span data-stu-id="b1f03-123">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="b1f03-124">Кроме того сеанс не может быть ограничен до одного пользователя; Следующий пользователь может по-прежнему с того же сеанса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-124">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="b1f03-125">Поставщик сеансов в памяти сохраняет данные сеанса на локальном сервере.</span><span class="sxs-lookup"><span data-stu-id="b1f03-125">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="b1f03-126">Если вы планируете запускать веб-приложения на ферме серверов, необходимо использовать закрепленные сеансы для каждого сеанса к определенному серверу перегрузки.</span><span class="sxs-lookup"><span data-stu-id="b1f03-126">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="b1f03-127">По умолчанию платформа веб-сайтов Windows Azure для прикрепленных сеансов (маршрутизации запросов приложений или ARR).</span><span class="sxs-lookup"><span data-stu-id="b1f03-127">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="b1f03-128">Тем не менее закрепленные сеансы могут повлиять на масштабируемость и усложнить обновления веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="b1f03-128">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="b1f03-129">Лучшим вариантом является использование команды Redis или кэширует распределенных SQL Server, не требующую закрепленных сеансов.</span><span class="sxs-lookup"><span data-stu-id="b1f03-129">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="b1f03-130">Дополнительные сведения см. в разделе [работа с распределенного кэша](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="b1f03-130">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="b1f03-131">Дополнительные сведения о настройке поставщиков услуг см. в разделе [Настройка сеанса](#configuring-session) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="b1f03-131">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="b1f03-132">TempData</span><span class="sxs-lookup"><span data-stu-id="b1f03-132">TempData</span></span>

<span data-ttu-id="b1f03-133">Основные ASP.NET MVC предоставляет [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) свойство [контроллера](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="b1f03-133">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="b1f03-134">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="b1f03-134">This property stores data until it is read.</span></span> <span data-ttu-id="b1f03-135">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="b1f03-135">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="b1f03-136">`TempData`особенно полезна для перенаправления, когда данные, необходимые для более чем одного запроса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-136">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="b1f03-137">`TempData`реализуется TempData поставщиков, например, с помощью файлов cookie или с состоянием сеанса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-137">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="b1f03-138">Поставщики TempData</span><span class="sxs-lookup"><span data-stu-id="b1f03-138">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1f03-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1f03-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b1f03-140">В ASP.NET Core 2.0 и более поздних версиях поставщика на основе файлов cookie TempData используется по умолчанию для хранения TempData в файлах cookie.</span><span class="sxs-lookup"><span data-stu-id="b1f03-140">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="b1f03-141">Данные cookie кодируется с [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="b1f03-141">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="b1f03-142">Один файл cookie, так как файл cookie шифруется и фрагментирован, размером в ASP.NET Core 1.x не применяется ограничение.</span><span class="sxs-lookup"><span data-stu-id="b1f03-142">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="b1f03-143">Данные cookie не сжимается, поскольку сжатие зашифрованных данных можно привести к проблемам с безопасностью, например [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушения](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.</span><span class="sxs-lookup"><span data-stu-id="b1f03-143">The cookie data is not compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="b1f03-144">Дополнительные сведения на основе файлов cookie TempData поставщика см. в разделе [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="b1f03-144">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1f03-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1f03-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b1f03-146">В ASP.NET Core 1.0 и 1.1 поставщик TempData состояния сеанса используется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b1f03-146">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="b1f03-147">Выбор поставщика TempData</span><span class="sxs-lookup"><span data-stu-id="b1f03-147">Choosing a TempData provider</span></span>

<span data-ttu-id="b1f03-148">Выбор поставщика TempData включает в себя несколько факторов, таких как:</span><span class="sxs-lookup"><span data-stu-id="b1f03-148">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="b1f03-149">Используется ли приложение уже состояния сеанса для других целей?</span><span class="sxs-lookup"><span data-stu-id="b1f03-149">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="b1f03-150">В этом случае использование TempData поставщика состояния сеанса имеет без дополнительной оплаты (помимо объема данных) в приложение.</span><span class="sxs-lookup"><span data-stu-id="b1f03-150">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="b1f03-151">Приложение использует TempData только ограниченно для сравнительно небольших объемов данных (до 500 байт)?</span><span class="sxs-lookup"><span data-stu-id="b1f03-151">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="b1f03-152">Если таким образом, поставщик TempData куки-файл будет добавлен небольшой стоимость каждого запроса, который выполняет TempData.</span><span class="sxs-lookup"><span data-stu-id="b1f03-152">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="b1f03-153">В противном случае TempData поставщика состояния сеанса может быть полезно во избежание циклической обработки большой объем данных в каждом запросе до полного исчерпания TempData.</span><span class="sxs-lookup"><span data-stu-id="b1f03-153">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="b1f03-154">Приложение выполняется на веб-ферме (несколько серверов)?</span><span class="sxs-lookup"><span data-stu-id="b1f03-154">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="b1f03-155">В этом случае имеется дополнительная настройка не требуется для использования поставщика TempData куки-файл.</span><span class="sxs-lookup"><span data-stu-id="b1f03-155">If so, there is no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="b1f03-156">Большинство веб-клиентов (например, веб-браузеры) соблюдения ограничений на максимальный размер каждого файла cookie и общее количество файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="b1f03-156">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="b1f03-157">Таким образом при использовании поставщика TempData куки-файл, убедитесь, что приложение не будет превышать эти пределы.</span><span class="sxs-lookup"><span data-stu-id="b1f03-157">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="b1f03-158">Рассмотрим общий объем данных, учитывая накладных расходов, шифрования и фрагментации.</span><span class="sxs-lookup"><span data-stu-id="b1f03-158">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="b1f03-159">Настройка поставщика TempData</span><span class="sxs-lookup"><span data-stu-id="b1f03-159">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1f03-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1f03-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b1f03-161">По умолчанию включен поставщик TempData на основе файла cookie.</span><span class="sxs-lookup"><span data-stu-id="b1f03-161">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="b1f03-162">Следующие `Startup` класс код служит для настройки поставщика TempData на основе сеансов:</span><span class="sxs-lookup"><span data-stu-id="b1f03-162">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1f03-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1f03-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b1f03-164">Следующие `Startup` класс код служит для настройки поставщика TempData на основе сеансов:</span><span class="sxs-lookup"><span data-stu-id="b1f03-164">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="b1f03-165">Порядок важен для компонентов по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="b1f03-165">Ordering is critical for middleware components.</span></span> <span data-ttu-id="b1f03-166">В предыдущем примере исключение типа `InvalidOperationException` возникает, когда `UseSession` вызывается после `UseMvcWithDefaultRoute`.</span><span class="sxs-lookup"><span data-stu-id="b1f03-166">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="b1f03-167">В разделе [упорядочение по промежуточного слоя](xref:fundamentals/middleware#ordering) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="b1f03-167">See [Middleware Ordering](xref:fundamentals/middleware#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b1f03-168">Если для различных версий платформы .NET Framework и с помощью поставщика на основе сеансов, добавьте [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) пакет NuGet для проекта.</span><span class="sxs-lookup"><span data-stu-id="b1f03-168">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="b1f03-169">Строки запросов</span><span class="sxs-lookup"><span data-stu-id="b1f03-169">Query strings</span></span>

<span data-ttu-id="b1f03-170">Можно передать ограниченный объем данных от одного запроса в другой, добавьте его в строку запроса нового запроса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-170">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="b1f03-171">Это полезно для записи состояния постоянных способом, который позволяет связи внедренные состояния для совместного использования по электронной почте или социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="b1f03-171">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="b1f03-172">Однако по этой причине следует никогда не использовать строки запроса для конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="b1f03-172">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="b1f03-173">Помимо легко общедоступную, включая данные в строках запроса можно создать возможности для [подделкой межсайтовых запросов (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) атак, которые может сбить с толку пользователей в посещении вредоносных веб-узлов во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b1f03-173">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="b1f03-174">Злоумышленники могут затем похитить данные пользователя из приложения или вредоносных действий от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="b1f03-174">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="b1f03-175">Любое сохраненное состояние application или session необходимо защититься от атак CSRF.</span><span class="sxs-lookup"><span data-stu-id="b1f03-175">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="b1f03-176">Дополнительные сведения об атаках CSRF см. в разделе [Предотвращение межсайтовой запроса подделки XSRF-атак в ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="b1f03-176">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="b1f03-177">Данные POST и скрытые поля</span><span class="sxs-lookup"><span data-stu-id="b1f03-177">Post data and hidden fields</span></span>

<span data-ttu-id="b1f03-178">Данные можно сохранить в скрытые поля формы и отправки обратно при следующем запросе.</span><span class="sxs-lookup"><span data-stu-id="b1f03-178">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="b1f03-179">Обычно это происходит в многостраничных форм.</span><span class="sxs-lookup"><span data-stu-id="b1f03-179">This is common in multi-page forms.</span></span> <span data-ttu-id="b1f03-180">Тем не менее поскольку клиент потенциально могут изменять данные, сервер должен всегда тщательно проверяйте его.</span><span class="sxs-lookup"><span data-stu-id="b1f03-180">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="b1f03-181">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="b1f03-181">Cookies</span></span>

<span data-ttu-id="b1f03-182">Файлы cookie предоставляют способ хранения данных конкретного пользователя в веб-приложениях.</span><span class="sxs-lookup"><span data-stu-id="b1f03-182">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="b1f03-183">Поскольку файлы cookie, отправленных с каждым запросом, их размера должна быть сведена к минимуму.</span><span class="sxs-lookup"><span data-stu-id="b1f03-183">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="b1f03-184">В идеальном случае только идентификатор должны храниться в файле cookie с текущими данными, хранящимися на сервере.</span><span class="sxs-lookup"><span data-stu-id="b1f03-184">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="b1f03-185">Большинство браузеров ограничить файлы Cookie до 4096 байт.</span><span class="sxs-lookup"><span data-stu-id="b1f03-185">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="b1f03-186">Кроме того для каждого домена доступны только ограниченное число файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="b1f03-186">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="b1f03-187">Поскольку файлы cookie могут быть прочитаны, они должны быть проверены на сервере.</span><span class="sxs-lookup"><span data-stu-id="b1f03-187">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="b1f03-188">Несмотря на то, что надежность куки-файл на компьютере клиента регулируется вмешательства пользователя и срок действия, они обычно являются более долговременной формой сохранения данных на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="b1f03-188">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="b1f03-189">Файлы cookie часто используются для персонализации, если содержимое настраивается для известного пользователя.</span><span class="sxs-lookup"><span data-stu-id="b1f03-189">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="b1f03-190">Так как только после определения и не прошел проверку подлинности, в большинстве случаев пользователь, обычно можно защитить файл cookie путем сохранения имени пользователя, имя учетной записи или уникальный ID пользователя (например, GUID) в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="b1f03-190">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="b1f03-191">Затем можно использовать файл cookie для доступа к персональной инфраструктуре веб-узла.</span><span class="sxs-lookup"><span data-stu-id="b1f03-191">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="b1f03-192">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="b1f03-192">HttpContext.Items</span></span>

<span data-ttu-id="b1f03-193">`Items` Коллекция имеет хорошее расположение для хранения данных, надобности только во время обработки одного конкретного запроса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-193">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="b1f03-194">Содержимое коллекции удаляются после каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-194">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="b1f03-195">`Items` Коллекции лучше всего использовать как способ для компонентов или по промежуточного слоя для обмена данными, если они работают в разные моменты времени, во время запроса и имеют прямой способ передачи параметров.</span><span class="sxs-lookup"><span data-stu-id="b1f03-195">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="b1f03-196">Дополнительные сведения см. в разделе [работа с HttpContext.Items](#working-with-httpcontextitems)далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="b1f03-196">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="b1f03-197">Кэш</span><span class="sxs-lookup"><span data-stu-id="b1f03-197">Cache</span></span>

<span data-ttu-id="b1f03-198">Кэширование — эффективный способ хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="b1f03-198">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="b1f03-199">Можно управлять временем жизни кэшированных элементов на основе времени и другие вопросы.</span><span class="sxs-lookup"><span data-stu-id="b1f03-199">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="b1f03-200">Дополнительные сведения о [кэширование](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="b1f03-200">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="b1f03-201">Работа с состоянием сеанса</span><span class="sxs-lookup"><span data-stu-id="b1f03-201">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="b1f03-202">Настройка сеанса</span><span class="sxs-lookup"><span data-stu-id="b1f03-202">Configuring Session</span></span>

<span data-ttu-id="b1f03-203">`Microsoft.AspNetCore.Session` Пакет предоставляет по промежуточного слоя для управления состоянием сеанса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-203">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="b1f03-204">Чтобы включить сеанс по промежуточного слоя, `Startup` должен содержать:</span><span class="sxs-lookup"><span data-stu-id="b1f03-204">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="b1f03-205">Любой из [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) кэшей памяти.</span><span class="sxs-lookup"><span data-stu-id="b1f03-205">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="b1f03-206">`IDistributedCache` Реализация используется в качестве резервного хранилища для сеанса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-206">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="b1f03-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) вызов, требующий пакет NuGet «Microsoft.AspNetCore.Session».</span><span class="sxs-lookup"><span data-stu-id="b1f03-207">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="b1f03-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) вызова.</span><span class="sxs-lookup"><span data-stu-id="b1f03-208">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="b1f03-209">Ниже показано, как для настройки поставщика сеансов в памяти.</span><span class="sxs-lookup"><span data-stu-id="b1f03-209">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1f03-210">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1f03-210">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1f03-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1f03-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="b1f03-212">Можно ссылаться на сеанс из `HttpContext` после его установки и настройки.</span><span class="sxs-lookup"><span data-stu-id="b1f03-212">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="b1f03-213">Если при попытке доступа к `Session` перед `UseSession` был вызван, исключение `InvalidOperationException: Session has not been configured for this application or request` возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="b1f03-213">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="b1f03-214">При попытке создать новый `Session` (то есть, объекты cookie сеанса не был создан) после начала записи в `Response` потока, исключение `InvalidOperationException: The session cannot be established after the response has started` возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="b1f03-214">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="b1f03-215">Исключение, можно найти в журнале веб-сервера; он не будет отображаться в браузере.</span><span class="sxs-lookup"><span data-stu-id="b1f03-215">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="b1f03-216">Асинхронная загрузка сеанса</span><span class="sxs-lookup"><span data-stu-id="b1f03-216">Loading Session asynchronously</span></span> 

<span data-ttu-id="b1f03-217">Поставщик сеанса по умолчанию в ASP.NET Core запись сеанса загружается из основного [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) хранилище асинхронно только тогда, когда [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) до явного вызова метода  `TryGetValue`, `Set`, или `Remove` методы.</span><span class="sxs-lookup"><span data-stu-id="b1f03-217">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="b1f03-218">Если `LoadAsync` не вызывается во-первых, базовый сеанс записи загружается синхронно, который могут повлиять на возможность масштабирования приложения.</span><span class="sxs-lookup"><span data-stu-id="b1f03-218">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="b1f03-219">Чтобы применить этот шаблон приложения, перенос [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) и [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) реализации с версиями, если исключение `LoadAsync` метод не является Вызывается перед `TryGetValue`, `Set`, или `Remove`.</span><span class="sxs-lookup"><span data-stu-id="b1f03-219">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="b1f03-220">Зарегистрируйте оболочку версии в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="b1f03-220">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="b1f03-221">Сведения о реализации</span><span class="sxs-lookup"><span data-stu-id="b1f03-221">Implementation Details</span></span>

<span data-ttu-id="b1f03-222">Сеанс использует файл cookie для отслеживания и идентификации запросов из одного браузера.</span><span class="sxs-lookup"><span data-stu-id="b1f03-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="b1f03-223">По умолчанию этот файл cookie с именем «. AspNet.Session», который использует путь «/».</span><span class="sxs-lookup"><span data-stu-id="b1f03-223">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="b1f03-224">Так как файл cookie по умолчанию домен не указан, он не становится доступным клиентский сценарий на странице (поскольку `CookieHttpOnly` по умолчанию используется значение `true`).</span><span class="sxs-lookup"><span data-stu-id="b1f03-224">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="b1f03-225">Чтобы переопределить значения по умолчанию сеанс, используйте `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="b1f03-225">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b1f03-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b1f03-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b1f03-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b1f03-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="b1f03-228">Сервер использует `IdleTimeout` свойства, чтобы определить, как долго сеанс может быть неактивным до его содержимое оставляются.</span><span class="sxs-lookup"><span data-stu-id="b1f03-228">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="b1f03-229">Это свойство не зависит от срок действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="b1f03-229">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="b1f03-230">Каждый запрос, который проходит через по промежуточного слоя сеанса (чтение и запись для данных) сбрасывает время ожидания.</span><span class="sxs-lookup"><span data-stu-id="b1f03-230">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="b1f03-231">Поскольку `Session` — *неблокирующую*, если два запроса как попытка изменить содержимое сеанса последним переопределяет первый.</span><span class="sxs-lookup"><span data-stu-id="b1f03-231">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="b1f03-232">`Session`реализуется в виде *согласованного сеанса*, что означает, что все содержимое хранятся вместе.</span><span class="sxs-lookup"><span data-stu-id="b1f03-232">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="b1f03-233">Два запроса, изменяющие различные части сеанса (разные ключи) по-прежнему могут повлиять друг на друга.</span><span class="sxs-lookup"><span data-stu-id="b1f03-233">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="b1f03-234">Настройка и получение значения сеанса</span><span class="sxs-lookup"><span data-stu-id="b1f03-234">Setting and getting Session values</span></span>

<span data-ttu-id="b1f03-235">Сеанс осуществляется с помощью `Session` свойство `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="b1f03-235">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="b1f03-236">Это свойство является [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) реализации.</span><span class="sxs-lookup"><span data-stu-id="b1f03-236">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="b1f03-237">В следующем примере показано задание и получение int и строку:</span><span class="sxs-lookup"><span data-stu-id="b1f03-237">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="b1f03-238">При добавлении следующие методы расширения, можно задать и получить сериализуемые объекты для сеанса:</span><span class="sxs-lookup"><span data-stu-id="b1f03-238">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="b1f03-239">Следующий пример показано, как задать и получить сериализуемый объект:</span><span class="sxs-lookup"><span data-stu-id="b1f03-239">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="b1f03-240">Работа с HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="b1f03-240">Working with HttpContext.Items</span></span>

<span data-ttu-id="b1f03-241">`HttpContext` Абстракции обеспечивает поддержку для коллекции словаря типа `IDictionary<object, object>`, который называется `Items`.</span><span class="sxs-lookup"><span data-stu-id="b1f03-241">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="b1f03-242">Эта коллекция доступна с начала *HttpRequest* и удаляются в конце каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-242">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="b1f03-243">Его можно открыть, путем присвоения значения в запись с ключом, или с запросом на ввод значения для определенного ключа.</span><span class="sxs-lookup"><span data-stu-id="b1f03-243">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="b1f03-244">В приведенном ниже примере [по промежуточного слоя](middleware.md) добавляет `isVerified` для `Items` коллекции.</span><span class="sxs-lookup"><span data-stu-id="b1f03-244">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="b1f03-245">Далее в конвейере другого по промежуточного слоя может получить доступ к его:</span><span class="sxs-lookup"><span data-stu-id="b1f03-245">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="b1f03-246">По промежуточного слоя, который будет использовать только одно приложение `string` ключи являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="b1f03-246">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="b1f03-247">Тем не менее по промежуточного слоя, который будет совместно использоваться приложениями следует использовать уникальный объект во избежание любой вероятность возникновения конфликтов.</span><span class="sxs-lookup"><span data-stu-id="b1f03-247">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="b1f03-248">При разработке по промежуточного слоя, который будет работать с несколькими приложениями, используйте уникальный объект ключа, определенных в классе по промежуточного слоя, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="b1f03-248">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

<span data-ttu-id="b1f03-249">Другой код может получить доступ к значение, хранящееся в `HttpContext.Items` с помощью ключа, предоставляемые классом по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="b1f03-249">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="b1f03-250">Данный подход также имеет преимущество устраняя повторение «magic строки» в нескольких местах в коде.</span><span class="sxs-lookup"><span data-stu-id="b1f03-250">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="b1f03-251">Данные состояния приложения</span><span class="sxs-lookup"><span data-stu-id="b1f03-251">Application state data</span></span>

<span data-ttu-id="b1f03-252">Используйте [внедрения зависимостей](xref:fundamentals/dependency-injection) чтобы сделать данные доступными для всех пользователей:</span><span class="sxs-lookup"><span data-stu-id="b1f03-252">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="b1f03-253">Определения службы, содержащий данные (например, класс с именем `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="b1f03-253">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="b1f03-254">Добавление класса службы для `ConfigureServices` (например `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="b1f03-254">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="b1f03-255">Использование класса службы данных в каждом контроллере:</span><span class="sxs-lookup"><span data-stu-id="b1f03-255">Consume the data service class in each controller:</span></span>

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="b1f03-256">Распространенные ошибки при работе с сеансом</span><span class="sxs-lookup"><span data-stu-id="b1f03-256">Common errors when working with session</span></span>

* <span data-ttu-id="b1f03-257">«Не удается разрешить службу типа «Microsoft.Extensions.Caching.Distributed.IDistributedCache» при попытке активации «Microsoft.AspNetCore.Session.DistributedSessionStore»».</span><span class="sxs-lookup"><span data-stu-id="b1f03-257">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="b1f03-258">Обычно это вызвано неправильная настройка по крайней мере один `IDistributedCache` реализации.</span><span class="sxs-lookup"><span data-stu-id="b1f03-258">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="b1f03-259">Дополнительные сведения см. в разделе [работа с распределенного кэша](xref:performance/caching/distributed) и [в кэширование памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="b1f03-259">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="b1f03-260">В случае, когда сеанс по промежуточного слоя не удается сохранить сеанс (например: Если база данных недоступна), он регистрирует исключение и поглощает его.</span><span class="sxs-lookup"><span data-stu-id="b1f03-260">In the event that the session middleware fails to persist a session (for example: if the database is not available), it logs the exception and swallows it.</span></span> <span data-ttu-id="b1f03-261">Запрос обычно будет продолжена что приводит к очень к непредсказуемому поведению.</span><span class="sxs-lookup"><span data-stu-id="b1f03-261">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="b1f03-262">Типичный пример:</span><span class="sxs-lookup"><span data-stu-id="b1f03-262">A typical example:</span></span>

<span data-ttu-id="b1f03-263">Кто-то сохраняет корзину для покупок в сеансе.</span><span class="sxs-lookup"><span data-stu-id="b1f03-263">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="b1f03-264">Пользователь добавляет элемент, но фиксация завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="b1f03-264">The user adds an item but the commit fails.</span></span> <span data-ttu-id="b1f03-265">Приложение не знает о сбое, он выводит сообщение «добавлен элемент», не так.</span><span class="sxs-lookup"><span data-stu-id="b1f03-265">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="b1f03-266">Проверка на наличие таких ошибок рекомендуется вызывать `await feature.Session.CommitAsync();` из кода приложения по окончании записи сеанса.</span><span class="sxs-lookup"><span data-stu-id="b1f03-266">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="b1f03-267">Затем можно сделать, что вам понравилось с ошибкой.</span><span class="sxs-lookup"><span data-stu-id="b1f03-267">Then you can do what you like with the error.</span></span> <span data-ttu-id="b1f03-268">Он работает так же, как при вызове `LoadAsync`.</span><span class="sxs-lookup"><span data-stu-id="b1f03-268">It works the same way when calling `LoadAsync`.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="b1f03-269">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b1f03-269">Additional Resources</span></span>


* [<span data-ttu-id="b1f03-270">ASP.NET Core 1.x: пример кода, используемого в данном документе</span><span class="sxs-lookup"><span data-stu-id="b1f03-270">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="b1f03-271">ASP.NET Core 2.x: пример кода, используемого в данном документе</span><span class="sxs-lookup"><span data-stu-id="b1f03-271">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
