---
title: "Состояние сеанса и приложения в ASP.NET Core"
author: rick-anderson
description: "Методы сохранения приложения и состояние пользователя (сеанс) между запросами."
keywords: "Учет ASP.NET Core, состояние приложения, состояние сеанса, строки запроса,"
ms.author: riande
manager: wpickett
ms.date: 06/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b451bde1e3180d12781d55113638cc1a99182c8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a><span data-ttu-id="7b271-104">Общие сведения о состоянии сеанса и приложения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b271-104">Introduction to session and application state in ASP.NET Core</span></span>

<span data-ttu-id="7b271-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Стив Смит](https://ardalis.com/), и [Diana LaRose](https://github.com/DianaLaRose)</span><span class="sxs-lookup"><span data-stu-id="7b271-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="7b271-106">Протокол HTTP является протоколом без состояния.</span><span class="sxs-lookup"><span data-stu-id="7b271-106">HTTP is a stateless protocol.</span></span> <span data-ttu-id="7b271-107">Веб-сервер обрабатывает каждый HTTP-запрос как независимый запрос и не сохраняет значения пользователя из предыдущих запросов.</span><span class="sxs-lookup"><span data-stu-id="7b271-107">A  web server treats each HTTP request as an independent request and does not retain user values from previous requests.</span></span> <span data-ttu-id="7b271-108">В этой статье описаны различные способы сохранить состояние приложения и сеанса между запросами.</span><span class="sxs-lookup"><span data-stu-id="7b271-108">This article discusses different ways to preserve application and session state between requests.</span></span> 

## <a name="session-state"></a><span data-ttu-id="7b271-109">Состояние сеанса</span><span class="sxs-lookup"><span data-stu-id="7b271-109">Session state</span></span>

<span data-ttu-id="7b271-110">Состояние сеанса — это функция в ASP.NET Core, можно использовать для сохранения и сохранить данные пользователя, пока пользователь просматривает веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="7b271-110">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="7b271-111">Состоящий из словаря или хэш-таблицы на сервере, состояние сеанса сохраняет данные для запросов из браузера.</span><span class="sxs-lookup"><span data-stu-id="7b271-111">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="7b271-112">Поддерживаемый кэш данных сеанса.</span><span class="sxs-lookup"><span data-stu-id="7b271-112">The session data is backed by a cache.</span></span>

<span data-ttu-id="7b271-113">ASP.NET Core сохраняет состояние сеанса, предоставляя файл cookie, который содержит идентификатор сеанса, который отправляется на сервер при каждом запросе клиента.</span><span class="sxs-lookup"><span data-stu-id="7b271-113">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="7b271-114">Сервер использует идентификатор сеанса для выборки данных сеанса.</span><span class="sxs-lookup"><span data-stu-id="7b271-114">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="7b271-115">Так как файл cookie сеанса является специфичным для браузера, не могут совместно использовать сеансы браузерами.</span><span class="sxs-lookup"><span data-stu-id="7b271-115">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="7b271-116">Файлы cookie сеанса, удаляются только в том случае, при завершении сеанса браузера.</span><span class="sxs-lookup"><span data-stu-id="7b271-116">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="7b271-117">Если файл cookie поступает просроченного сеанса, создается новый сеанс, в котором используется один и тот же файл cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="7b271-117">If a cookie is received for an expired session, a new session that uses the same session cookie  is created.</span></span> 

