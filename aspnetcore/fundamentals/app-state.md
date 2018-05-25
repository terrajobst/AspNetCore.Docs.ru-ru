---
title: Состояние сеанса и приложения в ASP.NET Core
author: rick-anderson
description: Методы для сохранения состояния приложения и пользователя (сеанса) между запросами.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 887aefdeaa45957f7b95bfe8df342eb34d267e3a
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/18/2018
---
# <a name="session-and-application-state-in-aspnet-core"></a><span data-ttu-id="42110-103">Состояние сеанса и приложения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42110-103">Session and application state in ASP.NET Core</span></span>

<span data-ttu-id="42110-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Стив Смит](https://ardalis.com/) (Steve Smith) и [Диана Лароуз](https://github.com/DianaLaRose) (Diana LaRose)</span><span class="sxs-lookup"><span data-stu-id="42110-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Diana LaRose](https://github.com/DianaLaRose)</span></span>

<span data-ttu-id="42110-105">HTTP — это протокол без отслеживания состояния.</span><span class="sxs-lookup"><span data-stu-id="42110-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="42110-106">Веб-сервер обрабатывает каждый HTTP-запрос независимо и не сохраняет пользовательские значения из предыдущих запросов.</span><span class="sxs-lookup"><span data-stu-id="42110-106">A web server treats each HTTP request as an independent request and doesn't retain user values from previous requests.</span></span> <span data-ttu-id="42110-107">Эта статья описывает различные способы для сохранения состояния приложения и сеанса между запросами.</span><span class="sxs-lookup"><span data-stu-id="42110-107">This article discusses different ways to preserve application and session state between requests.</span></span>

## <a name="session-state"></a><span data-ttu-id="42110-108">Состояние сеанса</span><span class="sxs-lookup"><span data-stu-id="42110-108">Session state</span></span>

<span data-ttu-id="42110-109">Состояние сеанса — это функция в ASP.NET Core, которую можно использовать для сохранения пользовательских данных во время просмотра веб-приложения пользователем.</span><span class="sxs-lookup"><span data-stu-id="42110-109">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span> <span data-ttu-id="42110-110">Состояние сеанса, состоящее из словаря или хэш-таблицы на сервере, сохраняет данные между запросами из браузера.</span><span class="sxs-lookup"><span data-stu-id="42110-110">Consisting of a dictionary or hash table on the server, session state persists data across requests from a browser.</span></span> <span data-ttu-id="42110-111">Данные сеанса хранятся в кэше.</span><span class="sxs-lookup"><span data-stu-id="42110-111">The session data is backed by a cache.</span></span>

<span data-ttu-id="42110-112">ASP.NET Core сохраняет состояние сеанса, предоставляя клиенту файл cookie, содержащий идентификатор сеанса, который отправляется на сервер вместе с каждым запросом.</span><span class="sxs-lookup"><span data-stu-id="42110-112">ASP.NET Core maintains session state by giving the client a cookie that contains the session ID, which is sent to the server with each request.</span></span> <span data-ttu-id="42110-113">Сервер использует этот идентификатор для получения данных сеанса.</span><span class="sxs-lookup"><span data-stu-id="42110-113">The server uses the session ID to fetch the session data.</span></span> <span data-ttu-id="42110-114">Так как файл cookie сеанса относится к конкретному браузеру, обеспечить общий доступ к сеансам из разных браузеров невозможно.</span><span class="sxs-lookup"><span data-stu-id="42110-114">Because the session cookie is specific to the browser, you cannot share sessions across browsers.</span></span> <span data-ttu-id="42110-115">Файлы cookie сеанса удаляются только при завершении сеанса браузера.</span><span class="sxs-lookup"><span data-stu-id="42110-115">Session cookies are deleted only when the browser session ends.</span></span> <span data-ttu-id="42110-116">Если получен файл cookie для просроченного сеанса, создается сеанс, использующий этот файл.</span><span class="sxs-lookup"><span data-stu-id="42110-116">If a cookie is received for an expired session, a new session that uses the same session cookie is created.</span></span>

<span data-ttu-id="42110-117">Сервер хранит сеанс некоторое время после последнего запроса.</span><span class="sxs-lookup"><span data-stu-id="42110-117">The server retains a session for a limited time after the last request.</span></span> <span data-ttu-id="42110-118">Установите время ожидания сеанса или используйте значение по умолчанию, равное 20 минутам.</span><span class="sxs-lookup"><span data-stu-id="42110-118">Either set the session timeout or use the default value of 20 minutes.</span></span> <span data-ttu-id="42110-119">Состояние сеанса оптимально подходит для сохранения пользовательских данных, относящихся к определенному сеансу, которые не требуется хранить бессрочно.</span><span class="sxs-lookup"><span data-stu-id="42110-119">Session state is ideal for storing user data that's specific to a particular session but doesn't need to be persisted permanently.</span></span> <span data-ttu-id="42110-120">Данные удаляются из резервного хранилища либо при вызове `Session.Clear`, либо после окончания срока действия сеанса в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="42110-120">Data is deleted from the backing store either when calling `Session.Clear` or when the session expires in the data store.</span></span> <span data-ttu-id="42110-121">Серверу неизвестно, когда закрыт браузер или удален файл cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="42110-121">The server doesn't know when the browser is closed or when the session cookie is deleted.</span></span>

> [!WARNING]
> <span data-ttu-id="42110-122">Не храните конфиденциальные данные в сеансе.</span><span class="sxs-lookup"><span data-stu-id="42110-122">Don't store sensitive data in session.</span></span> <span data-ttu-id="42110-123">Клиент может не закрыть браузер и не удалить файл cookie сеанса (а некоторые браузеры используют файлы cookie сеанса в разных окнах).</span><span class="sxs-lookup"><span data-stu-id="42110-123">The client might not close the browser and clear the session cookie (and some browsers keep session cookies alive across windows).</span></span> <span data-ttu-id="42110-124">Кроме того, сеанс может быть не ограничен одним пользователем, то есть следующий пользователь продолжит работу с тем же сеансом.</span><span class="sxs-lookup"><span data-stu-id="42110-124">Also, a session might not be restricted to a single user; the next user might continue with the same session.</span></span>

<span data-ttu-id="42110-125">Поставщик сеансов в памяти сохраняет данные сеанса на локальном сервере.</span><span class="sxs-lookup"><span data-stu-id="42110-125">The in-memory session provider stores session data on the local server.</span></span> <span data-ttu-id="42110-126">Если вы планируете запустить веб-приложение на ферме серверов, нужно использовать прикрепленные сеансы, чтобы связать каждый сеанс с определенным сервером.</span><span class="sxs-lookup"><span data-stu-id="42110-126">If you plan to run your web app on a server farm, you must use sticky sessions to tie each session to a specific server.</span></span> <span data-ttu-id="42110-127">По умолчанию платформа "Веб-сайты Microsoft Azure" использует прикрепленные сеансы (это называется маршрутизацией запросов приложений или ARR).</span><span class="sxs-lookup"><span data-stu-id="42110-127">The Windows Azure Web Sites platform defaults to sticky sessions (Application Request Routing or ARR).</span></span> <span data-ttu-id="42110-128">Однако прикрепленные сеансы могут негативно повлиять на масштабируемость и усложнить обновление веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="42110-128">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="42110-129">Лучше использовать распределенные кэши SQL Server или Redis, для которых прикрепленные сеансы не требуются.</span><span class="sxs-lookup"><span data-stu-id="42110-129">A better option is to use the Redis or SQL Server distributed caches, which don't require sticky sessions.</span></span> <span data-ttu-id="42110-130">Дополнительные сведения см. в разделе [Работа с распределенным кэшем](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="42110-130">For more information, see [Work with a Distributed Cache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="42110-131">Дополнительные сведения о настройке поставщиков услуг см. в разделе [Настройка сеанса](#configuring-session) ниже.</span><span class="sxs-lookup"><span data-stu-id="42110-131">For details on setting up service providers, see [Configuring Session](#configuring-session) later in this article.</span></span>

## <a name="tempdata"></a><span data-ttu-id="42110-132">TempData</span><span class="sxs-lookup"><span data-stu-id="42110-132">TempData</span></span>

<span data-ttu-id="42110-133">ASP.NET Core MVC позволяет использовать свойство [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) в [контроллере](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="42110-133">ASP.NET Core MVC exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0).</span></span> <span data-ttu-id="42110-134">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="42110-134">This property stores data until it's read.</span></span> <span data-ttu-id="42110-135">Для проверки данных без удаления можно использовать методы `Keep` и `Peek`.</span><span class="sxs-lookup"><span data-stu-id="42110-135">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="42110-136">`TempData` особенно удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="42110-136">`TempData` is particularly useful for redirection, when data is needed for more than a single request.</span></span> <span data-ttu-id="42110-137">`TempData` реализуется поставщиками TempData, например с помощью файлов cookie или состояния сеанса.</span><span class="sxs-lookup"><span data-stu-id="42110-137">`TempData` is implemented by TempData providers, for example, using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="42110-138">Поставщики TempData</span><span class="sxs-lookup"><span data-stu-id="42110-138">TempData providers</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42110-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42110-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="42110-140">В ASP.NET Core 2.0 и более поздних версий поставщик TempData, основанный на файлах cookie, используется по умолчанию для хранения TempData в файлах cookie.</span><span class="sxs-lookup"><span data-stu-id="42110-140">In ASP.NET Core 2.0 and later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="42110-141">Данные в файле cookie шифруются с помощью [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), кодируются с помощью [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), а затем фрагментируются.</span><span class="sxs-lookup"><span data-stu-id="42110-141">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="42110-142">Так как файл cookie фрагментируется, ограничение на размер одного такого файла, заданное в ASP.NET Core 1.x, не применяется.</span><span class="sxs-lookup"><span data-stu-id="42110-142">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="42110-143">Данные файла cookie не сжимаются, так как сжатие зашифрованных данных может привести к проблемам с безопасностью, например атакам [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).</span><span class="sxs-lookup"><span data-stu-id="42110-143">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="42110-144">Дополнительные сведения на поставщике TempData, основанном на файлах cookie, см. в разделе [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span><span class="sxs-lookup"><span data-stu-id="42110-144">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42110-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42110-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="42110-146">В ASP.NET Core 1.0 и 1.1 для состояния сеанса по умолчанию используется поставщик TempData.</span><span class="sxs-lookup"><span data-stu-id="42110-146">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default.</span></span>

---

### <a name="choosing-a-tempdata-provider"></a><span data-ttu-id="42110-147">Выбор поставщика TempData</span><span class="sxs-lookup"><span data-stu-id="42110-147">Choosing a TempData provider</span></span>

<span data-ttu-id="42110-148">Выбор поставщика TempData включает в себя несколько аспектов:</span><span class="sxs-lookup"><span data-stu-id="42110-148">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="42110-149">Использует ли приложение состояние сеанса для других целей?</span><span class="sxs-lookup"><span data-stu-id="42110-149">Does the application already use session state for other purposes?</span></span> <span data-ttu-id="42110-150">Если это так, использование поставщика TempData для состояния сеанса не требует от приложения никаких дополнительных издержек (кроме объема данных).</span><span class="sxs-lookup"><span data-stu-id="42110-150">If so, using the session state TempData provider has no additional cost to the application (aside from the size of the data).</span></span>
2. <span data-ttu-id="42110-151">Приложение использует TempData лишь ограниченно, для сравнительно небольших объемов данных (до 500 байт)?</span><span class="sxs-lookup"><span data-stu-id="42110-151">Does the application use TempData only sparingly, for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="42110-152">Если это так, поставщик TempData на основе файлов cookie будет добавлять небольшие издержки для каждого запроса, переносящего TempData.</span><span class="sxs-lookup"><span data-stu-id="42110-152">If so, the cookie TempData provider will add a small cost to each request that carries TempData.</span></span> <span data-ttu-id="42110-153">В противном случае поставщик TempData для состояния сеанса удобно использовать, чтобы устранить круговой обход большого объема данных в каждом запросе до полного использования TempData.</span><span class="sxs-lookup"><span data-stu-id="42110-153">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="42110-154">Приложение выполняется на веб-ферме (нескольких серверах)?</span><span class="sxs-lookup"><span data-stu-id="42110-154">Does the application run in a web farm (multiple servers)?</span></span> <span data-ttu-id="42110-155">Если это так, для использования поставщика TempData на основе файлов cookie дополнительная настройка не требуется.</span><span class="sxs-lookup"><span data-stu-id="42110-155">If so, there's no additional configuration needed to use the cookie TempData provider.</span></span>

> [!NOTE]
> <span data-ttu-id="42110-156">Большинство веб-клиентов (например, веб-браузеры) налагают ограничение на максимальный размер каждого файла cookie и (или) общее количество таких файлов.</span><span class="sxs-lookup"><span data-stu-id="42110-156">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="42110-157">Поэтому при использовании поставщика TempData на основе файлов cookie убедитесь, что приложение не нарушает эти ограничения.</span><span class="sxs-lookup"><span data-stu-id="42110-157">Therefore, when using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="42110-158">При оценке учитывайте общий объем данных, включающий в себя затраты на шифрование и фрагментацию.</span><span class="sxs-lookup"><span data-stu-id="42110-158">Consider the total size of the data, accounting for the overheads of encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="42110-159">Настройка поставщика TempData</span><span class="sxs-lookup"><span data-stu-id="42110-159">Configure the TempData provider</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42110-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42110-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="42110-161">Поставщик TempData на основе файлов cookie включен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="42110-161">The cookie-based TempData provider is enabled by default.</span></span> <span data-ttu-id="42110-162">Следующий код класса `Startup` настраивает поставщик TempData на основе сеансов:</span><span class="sxs-lookup"><span data-stu-id="42110-162">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42110-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42110-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="42110-164">Следующий код класса `Startup` настраивает поставщик TempData на основе сеансов:</span><span class="sxs-lookup"><span data-stu-id="42110-164">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

<span data-ttu-id="42110-165">Для компонентов промежуточного слоя критически важен порядок.</span><span class="sxs-lookup"><span data-stu-id="42110-165">Ordering is critical for middleware components.</span></span> <span data-ttu-id="42110-166">В предыдущем примере исключение типа `InvalidOperationException` возникает при вызове `UseSession` после `UseMvcWithDefaultRoute`.</span><span class="sxs-lookup"><span data-stu-id="42110-166">In the preceding example, an exception of type `InvalidOperationException` occurs when `UseSession` is invoked after `UseMvcWithDefaultRoute`.</span></span> <span data-ttu-id="42110-167">Дополнительные сведения см. в разделе [Порядок ПО промежуточного слоя](xref:fundamentals/middleware/index#ordering).</span><span class="sxs-lookup"><span data-stu-id="42110-167">See [Middleware Ordering](xref:fundamentals/middleware/index#ordering) for more detail.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42110-168">Если вы ориентируетесь на .NET Framework и используете поставщик на основе сеансов, добавьте в проект пакет NuGet [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session).</span><span class="sxs-lookup"><span data-stu-id="42110-168">If targeting .NET Framework and using the session-based provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet package to your project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="42110-169">Строки запроса</span><span class="sxs-lookup"><span data-stu-id="42110-169">Query strings</span></span>

<span data-ttu-id="42110-170">Вы можете передать ограниченный объем данных из одного запроса в другой, добавив их в строку запроса нового запроса.</span><span class="sxs-lookup"><span data-stu-id="42110-170">You can pass a limited amount of data from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="42110-171">Это удобно для захвата состояния в сохраняемом виде, что позволяет обмениваться ссылками с внедренным состоянием по электронной почте или через социальные сети.</span><span class="sxs-lookup"><span data-stu-id="42110-171">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="42110-172">Однако по этой причине никогда не следует использовать строки запроса для конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="42110-172">However, for this reason, you should never use query strings for sensitive data.</span></span> <span data-ttu-id="42110-173">Кроме удобного общего доступа, включение данных в строки запроса может открыть возможности для атак с [подделкой межсайтовых запросов (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), которые могут обманом вынудить пользователей посещать вредоносные веб-сайты во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="42110-173">In addition to being easily shared, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="42110-174">Это позволит злоумышленникам похитить данные пользователей из приложения или предпринимать вредоносные действия от их имени.</span><span class="sxs-lookup"><span data-stu-id="42110-174">Attackers can then steal user data from your app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="42110-175">Любое сохраненное состояние приложения или сеанса необходимо защитить от атак CSRF.</span><span class="sxs-lookup"><span data-stu-id="42110-175">Any preserved application or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="42110-176">Дополнительные сведения об атаках CSRF см. в статье [Предотвращение атак с подделкой межсайтовых запросов (XSRF/CSRF)](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="42110-176">For more information on CSRF attacks, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="post-data-and-hidden-fields"></a><span data-ttu-id="42110-177">Публикация данных и скрытые поля</span><span class="sxs-lookup"><span data-stu-id="42110-177">Post data and hidden fields</span></span>

<span data-ttu-id="42110-178">Данные можно сохранить в скрытых полях формы и опубликовать обратно при следующем запросе.</span><span class="sxs-lookup"><span data-stu-id="42110-178">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="42110-179">Это типичное поведение для многостраничных форм.</span><span class="sxs-lookup"><span data-stu-id="42110-179">This is common in multi-page forms.</span></span> <span data-ttu-id="42110-180">Однако так как клиент может незаконно изменить данные, сервер всегда должен перепроверять их.</span><span class="sxs-lookup"><span data-stu-id="42110-180">However, because the client can potentially tamper with the data, the server must always revalidate it.</span></span>

## <a name="cookies"></a><span data-ttu-id="42110-181">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="42110-181">Cookies</span></span>

<span data-ttu-id="42110-182">Файлы cookie позволяют сохранять данные для конкретного пользователя в веб-приложениях.</span><span class="sxs-lookup"><span data-stu-id="42110-182">Cookies provide a way to store user-specific data in web applications.</span></span> <span data-ttu-id="42110-183">Так как файлы cookie отправляются с каждым запросом, их размер нужно свести к минимуму.</span><span class="sxs-lookup"><span data-stu-id="42110-183">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="42110-184">В идеальном случае файл cookie должен содержать лишь идентификатор, а сами данные — храниться на сервере.</span><span class="sxs-lookup"><span data-stu-id="42110-184">Ideally, only an identifier should be stored in a cookie with the actual data stored on the server.</span></span> <span data-ttu-id="42110-185">Большинство браузеров позволяют использовать файлы cookie размером до 4096 байт.</span><span class="sxs-lookup"><span data-stu-id="42110-185">Most browsers restrict cookies to 4096 bytes.</span></span> <span data-ttu-id="42110-186">Кроме того, для каждого домена доступно лишь ограниченное число файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="42110-186">In addition, only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="42110-187">Так как файлы cookie могут быть незаконно изменены, их нужно проверять на сервере.</span><span class="sxs-lookup"><span data-stu-id="42110-187">Because cookies are subject to tampering, they must be validated on the server.</span></span> <span data-ttu-id="42110-188">Несмотря на то, что надежность файлов cookie на клиенте зависит от вмешательства пользователя и срока действия, обычно они являются наиболее надежной формой сохранения данных на клиенте.</span><span class="sxs-lookup"><span data-stu-id="42110-188">Although the durability of the cookie on a client is subject to user intervention and expiration, they're generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="42110-189">Файлы cookie часто используются для персонализации, когда содержимое настраивается под известного пользователя.</span><span class="sxs-lookup"><span data-stu-id="42110-189">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="42110-190">Так как в большинстве случаев пользователь лишь идентифицируется, но не проходит проверку подлинности, можно защитить файл cookie, сохранив в нем имя пользователя, имя учетной записи или уникальный идентификатор пользователя (например, GUID).</span><span class="sxs-lookup"><span data-stu-id="42110-190">Because the user is only identified and not authenticated in most cases, you can typically secure a cookie by storing the user name, account name, or a unique user ID (such as a GUID) in the cookie.</span></span> <span data-ttu-id="42110-191">Затем можно использовать этот файл для доступа к инфраструктуре персонализации пользователя на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="42110-191">You can then use the cookie to access the user personalization infrastructure of a site.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="42110-192">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="42110-192">HttpContext.Items</span></span>

<span data-ttu-id="42110-193">Коллекция `Items` хорошо подходит для хранения данных, которые требуются только при обработке одного конкретного запроса.</span><span class="sxs-lookup"><span data-stu-id="42110-193">The `Items` collection is a good location to store data that's needed only while processing one particular request.</span></span> <span data-ttu-id="42110-194">Ее содержимое удаляется после каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="42110-194">The collection's contents are discarded after each request.</span></span> <span data-ttu-id="42110-195">Коллекцию `Items` лучше всего использовать для обеспечения взаимодействия компонентов или ПО промежуточного слоя, когда они выполняются в разные моменты времени во время обработки запроса и не могут передавать параметры напрямую.</span><span class="sxs-lookup"><span data-stu-id="42110-195">The `Items` collection is best used as a way for components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span> <span data-ttu-id="42110-196">Дополнительные сведения см. в разделе [Работа с HttpContext.Items](#working-with-httpcontextitems) ниже.</span><span class="sxs-lookup"><span data-stu-id="42110-196">For more information, see [Work with HttpContext.Items](#working-with-httpcontextitems), later in this article.</span></span>

## <a name="cache"></a><span data-ttu-id="42110-197">Кэш</span><span class="sxs-lookup"><span data-stu-id="42110-197">Cache</span></span>

<span data-ttu-id="42110-198">Кэширование — это эффективный способ хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="42110-198">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="42110-199">Вы можете управлять временем существования кэшированных элементов по времени и другим факторам.</span><span class="sxs-lookup"><span data-stu-id="42110-199">You can control the lifetime of cached items based on time and other considerations.</span></span> <span data-ttu-id="42110-200">Дополнительные сведения о [методах кэширования](../performance/caching/index.md).</span><span class="sxs-lookup"><span data-stu-id="42110-200">Learn more about [how to cache](../performance/caching/index.md).</span></span>

## <a name="working-with-session-state"></a><span data-ttu-id="42110-201">Работа с состоянием сеанса</span><span class="sxs-lookup"><span data-stu-id="42110-201">Working with Session State</span></span>

### <a name="configuring-session"></a><span data-ttu-id="42110-202">Настройка сеанса</span><span class="sxs-lookup"><span data-stu-id="42110-202">Configuring Session</span></span>

<span data-ttu-id="42110-203">Пакет `Microsoft.AspNetCore.Session` предоставляет ПО промежуточного слоя для управления состоянием сеанса.</span><span class="sxs-lookup"><span data-stu-id="42110-203">The `Microsoft.AspNetCore.Session` package provides middleware for managing session state.</span></span> <span data-ttu-id="42110-204">Чтобы включить сеанс ПО промежуточного слоя для сеансов, `Startup` должен содержать:</span><span class="sxs-lookup"><span data-stu-id="42110-204">To enable the session middleware, `Startup` must contain:</span></span>

- <span data-ttu-id="42110-205">Любой из кэшей памяти [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache),</span><span class="sxs-lookup"><span data-stu-id="42110-205">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="42110-206">при этом реализация `IDistributedCache` используется в качестве резервного хранилища для сеанса.</span><span class="sxs-lookup"><span data-stu-id="42110-206">The `IDistributedCache` implementation is used as a backing store for session.</span></span>
- <span data-ttu-id="42110-207">Вызов [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_), требующий пакет NuGet "Microsoft.AspNetCore.Session".</span><span class="sxs-lookup"><span data-stu-id="42110-207">[AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) call, which requires NuGet package "Microsoft.AspNetCore.Session".</span></span>
- <span data-ttu-id="42110-208">Вызов [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_).</span><span class="sxs-lookup"><span data-stu-id="42110-208">[UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) call.</span></span>

<span data-ttu-id="42110-209">Следующий код показывает, как настроить поставщик сеансов в памяти.</span><span class="sxs-lookup"><span data-stu-id="42110-209">The following code shows how to set up the in-memory session provider.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42110-210">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42110-210">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42110-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42110-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

<span data-ttu-id="42110-212">Вы можете сослаться на сеанс из `HttpContext` после его установки и настройки.</span><span class="sxs-lookup"><span data-stu-id="42110-212">You can reference Session from `HttpContext` once it's installed and configured.</span></span>

<span data-ttu-id="42110-213">Если попытаться обратиться к `Session` до вызова `UseSession`, возникает исключение `InvalidOperationException: Session has not been configured for this application or request`.</span><span class="sxs-lookup"><span data-stu-id="42110-213">If you try to access `Session` before `UseSession` has been called, the exception `InvalidOperationException: Session has not been configured for this application or request` is thrown.</span></span>

<span data-ttu-id="42110-214">Если попытаться создать `Session` (то есть без создания файла cookie сеанса) после начала записи в поток `Response`, возникает исключение `InvalidOperationException: The session cannot be established after the response has started`.</span><span class="sxs-lookup"><span data-stu-id="42110-214">If you try to create a new `Session` (that is, no session cookie has been created) after you have already begun writing to the `Response` stream, the exception `InvalidOperationException: The session cannot be established after the response has started` is thrown.</span></span> <span data-ttu-id="42110-215">Это исключение можно найти в журнале веб-сервера, в браузере оно не отображается.</span><span class="sxs-lookup"><span data-stu-id="42110-215">The exception can be found in the web server log; it will not be displayed in the browser.</span></span>

### <a name="loading-session-asynchronously"></a><span data-ttu-id="42110-216">Асинхронная загрузка сеанса</span><span class="sxs-lookup"><span data-stu-id="42110-216">Loading Session asynchronously</span></span>

<span data-ttu-id="42110-217">Поставщик сеансов по умолчанию в ASP.NET Core загружает запись сеанса из базового хранилища [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) в асинхронном режиме только при явном вызове метода [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) перед методами `TryGetValue`, `Set` или `Remove`.</span><span class="sxs-lookup"><span data-stu-id="42110-217">The default session provider in ASP.NET Core loads the session record from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) method is explicitly called before  the `TryGetValue`, `Set`, or `Remove` methods.</span></span> <span data-ttu-id="42110-218">Если `LoadAsync` не вызывается первым, базовая запись сеанса загружается синхронно, что может негативно повлиять на возможность масштабирования приложения.</span><span class="sxs-lookup"><span data-stu-id="42110-218">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which could potentially impact the ability of the app to scale.</span></span>

<span data-ttu-id="42110-219">Чтобы принудительно использовать этот режим в приложениях, используйте для реализаций [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) и [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) оболочку из версий, которые выдают исключение, когда метод `LoadAsync` не вызывается перед `TryGetValue`, `Set` или `Remove`.</span><span class="sxs-lookup"><span data-stu-id="42110-219">To have applications enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="42110-220">Зарегистрируйте версии оболочки в контейнере служб.</span><span class="sxs-lookup"><span data-stu-id="42110-220">Register the wrapped versions in the services container.</span></span>

### <a name="implementation-details"></a><span data-ttu-id="42110-221">Сведения о реализации</span><span class="sxs-lookup"><span data-stu-id="42110-221">Implementation details</span></span>

<span data-ttu-id="42110-222">Сеанс использует файл cookie для отслеживания и идентификации запросов в одном браузере.</span><span class="sxs-lookup"><span data-stu-id="42110-222">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="42110-223">По умолчанию этот файл называется ".AspNet.Session" и использует путь "/".</span><span class="sxs-lookup"><span data-stu-id="42110-223">By default, this cookie is named ".AspNet.Session", and it uses a path of "/".</span></span> <span data-ttu-id="42110-224">Так как по умолчанию файл cookie не указывает домен, он остается недоступным для клиентского сценария на странице (так как `CookieHttpOnly` по умолчанию имеет значение `true`).</span><span class="sxs-lookup"><span data-stu-id="42110-224">Because the cookie default doesn't specify a domain, it's not made available to the client-side script on the page (because `CookieHttpOnly` defaults to `true`).</span></span>

<span data-ttu-id="42110-225">Чтобы переопределить значения по умолчанию для сеанса, используйте `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="42110-225">To override session defaults, use `SessionOptions`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42110-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42110-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42110-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42110-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

<span data-ttu-id="42110-228">Сервер использует свойство `IdleTimeout`, чтобы определить, как долго сеанс может оставаться неактивным до сброса его содержимого.</span><span class="sxs-lookup"><span data-stu-id="42110-228">The server uses the `IdleTimeout` property to determine how long a session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="42110-229">Это свойство не зависит от срока действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="42110-229">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="42110-230">Каждый запрос, проходящий через ПО промежуточного слоя сеанса (посредством чтения или записи), сбрасывает это время ожидания.</span><span class="sxs-lookup"><span data-stu-id="42110-230">Each request that passes through the Session middleware (read from or written to) resets the timeout.</span></span>

<span data-ttu-id="42110-231">Так как `Session` является *неблокирующим*, когда два запроса пытаются изменить содержимое сеанса, последний из них переопределяет первый.</span><span class="sxs-lookup"><span data-stu-id="42110-231">Because `Session` is *non-locking*, if two requests both attempt to modify the contents of session, the last one overrides the first.</span></span> <span data-ttu-id="42110-232">`Session` реализован в виде *согласованного сеанса*, что означает, что все содержимое хранится вместе.</span><span class="sxs-lookup"><span data-stu-id="42110-232">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="42110-233">Два запроса, изменяющие разные части сеанса (разные ключи), по-прежнему могут повлиять друг на друга.</span><span class="sxs-lookup"><span data-stu-id="42110-233">Two requests that are modifying different parts of the session (different keys) might still impact each other.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="42110-234">Установка и получение значений сеанса</span><span class="sxs-lookup"><span data-stu-id="42110-234">Set and get Session values</span></span>

<span data-ttu-id="42110-235">Доступ к сеансу можно получить со страницы или представления Razor через `Context.Session`:</span><span class="sxs-lookup"><span data-stu-id="42110-235">Session is accessed from a Razor Page or view with `Context.Session`:</span></span>

[!code-cshtml[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Views/Home/About.cshtml)]

<span data-ttu-id="42110-236">Доступ к сеансу можно получить из класса `PageModel` или контроллера с `HttpContext.Session`.</span><span class="sxs-lookup"><span data-stu-id="42110-236">Session is accessed from a `PageModel` class or controller with `HttpContext.Session`.</span></span> <span data-ttu-id="42110-237">Оно является реализацией [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).</span><span class="sxs-lookup"><span data-stu-id="42110-237">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

<span data-ttu-id="42110-238">Следующий пример показывает задание и получение значения int и строки:</span><span class="sxs-lookup"><span data-stu-id="42110-238">The following example shows setting and getting an int and a string:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

<span data-ttu-id="42110-239">Добавив приведенные ниже методы расширения, можно задавать и получать сериализуемые объекты для сеанса:</span><span class="sxs-lookup"><span data-stu-id="42110-239">If you add the following extension methods, you can set and get serializable objects to Session:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

<span data-ttu-id="42110-240">Следующий пример показывает, как задать и получить сериализуемый объект:</span><span class="sxs-lookup"><span data-stu-id="42110-240">The following sample shows how to set and get a serializable object:</span></span>

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]

## <a name="working-with-httpcontextitems"></a><span data-ttu-id="42110-241">Работа с HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="42110-241">Working with HttpContext.Items</span></span>

<span data-ttu-id="42110-242">Абстракция `HttpContext` обеспечивает поддержку для коллекции словаря типа `IDictionary<object, object>`, называемой `Items`.</span><span class="sxs-lookup"><span data-stu-id="42110-242">The `HttpContext` abstraction provides support for a dictionary collection of type `IDictionary<object, object>`, called `Items`.</span></span> <span data-ttu-id="42110-243">Эта коллекция доступна с начала *HttpRequest* и удаляется в конце каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="42110-243">This collection is available from the start of an *HttpRequest* and is discarded at the end of each request.</span></span> <span data-ttu-id="42110-244">Вы можете обратиться к ней, присвоив значение записи с ключом или запросив значение для определенного ключа.</span><span class="sxs-lookup"><span data-stu-id="42110-244">You can access it by  assigning a value to a keyed entry, or by requesting the value for a particular key.</span></span>

<span data-ttu-id="42110-245">В следующем примере [ПО промежуточного слоя](xref:fundamentals/middleware/index) добавляет `isVerified` в коллекцию `Items`.</span><span class="sxs-lookup"><span data-stu-id="42110-245">In the following sample, [Middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="42110-246">Далее в конвейере другое ПО промежуточного слоя может получить к нему доступ:</span><span class="sxs-lookup"><span data-stu-id="42110-246">Later in the pipeline, another middleware could access it:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " +
        context.Items["isVerified"]);
});
```

<span data-ttu-id="42110-247">Для ПО промежуточного слоя, которое будет использоваться всего одним приложением, допустимы ключи `string`.</span><span class="sxs-lookup"><span data-stu-id="42110-247">For middleware that will only be used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="42110-248">Однако ПО промежуточного слоя, которое будет совместно использоваться несколькими приложениями, во избежание конфликтов должно использовать уникальные ключи объекта.</span><span class="sxs-lookup"><span data-stu-id="42110-248">However, middleware that will be shared between applications should use unique object keys to avoid any chance of key collisions.</span></span> <span data-ttu-id="42110-249">Если вы разрабатываете ПО промежуточного слоя, которое будет работать с несколькими приложениями, используйте уникальный ключ объекта, определенный в классе ПО промежуточного слоя, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="42110-249">If you are developing middleware that must work across multiple applications, use a unique object key defined in your middleware class as shown below:</span></span>

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

<span data-ttu-id="42110-250">Другой код может обратиться к значению, хранящемуся в `HttpContext.Items`, с помощью ключа, предоставляемого классом ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="42110-250">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

<span data-ttu-id="42110-251">Данный подход также позволяет устранить повторение соответствующих строк в нескольких местах в коде.</span><span class="sxs-lookup"><span data-stu-id="42110-251">This approach also has the advantage of eliminating repetition of "magic strings" in multiple places in the code.</span></span>

## <a name="application-state-data"></a><span data-ttu-id="42110-252">Данные о состоянии приложения</span><span class="sxs-lookup"><span data-stu-id="42110-252">Application state data</span></span>

<span data-ttu-id="42110-253">Используйте [внедрение зависимостей](xref:fundamentals/dependency-injection), чтобы сделать данные доступными для всех пользователей:</span><span class="sxs-lookup"><span data-stu-id="42110-253">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="42110-254">Определите службу, содержащую данные (например, класс с именем `MyAppData`).</span><span class="sxs-lookup"><span data-stu-id="42110-254">Define a service containing the data (for example, a class named `MyAppData`).</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties/methods/etc.
    } 
    ```

2. <span data-ttu-id="42110-255">Добавление класс службы в `ConfigureServices` (например, `services.AddSingleton<MyAppData>();`).</span><span class="sxs-lookup"><span data-stu-id="42110-255">Add the service class to `ConfigureServices` (for example `services.AddSingleton<MyAppData>();`).</span></span>

3. <span data-ttu-id="42110-256">Используйте класс службы данных в каждом контроллере:</span><span class="sxs-lookup"><span data-stu-id="42110-256">Consume the data service class in each controller:</span></span>

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

## <a name="common-errors-when-working-with-session"></a><span data-ttu-id="42110-257">Типичные ошибки при работе с сеансом</span><span class="sxs-lookup"><span data-stu-id="42110-257">Common errors when working with session</span></span>

* <span data-ttu-id="42110-258">"Unable to resolve service for type "Microsoft.Extensions.Caching.Distributed.IDistributedCache" while attempting to activate "Microsoft.AspNetCore.Session.DistributedSessionStore"" (Не удается разрешить службу для типа "Microsoft.Extensions.Caching.Distributed.IDistributedCache" при попытке активировать "Microsoft.AspNetCore.Session.DistributedSessionStore").</span><span class="sxs-lookup"><span data-stu-id="42110-258">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="42110-259">Обычно она вызвана невозможностью настройки по меньшей мере одной реализации `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="42110-259">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="42110-260">Дополнительные сведения см. в разделах [Работа с распределенным кэшем](xref:performance/caching/distributed) и [Кэширование в памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="42110-260">For more information, see [Work with a Distributed Cache](xref:performance/caching/distributed) and [In memory caching](xref:performance/caching/memory).</span></span>

* <span data-ttu-id="42110-261">Если ПО промежуточного слоя для сеанса не удается сохранить сеанс (например, когда недоступна база данных), оно регистрирует исключение и поглощает его.</span><span class="sxs-lookup"><span data-stu-id="42110-261">In the event that the session middleware fails to persist a session (for example: if the database isn't available), it logs the exception and swallows it.</span></span> <span data-ttu-id="42110-262">После этого запрос продолжает выполняться как обычно, что приводит к очень непредсказуемому поведению.</span><span class="sxs-lookup"><span data-stu-id="42110-262">The request will then continue normally, which leads to very unpredictable behavior.</span></span>

<span data-ttu-id="42110-263">Рассмотрим типичный пример:</span><span class="sxs-lookup"><span data-stu-id="42110-263">A typical example:</span></span>

<span data-ttu-id="42110-264">Кто-то сохраняет в сеансе корзину для покупок.</span><span class="sxs-lookup"><span data-stu-id="42110-264">Someone stores a shopping basket in session.</span></span> <span data-ttu-id="42110-265">Пользователь добавляет элемент, но фиксация завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="42110-265">The user adds an item but the commit fails.</span></span> <span data-ttu-id="42110-266">Приложению неизвестно о сбое, поэтому оно выводит сообщение "Элемент добавлен", хотя это не так.</span><span class="sxs-lookup"><span data-stu-id="42110-266">The app doesn't know about the failure so it reports the message "The item has been added", which isn't true.</span></span>

<span data-ttu-id="42110-267">Для проверки на наличие таких ошибок рекомендуется вызывать `await feature.Session.CommitAsync();` из кода приложения по окончании записи в сеанс.</span><span class="sxs-lookup"><span data-stu-id="42110-267">The recommended way to check for such errors is to call `await feature.Session.CommitAsync();` from app code when you're done writing to the session.</span></span> <span data-ttu-id="42110-268">После этого ошибку можно обработать любым удобным вам образом.</span><span class="sxs-lookup"><span data-stu-id="42110-268">Then you can do what you like with the error.</span></span> <span data-ttu-id="42110-269">Это работает аналогично и при вызове `LoadAsync`.</span><span class="sxs-lookup"><span data-stu-id="42110-269">It works the same way when calling `LoadAsync`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="42110-270">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="42110-270">Additional resources</span></span>

* [<span data-ttu-id="42110-271">ASP.NET Core 1.x: пример кода, используемый в этом документе</span><span class="sxs-lookup"><span data-stu-id="42110-271">ASP.NET Core 1.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [<span data-ttu-id="42110-272">ASP.NET Core 2.x: пример кода, используемый в этом документе</span><span class="sxs-lookup"><span data-stu-id="42110-272">ASP.NET Core 2.x: Sample code used in this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
