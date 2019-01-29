---
title: Состояние сеанса и приложения в ASP.NET Core
author: rick-anderson
description: Методы обнаружения для сохранения состояния сеанса и приложения между запросами.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: a510e4f49e158203dd7c5e1e0bd28472541f7925
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836341"
---
# <a name="session-and-app-state-in-aspnet-core"></a><span data-ttu-id="0a915-103">Состояние сеанса и приложения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a915-103">Session and app state in ASP.NET Core</span></span>

<span data-ttu-id="0a915-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson), [Стив Смит](https://ardalis.com/) (Steve Smith), [Диана Лароуз](https://github.com/DianaLaRose) (Diana LaRose) и [Люк Лэтэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="0a915-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), [Diana LaRose](https://github.com/DianaLaRose), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0a915-105">HTTP — это протокол без отслеживания состояния.</span><span class="sxs-lookup"><span data-stu-id="0a915-105">HTTP is a stateless protocol.</span></span> <span data-ttu-id="0a915-106">Без дополнительных действий HTTP-запросы являются независимыми сообщениями, которые не сохраняют значения пользователя или состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="0a915-106">Without taking additional steps, HTTP requests are independent messages that don't retain user values or app state.</span></span> <span data-ttu-id="0a915-107">В этой статье описывается несколько подходов для сохранения данных пользователя и состояния приложения между запросами.</span><span class="sxs-lookup"><span data-stu-id="0a915-107">This article describes several approaches to preserve user data and app state between requests.</span></span>

<span data-ttu-id="0a915-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a915-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="state-management"></a><span data-ttu-id="0a915-109">Управление состоянием</span><span class="sxs-lookup"><span data-stu-id="0a915-109">State management</span></span>

<span data-ttu-id="0a915-110">Состояние можно сохранить несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="0a915-110">State can be stored using several approaches.</span></span> <span data-ttu-id="0a915-111">Эти способы обсуждается ниже в данном разделе.</span><span class="sxs-lookup"><span data-stu-id="0a915-111">Each approach is described later in this topic.</span></span>

| <span data-ttu-id="0a915-112">Способ хранения данных</span><span class="sxs-lookup"><span data-stu-id="0a915-112">Storage approach</span></span> | <span data-ttu-id="0a915-113">Механизм хранения</span><span class="sxs-lookup"><span data-stu-id="0a915-113">Storage mechanism</span></span> |
| ---------------- | ----------------- |
| [<span data-ttu-id="0a915-114">Файлы "cookie"</span><span class="sxs-lookup"><span data-stu-id="0a915-114">Cookies</span></span>](#cookies) | <span data-ttu-id="0a915-115">Файлы cookie HTTP (могут содержать данные, сохраненные с помощью кода приложения на стороне сервера)</span><span class="sxs-lookup"><span data-stu-id="0a915-115">HTTP cookies (may include data stored using server-side app code)</span></span> |
| [<span data-ttu-id="0a915-116">Состояние сеанса</span><span class="sxs-lookup"><span data-stu-id="0a915-116">Session state</span></span>](#session-state) | <span data-ttu-id="0a915-117">Файлы cookie HTTP и код приложения на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="0a915-117">HTTP cookies and server-side app code</span></span> |
| [<span data-ttu-id="0a915-118">TempData</span><span class="sxs-lookup"><span data-stu-id="0a915-118">TempData</span></span>](#tempdata) | <span data-ttu-id="0a915-119">Файлы cookie HTTP или состояние сеанса</span><span class="sxs-lookup"><span data-stu-id="0a915-119">HTTP cookies or session state</span></span> |
| [<span data-ttu-id="0a915-120">Строки запросов</span><span class="sxs-lookup"><span data-stu-id="0a915-120">Query strings</span></span>](#query-strings) | <span data-ttu-id="0a915-121">Строки запросов HTTP</span><span class="sxs-lookup"><span data-stu-id="0a915-121">HTTP query strings</span></span> |
| [<span data-ttu-id="0a915-122">Скрытые поля</span><span class="sxs-lookup"><span data-stu-id="0a915-122">Hidden fields</span></span>](#hidden-fields) | <span data-ttu-id="0a915-123">Поля формы HTTP</span><span class="sxs-lookup"><span data-stu-id="0a915-123">HTTP form fields</span></span> |
| [<span data-ttu-id="0a915-124">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="0a915-124">HttpContext.Items</span></span>](#httpcontextitems) | <span data-ttu-id="0a915-125">Код приложения на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="0a915-125">Server-side app code</span></span> |
| [<span data-ttu-id="0a915-126">Кэш</span><span class="sxs-lookup"><span data-stu-id="0a915-126">Cache</span></span>](#cache) | <span data-ttu-id="0a915-127">Код приложения на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="0a915-127">Server-side app code</span></span> |
| [<span data-ttu-id="0a915-128">Введение зависимостей</span><span class="sxs-lookup"><span data-stu-id="0a915-128">Dependency Injection</span></span>](#dependency-injection) | <span data-ttu-id="0a915-129">Код приложения на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="0a915-129">Server-side app code</span></span> |

## <a name="cookies"></a><span data-ttu-id="0a915-130">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="0a915-130">Cookies</span></span>

<span data-ttu-id="0a915-131">Файлы cookie хранят данные между запросами.</span><span class="sxs-lookup"><span data-stu-id="0a915-131">Cookies store data across requests.</span></span> <span data-ttu-id="0a915-132">Так как файлы cookie отправляются с каждым запросом, их размер нужно свести к минимуму.</span><span class="sxs-lookup"><span data-stu-id="0a915-132">Because cookies are sent with every request, their size should be kept to a minimum.</span></span> <span data-ttu-id="0a915-133">В идеальном случае файл cookie должен содержать лишь идентификатор, а сами данные хранятся в приложении.</span><span class="sxs-lookup"><span data-stu-id="0a915-133">Ideally, only an identifier should be stored in a cookie with the data stored by the app.</span></span> <span data-ttu-id="0a915-134">Большинство браузеров позволяют использовать файлы cookie размером до 4096 байт.</span><span class="sxs-lookup"><span data-stu-id="0a915-134">Most browsers restrict cookie size to 4096 bytes.</span></span> <span data-ttu-id="0a915-135">Для каждого домена доступно лишь ограниченное число файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="0a915-135">Only a limited number of cookies are available for each domain.</span></span>

<span data-ttu-id="0a915-136">Так как файлы cookie могут быть незаконно изменены, их нужно проверять в приложении.</span><span class="sxs-lookup"><span data-stu-id="0a915-136">Because cookies are subject to tampering, they must be validated by the app.</span></span> <span data-ttu-id="0a915-137">Файлы cookie могут удаляться пользователем и имеют ограниченный срок хранения на клиентах.</span><span class="sxs-lookup"><span data-stu-id="0a915-137">Cookies can be deleted by users and expire on clients.</span></span> <span data-ttu-id="0a915-138">Тем не менее файлы cookie обычно являются самым надежным способом хранения данных на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0a915-138">However, cookies are generally the most durable form of data persistence on the client.</span></span>

<span data-ttu-id="0a915-139">Файлы cookie часто используются для персонализации, когда содержимое настраивается под известного пользователя.</span><span class="sxs-lookup"><span data-stu-id="0a915-139">Cookies are often used for personalization, where content is customized for a known user.</span></span> <span data-ttu-id="0a915-140">В большинстве случаев пользователь проходит только идентификацию, а не проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="0a915-140">The user is only identified and not authenticated in most cases.</span></span> <span data-ttu-id="0a915-141">Файл cookie может хранить имя пользователя, имя учетной записи или уникальный идентификатор пользователя (например, GUID).</span><span class="sxs-lookup"><span data-stu-id="0a915-141">The cookie can store the user's name, account name, or unique user ID (such as a GUID).</span></span> <span data-ttu-id="0a915-142">Затем можно использовать файл cookie для доступа к личным настройкам пользователя, таким как цвет фона на веб-сайте.</span><span class="sxs-lookup"><span data-stu-id="0a915-142">You can then use the cookie to access the user's personalized settings, such as their preferred website background color.</span></span>

<span data-ttu-id="0a915-143">Помните об [Общем регламенте по защите данных в ЕС (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) при создании файлов cookie и решении вопросов с конфиденциальностью.</span><span class="sxs-lookup"><span data-stu-id="0a915-143">Be mindful of the [European Union General Data Protection Regulations (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection) when issuing cookies and dealing with privacy concerns.</span></span> <span data-ttu-id="0a915-144">Дополнительные сведения см. в разделе [Общий регламент по защите данных (GDPR), принятый в ЕС, в ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0a915-144">For more information, see [General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr).</span></span>

## <a name="session-state"></a><span data-ttu-id="0a915-145">Состояние сеанса</span><span class="sxs-lookup"><span data-stu-id="0a915-145">Session state</span></span>

<span data-ttu-id="0a915-146">Состояние сеанса — это сценарий ASP.NET Core для хранения пользовательских данных, когда пользователь находится в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="0a915-146">Session state is an ASP.NET Core scenario for storage of user data while the user browses a web app.</span></span> <span data-ttu-id="0a915-147">Состояние сеанса использует хранилище, обслуживаемое приложением, для сохранения данных между запросами от клиента.</span><span class="sxs-lookup"><span data-stu-id="0a915-147">Session state uses a store maintained by the app to persist data across requests from a client.</span></span> <span data-ttu-id="0a915-148">Данные сеанса поддерживаются кэшем и считаются временными, &mdash; сайт должен работать и без данных сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-148">The session data is backed by a cache and considered ephemeral data&mdash;the site should continue to function without the session data.</span></span> <span data-ttu-id="0a915-149">Критически важные данные приложения должны храниться в пользовательской базе данных и кэшироваться в сеансе только в качестве оптимизации производительности.</span><span class="sxs-lookup"><span data-stu-id="0a915-149">Critical application data should be stored in the user database and cached in session only as a performance optimization.</span></span>

> [!NOTE]
> <span data-ttu-id="0a915-150">Сеанс не поддерживается в приложениях [SignalR](xref:signalr/index), так как [хаб SignalR](xref:signalr/hubs) может выполняться независимо от контекста HTTP.</span><span class="sxs-lookup"><span data-stu-id="0a915-150">Session isn't supported in [SignalR](xref:signalr/index) apps because a [SignalR Hub](xref:signalr/hubs) may execute independent of an HTTP context.</span></span> <span data-ttu-id="0a915-151">Например, это может произойти, если хаб поддерживает длительный запрос на опрос открытым, когда время существования контекста HTTP для запроса уже истекло.</span><span class="sxs-lookup"><span data-stu-id="0a915-151">For example, this can occur when a long polling request is held open by a hub beyond the lifetime of the request's HTTP context.</span></span>

<span data-ttu-id="0a915-152">ASP.NET Core сохраняет состояние сеанса, предоставляя клиенту файл cookie, содержащий идентификатор сеанса, который отправляется в приложение вместе с каждым запросом.</span><span class="sxs-lookup"><span data-stu-id="0a915-152">ASP.NET Core maintains session state by providing a cookie to the client that contains a session ID, which is sent to the app with each request.</span></span> <span data-ttu-id="0a915-153">Приложение использует этот идентификатор для получения данных сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-153">The app uses the session ID to fetch the session data.</span></span>

<span data-ttu-id="0a915-154">Состояние сеанса реагирует на события следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0a915-154">Session state exhibits the following behaviors:</span></span>

* <span data-ttu-id="0a915-155">Так как файл cookie сеанса относится к конкретному браузеру, обеспечить общий доступ к сеансам из разных браузеров невозможно.</span><span class="sxs-lookup"><span data-stu-id="0a915-155">Because the session cookie is specific to the browser, sessions aren't shared across browsers.</span></span>
* <span data-ttu-id="0a915-156">Файлы cookie сеанса удаляются при завершении сеанса браузера.</span><span class="sxs-lookup"><span data-stu-id="0a915-156">Session cookies are deleted when the browser session ends.</span></span>
* <span data-ttu-id="0a915-157">Если получен файл cookie для просроченного сеанса, создается сеанс, использующий этот же файл.</span><span class="sxs-lookup"><span data-stu-id="0a915-157">If a cookie is received for an expired session, a new session is created that uses the same session cookie.</span></span>
* <span data-ttu-id="0a915-158">Пустые сеансы не сохраняются &mdash; в сеансе должно быть хотя бы одно значение, чтобы его можно быть сохранить между запросами.</span><span class="sxs-lookup"><span data-stu-id="0a915-158">Empty sessions aren't retained&mdash;the session must have at least one value set into it to persist the session across requests.</span></span> <span data-ttu-id="0a915-159">Если сеанс не сохраняется, для каждого нового запроса создается новый идентификатор сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-159">When a session isn't retained, a new session ID is generated for each new request.</span></span>
* <span data-ttu-id="0a915-160">Приложение хранит сеанс некоторое время после последнего запроса.</span><span class="sxs-lookup"><span data-stu-id="0a915-160">The app retains a session for a limited time after the last request.</span></span> <span data-ttu-id="0a915-161">Приложение устанавливает время ожидания сеанса или использует значение по умолчанию, равное 20 минутам.</span><span class="sxs-lookup"><span data-stu-id="0a915-161">The app either sets the session timeout or uses the default value of 20 minutes.</span></span> <span data-ttu-id="0a915-162">Состояние сеанса оптимально подходит для сохранения пользовательских данных, относящихся к определенному сеансу, которые не требуется хранить бессрочно для нескольких сеансов.</span><span class="sxs-lookup"><span data-stu-id="0a915-162">Session state is ideal for storing user data that's specific to a particular session but where the data doesn't require permanent storage across sessions.</span></span>
* <span data-ttu-id="0a915-163">Данные сеанса удаляются при вызове реализации [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) или когда истекает срок действия сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-163">Session data is deleted either when the [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) implementation is called or when the session expires.</span></span>
* <span data-ttu-id="0a915-164">Не существует механизма по умолчанию, который бы информировал код приложения о том, что браузер клиента закрыт или файл cookie сеанса удален или просрочен на клиенте.</span><span class="sxs-lookup"><span data-stu-id="0a915-164">There's no default mechanism to inform app code that a client browser has been closed or when the session cookie is deleted or expired on the client.</span></span>
<span data-ttu-id="0a915-165">Шаблоны ASP.NET Core MVC и Razor Pages включают поддержку [общего регламента по защите данных (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="0a915-165">The ASP.NET Core MVC and Razor pages templates include support for [General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span> <span data-ttu-id="0a915-166">[Файлы cookie для состояния сеанса не имеют существенного значения](xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential), состояние сеанса недоступно, если отключено отслеживание.</span><span class="sxs-lookup"><span data-stu-id="0a915-166">[Session state cookies aren't essential](xref:security/gdpr#tempdata-provider-and-session-state-cookies-are-not-essential), session state isn't functional when tracking is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="0a915-167">Не храните конфиденциальные данные в состоянии сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-167">Don't store sensitive data in session state.</span></span> <span data-ttu-id="0a915-168">Пользователь может не закрывать браузер и очистить файл cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-168">The user might not close the browser and clear the session cookie.</span></span> <span data-ttu-id="0a915-169">Некоторые браузеры сохраняют файлы cookie сеанса в нескольких окнах браузера.</span><span class="sxs-lookup"><span data-stu-id="0a915-169">Some browsers maintain valid session cookies across browser windows.</span></span> <span data-ttu-id="0a915-170">Сеанс может быть не ограничен одним пользователем &mdash; следующий пользователь продолжит использовать приложение с тем же файлом cookie сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-170">A session might not be restricted to a single user&mdash;the next user might continue to browse the app with the same session cookie.</span></span>

<span data-ttu-id="0a915-171">Поставщик кэша в памяти хранит данные сеанса в памяти сервера, содержащего приложение.</span><span class="sxs-lookup"><span data-stu-id="0a915-171">The in-memory cache provider stores session data in the memory of the server where the app resides.</span></span> <span data-ttu-id="0a915-172">В сценарии с фермой серверов:</span><span class="sxs-lookup"><span data-stu-id="0a915-172">In a server farm scenario:</span></span>

* <span data-ttu-id="0a915-173">Используйте *прикрепленные сеансы* для привязки сеанса к конкретному экземпляру приложения на отдельном сервере.</span><span class="sxs-lookup"><span data-stu-id="0a915-173">Use *sticky sessions* to tie each session to a specific app instance on an individual server.</span></span> <span data-ttu-id="0a915-174">[Служба приложений Azure](https://azure.microsoft.com/services/app-service/) использует [маршрутизацию запросов приложений (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module), чтобы прикрепленные сеансы создавались по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0a915-174">[Azure App Service](https://azure.microsoft.com/services/app-service/) uses [Application Request Routing (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) to enforce sticky sessions by default.</span></span> <span data-ttu-id="0a915-175">Однако прикрепленные сеансы могут негативно повлиять на масштабируемость и усложнить обновление веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="0a915-175">However, sticky sessions can affect scalability and complicate web app updates.</span></span> <span data-ttu-id="0a915-176">Лучше использовать распределенные кэши SQL Server или Redis, для которых прикрепленные сеансы не требуются.</span><span class="sxs-lookup"><span data-stu-id="0a915-176">A better approach is to use a Redis or SQL Server distributed cache, which doesn't require sticky sessions.</span></span> <span data-ttu-id="0a915-177">Для получения дополнительной информации см. <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="0a915-177">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="0a915-178">Файл cookie сеанса шифруется через [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span><span class="sxs-lookup"><span data-stu-id="0a915-178">The session cookie is encrypted via [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector).</span></span> <span data-ttu-id="0a915-179">Необходимо правильно настроить защиту данных, чтобы читать файлы cookie сеанса на каждом компьютере.</span><span class="sxs-lookup"><span data-stu-id="0a915-179">Data Protection must be properly configured to read session cookies on each machine.</span></span> <span data-ttu-id="0a915-180">Дополнительные сведения см. статьях <xref:security/data-protection/introduction> и [Key storage providers in ASP.NET Core](xref:security/data-protection/implementation/key-storage-providers) (Поставщики хранилища ключей в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="0a915-180">For more information, see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

### <a name="configure-session-state"></a><span data-ttu-id="0a915-181">Настройка состояния сеанса</span><span class="sxs-lookup"><span data-stu-id="0a915-181">Configure session state</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0a915-182">Пакет [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/), который входит в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), предоставляет ПО промежуточного слоя для управления состоянием сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-182">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), provides middleware for managing session state.</span></span> <span data-ttu-id="0a915-183">Чтобы включить сеанс ПО промежуточного слоя для сеансов, `Startup` должен содержать:</span><span class="sxs-lookup"><span data-stu-id="0a915-183">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0a915-184">Пакет [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) предоставляет ПО промежуточного слоя для управления состоянием сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-184">The [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package provides middleware for managing session state.</span></span> <span data-ttu-id="0a915-185">Чтобы включить сеанс ПО промежуточного слоя для сеансов, `Startup` должен содержать:</span><span class="sxs-lookup"><span data-stu-id="0a915-185">To enable the session middleware, `Startup` must contain:</span></span>

::: moniker-end

* <span data-ttu-id="0a915-186">Любой из кэшей памяти [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache),</span><span class="sxs-lookup"><span data-stu-id="0a915-186">Any of the [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) memory caches.</span></span> <span data-ttu-id="0a915-187">при этом реализация `IDistributedCache` используется в качестве резервного хранилища для сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-187">The `IDistributedCache` implementation is used as a backing store for session.</span></span> <span data-ttu-id="0a915-188">Для получения дополнительной информации см. <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="0a915-188">For more information, see <xref:performance/caching/distributed>.</span></span>
* <span data-ttu-id="0a915-189">Вызов [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0a915-189">A call to [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession) in `ConfigureServices`.</span></span>
* <span data-ttu-id="0a915-190">Вызов [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) в `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0a915-190">A call to [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) in `Configure`.</span></span>

<span data-ttu-id="0a915-191">Следующий код показывает, как настроить поставщик сеансов в памяти с реализацией в памяти `IDistributedCache` по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="0a915-191">The following code shows how to set up the in-memory session provider with a default in-memory implementation of `IDistributedCache`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

<span data-ttu-id="0a915-192">Порядок ПО промежуточного слоя важен.</span><span class="sxs-lookup"><span data-stu-id="0a915-192">The order of middleware is important.</span></span> <span data-ttu-id="0a915-193">В предыдущем примере исключение `InvalidOperationException` возникает при вызове `UseSession` после `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="0a915-193">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="0a915-194">Дополнительные сведения см. в разделе [Порядок расположения ПО промежуточного слоя](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="0a915-194">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

<span data-ttu-id="0a915-195">После настройки состояния сеанса доступно свойство [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="0a915-195">[HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session) is available after session state is configured.</span></span>

<span data-ttu-id="0a915-196">Невозможно получить доступ к `HttpContext.Session` до вызова `UseSession`.</span><span class="sxs-lookup"><span data-stu-id="0a915-196">`HttpContext.Session` can't be accessed before `UseSession` has been called.</span></span>

<span data-ttu-id="0a915-197">Невозможно создать новый сеанс с новым файлом cookie сеанса после того, как приложение начинает запись в поток ответа.</span><span class="sxs-lookup"><span data-stu-id="0a915-197">A new session with a new session cookie can't be created after the app has begun writing to the response stream.</span></span> <span data-ttu-id="0a915-198">Исключение записывается в журнал веб-сервера и не отображается в браузере.</span><span class="sxs-lookup"><span data-stu-id="0a915-198">The exception is recorded in the web server log and not displayed in the browser.</span></span>

### <a name="load-session-state-asynchronously"></a><span data-ttu-id="0a915-199">Асинхронная загрузка состояния сеанса</span><span class="sxs-lookup"><span data-stu-id="0a915-199">Load session state asynchronously</span></span>

<span data-ttu-id="0a915-200">Поставщик сеансов по умолчанию в ASP.NET Core загружает запись сеанса из резервного хранилища [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) в асинхронном режиме только при явном вызове метода [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) перед методами [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set) или [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove).</span><span class="sxs-lookup"><span data-stu-id="0a915-200">The default session provider in ASP.NET Core loads session records from the underlying [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) backing store asynchronously only if the [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) method is explicitly called before the [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue), [Set](/dotnet/api/microsoft.aspnetcore.http.isession.set), or [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) methods.</span></span> <span data-ttu-id="0a915-201">Если `LoadAsync` не вызывается первым, базовая запись сеанса загружается синхронно, что может негативно повлиять на производительность.</span><span class="sxs-lookup"><span data-stu-id="0a915-201">If `LoadAsync` isn't called first, the underlying session record is loaded synchronously, which can incur a performance penalty at scale.</span></span>

<span data-ttu-id="0a915-202">Чтобы принудительно использовать этот режим в приложениях, используйте для реализаций [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) и [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) оболочку из версий, которые выдают исключение, когда метод `LoadAsync` не вызывается перед `TryGetValue`, `Set` или `Remove`.</span><span class="sxs-lookup"><span data-stu-id="0a915-202">To have apps enforce this pattern, wrap the [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) and [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) implementations with versions that throw an exception if the `LoadAsync` method isn't called before `TryGetValue`, `Set`, or `Remove`.</span></span> <span data-ttu-id="0a915-203">Зарегистрируйте версии оболочки в контейнере служб.</span><span class="sxs-lookup"><span data-stu-id="0a915-203">Register the wrapped versions in the services container.</span></span>

### <a name="session-options"></a><span data-ttu-id="0a915-204">Параметры сеанса</span><span class="sxs-lookup"><span data-stu-id="0a915-204">Session options</span></span>

<span data-ttu-id="0a915-205">Чтобы переопределить значения по умолчанию для сеанса, используйте [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span><span class="sxs-lookup"><span data-stu-id="0a915-205">To override session defaults, use [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions).</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="0a915-206">Параметр</span><span class="sxs-lookup"><span data-stu-id="0a915-206">Option</span></span> | <span data-ttu-id="0a915-207">Описание</span><span class="sxs-lookup"><span data-stu-id="0a915-207">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="0a915-208">Файл cookie</span><span class="sxs-lookup"><span data-stu-id="0a915-208">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | <span data-ttu-id="0a915-209">Определяет параметры, используемые для создания файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="0a915-209">Determines the settings used to create the cookie.</span></span> <span data-ttu-id="0a915-210">Параметр [Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) по умолчанию имеет значение [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="0a915-210">[Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) defaults to [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> <span data-ttu-id="0a915-211">Параметр [Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) по умолчанию имеет значение [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="0a915-211">[Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> <span data-ttu-id="0a915-212">Параметр [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) по умолчанию имеет значение [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span><span class="sxs-lookup"><span data-stu-id="0a915-212">[SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) defaults to [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`).</span></span> <span data-ttu-id="0a915-213">Параметр [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) по умолчанию имеет значение `true`.</span><span class="sxs-lookup"><span data-stu-id="0a915-213">[HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`.</span></span> <span data-ttu-id="0a915-214">Параметр [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) по умолчанию имеет значение `false`.</span><span class="sxs-lookup"><span data-stu-id="0a915-214">[IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) defaults to `false`.</span></span> |
| [<span data-ttu-id="0a915-215">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="0a915-215">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="0a915-216">`IdleTimeout` указывает, как долго сеанс может быть неактивным, прежде чем его содержимое отбрасывается.</span><span class="sxs-lookup"><span data-stu-id="0a915-216">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="0a915-217">Каждый доступ к сеансу сбрасывает время ожидания.</span><span class="sxs-lookup"><span data-stu-id="0a915-217">Each session access resets the timeout.</span></span> <span data-ttu-id="0a915-218">Этот параметр применяется только к содержимому сеанса, а не к файлу cookie.</span><span class="sxs-lookup"><span data-stu-id="0a915-218">This setting only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="0a915-219">Значение по умолчанию — 20 минут.</span><span class="sxs-lookup"><span data-stu-id="0a915-219">The default is 20 minutes.</span></span> |
| [<span data-ttu-id="0a915-220">IOTimeout</span><span class="sxs-lookup"><span data-stu-id="0a915-220">IOTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | <span data-ttu-id="0a915-221">Максимальное количество времени для загрузки сеанса из хранилища или его сохранения обратно в хранилище.</span><span class="sxs-lookup"><span data-stu-id="0a915-221">The maximim amount of time allowed to load a session from the store or to commit it back to the store.</span></span> <span data-ttu-id="0a915-222">Этот параметр может применяться только к асинхронным операциям.</span><span class="sxs-lookup"><span data-stu-id="0a915-222">This setting may only apply to asynchronous operations.</span></span> <span data-ttu-id="0a915-223">Время ожидания можно отключить с помощью [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span><span class="sxs-lookup"><span data-stu-id="0a915-223">This timeout can be disabled using [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan).</span></span> <span data-ttu-id="0a915-224">Значение по умолчанию — 1 минута.</span><span class="sxs-lookup"><span data-stu-id="0a915-224">The default is 1 minute.</span></span> |

<span data-ttu-id="0a915-225">Сеанс использует файл cookie для отслеживания и идентификации запросов в одном браузере.</span><span class="sxs-lookup"><span data-stu-id="0a915-225">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="0a915-226">По умолчанию этот файл cookie называется `.AspNetCore.Session` и использует путь `/`.</span><span class="sxs-lookup"><span data-stu-id="0a915-226">By default, this cookie is named `.AspNetCore.Session`, and it uses a path of `/`.</span></span> <span data-ttu-id="0a915-227">Так как по умолчанию файл cookie не указывает домен, он остается недоступным для клиентского сценария на странице (так как [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) по умолчанию имеет значение `true`).</span><span class="sxs-lookup"><span data-stu-id="0a915-227">Because the cookie default doesn't specify a domain, it isn't made available to the client-side script on the page (because [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) defaults to `true`).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="0a915-228">Параметр</span><span class="sxs-lookup"><span data-stu-id="0a915-228">Option</span></span> | <span data-ttu-id="0a915-229">Описание</span><span class="sxs-lookup"><span data-stu-id="0a915-229">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="0a915-230">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="0a915-230">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | <span data-ttu-id="0a915-231">Определяет домен, используемый для создания файла cookie.</span><span class="sxs-lookup"><span data-stu-id="0a915-231">Determines the domain used to create the cookie.</span></span> <span data-ttu-id="0a915-232">`CookieDomain` не устанавливается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0a915-232">`CookieDomain` isn't set by default.</span></span> |
| [<span data-ttu-id="0a915-233">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="0a915-233">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | <span data-ttu-id="0a915-234">Определяет, будет ли браузер предоставлять доступ к файлу cookie для JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0a915-234">Determines if the browser should allow the cookie to be accessed by client-side JavaScript.</span></span> <span data-ttu-id="0a915-235">Значение по умолчанию — `true`, то есть файл cookie передается только HTTP-запросам и не доступен для скрипта на странице.</span><span class="sxs-lookup"><span data-stu-id="0a915-235">The default is `true`, which means the cookie is only passed to HTTP requests and isn't made available to script on the page.</span></span> |
| [<span data-ttu-id="0a915-236">CookieName</span><span class="sxs-lookup"><span data-stu-id="0a915-236">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | <span data-ttu-id="0a915-237">Определяет имя файла cookie, используемое для хранения идентификатора сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-237">Determines the cookie name used to persist the session ID.</span></span> <span data-ttu-id="0a915-238">Значение по умолчанию — [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span><span class="sxs-lookup"><span data-stu-id="0a915-238">The default is [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`).</span></span> |
| [<span data-ttu-id="0a915-239">CookiePath</span><span class="sxs-lookup"><span data-stu-id="0a915-239">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | <span data-ttu-id="0a915-240">Определяет путь, используемый для создания файла cookie.</span><span class="sxs-lookup"><span data-stu-id="0a915-240">Determines the path used to create the cookie.</span></span> <span data-ttu-id="0a915-241">Значение по умолчанию — [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span><span class="sxs-lookup"><span data-stu-id="0a915-241">Defaults to [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`).</span></span> |
| [<span data-ttu-id="0a915-242">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="0a915-242">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | <span data-ttu-id="0a915-243">Определяет, должен ли файл cookie передаваться только по HTTPS-запросу.</span><span class="sxs-lookup"><span data-stu-id="0a915-243">Determines if the cookie should only be transmitted on HTTPS requests.</span></span> <span data-ttu-id="0a915-244">Значение по умолчанию — [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span><span class="sxs-lookup"><span data-stu-id="0a915-244">The default is [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`).</span></span> |
| [<span data-ttu-id="0a915-245">IdleTimeout</span><span class="sxs-lookup"><span data-stu-id="0a915-245">IdleTimeout</span></span>](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | <span data-ttu-id="0a915-246">`IdleTimeout` указывает, как долго сеанс может быть неактивным, прежде чем его содержимое отбрасывается.</span><span class="sxs-lookup"><span data-stu-id="0a915-246">The `IdleTimeout` indicates how long the session can be idle before its contents are abandoned.</span></span> <span data-ttu-id="0a915-247">Каждый доступ к сеансу сбрасывает время ожидания.</span><span class="sxs-lookup"><span data-stu-id="0a915-247">Each session access resets the timeout.</span></span> <span data-ttu-id="0a915-248">Это относится только к содержимому сеанса, а не к файлу cookie.</span><span class="sxs-lookup"><span data-stu-id="0a915-248">Note this only applies to the content of the session, not the cookie.</span></span> <span data-ttu-id="0a915-249">Значение по умолчанию — 20 минут.</span><span class="sxs-lookup"><span data-stu-id="0a915-249">The default is 20 minutes.</span></span> |

<span data-ttu-id="0a915-250">Сеанс использует файл cookie для отслеживания и идентификации запросов в одном браузере.</span><span class="sxs-lookup"><span data-stu-id="0a915-250">Session uses a cookie to track and identify requests from a single browser.</span></span> <span data-ttu-id="0a915-251">По умолчанию этот файл cookie называется `.AspNet.Session` и использует путь `/`.</span><span class="sxs-lookup"><span data-stu-id="0a915-251">By default, this cookie is named `.AspNet.Session`, and it uses a path of `/`.</span></span>

::: moniker-end

<span data-ttu-id="0a915-252">Чтобы переопределить файл cookie по умолчанию для сеанса, используйте `SessionOptions`:</span><span class="sxs-lookup"><span data-stu-id="0a915-252">To override cookie session defaults, use `SessionOptions`:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

<span data-ttu-id="0a915-253">Приложение использует свойство [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout), чтобы определить, как долго сеанс может оставаться неактивным до сброса его содержимого в кэше сервера.</span><span class="sxs-lookup"><span data-stu-id="0a915-253">The app uses the [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) property to determine how long a session can be idle before its contents in the server's cache are abandoned.</span></span> <span data-ttu-id="0a915-254">Это свойство не зависит от срока действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="0a915-254">This property is independent of the cookie expiration.</span></span> <span data-ttu-id="0a915-255">Каждый запрос, проходящий через [ПО промежуточного слоя сеанса](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware), сбрасывает это время ожидания.</span><span class="sxs-lookup"><span data-stu-id="0a915-255">Each request that passes through the [Session Middleware](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware) resets the timeout.</span></span>

<span data-ttu-id="0a915-256">Состояние сеанса является *неблокирующим*.</span><span class="sxs-lookup"><span data-stu-id="0a915-256">Session state is *non-locking*.</span></span> <span data-ttu-id="0a915-257">Когда два запроса пытаются изменить содержимое сеанса, последний из них переопределяет первый.</span><span class="sxs-lookup"><span data-stu-id="0a915-257">If two requests simultaneously attempt to modify the contents of a session, the last request overrides the first.</span></span> <span data-ttu-id="0a915-258">`Session` реализован в виде *согласованного сеанса*, что означает, что все содержимое хранится вместе.</span><span class="sxs-lookup"><span data-stu-id="0a915-258">`Session` is implemented as a *coherent session*, which means that all the contents are stored together.</span></span> <span data-ttu-id="0a915-259">Когда два запроса пытаются изменить разные значения сеанса, последний запрос может переопределить изменения, внесенные первым.</span><span class="sxs-lookup"><span data-stu-id="0a915-259">When two requests seek to modify different session values, the last request may override session changes made by the first.</span></span>

### <a name="set-and-get-session-values"></a><span data-ttu-id="0a915-260">Установка и получение значений сеанса</span><span class="sxs-lookup"><span data-stu-id="0a915-260">Set and get Session values</span></span>

<span data-ttu-id="0a915-261">Получить доступ к состоянию сеанса можно через класс Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) или класс MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) со свойством [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span><span class="sxs-lookup"><span data-stu-id="0a915-261">Session state is accessed from a Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) class or MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) class with [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session).</span></span> <span data-ttu-id="0a915-262">Оно является реализацией [ISession](/dotnet/api/microsoft.aspnetcore.http.isession).</span><span class="sxs-lookup"><span data-stu-id="0a915-262">This property is an [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) implementation.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0a915-263">Реализация `ISession` предоставляет несколько методов расширения для задания и извлечения значений типа integer и string.</span><span class="sxs-lookup"><span data-stu-id="0a915-263">The `ISession` implementation provides several extension methods to set and retrieve integer and string values.</span></span> <span data-ttu-id="0a915-264">Методы расширения находятся в пространстве имен [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (добавьте оператор `using Microsoft.AspNetCore.Http;`, чтобы получить доступ к методам расширения), когда проект ссылается на пакет [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="0a915-264">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span> <span data-ttu-id="0a915-265">Оба пакета входят в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0a915-265">Both packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0a915-266">Реализация `ISession` предоставляет несколько методов расширения для задания и извлечения значений integer и string.</span><span class="sxs-lookup"><span data-stu-id="0a915-266">The `ISession` implementation provides several extension methods to set and retreive integer and string values.</span></span> <span data-ttu-id="0a915-267">Методы расширения находятся в пространстве имен [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) (добавьте оператор `using Microsoft.AspNetCore.Http;`, чтобы получить доступ к методам расширения), когда проект ссылается на пакет [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="0a915-267">The extension methods are in the [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) namespace (add a `using Microsoft.AspNetCore.Http;` statement to gain access to the extension methods) when the [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) package is referenced by the project.</span></span>

::: moniker-end

<span data-ttu-id="0a915-268">Методы расширения `ISession`:</span><span class="sxs-lookup"><span data-stu-id="0a915-268">`ISession` extension methods:</span></span>

* [<span data-ttu-id="0a915-269">Get(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="0a915-269">Get(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [<span data-ttu-id="0a915-270">GetInt32(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="0a915-270">GetInt32(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [<span data-ttu-id="0a915-271">GetString(ISession, String)</span><span class="sxs-lookup"><span data-stu-id="0a915-271">GetString(ISession, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [<span data-ttu-id="0a915-272">SetInt32(ISession, String, Int32)</span><span class="sxs-lookup"><span data-stu-id="0a915-272">SetInt32(ISession, String, Int32)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [<span data-ttu-id="0a915-273">SetString(ISession, String, String)</span><span class="sxs-lookup"><span data-stu-id="0a915-273">SetString(ISession, String, String)</span></span>](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

<span data-ttu-id="0a915-274">В следующем примере извлекается значение сеанса для ключа `IndexModel.SessionKeyName` (`_Name` в примере приложения) на странице Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="0a915-274">The following example retrieves the session value for the `IndexModel.SessionKeyName` key (`_Name` in the sample app) in a Razor Pages page:</span></span>

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

<span data-ttu-id="0a915-275">В следующем примере показано, как задать, а затем получить значения integer и string:</span><span class="sxs-lookup"><span data-stu-id="0a915-275">The following example shows how to set and get an integer and a string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

<span data-ttu-id="0a915-276">Все данные сеанса должны быть сериализованы, чтобы использовать сценарий-распределенного кэша даже при использовании кэша в памяти.</span><span class="sxs-lookup"><span data-stu-id="0a915-276">All session data must be serialized to enable a distributed cache scenario, even when using the in-memory cache.</span></span> <span data-ttu-id="0a915-277">Предоставляются минимальные сериализаторы строки и числа (см. методы и методы расширения [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span><span class="sxs-lookup"><span data-stu-id="0a915-277">Minimal string and number serializers are provided (see the methods and extension methods of [ISession](/dotnet/api/microsoft.aspnetcore.http.isession)).</span></span> <span data-ttu-id="0a915-278">Сложные типы должны быть сериализованы пользователем с помощью другого механизма, например JSON.</span><span class="sxs-lookup"><span data-stu-id="0a915-278">Complex types must be serialized by the user using another mechanism, such as JSON.</span></span>

<span data-ttu-id="0a915-279">Добавьте приведенные ниже методы расширения, чтобы задавать и получать сериализуемые объекты:</span><span class="sxs-lookup"><span data-stu-id="0a915-279">Add the following extension methods to set and get serializable objects:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0a915-280">Следующий пример показывает, как задать и получить сериализуемый объект с помощью методов расширения:</span><span class="sxs-lookup"><span data-stu-id="0a915-280">The following example shows how to set and get a serializable object with the extension methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="0a915-281">TempData</span><span class="sxs-lookup"><span data-stu-id="0a915-281">TempData</span></span>

<span data-ttu-id="0a915-282">ASP.NET Core предоставляет [свойство TempData модели страницы Razor Pages](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) или [TempData контроллера MVC](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span><span class="sxs-lookup"><span data-stu-id="0a915-282">ASP.NET Core exposes the [TempData property of a Razor Pages page model](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata) or [TempData of an MVC controller](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata).</span></span> <span data-ttu-id="0a915-283">Это свойство хранит данные до тех пор, пока они не будут прочитаны.</span><span class="sxs-lookup"><span data-stu-id="0a915-283">This property stores data until it's read.</span></span> <span data-ttu-id="0a915-284">Для проверки данных без удаления можно использовать методы [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) и [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek).</span><span class="sxs-lookup"><span data-stu-id="0a915-284">The [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) and [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="0a915-285">TempData особенно удобно использовать для перенаправления, когда данные требуются больше чем для одного запроса.</span><span class="sxs-lookup"><span data-stu-id="0a915-285">TempData is particularly useful for redirection when data is required for more than a single request.</span></span> <span data-ttu-id="0a915-286">TempData реализуется поставщиками TempData, например с помощью файлов cookie или состояния сеанса.</span><span class="sxs-lookup"><span data-stu-id="0a915-286">TempData is implemented by TempData providers using either cookies or session state.</span></span>

### <a name="tempdata-providers"></a><span data-ttu-id="0a915-287">Поставщики TempData</span><span class="sxs-lookup"><span data-stu-id="0a915-287">TempData providers</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0a915-288">В ASP.NET Core 2.0 и более поздних версий поставщик TempData, основанный на файлах cookie, используется по умолчанию для хранения TempData в файлах cookie.</span><span class="sxs-lookup"><span data-stu-id="0a915-288">In ASP.NET Core 2.0 or later, the cookie-based TempData provider is used by default to store TempData in cookies.</span></span>

<span data-ttu-id="0a915-289">Данные в файле cookie шифруются с помощью [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), кодируются с помощью [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), а затем фрагментируются.</span><span class="sxs-lookup"><span data-stu-id="0a915-289">The cookie data is encrypted using [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector), encoded with [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder), then chunked.</span></span> <span data-ttu-id="0a915-290">Так как файл cookie фрагментируется, ограничение на размер одного такого файла, заданное в ASP.NET Core 1.x, не применяется.</span><span class="sxs-lookup"><span data-stu-id="0a915-290">Because the cookie is chunked, the single cookie size limit found in ASP.NET Core 1.x doesn't apply.</span></span> <span data-ttu-id="0a915-291">Данные файла cookie не сжимаются, так как сжатие зашифрованных данных может привести к проблемам с безопасностью, например атакам [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) и [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).</span><span class="sxs-lookup"><span data-stu-id="0a915-291">The cookie data isn't compressed because compressing encrypted data can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span> <span data-ttu-id="0a915-292">Дополнительные сведения на поставщике TempData, основанном на файлах cookie, см. в разделе [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span><span class="sxs-lookup"><span data-stu-id="0a915-292">For more information on the cookie-based TempData provider, see [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0a915-293">В ASP.NET Core 1.0 и 1.1 для состояния сеанса по умолчанию используется поставщик TempData.</span><span class="sxs-lookup"><span data-stu-id="0a915-293">In ASP.NET Core 1.0 and 1.1, the session state TempData provider is the default provider.</span></span>

::: moniker-end

### <a name="choose-a-tempdata-provider"></a><span data-ttu-id="0a915-294">Выбор поставщика TempData</span><span class="sxs-lookup"><span data-stu-id="0a915-294">Choose a TempData provider</span></span>

<span data-ttu-id="0a915-295">Выбор поставщика TempData включает в себя несколько аспектов:</span><span class="sxs-lookup"><span data-stu-id="0a915-295">Choosing a TempData provider involves several considerations, such as:</span></span>

1. <span data-ttu-id="0a915-296">Приложение уже использует состояние сеанса?</span><span class="sxs-lookup"><span data-stu-id="0a915-296">Does the app already use session state?</span></span> <span data-ttu-id="0a915-297">Если это так, использование поставщика TempData для состояния сеанса не требует от приложения никаких дополнительных издержек (кроме объема данных).</span><span class="sxs-lookup"><span data-stu-id="0a915-297">If so, using the session state TempData provider has no additional cost to the app (aside from the size of the data).</span></span>
2. <span data-ttu-id="0a915-298">Приложение использует TempData лишь ограниченно, для сравнительно небольших объемов данных (до 500 байт)?</span><span class="sxs-lookup"><span data-stu-id="0a915-298">Does the app use TempData only sparingly for relatively small amounts of data (up to 500 bytes)?</span></span> <span data-ttu-id="0a915-299">Если это так, поставщик TempData на основе файлов cookie незначительно увеличивает издержки для каждого запроса, переносящего TempData.</span><span class="sxs-lookup"><span data-stu-id="0a915-299">If so, the cookie TempData provider adds a small cost to each request that carries TempData.</span></span> <span data-ttu-id="0a915-300">В противном случае поставщик TempData для состояния сеанса удобно использовать, чтобы устранить круговой обход большого объема данных в каждом запросе до полного использования TempData.</span><span class="sxs-lookup"><span data-stu-id="0a915-300">If not, the session state TempData provider can be beneficial to avoid round-tripping a large amount of data in each request until the TempData is consumed.</span></span>
3. <span data-ttu-id="0a915-301">Приложение выполняется на ферме серверов на нескольких серверах?</span><span class="sxs-lookup"><span data-stu-id="0a915-301">Does the app run in a server farm on multiple servers?</span></span> <span data-ttu-id="0a915-302">Если это так, не требуется дополнительная настройка для использования поставщика TempData для файлов cookie, за исключением защиты данных (см. статьи <xref:security/data-protection/introduction> и [Key storage providers in ASP.NET Core](xref:security/data-protection/implementation/key-storage-providers) (Поставщики хранилища ключей в ASP.NET Core)).</span><span class="sxs-lookup"><span data-stu-id="0a915-302">If so, there's no additional configuration required to use the cookie TempData provider outside of Data Protection (see <xref:security/data-protection/introduction> and [Key storage providers](xref:security/data-protection/implementation/key-storage-providers)).</span></span>

> [!NOTE]
> <span data-ttu-id="0a915-303">Большинство веб-клиентов (например, веб-браузеры) налагают ограничение на максимальный размер каждого файла cookie и (или) общее количество таких файлов.</span><span class="sxs-lookup"><span data-stu-id="0a915-303">Most web clients (such as web browsers) enforce limits on the maximum size of each cookie, the total number of cookies, or both.</span></span> <span data-ttu-id="0a915-304">При использовании поставщика TempData на основе файлов cookie убедитесь, что приложение не нарушает эти ограничения.</span><span class="sxs-lookup"><span data-stu-id="0a915-304">When using the cookie TempData provider, verify the app won't exceed these limits.</span></span> <span data-ttu-id="0a915-305">Учитывайте общий объем данных.</span><span class="sxs-lookup"><span data-stu-id="0a915-305">Consider the total size of the data.</span></span> <span data-ttu-id="0a915-306">Учитывайте увеличение размеров файла cookie в результате шифрования и фрагментирования.</span><span class="sxs-lookup"><span data-stu-id="0a915-306">Account for increases in cookie size due to encryption and chunking.</span></span>

### <a name="configure-the-tempdata-provider"></a><span data-ttu-id="0a915-307">Настройка поставщика TempData</span><span class="sxs-lookup"><span data-stu-id="0a915-307">Configure the TempData provider</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="0a915-308">Поставщик TempData на основе файлов cookie включен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0a915-308">The cookie-based TempData provider is enabled by default.</span></span>

<span data-ttu-id="0a915-309">Чтобы включить поставщик TempData на основе сеанса, используйте метод расширения [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider):</span><span class="sxs-lookup"><span data-stu-id="0a915-309">To enable the session-based TempData provider, use the [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) extension method:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="0a915-310">Следующий код класса `Startup` настраивает поставщик TempData на основе сеансов:</span><span class="sxs-lookup"><span data-stu-id="0a915-310">The following `Startup` class code configures the session-based TempData provider:</span></span>

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

<span data-ttu-id="0a915-311">Порядок ПО промежуточного слоя важен.</span><span class="sxs-lookup"><span data-stu-id="0a915-311">The order of middleware is important.</span></span> <span data-ttu-id="0a915-312">В предыдущем примере исключение `InvalidOperationException` возникает при вызове `UseSession` после `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="0a915-312">In the preceding example, an `InvalidOperationException` exception occurs when `UseSession` is invoked after `UseMvc`.</span></span> <span data-ttu-id="0a915-313">Дополнительные сведения см. в разделе [Порядок расположения ПО промежуточного слоя](xref:fundamentals/middleware/index#order).</span><span class="sxs-lookup"><span data-stu-id="0a915-313">For more information, see [Middleware Ordering](xref:fundamentals/middleware/index#order).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a915-314">Если вы ориентируетесь на .NET Framework и используете поставщик TempData на основе сеансов, добавьте в проект пакет [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/).</span><span class="sxs-lookup"><span data-stu-id="0a915-314">If targeting .NET Framework and using the session-based TempData provider, add the [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) package to the project.</span></span>

## <a name="query-strings"></a><span data-ttu-id="0a915-315">Строки запроса</span><span class="sxs-lookup"><span data-stu-id="0a915-315">Query strings</span></span>

<span data-ttu-id="0a915-316">Вы можете передать ограниченный объем данных из одного запроса в другой, добавив их в строку запроса нового запроса.</span><span class="sxs-lookup"><span data-stu-id="0a915-316">A limited amount of data can be passed from one request to another by adding it to the new request's query string.</span></span> <span data-ttu-id="0a915-317">Это удобно для захвата состояния в сохраняемом виде, что позволяет обмениваться ссылками с внедренным состоянием по электронной почте или через социальные сети.</span><span class="sxs-lookup"><span data-stu-id="0a915-317">This is useful for capturing state in a persistent manner that allows links with embedded state to be shared through email or social networks.</span></span> <span data-ttu-id="0a915-318">Поскольку строки запроса URL-адреса являются открытыми, никогда не используйте их для конфиденциальных данных.</span><span class="sxs-lookup"><span data-stu-id="0a915-318">Because URL query strings are public, never use query strings for sensitive data.</span></span>

<span data-ttu-id="0a915-319">Вдобавок к случайному раскрытию информации включение данных в строки запроса может открыть возможности для атак с [подделкой межсайтовых запросов (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), которые могут обманом вынудить пользователей посещать вредоносные веб-сайты во время проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0a915-319">In addition to unintended sharing, including data in query strings can create opportunities for [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) attacks, which can trick users into visiting malicious sites while authenticated.</span></span> <span data-ttu-id="0a915-320">Это позволит злоумышленникам похитить данные пользователей из приложения или предпринимать вредоносные действия от их имени.</span><span class="sxs-lookup"><span data-stu-id="0a915-320">Attackers can then steal user data from the app or take malicious actions on behalf of the user.</span></span> <span data-ttu-id="0a915-321">Любое сохраненное состояние приложения или сеанса необходимо защитить от атак CSRF.</span><span class="sxs-lookup"><span data-stu-id="0a915-321">Any preserved app or session state must protect against CSRF attacks.</span></span> <span data-ttu-id="0a915-322">Дополнительные сведения см. в статье [Предотвращение атак с подделкой межсайтовых запросов (XSRF/CSRF)](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="0a915-322">For more information, see [Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery).</span></span>

## <a name="hidden-fields"></a><span data-ttu-id="0a915-323">Скрытые поля</span><span class="sxs-lookup"><span data-stu-id="0a915-323">Hidden fields</span></span>

<span data-ttu-id="0a915-324">Данные можно сохранить в скрытых полях формы и опубликовать обратно при следующем запросе.</span><span class="sxs-lookup"><span data-stu-id="0a915-324">Data can be saved in hidden form fields and posted back on the next request.</span></span> <span data-ttu-id="0a915-325">Это типичное поведение для многостраничных форм.</span><span class="sxs-lookup"><span data-stu-id="0a915-325">This is common in multi-page forms.</span></span> <span data-ttu-id="0a915-326">Так как клиент может незаконно изменить данные, приложение всегда должно перепроверять данные в скрытых полях.</span><span class="sxs-lookup"><span data-stu-id="0a915-326">Because the client can potentially tamper with the data, the app must always revalidate the data stored in hidden fields.</span></span>

## <a name="httpcontextitems"></a><span data-ttu-id="0a915-327">HttpContext.Items</span><span class="sxs-lookup"><span data-stu-id="0a915-327">HttpContext.Items</span></span>

<span data-ttu-id="0a915-328">Коллекция [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) используется для хранения данных при обработке одного запроса.</span><span class="sxs-lookup"><span data-stu-id="0a915-328">The [HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) collection is used to store data while processing a single request.</span></span> <span data-ttu-id="0a915-329">Ее содержимое удаляется после обработки каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="0a915-329">The collection's contents are discarded after a request is processed.</span></span> <span data-ttu-id="0a915-330">Коллекцию `Items` часто используют для обеспечения взаимодействия компонентов или ПО промежуточного слоя, когда они выполняются в разные моменты времени во время обработки запроса и не могут передавать параметры напрямую.</span><span class="sxs-lookup"><span data-stu-id="0a915-330">The `Items` collection is often used to allow components or middleware to communicate when they operate at different points in time during a request and have no direct way to pass parameters.</span></span>

<span data-ttu-id="0a915-331">В следующем примере [ПО промежуточного слоя](xref:fundamentals/middleware/index) добавляет `isVerified` в коллекцию `Items`.</span><span class="sxs-lookup"><span data-stu-id="0a915-331">In the following example, [middleware](xref:fundamentals/middleware/index) adds `isVerified` to the `Items` collection.</span></span>

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

<span data-ttu-id="0a915-332">Далее в конвейере другое ПО промежуточного слоя может получить доступ к значению `isVerified`:</span><span class="sxs-lookup"><span data-stu-id="0a915-332">Later in the pipeline, another middleware can access the value of `isVerified`:</span></span>

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

<span data-ttu-id="0a915-333">Для ПО промежуточного слоя, которое используется всего одним приложением, допустимы ключи `string`.</span><span class="sxs-lookup"><span data-stu-id="0a915-333">For middleware that's only used by a single app, `string` keys are acceptable.</span></span> <span data-ttu-id="0a915-334">ПО промежуточного слоя, используемое несколькими экземплярами приложения, должно использовать уникальные ключи объекта во избежание конфликтов.</span><span class="sxs-lookup"><span data-stu-id="0a915-334">Middleware shared between app instances should use unique object keys to avoid key collisions.</span></span> <span data-ttu-id="0a915-335">В следующем примере показано, как использовать уникальный ключ объекта, определенный в классе ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="0a915-335">The following example shows how to use a unique object key defined in a middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

<span data-ttu-id="0a915-336">Другой код может обратиться к значению, хранящемуся в `HttpContext.Items`, с помощью ключа, предоставляемого классом ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="0a915-336">Other code can access the value stored in `HttpContext.Items` using the key exposed by the middleware class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="0a915-337">Данный подход также позволяет не использовать строки ключей в коде.</span><span class="sxs-lookup"><span data-stu-id="0a915-337">This approach also has the advantage of eliminating the use of key strings in the code.</span></span>

## <a name="cache"></a><span data-ttu-id="0a915-338">Кэш</span><span class="sxs-lookup"><span data-stu-id="0a915-338">Cache</span></span>

<span data-ttu-id="0a915-339">Кэширование — это эффективный способ хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="0a915-339">Caching is an efficient way to store and retrieve data.</span></span> <span data-ttu-id="0a915-340">Приложение может управлять временем существования элементов кэша.</span><span class="sxs-lookup"><span data-stu-id="0a915-340">The app can control the lifetime of cached items.</span></span>

<span data-ttu-id="0a915-341">Кэшированные данные не связаны с конкретным запросом, пользователем или сеансом.</span><span class="sxs-lookup"><span data-stu-id="0a915-341">Cached data isn't associated with a specific request, user, or session.</span></span> <span data-ttu-id="0a915-342">**Будьте внимательны и не кэшируйте данные пользователя, которые можно извлечь по запросу другого пользователя.**</span><span class="sxs-lookup"><span data-stu-id="0a915-342">**Be careful not to cache user-specific data that may be retrieved by other users' requests.**</span></span>

<span data-ttu-id="0a915-343">Для получения дополнительной информации см. <xref:performance/caching/response>.</span><span class="sxs-lookup"><span data-stu-id="0a915-343">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="0a915-344">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="0a915-344">Dependency Injection</span></span>

<span data-ttu-id="0a915-345">Используйте [внедрение зависимостей](xref:fundamentals/dependency-injection), чтобы сделать данные доступными для всех пользователей:</span><span class="sxs-lookup"><span data-stu-id="0a915-345">Use [Dependency Injection](xref:fundamentals/dependency-injection) to make data available to all users:</span></span>

1. <span data-ttu-id="0a915-346">Определите службу, содержащую данные.</span><span class="sxs-lookup"><span data-stu-id="0a915-346">Define a service containing the data.</span></span> <span data-ttu-id="0a915-347">Например, определяется класс с именем `MyAppData`:</span><span class="sxs-lookup"><span data-stu-id="0a915-347">For example, a class named `MyAppData` is defined:</span></span>

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. <span data-ttu-id="0a915-348">Добавьте класс службы в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0a915-348">Add the service class to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. <span data-ttu-id="0a915-349">Используйте класс службы данных:</span><span class="sxs-lookup"><span data-stu-id="0a915-349">Consume the data service class:</span></span>

    ::: moniker range=">= aspnetcore-2.0"

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a><span data-ttu-id="0a915-350">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="0a915-350">Common errors</span></span>

* <span data-ttu-id="0a915-351">"Unable to resolve service for type "Microsoft.Extensions.Caching.Distributed.IDistributedCache" while attempting to activate "Microsoft.AspNetCore.Session.DistributedSessionStore"" (Не удается разрешить службу для типа "Microsoft.Extensions.Caching.Distributed.IDistributedCache" при попытке активировать "Microsoft.AspNetCore.Session.DistributedSessionStore").</span><span class="sxs-lookup"><span data-stu-id="0a915-351">"Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'."</span></span>

  <span data-ttu-id="0a915-352">Обычно она вызвана невозможностью настройки по меньшей мере одной реализации `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="0a915-352">This is usually caused by failing to configure at least one `IDistributedCache` implementation.</span></span> <span data-ttu-id="0a915-353">Дополнительные сведения см. в разделах <xref:performance/caching/distributed> и <xref:performance/caching/memory>.</span><span class="sxs-lookup"><span data-stu-id="0a915-353">For more information, see <xref:performance/caching/distributed> and <xref:performance/caching/memory>.</span></span>

* <span data-ttu-id="0a915-354">Если ПО промежуточного слоя для сеанса не удается сохранить сеанс (например, резервное хранилище недоступно), оно регистрирует исключение и запрос выполняется обычным образом.</span><span class="sxs-lookup"><span data-stu-id="0a915-354">In the event that the session middleware fails to persist a session (for example, if the backing store isn't available), the middleware logs the exception and the request continues normally.</span></span> <span data-ttu-id="0a915-355">Это приводит к непредсказуемому поведению.</span><span class="sxs-lookup"><span data-stu-id="0a915-355">This leads to unpredictable behavior.</span></span>

  <span data-ttu-id="0a915-356">Например, пользователь сохраняет корзину для покупок в сеансе.</span><span class="sxs-lookup"><span data-stu-id="0a915-356">For example, a user stores a shopping cart in session.</span></span> <span data-ttu-id="0a915-357">Он добавляет товар в корзину, но фиксация завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="0a915-357">The user adds an item to the cart but the commit fails.</span></span> <span data-ttu-id="0a915-358">Приложению неизвестно о сбое, поэтому оно сообщает пользователю, что товар добавлен в корзину, хотя это не так.</span><span class="sxs-lookup"><span data-stu-id="0a915-358">The app doesn't know about the failure so it reports to the user that the item was added to their cart, which isn't true.</span></span>

  <span data-ttu-id="0a915-359">Для проверки на наличие таких ошибок рекомендуется вызывать `await feature.Session.CommitAsync();` из кода приложения по окончании записи в сеанс.</span><span class="sxs-lookup"><span data-stu-id="0a915-359">The recommended approach to check for errors is to call `await feature.Session.CommitAsync();` from app code when the app is done writing to the session.</span></span> <span data-ttu-id="0a915-360">`CommitAsync` создает исключение, если резервное хранилище недоступно.</span><span class="sxs-lookup"><span data-stu-id="0a915-360">`CommitAsync` throws an exception if the backing store is unavailable.</span></span> <span data-ttu-id="0a915-361">Если `CommitAsync` завершается ошибкой, приложение может обработать исключение.</span><span class="sxs-lookup"><span data-stu-id="0a915-361">If `CommitAsync` fails, the app can process the exception.</span></span> <span data-ttu-id="0a915-362">Исключение `LoadAsync` возникает в таких же условиях, когда хранилище данных недоступно.</span><span class="sxs-lookup"><span data-stu-id="0a915-362">`LoadAsync` throws under the same conditions where the data store is unavailable.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a915-363">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0a915-363">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