<span data-ttu-id="7b271-118">Сервер сохраняет сеанс в течение ограниченного времени после выполнения последнего запроса.</span><span class="sxs-lookup"><span data-stu-id="7b271-118">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="7b271-119">Можно задать время ожидания сеанса или использовать значение по умолчанию 20 минут.</span><span class="sxs-lookup"><span data-stu-id="7b271-119">You can either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="7b271-120">Состояние сеанса идеально подходит для хранения пользовательских данных, связанную с определенным сеансом, но может не быть сохранен без возможности восстановления.</span><span class="sxs-lookup"><span data-stu-id="7b271-120">Session state is ideal for storing user data that is specific to a particular session but doesn’t need to be persisted permanently.</span></span> <span data-ttu-id="7b271-121">Данные удаляются из резервного хранилища либо при вызове `Session.Clear` или после окончания сеанса в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="7b271-121">Data is deleted from the backing store either when you call `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="7b271-122">Сервер не может определить при закрытии браузера или при удалении файла cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="7b271-122">The server does not know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="7b271-123">Не храните конфиденциальные данные в сеансе.</span><span class="sxs-lookup"><span data-stu-id="7b271-123">Do not store sensitive data in session.</span></span> <span data-ttu-id="7b271-124">Клиент не может закрыть браузер и очистить файл cookie сеанса (и некоторые браузеры сохранения файлов cookie сеанса через windows).</span><span class="sxs-lookup"><span data-stu-id="7b271-124">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="7b271-125">Кроме того сеанс не может быть ограничен до одного пользователя; Следующий пользователь может по-прежнему с того же сеанса.</span><span class="sxs-lookup"><span data-stu-id="7b271-125">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="7b271-126">Поставщик сеансов в памяти сохраняет данные сеанса на локальном сервере.</span><span class="sxs-lookup"><span data-stu-id="7b271-126">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="7b271-127">Если вы планируете запускать веб-приложения на ферме серверов, необходимо использовать закрепленные сеансы для каждого сеанса к определенному серверу перегрузки.</span><span class="sxs-lookup"><span data-stu-id="7b271-127">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="7b271-128">По умолчанию платформа веб-сайтов Windows Azure для прикрепленных сеансов (маршрутизации запросов приложений или ARR).</span><span class="sxs-lookup"><span data-stu-id="7b271-128">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="7b271-129">Тем не менее закрепленные сеансы могут повлиять на масштабируемость и усложнить обновления веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="7b271-129">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="7b271-130">Лучшим вариантом является использование команды Redis или кэширует распределенных SQL Server, не требующую закрепленных сеансов.</span><span class="sxs-lookup"><span data-stu-id="7b271-130">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="7b271-131">Дополнительные сведения см. в разделе [работа с распределенного кэша](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="7b271-131">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="7b271-132">Дополнительные сведения о настройке поставщиков услуг см. в разделе [Настройка сеанса](#configuring-session) далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="7b271-132">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

<span data-ttu-id="7b271-133">В оставшейся части этого раздела описываются параметры для хранения пользовательских данных.</span><span class="sxs-lookup"><span data-stu-id="7b271-133">The remainder of this section describes the options for storing user data.</span></span>

<a name="temp"></a>
### <a name="tempdata"></a><span data-ttu-id="7b271-134">TempData</span><span class="sxs-lookup"><span data-stu-id="7b271-134">TempData</span></span>

<span data-ttu-id="7b271-135">Основные ASP.NET MVC предоставляет [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) свойство [контроллера](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="7b271-135">ASP.NET Core MVC exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="7b271-136">Это свойство хранит данные до чтения.</span><span class="sxs-lookup"><span data-stu-id="7b271-136">This property stores data until it is read.</span></span> <span data-ttu-id="7b271-137">`Keep` И `Peek` методы можно использовать для изучения данных без удаления.</span><span class="sxs-lookup"><span data-stu-id="7b271-137">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="7b271-138">`TempData`особенно полезна для перенаправления, когда данные, необходимые для более чем одного запроса.</span><span class="sxs-lookup"><span data-stu-id="7b271-138">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="7b271-139">`TempData`построено на основе состояния сеанса.</span><span class="sxs-lookup"><span data-stu-id="7b271-139">`TempData` is built on top of session state.</span></span> 

## <a name="cookie-based-tempdata-provider"></a><span data-ttu-id="7b271-140">На основе файла cookie TempData поставщика</span><span class="sxs-lookup"><span data-stu-id="7b271-140">Cookie-based TempData provider</span></span> 

<span data-ttu-id="7b271-141">В ASP.NET Core 1.1 и более поздних версиях можно использовать поставщик TempData основе файлов cookie для хранения TempData пользователя в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="7b271-141">In ASP.NET Core 1.1 and higher, you can use the cookie-based TempData provider to store a user's TempData in a cookie.</span></span> <span data-ttu-id="7b271-142">Чтобы включить поставщик TempData основе файлов cookie, зарегистрировать `CookieTempDataProvider` в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7b271-142">To enable the  cookie-based TempData provider, register the `CookieTempDataProvider` service in `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add CookieTempDataProvider after AddMvc and include ViewFeatures.
    // using Microsoft.AspNetCore.Mvc.ViewFeatures;
    services.AddSingleton<ITempDataProvider, CookieTempDataProvider>();
}
```

<span data-ttu-id="7b271-143">Данные cookie кодируется с [Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder).</span><span class="sxs-lookup"><span data-stu-id="7b271-143">The cookie data is encoded with the [Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder).</span></span> <span data-ttu-id="7b271-144">Так как файл cookie шифруется и фрагментирован, предельный размер одного файла cookie не применяется.</span><span class="sxs-lookup"><span data-stu-id="7b271-144">Because the cookie is encrypted and chunked, the single cookie size limit does not apply.</span></span> <span data-ttu-id="7b271-145">Данные cookie не сжимается, поскольку сжатие данных encryped может привести к проблемам безопасности таких как [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [нарушения](https://wikipedia.org/wiki/BREACH_(security_exploit)) атак.</span><span class="sxs-lookup"><span data-stu-id="7b271-145">The cookie data is not compressed, because compressing encryped data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="7b271-146">Дополнительные сведения на основе файлов cookie TempData поставщика см. в разделе [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="7b271-146">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

### <a name="query-strings"></a><span data-ttu-id="7b271-147">Строки запросов</span><span class="sxs-lookup"><span data-stu-id="7b271-147">Query strings</span></span>

<span data-ttu-id="7b271-148">Можно передать ограниченный объем данных от одного запроса в другой, добавьте его в строку запроса нового запроса.</span><span class="sxs-lookup"><span data-stu-id="7b271-148">You can pass a limited amount of data from one request to another by adding it to the new request’s query string.</span></span> <span data-ttu-id="7b271-149">Это полезно для записи состояния постоянных способом, который позволяет связи внедренные состояния для совместного использования по электронной почте или социальных сетей.</span><span class="sxs-lookup"><span data-stu-id="7b271-149">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="7b271-150">Однако по этой причине следует никогда не использовать строки запроса для конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="7b271-150">However, for this reason,  you should never use query strings for sensitive data.</span></span> <span data-ttu-id="7b271-151">Помимо легко общедоступную, включая данные в строках запроса можно создать возможности для [подделкой межсайтовых запросов (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) атак, которые может сбить с толку пользователей в посещении вредоносных веб-узлов во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="7b271-151">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="7b271-152">Злоумышленники могут затем похитить данные пользователя из приложения или вредоносных действий от имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="7b271-152">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="7b271-153">Любое сохраненное состояние application или session необходимо защититься от атак CSRF.</span><span class="sxs-lookup"><span data-stu-id="7b271-153">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="7b271-154">Дополнительные сведения об атаках CSRF см. в разделе [Предотвращение межсайтовой запроса подделки XSRF-атак в ASP.NET Core](../security/anti-request-forgery.md).</span><span class="sxs-lookup"><span data-stu-id="7b271-154">For more information on CSRF attacks, see [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core](../security/anti-request-forgery.md).</span></span>

### <a name="post-data-and-hidden-fields"></a><span data-ttu-id="7b271-155">Данные POST и скрытые поля</span><span class="sxs-lookup"><span data-stu-id="7b271-155">Post data and hidden fields</span></span>

<span data-ttu-id="7b271-156">Данные можно сохранить в скрытые поля формы и отправки обратно при следующем запросе.</span><span class="sxs-lookup"><span data-stu-id="7b271-156">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="7b271-157">Обычно это происходит в многостраничных форм.</span><span class="sxs-lookup"><span data-stu-id="7b271-157">This is common in multipage forms.</span></span> <span data-ttu-id="7b271-158">Тем не менее поскольку клиент потенциально могут изменять данные, сервер должен всегда тщательно проверяйте его.</span><span class="sxs-lookup"><span data-stu-id="7b271-158">However, because the  client can potentially tamper with the data, the server must always revalidate it.</span></span> 

### <a name="cookies"></a><span data-ttu-id="7b271-159">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="7b271-159">Cookies</span></span>

<span data-ttu-id="7b271-160">Файлы cookie предоставляют способ хранения данных конкретного пользователя в веб-приложениях.</span><span class="sxs-lookup"><span data-stu-id="7b271-160">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="7b271-161">Поскольку файлы cookie, отправленных с каждым запросом, их размера должна быть сведена к минимуму.</span><span class="sxs-lookup"><span data-stu-id="7b271-161">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="7b271-162">В идеальном случае только идентификатор должны храниться в файле cookie с текущими данными, хранящимися на сервере.</span><span class="sxs-lookup"><span data-stu-id="7b271-162">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="7b271-163">Большинство браузеров ограничить файлы Cookie до 4096 байт.</span><span class="sxs-lookup"><span data-stu-id="7b271-163">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="7b271-164">Кроме того для каждого домена доступны только ограниченное число файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="7b271-164">In addition, only a limited number of cookies are available for each domain.</span></span>  

<span data-ttu-id="7b271-165">Поскольку файлы cookie могут быть прочитаны, они должны быть проверены на сервере.</span><span class="sxs-lookup"><span data-stu-id="7b271-165">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="7b271-166">Несмотря на то, что надежность куки-файл на компьютере клиента регулируется вмешательства пользователя и срок действия, они обычно являются более долговременной формой сохранения данных на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7b271-166">Although the durability of the cookie on a client is subject to user intervention and expiration, they are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="7b271-167">Файлы cookie часто используются для персонализации, если содержимое настраивается для известного пользователя.</span><span class="sxs-lookup"><span data-stu-id="7b271-167">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="7b271-168">Так как только после определения и не прошел проверку подлинности, в большинстве случаев пользователь, обычно можно защитить файл cookie путем сохранения имени пользователя, имя учетной записи или уникальный ID пользователя (например, GUID) в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="7b271-168">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="7b271-169">Затем можно использовать файл cookie для доступа к персональной инфраструктуре веб-узла.</span><span class="sxs-lookup"><span data-stu-id="7b271-169">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

### <a name="httpcontextitems"></a><span data-ttu-id="7b271-170">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="7b271-170">HttpContext.Items</span></span>

<span data-ttu-id="7b271-171">`Items` Коллекция имеет хорошее расположение для хранения данных, надобности только во время обработки одного конкретного запроса.</span><span class="sxs-lookup"><span data-stu-id="7b271-171">The `Items` collection is a good location to store data that is needed only while processing one particular request.</span></span> <span data-ttu-id="7b271-172">Содержимое коллекции удаляются после каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="7b271-172">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="7b271-173">`Items` Коллекции лучше всего использовать как способ для компонентов или по промежуточного слоя для обмена данными, если они работают в разные моменты времени, во время запроса и имеют прямой способ передачи параметров.</span><span class="sxs-lookup"><span data-stu-id="7b271-173">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="7b271-174">Дополнительные сведения см. в разделе [работа с HttpContext.Items](#working-with-httpcontextitems)далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="7b271-174">For more information, see [Working with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

### <a name="cache"></a><span data-ttu-id="7b271-175">Кэш</span><span class="sxs-lookup"><span data-stu-id="7b271-175">Cache</span></span>

<span data-ttu-id="7b271-176">Кэширование — эффективный способ хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="7b271-176">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="7b271-177">Можно управлять временем жизни кэшированных элементов на основе времени и другие вопросы.</span><span class="sxs-lookup"><span data-stu-id="7b271-177">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="7b271-178">Дополнительные сведения о [кэширование](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="7b271-178">Learn more about [Caching](../performance/caching/index.md).</span></span>

<a name=session></a>

## <a name="configuring-session"></a><span data-ttu-id="7b271-179">Настройка сеанса</span><span class="sxs-lookup"><span data-stu-id="7b271-179">Configuring Session</span></span>

<span data-ttu-id="7b271-180">`Microsoft.AspNetCore.Session` Пакет предоставляет по промежуточного слоя для управления состоянием сеанса.</span><span class="sxs-lookup"><span data-stu-id="7b271-180">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="7b271-181">Чтобы включить сеанс по промежуточного слоя, `Startup`должен содержать:</span><span class="sxs-lookup"><span data-stu-id="7b271-181">To enable the session middleware, `Startup`must contain:</span></span>

- <span data-ttu-id="7b271-182">Любой из [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) кэшей памяти.</span><span class="sxs-lookup"><span data-stu-id="7b271-182">Any of the [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="7b271-183">`IDistributedCache` Реализация используется в качестве резервного хранилища для сеанса.</span><span class="sxs-lookup"><span data-stu-id="7b271-183">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="7b271-184">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) вызов, требующий пакет NuGet «Microsoft.AspNetCore.Session».</span><span class="sxs-lookup"><span data-stu-id="7b271-184">[AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="7b271-185">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) вызова.</span><span class="sxs-lookup"><span data-stu-id="7b271-185">[UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="7b271-186">Ниже показано, как для настройки поставщика сеансов в памяти.</span><span class="sxs-lookup"><span data-stu-id="7b271-186">The following code shows how to set up the in-memory session provider.</span></span>

<span data-ttu-id="7b271-187">[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]</span><span class="sxs-lookup"><span data-stu-id="7b271-187">[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]</span></span>

<span data-ttu-id="7b271-188">Можно ссылаться на сеанс из `HttpContext` после его установки и настройки.</span><span class="sxs-lookup"><span data-stu-id="7b271-188">You can reference Session from `HttpContext` once it is installed and configured.</span></span>

<span data-ttu-id="7b271-189">Если при попытке доступа к `Session` перед `UseSession` был вызван, исключение `InvalidOperationException: Session has not been configured for this application or request` возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="7b271-189">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="7b271-190">При попытке создать новый `Session` (то есть, объекты cookie сеанса не был создан) после начала записи в `Response` потока, исключение `InvalidOperationException: The session cannot be established after the response has started` возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="7b271-190">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="7b271-191">Исключение, можно найти в журнале веб-сервера; он не будет отображаться в браузере.</span><span class="sxs-lookup"><span data-stu-id="7b271-191">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="7b271-192">Асинхронная загрузка сеанса</span><span class="sxs-lookup"><span data-stu-id="7b271-192">Loading Session asynchronously</span></span> 

<span data-ttu-id="7b271-193">Поставщик сеанса по умолчанию в ASP.NET Core запись сеанса загружается из основного [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) хранилище асинхронно только тогда, когда [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) до явного вызова метода  `TryGetValue`, `Set`, или `Remove` методы.</span><span class="sxs-lookup"><span data-stu-id="7b271-193">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="7b271-194">Если `LoadAsync` не вызывается во-первых, базовый сеанс записи загружается синхронно, который могут повлиять на возможность масштабирования приложения.</span><span class="sxs-lookup"><span data-stu-id="7b271-194">If `LoadAsync` is not called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="7b271-195">Чтобы применить этот шаблон приложения, перенос [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) и [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) реализации с версиями, если исключение `LoadAsync` метод не является Вызывается перед `TryGetValue`, `Set`, или `Remove`.</span><span class="sxs-lookup"><span data-stu-id="7b271-195">To have applications enforce this pattern, wrap the [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method is not called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="7b271-196">Зарегистрируйте оболочку версии в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="7b271-196">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="7b271-197">Сведения о реализации</span><span class="sxs-lookup"><span data-stu-id="7b271-197">Implementation Details</span></span>

<span data-ttu-id="7b271-198">Сеанс использует файл cookie для отслеживания и идентификации запросов из одного браузера.</span><span class="sxs-lookup"><span data-stu-id="7b271-198">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="7b271-199">По умолчанию этот файл cookie с именем «. AspNet.Session», который использует путь «/».</span><span class="sxs-lookup"><span data-stu-id="7b271-199">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="7b271-200">Так как файл cookie по умолчанию домен не указан, он не становится доступным клиентский сценарий на странице (поскольку `CookieHttpOnly` по умолчанию используется значение `true`).</span><span class="sxs-lookup"><span data-stu-id="7b271-200">Because the cookie default does not specify a domain, it is not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="7b271-201">Чтобы переопределить значения по умолчанию сеанс, используйте `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="7b271-201">To override session defaults, use `SessionOptions`:</span></span>

