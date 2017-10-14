---
title: "Состояние сеанса и приложения в ASP.NET Core"
author: rick-anderson
description: "Методы сохранения приложения и состояние пользователя (сеанс) между запросами."
keywords: "Учет ASP.NET Core, состояние приложения, состояние сеанса, строки запроса,"
ms.author: riande
manager: wpickett
ms.date: 10/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d4d10ef45d562f34c3f8b5ce025abaf763c862d3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="021e0-104">Общие сведения о состоянии сеанса и приложения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="021e0-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="021e0-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Стив Смит](https://ardalis.com/), и [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="021e0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="021e0-106">Протокол HTTP является протоколом без состояния.</span><span class="sxs-lookup"><span data-stu-id="021e0-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="021e0-107">Веб-сервер обрабатывает каждый HTTP-запрос как независимый запрос и не сохраняет значения пользователя из предыдущих запросов.</span><span class="sxs-lookup"><span data-stu-id="021e0-107">A web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="021e0-108">В этой статье описаны различные способы сохранить состояние приложения и сеанса между запросами.</span><span class="sxs-lookup"><span data-stu-id="021e0-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="021e0-109">Состояние сеанса</span><span class="sxs-lookup"><span data-stu-id="021e0-109">Session state</span></span>

<span data-ttu-id="021e0-110">Состояние сеанса — это функция в ASP.NET Core, которую можно использовать для сохранения пользовательских данных во время просмотра веб-приложения пользователем.</span><span class="sxs-lookup"><span data-stu-id="021e0-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="021e0-111">Состоящий из словаря или хэш-таблицы на сервере, состояние сеанса сохраняет данные для запросов из браузера.</span><span class="sxs-lookup"><span data-stu-id="021e0-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="021e0-112">Поддерживаемый кэш данных сеанса.</span><span class="sxs-lookup"><span data-stu-id="021e0-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="021e0-113">ASP.NET Core сохраняет состояние сеанса, предоставляя файл cookie, который содержит идентификатор сеанса, который отправляется на сервер при каждом запросе клиента.</span><span class="sxs-lookup"><span data-stu-id="021e0-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="021e0-114">Сервер использует идентификатор сеанса для выборки данных сеанса.</span><span class="sxs-lookup"><span data-stu-id="021e0-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="021e0-115">Так как файл cookie сеанса является специфичным для браузера, не могут совместно использовать сеансы браузерами.</span><span class="sxs-lookup"><span data-stu-id="021e0-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="021e0-116">Файлы cookie сеанса, удаляются только в том случае, при завершении сеанса браузера.</span><span class="sxs-lookup"><span data-stu-id="021e0-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="021e0-117">Если файл cookie поступает просроченного сеанса, создается новый сеанс, в котором используется один и тот же файл cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="021e0-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="021e0-118">Сервер сохраняет сеанс в течение ограниченного времени после выполнения последнего запроса.</span><span class="sxs-lookup"><span data-stu-id="021e0-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="021e0-119">Можно задать время ожидания сеанса или использовать значение по умолчанию 20 минут.</span><span class="sxs-lookup"><span data-stu-id="021e0-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="021e0-120">Состояние сеанса идеально подходит для хранения пользовательских данных, связанную с определенным сеансом, но может не быть сохранен без возможности восстановления.</span><span class="sxs-lookup"><span data-stu-id="021e0-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="021e0-121">Данные удаляются из резервного хранилища либо при вызове `Session.Clear` или после окончания сеанса в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="021e0-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="021e0-122">Сервер не может определить при закрытии браузера или при удалении файла cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="021e0-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="021e0-123">Не храните конфиденциальные данные в сеансе.</span><span class="sxs-lookup"><span data-stu-id="021e0-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="021e0-124">Клиент не может закрыть браузер и очистить файл cookie сеанса (и некоторые браузеры сохранения файлов cookie сеанса через windows).</span><span class="sxs-lookup"><span data-stu-id="021e0-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="021e0-125">Кроме того сеанс не может быть ограничен до одного пользователя; Следующий пользователь может по-прежнему с того же сеанса.</span><span class="sxs-lookup"><span data-stu-id="021e0-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="021e0-126">Поставщик сеансов в памяти сохраняет данные сеанса на локальном сервере.</span><span class="sxs-lookup"><span data-stu-id="021e0-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="021e0-127">Если вы планируете запускать веб-приложения на ферме серверов, необходимо использовать закрепленные сеансы для каждого сеанса к определенному серверу перегрузки.</span><span class="sxs-lookup"><span data-stu-id="021e0-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="021e0-128">По умолчанию платформа веб-сайтов Windows Azure для прикрепленных сеансов (маршрутизации запросов приложений или ARR).</span><span class="sxs-lookup"><span data-stu-id="021e0-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="021e0-129">Тем не менее закрепленные сеансы могут повлиять на масштабируемость и усложнить обновления веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="021e0-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="021e0-130">Лучшим вариантом является использование команды Redis или кэширует распределенных SQL Server, не требующую закрепленных сеансов.</span><span class="sxs-lookup"><span data-stu-id="021e0-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="021e0-131">Дополнительные сведения см. в разделе [работа с распределенного кэша](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="021e0-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="021e0-132">Дополнительные сведения о настройке поставщиков услуг см. в разделе [Настройка сеанса](#configuring-session) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="021e0-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>


<a name="temp"></a>
## <a name="tempdata"></a><span data-ttu-id="021e0-133">TempData</span><span class="sxs-lookup"><span data-stu-id="021e0-133">TempData</span></span>

<span data-ttu-id="021e0-134">Основные ASP.NET MVC предоставляет [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) свойство [контроллера](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="021e0-134">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="021e0-135">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="021e0-135">This property stores data until it is read.</span></span> <span data-ttu-id="021e0-136">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="021e0-136">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="021e0-137">`TempData`особенно полезна для перенаправления, когда данные, необходимые для более чем одного запроса.</span><span class="sxs-lookup"><span data-stu-id="021e0-137">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="021e0-138">`TempData`реализуется TempData поставщиков, например, с помощью файлов cookie или с состоянием сеанса.</span><span class="sxs-lookup"><span data-stu-id="021e0-138">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a><span data-ttu-id="021e0-139">Поставщики TempData</span><span class="sxs-lookup"><span data-stu-id="021e0-139">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="021e0-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="021e0-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="021e0-141">В ASP.NET Core 2.0 и более поздних версиях поставщика на основе файлов cookie TempData используется по умолчанию для хранения TempData в файлах cookie.</span><span class="sxs-lookup"><span data-stu-id="021e0-141">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="021e0-142">Данные cookie кодируется с [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="021e0-142">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0).</span></span> <span data-ttu-id="021e0-143">Один файл cookie, так как файл cookie шифруется и фрагментирован, размером в ASP.NET Core 1.x не применяется ограничение.</span><span class="sxs-lookup"><span data-stu-id="021e0-143">Because the cookie is encrypted and chunked, the single cookie size limit found in ASP.NET Core 1.x does not apply.</span></span> <span data-ttu-id="021e0-144">Данные cookie не сжимается, поскольку сжатие данных encryped может привести к проблемам безопасности таких как [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушения](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.</span><span class="sxs-lookup"><span data-stu-id="021e0-144">The cookie data is not compressed because compressing encryped data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="021e0-145">Дополнительные сведения на основе файлов cookie TempData поставщика см. в разделе [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="021e0-145">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="021e0-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="021e0-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="021e0-147">В ASP.NET Core 1.0 и 1.1 поставщик TempData состояния сеанса используется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="021e0-147">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

--------------

### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="021e0-148">Выбор поставщика TempData</span><span class="sxs-lookup"><span data-stu-id="021e0-148">Choosing a TempData provider</span></span>

<span data-ttu-id="021e0-149">Выбор поставщика TempData включает в себя несколько факторов, таких как:</span><span class="sxs-lookup"><span data-stu-id="021e0-149">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="021e0-150">Используется ли приложение уже состояния сеанса для других целей?</span><span class="sxs-lookup"><span data-stu-id="021e0-150">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="021e0-151">В этом случае использование TempData поставщика состояния сеанса имеет без дополнительной оплаты (помимо объема данных) в приложение.</span><span class="sxs-lookup"><span data-stu-id="021e0-151">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="021e0-152">Приложение использует TempData только ограниченно для сравнительно небольших объемов данных (до 500 байт)?</span><span class="sxs-lookup"><span data-stu-id="021e0-152">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="021e0-153">Если таким образом, поставщик TempData куки-файл будет добавлен небольшой стоимость каждого запроса, который выполняет TempData.</span><span class="sxs-lookup"><span data-stu-id="021e0-153">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="021e0-154">В противном случае TempData поставщика состояния сеанса может быть полезно во избежание циклической обработки большой объем данных в каждом запросе до полного исчерпания TempData.</span><span class="sxs-lookup"><span data-stu-id="021e0-154">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="021e0-155">Приложение выполняется на веб-ферме (несколько серверов)?</span><span class="sxs-lookup"><span data-stu-id="021e0-155">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="021e0-156">В этом случае имеется дополнительная настройка не требуется для использования поставщика TempData куки-файл.</span><span class="sxs-lookup"><span data-stu-id="021e0-156">If so, there is no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="021e0-157">Большинство веб-клиентов (например, веб-браузеры) соблюдения ограничений на максимальный размер каждого файла cookie и общее количество файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="021e0-157">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="021e0-158">Таким образом при использовании поставщика TempData куки-файл, убедитесь, что приложение не будет превышать эти пределы.</span><span class="sxs-lookup"><span data-stu-id="021e0-158">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="021e0-159">Рассмотрим общий объем данных, учитывая накладных расходов, шифрования и фрагментации.</span><span class="sxs-lookup"><span data-stu-id="021e0-159">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

<span data-ttu-id="021e0-160">Чтобы настроить поставщик TempData для приложения, зарегистрируйте реализации поставщика TempData в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="021e0-160">To configure the TempData provider for an application, register a TempData provider implementation in `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddMvc()
        .AddSessionStateTempDataProvider();

    // The Session State TempData Provider requires adding the session state service
    services.AddSession();
}
```

## <a name="query-strings"></a><span data-ttu-id="021e0-161">Строки запросов</span><span class="sxs-lookup"><span data-stu-id="021e0-161">Query strings</span></span>

<span data-ttu-id="021e0-162">Можно передать ограниченный объем данных от одного запроса в другой, добавьте его в строку запроса нового запроса.</span><span class="sxs-lookup"><span data-stu-id="021e0-162">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="021e0-163">Это полезно для записи состояния постоянных способом, который позволяет связи внедренные состояния для совместного использования по электронной почте или социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="021e0-163">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="021e0-164">Однако по этой причине следует никогда не использовать строки запроса для конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="021e0-164">However, for this reason,  you should never use query strings for sensitive data.</span></span> <span data-ttu-id="021e0-165">Помимо легко общедоступную, включая данные в строках запроса можно создать возможности для [подделкой межсайтовых запросов (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) атак, которые может сбить с толку пользователей в посещении вредоносных веб-узлов во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="021e0-165">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="021e0-166">Злоумышленники могут затем похитить данные пользователя из приложения или вредоносных действий от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="021e0-166">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="021e0-167">Любое сохраненное состояние application или session необходимо защититься от атак CSRF.</span><span class="sxs-lookup"><span data-stu-id="021e0-167">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="021e0-168">Дополнительные сведения об атаках CSRF см. в разделе [Предотвращение межсайтовой запроса подделки XSRF-атак в ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="021e0-168">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="021e0-169">Данные POST и скрытые поля</span><span class="sxs-lookup"><span data-stu-id="021e0-169">Post data and hidden fields</span></span>

<span data-ttu-id="021e0-170">Данные можно сохранить в скрытые поля формы и отправки обратно при следующем запросе.</span><span class="sxs-lookup"><span data-stu-id="021e0-170">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="021e0-171">Обычно это происходит в многостраничных форм.</span><span class="sxs-lookup"><span data-stu-id="021e0-171">This is common in multipage forms.</span></span> <span data-ttu-id="021e0-172">Тем не менее поскольку клиент потенциально могут изменять данные, сервер должен всегда тщательно проверяйте его.</span><span class="sxs-lookup"><span data-stu-id="021e0-172">However, because the  client can potentially tamper with the data, the server must always revalidate it.</span></span> 

## <a name="cookies"></a><span data-ttu-id="021e0-173">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="021e0-173">Cookies</span></span>

<span data-ttu-id="021e0-174">Файлы cookie предоставляют способ хранения данных конкретного пользователя в веб-приложениях.</span><span class="sxs-lookup"><span data-stu-id="021e0-174">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="021e0-175">Поскольку файлы cookie, отправленных с каждым запросом, их размера должна быть сведена к минимуму.</span><span class="sxs-lookup"><span data-stu-id="021e0-175">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="021e0-176">В идеальном случае только идентификатор должны храниться в файле cookie с текущими данными, хранящимися на сервере.</span><span class="sxs-lookup"><span data-stu-id="021e0-176">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="021e0-177">Большинство браузеров ограничить файлы Cookie до 4096 байт.</span><span class="sxs-lookup"><span data-stu-id="021e0-177">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="021e0-178">Кроме того для каждого домена доступны только ограниченное число файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="021e0-178">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="021e0-179">Поскольку файлы cookie могут быть прочитаны, они должны быть проверены на сервере.</span><span class="sxs-lookup"><span data-stu-id="021e0-179">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="021e0-180">Несмотря на то, что надежность куки-файл на компьютере клиента регулируется вмешательства пользователя и срок действия, они обычно являются более долговременной формой сохранения данных на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="021e0-180">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="021e0-181">Файлы cookie часто используются для персонализации, если содержимое настраивается для известного пользователя.</span><span class="sxs-lookup"><span data-stu-id="021e0-181">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="021e0-182">Так как только после определения и не прошел проверку подлинности, в большинстве случаев пользователь, обычно можно защитить файл cookie путем сохранения имени пользователя, имя учетной записи или уникальный ID пользователя (например, GUID) в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="021e0-182">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="021e0-183">Затем можно использовать файл cookie для доступа к персональной инфраструктуре веб-узла.</span><span class="sxs-lookup"><span data-stu-id="021e0-183">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="021e0-184">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="021e0-184">HttpContext.Items</span></span>

<span data-ttu-id="021e0-185">`Items` Коллекция имеет хорошее расположение для хранения данных, надобности только во время обработки одного конкретного запроса.</span><span class="sxs-lookup"><span data-stu-id="021e0-185">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="021e0-186">Содержимое коллекции удаляются после каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="021e0-186">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="021e0-187">`Items` Коллекции лучше всего использовать как способ для компонентов или по промежуточного слоя для обмена данными, если они работают в разные моменты времени, во время запроса и имеют прямой способ передачи параметров.</span><span class="sxs-lookup"><span data-stu-id="021e0-187">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="021e0-188">Дополнительные сведения см. в разделе [работа с HttpContext.Items](#working-with-httpcontextitems)далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="021e0-188">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="021e0-189">Кэш</span><span class="sxs-lookup"><span data-stu-id="021e0-189">Cache</span></span>

<span data-ttu-id="021e0-190">Кэширование — эффективный способ хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="021e0-190">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="021e0-191">Можно управлять временем жизни кэшированных элементов на основе времени и другие вопросы.</span><span class="sxs-lookup"><span data-stu-id="021e0-191">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="021e0-192">Дополнительные сведения о [кэширование](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="021e0-192">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name="session"></a>
## <a name="working-with-session-state"></a><span data-ttu-id="021e0-193">Работа с состоянием сеанса</span><span class="sxs-lookup"><span data-stu-id="021e0-193">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="021e0-194">Настройка сеанса</span><span class="sxs-lookup"><span data-stu-id="021e0-194">Configuring Session</span></span>

<span data-ttu-id="021e0-195">`Microsoft.AspNetCore.Session` Пакет предоставляет по промежуточного слоя для управления состоянием сеанса.</span><span class="sxs-lookup"><span data-stu-id="021e0-195">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="021e0-196">Чтобы включить сеанс по промежуточного слоя, `Startup`должен содержать:</span><span class="sxs-lookup"><span data-stu-id="021e0-196">To enable the session middleware, `Startup`must contain:</span></span>

- <span data-ttu-id="021e0-197">Любой из [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) кэшей памяти.</span><span class="sxs-lookup"><span data-stu-id="021e0-197">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="021e0-198">`IDistributedCache` Реализация используется в качестве резервного хранилища для сеанса.</span><span class="sxs-lookup"><span data-stu-id="021e0-198">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="021e0-199">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) вызов, требующий пакет NuGet «Microsoft.AspNetCore.Session».</span><span class="sxs-lookup"><span data-stu-id="021e0-199">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="021e0-200">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) вызова.</span><span class="sxs-lookup"><span data-stu-id="021e0-200">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="021e0-201">Ниже показано, как для настройки поставщика сеансов в памяти.</span><span class="sxs-lookup"><span data-stu-id="021e0-201">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="021e0-202">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="021e0-202">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="021e0-203">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="021e0-203">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="021e0-204">Можно ссылаться на сеанс из `HttpContext` после его установки и настройки.</span><span class="sxs-lookup"><span data-stu-id="021e0-204">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="021e0-205">Если при попытке доступа к `Session` перед `UseSession` был вызван, исключение `InvalidOperationException: Session has not been configured for this application or request` возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="021e0-205">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="021e0-206">При попытке создать новый `Session` (то есть, объекты cookie сеанса не был создан) после начала записи в `Response` потока, исключение `InvalidOperationException: The session cannot be established after the response has started` возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="021e0-206">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="021e0-207">Исключение, можно найти в журнале веб-сервера; он не будет отображаться в браузере.</span><span class="sxs-lookup"><span data-stu-id="021e0-207">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="021e0-208">Асинхронная загрузка сеанса</span><span class="sxs-lookup"><span data-stu-id="021e0-208">Loading Session asynchronously</span></span> 

<span data-ttu-id="021e0-209">Поставщик сеанса по умолчанию в ASP.NET Core запись сеанса загружается из основного [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) хранилище асинхронно только тогда, когда [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) до явного вызова метода  `TryGetValue`, `Set`, или `Remove` методы.</span><span class="sxs-lookup"><span data-stu-id="021e0-209">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="021e0-210">Если `LoadAsync` не вызывается во-первых, базовый сеанс записи загружается синхронно, который могут повлиять на возможность масштабирования приложения.</span><span class="sxs-lookup"><span data-stu-id="021e0-210">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="021e0-211">Чтобы применить этот шаблон приложения, перенос [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) и [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) реализации с версиями, если исключение `LoadAsync` метод не является Вызывается перед `TryGetValue`, `Set`, или `Remove`.</span><span class="sxs-lookup"><span data-stu-id="021e0-211">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="021e0-212">Зарегистрируйте оболочку версии в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="021e0-212">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="021e0-213">Сведения о реализации</span><span class="sxs-lookup"><span data-stu-id="021e0-213">Implementation Details</span></span>

<span data-ttu-id="021e0-214">Сеанс использует файл cookie для отслеживания и идентификации запросов из одного браузера.</span><span class="sxs-lookup"><span data-stu-id="021e0-214">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="021e0-215">По умолчанию этот файл cookie с именем «. AspNet.Session», который использует путь «/».</span><span class="sxs-lookup"><span data-stu-id="021e0-215">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="021e0-216">Так как файл cookie по умолчанию домен не указан, он не становится доступным клиентский сценарий на странице (поскольку `CookieHttpOnly` по умолчанию используется значение `true`).</span><span class="sxs-lookup"><span data-stu-id="021e0-216">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="021e0-217">Чтобы переопределить значения по умолчанию сеанс, используйте `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="021e0-217">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="021e0-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="021e0-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="021e0-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="021e0-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="021e0-220">Сервер использует `IdleTimeout` свойства, чтобы определить, как долго сеанс может быть неактивным до его содержимое оставляются.</span><span class="sxs-lookup"><span data-stu-id="021e0-220">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="021e0-221">Это свойство не зависит от срок действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="021e0-221">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="021e0-222">Каждый запрос, который проходит через по промежуточного слоя сеанса (чтение и запись для данных) сбрасывает время ожидания.</span><span class="sxs-lookup"><span data-stu-id="021e0-222">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="021e0-223">Поскольку `Session` — *неблокирующую*, если два запроса как попытка изменить содержимое сеанса последним переопределяет первый.</span><span class="sxs-lookup"><span data-stu-id="021e0-223">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="021e0-224">`Session`реализуется в виде *согласованного сеанса*, что означает, что все содержимое хранятся вместе.</span><span class="sxs-lookup"><span data-stu-id="021e0-224">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="021e0-225">Два запроса, изменяющие различные части сеанса (разные ключи) по-прежнему могут повлиять друг на друга.</span><span class="sxs-lookup"><span data-stu-id="021e0-225">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="setting-and-getting-session-values"></a><span data-ttu-id="021e0-226">Настройка и получение значения сеанса</span><span class="sxs-lookup"><span data-stu-id="021e0-226">Setting and getting Session values</span></span>

<span data-ttu-id="021e0-227">Сеанс осуществляется с помощью `Session` свойство `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="021e0-227">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="021e0-228">Это свойство является [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) реализации.</span><span class="sxs-lookup"><span data-stu-id="021e0-228">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="021e0-229">В следующем примере показано задание и получение int и строку:</span><span class="sxs-lookup"><span data-stu-id="021e0-229">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="021e0-230">При добавлении следующие методы расширения, можно задать и получить сериализуемые объекты для сеанса:</span><span class="sxs-lookup"><span data-stu-id="021e0-230">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="021e0-231">Следующий пример показано, как задать и получить сериализуемый объект:</span><span class="sxs-lookup"><span data-stu-id="021e0-231">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="021e0-232">Работа с HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="021e0-232">Working with HttpContext.Items</span></span>

<span data-ttu-id="021e0-233">`HttpContext` Абстракции обеспечивает поддержку для коллекции словаря типа `IDictionary<object, object>`, который называется `Items`.</span><span class="sxs-lookup"><span data-stu-id="021e0-233">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="021e0-234">Эта коллекция доступна с начала *HttpRequest* и удаляются в конце каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="021e0-234">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="021e0-235">Его можно открыть, путем присвоения значения в запись с ключом, или с запросом на ввод значения для определенного ключа.</span><span class="sxs-lookup"><span data-stu-id="021e0-235">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="021e0-236">В приведенном ниже примере [по промежуточного слоя](middleware.md) добавляет `isVerified` для `Items` коллекции.</span><span class="sxs-lookup"><span data-stu-id="021e0-236">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="021e0-237">Далее в конвейере другого по промежуточного слоя может получить доступ к его:</span><span class="sxs-lookup"><span data-stu-id="021e0-237">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="021e0-238">По промежуточного слоя, который будет использовать только одно приложение `string` ключи являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="021e0-238">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="021e0-239">Тем не менее по промежуточного слоя, который будет совместно использоваться приложениями следует использовать уникальный объект во избежание любой вероятность возникновения конфликтов.</span><span class="sxs-lookup"><span data-stu-id="021e0-239">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="021e0-240">При разработке по промежуточного слоя, который будет работать с несколькими приложениями, используйте уникальный объект ключа, определенных в классе по промежуточного слоя, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="021e0-240">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

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

<span data-ttu-id="021e0-241">Другой код может получить доступ к значение, хранящееся в `HttpContext.Items` с помощью ключа, предоставляемые классом по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="021e0-241">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="021e0-242">Данный подход также имеет преимущество устраняя повторение «magic строки» в нескольких местах в коде.</span><span class="sxs-lookup"><span data-stu-id="021e0-242">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name="appstate-errors"></a>

## <a name="application-state-data"></a><span data-ttu-id="021e0-243">Данные состояния приложения</span><span class="sxs-lookup"><span data-stu-id="021e0-243">Application state data</span></span>

<span data-ttu-id="021e0-244">Используйте [внедрения зависимостей](xref:fundamentals/dependency-injection) чтобы сделать данные доступными для всех пользователей:</span><span class="sxs-lookup"><span data-stu-id="021e0-244">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="021e0-245">Определения службы, содержащий данные (например, класс с именем `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="021e0-245">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="021e0-246">Добавление класса службы для `ConfigureServices` (например `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="021e0-246">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="021e0-247">Использование класса службы данных в каждом контроллере:</span><span class="sxs-lookup"><span data-stu-id="021e0-247">Consume the data service class in each controller:</span></span>

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

### <a name="common-errors-when-working-with-session"></a><span data-ttu-id="021e0-248">Распространенные ошибки при работе с сеансом</span><span class="sxs-lookup"><span data-stu-id="021e0-248">Common errors when working with session</span></span>

* <span data-ttu-id="021e0-249">«Не удается разрешить службу типа «Microsoft.Extensions.Caching.Distributed.IDistributedCache» при попытке активации «Microsoft.AspNetCore.Session.DistributedSessionStore»».</span><span class="sxs-lookup"><span data-stu-id="021e0-249">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="021e0-250">Обычно это вызвано неправильная настройка по крайней мере один `IDistributedCache` реализации.</span><span class="sxs-lookup"><span data-stu-id="021e0-250">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="021e0-251">Дополнительные сведения см. в разделе [работа с распределенного кэша](xref:performance/caching/distributed) и [в кэширование памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="021e0-251">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="021e0-252">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="021e0-252">Additional Resources</span></span>


* [<span data-ttu-id="021e0-253">ASP.NET Core 1.x: пример кода, используемого в данном документе</span><span class="sxs-lookup"><span data-stu-id="021e0-253">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="021e0-254">ASP.NET Core 2.x: пример кода, используемого в данном документе</span><span class="sxs-lookup"><span data-stu-id="021e0-254">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