<span data-ttu-id="7b271-202">[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]</span><span class="sxs-lookup"><span data-stu-id="7b271-202">[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]</span></span>

<span data-ttu-id="7b271-203">Сервер использует `IdleTimeout` свойства, чтобы определить, как долго сеанс может быть неактивным до его содержимое оставляются.</span><span class="sxs-lookup"><span data-stu-id="7b271-203">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="7b271-204">Это свойство не зависит от срок действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="7b271-204">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="7b271-205">Каждый запрос, который проходит через по промежуточного слоя сеанса (чтение и запись для данных) сбрасывает время ожидания.</span><span class="sxs-lookup"><span data-stu-id="7b271-205">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="7b271-206">Поскольку `Session` — *неблокирующую*, если два запроса как попытка изменить содержимое сеанса последним переопределяет первый.</span><span class="sxs-lookup"><span data-stu-id="7b271-206">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="7b271-207">`Session`реализуется в виде *согласованного сеанса*, что означает, что все содержимое хранятся вместе.</span><span class="sxs-lookup"><span data-stu-id="7b271-207">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="7b271-208">Два запроса, изменяющие различные части сеанса (разные ключи) по-прежнему могут повлиять друг на друга.</span><span class="sxs-lookup"><span data-stu-id="7b271-208">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

## <a name="setting-and-getting-session-values"></a><span data-ttu-id="7b271-209">Настройка и получение значения сеанса</span><span class="sxs-lookup"><span data-stu-id="7b271-209">Setting and getting Session values</span></span>

<span data-ttu-id="7b271-210">Сеанс осуществляется с помощью `Session` свойство `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7b271-210">Session is accessed through the `Session` property on `HttpContext`.</span></span> <span data-ttu-id="7b271-211">Это свойство является [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) реализации.</span><span class="sxs-lookup"><span data-stu-id="7b271-211">This property is an [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="7b271-212">В следующем примере показано задание и получение int и строку:</span><span class="sxs-lookup"><span data-stu-id="7b271-212">The following example shows setting and getting an int and a string:</span></span>

<span data-ttu-id="7b271-213">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="7b271-213">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="7b271-214">При добавлении следующие методы расширения, можно задать и получить сериализуемые объекты для сеанса:</span><span class="sxs-lookup"><span data-stu-id="7b271-214">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

<span data-ttu-id="7b271-215">[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]</span><span class="sxs-lookup"><span data-stu-id="7b271-215">[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]</span></span>

<span data-ttu-id="7b271-216">Следующий пример показано, как задать и получить сериализуемый объект:</span><span class="sxs-lookup"><span data-stu-id="7b271-216">The following sample shows how to set and get a serializable object:</span></span>

<span data-ttu-id="7b271-217">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="7b271-217">[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]</span></span>


## <a name="working-with-httpcontextitems"></a><span data-ttu-id="7b271-218">Работа с HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="7b271-218">Working with HttpContext.Items</span></span>

<span data-ttu-id="7b271-219">`HttpContext` Абстракции обеспечивает поддержку для коллекции словаря типа `IDictionary<object, object>`, который называется `Items`.</span><span class="sxs-lookup"><span data-stu-id="7b271-219">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="7b271-220">Эта коллекция доступна с начала *HttpRequest* и удаляются в конце каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="7b271-220">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="7b271-221">Его можно открыть, путем присвоения значения в запись с ключом, или с запросом на ввод значения для определенного ключа.</span><span class="sxs-lookup"><span data-stu-id="7b271-221">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="7b271-222">В приведенном ниже примере [по промежуточного слоя](middleware.md) добавляет `isVerified` для `Items` коллекции.</span><span class="sxs-lookup"><span data-stu-id="7b271-222">In the sample below, [Middleware](middleware.md) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="7b271-223">Далее в конвейере другого по промежуточного слоя может получить доступ к его:</span><span class="sxs-lookup"><span data-stu-id="7b271-223">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

<span data-ttu-id="7b271-224">По промежуточного слоя, который будет использовать только одно приложение `string` ключи являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="7b271-224">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="7b271-225">Тем не менее по промежуточного слоя, который будет совместно использоваться приложениями следует использовать уникальный объект во избежание любой вероятность возникновения конфликтов.</span><span class="sxs-lookup"><span data-stu-id="7b271-225">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="7b271-226">При разработке по промежуточного слоя, который будет работать с несколькими приложениями, используйте уникальный объект ключа, определенных в классе по промежуточного слоя, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="7b271-226">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

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

<span data-ttu-id="7b271-227">Другой код может получить доступ к значение, хранящееся в `HttpContext.Items` с помощью ключа, предоставляемые классом по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="7b271-227">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="7b271-228">Данный подход также имеет преимущество устраняя повторение «magic строки» в нескольких местах в коде.</span><span class="sxs-lookup"><span data-stu-id="7b271-228">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

<a name=appstate-errors></a>

## <a name="application-state-data"></a><span data-ttu-id="7b271-229">Данные состояния приложения</span><span class="sxs-lookup"><span data-stu-id="7b271-229">Application state data</span></span>

<span data-ttu-id="7b271-230">Используйте [внедрения зависимостей](xref:fundamentals/dependency-injection) чтобы сделать данные доступными для всех пользователей:</span><span class="sxs-lookup"><span data-stu-id="7b271-230">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="7b271-231">Определения службы, содержащий данные (например, класс с именем `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="7b271-231">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. <span data-ttu-id="7b271-232">Добавление класса службы для `ConfigureServices` (например `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="7b271-232">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>
3. <span data-ttu-id="7b271-233">Использование класса службы данных в каждом контроллере:</span><span class="sxs-lookup"><span data-stu-id="7b271-233">Consume the data service class in each controller:</span></span>

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

### <a name="common-errors-when-working-with-session"></a><span data-ttu-id="7b271-234">Распространенные ошибки при работе с сеансом</span><span class="sxs-lookup"><span data-stu-id="7b271-234">Common errors when working with session</span></span>

* <span data-ttu-id="7b271-235">«Не удается разрешить службу типа «Microsoft.Extensions.Caching.Distributed.IDistributedCache» при попытке активации «Microsoft.AspNetCore.Session.DistributedSessionStore»».</span><span class="sxs-lookup"><span data-stu-id="7b271-235">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="7b271-236">Обычно это вызвано неправильная настройка по крайней мере один `IDistributedCache` реализации.</span><span class="sxs-lookup"><span data-stu-id="7b271-236">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="7b271-237">Дополнительные сведения см. в разделе [работа с распределенного кэша](xref:performance/caching/distributed) и [в кэширование памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="7b271-237">For more information, see [Working with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="7b271-238">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7b271-238">Additional Resources</span></span>


* [<span data-ttu-id="7b271-239">Пример кода, используемого в данном документе</span><span class="sxs-lookup"><span data-stu-id="7b271-239">Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
