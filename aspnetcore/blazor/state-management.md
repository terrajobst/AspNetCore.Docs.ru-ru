---
title: Управление состоянием ASP.NET Core Блазор
author: guardrex
description: Узнайте, как сохранить состояние в приложениях Блазор на стороне сервера.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/06/2019
uid: blazor/state-management
ms.openlocfilehash: b9dd2bb8f070a9a17e15e947f76de78cc517e22e
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68948414"
---
# <a name="aspnet-core-blazor-state-management"></a><span data-ttu-id="af3e1-103">Управление состоянием ASP.NET Core Блазор</span><span class="sxs-lookup"><span data-stu-id="af3e1-103">ASP.NET Core Blazor state management</span></span>

<span data-ttu-id="af3e1-104">Автор: [Стив Сандерсон](https://github.com/SteveSandersonMS) (Steve Sanderson)</span><span class="sxs-lookup"><span data-stu-id="af3e1-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="af3e1-105">Блазор на стороне сервера — это платформа приложений с отслеживанием состояния.</span><span class="sxs-lookup"><span data-stu-id="af3e1-105">Blazor server-side is a stateful app framework.</span></span> <span data-ttu-id="af3e1-106">В большинстве случаев приложение поддерживает текущее подключение к серверу.</span><span class="sxs-lookup"><span data-stu-id="af3e1-106">Most of the time, the app maintains an ongoing connection to the server.</span></span> <span data-ttu-id="af3e1-107">Состояние пользователя хранится в *канале*в памяти сервера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-107">The user's state is held in the server's memory in a *circuit*.</span></span> 

<span data-ttu-id="af3e1-108">Ниже приведены примеры состояния, удерживаемого для канала пользователя.</span><span class="sxs-lookup"><span data-stu-id="af3e1-108">Examples of state held for a user's circuit include:</span></span>

* <span data-ttu-id="af3e1-109">Отображаемый пользовательский&mdash;интерфейс. Иерархия экземпляров компонентов и их последние выходные данные рендеринга.</span><span class="sxs-lookup"><span data-stu-id="af3e1-109">The rendered UI&mdash;the hierarchy of component instances and their most recent render output.</span></span>
* <span data-ttu-id="af3e1-110">Значения всех полей и свойств в экземплярах компонента.</span><span class="sxs-lookup"><span data-stu-id="af3e1-110">The values of any fields and properties in component instances.</span></span>
* <span data-ttu-id="af3e1-111">Данные, хранящиеся в экземплярах службы [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection) , областью действия которых является цепь.</span><span class="sxs-lookup"><span data-stu-id="af3e1-111">Data held in [dependency injection (DI)](xref:fundamentals/dependency-injection) service instances that are scoped to the circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="af3e1-112">В этой статье рассматривается сохранение состояния в приложениях Блазор на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-112">This article addresses state persistence in Blazor server-side apps.</span></span> <span data-ttu-id="af3e1-113">Клиентские приложения блазор могут использовать возможности [сохранения состояния на стороне клиента в браузере,](#client-side-in-the-browser) но для этого требуются пользовательские решения или сторонние пакеты, не описанные в этой статье.</span><span class="sxs-lookup"><span data-stu-id="af3e1-113">Blazor client-side apps can take advantage of [client-side state persistence in the browser](#client-side-in-the-browser) but require custom solutions or 3rd party packages beyond the scope of this article.</span></span>

## <a name="blazor-circuits"></a><span data-ttu-id="af3e1-114">Цепи блазор</span><span class="sxs-lookup"><span data-stu-id="af3e1-114">Blazor circuits</span></span>

<span data-ttu-id="af3e1-115">Если пользователь испытывает временное снижение сетевого подключения, Блазор пытается повторно подключить пользователя к исходному каналу, чтобы он мог продолжать использовать приложение.</span><span class="sxs-lookup"><span data-stu-id="af3e1-115">If a user experiences a temporary network connection loss, Blazor attempts to reconnect the user to their original circuit so they can continue to use the app.</span></span> <span data-ttu-id="af3e1-116">Однако повторное подключение пользователя к исходному каналу в памяти сервера не всегда возможно:</span><span class="sxs-lookup"><span data-stu-id="af3e1-116">However, reconnecting a user to their original circuit in the server's memory isn't always possible:</span></span>

* <span data-ttu-id="af3e1-117">Сервер не может постоянно хранить отключенную цепь.</span><span class="sxs-lookup"><span data-stu-id="af3e1-117">The server can't retain a disconnected circuit forever.</span></span> <span data-ttu-id="af3e1-118">Сервер должен освободить отключенную цепь после истечения времени ожидания или при нехватке памяти для сервера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-118">The server must release a disconnected circuit after a timeout or when the server is under memory pressure.</span></span>
* <span data-ttu-id="af3e1-119">В многосерверных средах развертывания с балансировкой нагрузки любые запросы на обработку сервера могут стать недоступными в любой конкретный момент времени.</span><span class="sxs-lookup"><span data-stu-id="af3e1-119">In multiserver, load-balanced deployment environments, any server processing requests may become unavailable at any given time.</span></span> <span data-ttu-id="af3e1-120">Отдельные серверы могут завершаться сбоем или автоматически удаляться, если они больше не требуются для обработки общего объема запросов.</span><span class="sxs-lookup"><span data-stu-id="af3e1-120">Individual servers may fail or be automatically removed when no longer required to handle the overall volume of requests.</span></span> <span data-ttu-id="af3e1-121">Исходный сервер может быть недоступен, когда пользователь пытается повторно подключиться.</span><span class="sxs-lookup"><span data-stu-id="af3e1-121">The original server may not be available when the user attempts to reconnect.</span></span>
* <span data-ttu-id="af3e1-122">Пользователь может закрыть и снова открыть браузер или перезагрузить страницу, что приведет к удалению любого состояния, хранящегося в памяти браузера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-122">The user might close and re-open their browser or reload the page, which removes any state held in the browser's memory.</span></span> <span data-ttu-id="af3e1-123">Например, теряются значения, заданные через вызовы взаимодействия JavaScript.</span><span class="sxs-lookup"><span data-stu-id="af3e1-123">For example, values set through JavaScript interop calls are lost.</span></span>

<span data-ttu-id="af3e1-124">Если пользователь не может повторно подключиться к исходному каналу, он получает новый канал с пустым состоянием.</span><span class="sxs-lookup"><span data-stu-id="af3e1-124">When a user can't be reconnected to their original circuit, the user receives a new circuit with an empty state.</span></span> <span data-ttu-id="af3e1-125">Это эквивалентно закрытию и повторному открытию классического приложения.</span><span class="sxs-lookup"><span data-stu-id="af3e1-125">This is equivalent to closing and re-opening a desktop app.</span></span>

## <a name="preserve-state-across-circuits"></a><span data-ttu-id="af3e1-126">Сохранение состояния между цепями</span><span class="sxs-lookup"><span data-stu-id="af3e1-126">Preserve state across circuits</span></span>

<span data-ttu-id="af3e1-127">В некоторых сценариях желательно сохранять состояние между цепями.</span><span class="sxs-lookup"><span data-stu-id="af3e1-127">In some scenarios, preserving state across circuits is desirable.</span></span> <span data-ttu-id="af3e1-128">Приложение может хранить важные данные для пользователя, если:</span><span class="sxs-lookup"><span data-stu-id="af3e1-128">An app can retain important data for a user if:</span></span>

* <span data-ttu-id="af3e1-129">Веб-сервер становится недоступным.</span><span class="sxs-lookup"><span data-stu-id="af3e1-129">The web server becomes unavailable.</span></span>
* <span data-ttu-id="af3e1-130">Браузер пользователя вынужден начать новый канал с новым веб-сервером.</span><span class="sxs-lookup"><span data-stu-id="af3e1-130">The user's browser is forced to start a new circuit with a new web server.</span></span>

<span data-ttu-id="af3e1-131">Как правило, поддержание состояния между цепями применяется к сценариям, где пользователи активно могут создавать данные, а не просто считывать уже существующие данные.</span><span class="sxs-lookup"><span data-stu-id="af3e1-131">In general, maintaining state across circuits applies to scenarios where users are actively creating data, not simply reading data that already exists.</span></span>

<span data-ttu-id="af3e1-132">Чтобы сохранить состояние за пределами одного канала, *не храните данные в памяти сервера*.</span><span class="sxs-lookup"><span data-stu-id="af3e1-132">To preserve state beyond a single circuit, *don't merely store the data in the server's memory*.</span></span> <span data-ttu-id="af3e1-133">Приложение должно сохранять данные в другое место хранения.</span><span class="sxs-lookup"><span data-stu-id="af3e1-133">The app must persist the data to some other storage location.</span></span> <span data-ttu-id="af3e1-134">Сохранение состояния не является автоматически&mdash;. необходимо выполнить действия при разработке приложения для реализации сохраняемости данных с отслеживанием состояния.</span><span class="sxs-lookup"><span data-stu-id="af3e1-134">State persistence isn't automatic&mdash;you must take steps when developing the app to implement stateful data persistence.</span></span>

<span data-ttu-id="af3e1-135">Сохраняемость данных, как правило, требуется только для высокого значения состояния, затрачиваемого пользователями на создание.</span><span class="sxs-lookup"><span data-stu-id="af3e1-135">Data persistence is typically only required for high-value state that users have expended effort to create.</span></span> <span data-ttu-id="af3e1-136">В следующих примерах сохранение состояния экономит время или средства в коммерческих действиях:</span><span class="sxs-lookup"><span data-stu-id="af3e1-136">In the following examples, persisting state either saves time or aids in commercial activities:</span></span>

* <span data-ttu-id="af3e1-137">Многоэтапная веб &ndash; -форма. это время, требующее от пользователя повторного ввода данных для нескольких завершенных шагов многоэтапного процесса, если их состояние будет потеряно.</span><span class="sxs-lookup"><span data-stu-id="af3e1-137">Multistep webform &ndash; It's time-consuming for a user to re-enter data for several completed steps of a multistep process if their state is lost.</span></span> <span data-ttu-id="af3e1-138">Пользователь теряет состояние в этом сценарии, если он выходит из многоэтапной формы и снова возвращается в форму.</span><span class="sxs-lookup"><span data-stu-id="af3e1-138">A user loses state in this scenario if they navigate away from the multistep form and return to the form later.</span></span>
* <span data-ttu-id="af3e1-139">Корзина &ndash; для покупок. все коммерческие важные компоненты приложения, представляющие потенциальную прибыль, могут поддерживаться.</span><span class="sxs-lookup"><span data-stu-id="af3e1-139">Shopping cart &ndash; Any commercially important component of an app that represents potential revenue can be maintained.</span></span> <span data-ttu-id="af3e1-140">Пользователь, который теряет свое состояние, и, таким же покупательскую корзину, может приобрести меньше продуктов или услуг, когда они будут возвращены на сайт позже.</span><span class="sxs-lookup"><span data-stu-id="af3e1-140">A user who loses their state, and thus their shopping cart, may purchase fewer products or services when they return to the site later.</span></span>

<span data-ttu-id="af3e1-141">Обычно нет необходимости сохранять легко воссозданное состояние, например имя пользователя, введенное в диалоговое окно входа, которое еще не было отправлено.</span><span class="sxs-lookup"><span data-stu-id="af3e1-141">It's usually not necessary to preserve easily-recreated state, such as the username entered into a sign-in dialog that hasn't been submitted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="af3e1-142">Приложение может сохранять только *состояние приложения*.</span><span class="sxs-lookup"><span data-stu-id="af3e1-142">An app can only persist *app state*.</span></span> <span data-ttu-id="af3e1-143">Пользовательские интерфейсы не могут быть сохранены, например экземпляры компонентов и их деревья отрисовки.</span><span class="sxs-lookup"><span data-stu-id="af3e1-143">UIs can't be persisted, such as component instances and their render trees.</span></span> <span data-ttu-id="af3e1-144">Компоненты и деревья рендеринга обычно не являются сериализуемыми.</span><span class="sxs-lookup"><span data-stu-id="af3e1-144">Components and render trees aren't generally serializable.</span></span> <span data-ttu-id="af3e1-145">Чтобы сохранить нечто похожее на состояние пользовательского интерфейса, например развернутые узлы TreeView, приложение должно иметь пользовательский код для моделирования поведения как сериализуемого состояния приложения.</span><span class="sxs-lookup"><span data-stu-id="af3e1-145">To persist something similar to UI state, such as the expanded nodes of a TreeView, the app must have custom code to model the behavior as serializable app state.</span></span>

## <a name="where-to-persist-state"></a><span data-ttu-id="af3e1-146">Место сохранения состояния</span><span class="sxs-lookup"><span data-stu-id="af3e1-146">Where to persist state</span></span>

<span data-ttu-id="af3e1-147">Существует три общих расположения для сохранения состояния в приложении Блазор на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-147">Three common locations exist for persisting state in a Blazor server-side app.</span></span> <span data-ttu-id="af3e1-148">Каждый подход лучше всего подходит для различных сценариев и имеет разные предостережения:</span><span class="sxs-lookup"><span data-stu-id="af3e1-148">Each approach is best suited to different scenarios and has different caveats:</span></span>

* [<span data-ttu-id="af3e1-149">На стороне сервера в базе данных</span><span class="sxs-lookup"><span data-stu-id="af3e1-149">Server-side in a database</span></span>](#server-side-in-a-database)
* [<span data-ttu-id="af3e1-150">URL-адрес</span><span class="sxs-lookup"><span data-stu-id="af3e1-150">URL</span></span>](#url)
* [<span data-ttu-id="af3e1-151">На стороне клиента в браузере</span><span class="sxs-lookup"><span data-stu-id="af3e1-151">Client-side in the browser</span></span>](#client-side-in-the-browser)

### <a name="server-side-in-a-database"></a><span data-ttu-id="af3e1-152">На стороне сервера в базе данных</span><span class="sxs-lookup"><span data-stu-id="af3e1-152">Server-side in a database</span></span>

<span data-ttu-id="af3e1-153">Для постоянного сохранения данных или для любых данных, которые должны охватывать несколько пользователей или устройств, Независимая база данных на стороне сервера почти наверняка является лучшим выбором.</span><span class="sxs-lookup"><span data-stu-id="af3e1-153">For permanent data persistence or for any data that must span multiple users or devices, an independent server-side database is almost certainly the best choice.</span></span> <span data-ttu-id="af3e1-154">Возможны следующие значения.</span><span class="sxs-lookup"><span data-stu-id="af3e1-154">Options include:</span></span>

* <span data-ttu-id="af3e1-155">Реляционная база данных SQL</span><span class="sxs-lookup"><span data-stu-id="af3e1-155">Relational SQL database</span></span>
* <span data-ttu-id="af3e1-156">Хранилище "ключ — значение"</span><span class="sxs-lookup"><span data-stu-id="af3e1-156">Key-value store</span></span>
* <span data-ttu-id="af3e1-157">Хранилище больших двоичных объектов</span><span class="sxs-lookup"><span data-stu-id="af3e1-157">Blob store</span></span>
* <span data-ttu-id="af3e1-158">Хранилище таблиц</span><span class="sxs-lookup"><span data-stu-id="af3e1-158">Table store</span></span>

<span data-ttu-id="af3e1-159">После сохранения данных в базе данных пользователь может запустить новый канал в любое время.</span><span class="sxs-lookup"><span data-stu-id="af3e1-159">After data is saved in the database, a new circuit can be started by a user at any time.</span></span> <span data-ttu-id="af3e1-160">Данные пользователя сохранены и доступны в любом новом канале.</span><span class="sxs-lookup"><span data-stu-id="af3e1-160">The user's data is retained and available in any new circuit.</span></span>

<span data-ttu-id="af3e1-161">Дополнительные сведения о вариантах хранения данных Azure см. в [документации по службе хранилища Azure](/azure/storage/) и в [базах данных Azure](https://azure.microsoft.com/product-categories/databases/).</span><span class="sxs-lookup"><span data-stu-id="af3e1-161">For more information on Azure data storage options, see the [Azure Storage Documentation](/azure/storage/) and [Azure Databases](https://azure.microsoft.com/product-categories/databases/).</span></span>

### <a name="url"></a><span data-ttu-id="af3e1-162">URL-адрес</span><span class="sxs-lookup"><span data-stu-id="af3e1-162">URL</span></span>

<span data-ttu-id="af3e1-163">Для временных данных, представляющих состояние навигации, моделируют данные как часть URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="af3e1-163">For transient data representing navigation state, model the data as a part of the URL.</span></span> <span data-ttu-id="af3e1-164">Ниже приведены примеры состояний, которые моделируются в URL-адресе.</span><span class="sxs-lookup"><span data-stu-id="af3e1-164">Examples of state modeled in the URL include:</span></span>

* <span data-ttu-id="af3e1-165">ИДЕНТИФИКАТОР просматриваемой сущности.</span><span class="sxs-lookup"><span data-stu-id="af3e1-165">The ID of a viewed entity.</span></span>
* <span data-ttu-id="af3e1-166">Номер текущей страницы в страничной сетке.</span><span class="sxs-lookup"><span data-stu-id="af3e1-166">The current page number in a paged grid.</span></span>

<span data-ttu-id="af3e1-167">Содержимое адресной строки браузера будет храниться:</span><span class="sxs-lookup"><span data-stu-id="af3e1-167">The contents of the browser's address bar are retained:</span></span>

* <span data-ttu-id="af3e1-168">Если пользователь вручную перезагружает страницу.</span><span class="sxs-lookup"><span data-stu-id="af3e1-168">If the user manually reloads the page.</span></span>
* <span data-ttu-id="af3e1-169">Если веб-сервер становится недоступным&mdash;, пользователь вынужден перезагрузить страницу, чтобы подключиться к другому серверу.</span><span class="sxs-lookup"><span data-stu-id="af3e1-169">If the web server becomes unavailable&mdash;the user is forced to reload the page in order to connect to a different server.</span></span>

<span data-ttu-id="af3e1-170">Дополнительные сведения об определении шаблонов URL-адресов `@page` с помощью директивы см. в разделе. <xref:blazor/routing></span><span class="sxs-lookup"><span data-stu-id="af3e1-170">For information on defining URL patterns with the `@page` directive, see <xref:blazor/routing>.</span></span>

### <a name="client-side-in-the-browser"></a><span data-ttu-id="af3e1-171">На стороне клиента в браузере</span><span class="sxs-lookup"><span data-stu-id="af3e1-171">Client-side in the browser</span></span>

<span data-ttu-id="af3e1-172">Для временных данных, создаваемых пользователем, общим резервным хранилищем является браузер `localStorage` и `sessionStorage` коллекции.</span><span class="sxs-lookup"><span data-stu-id="af3e1-172">For transient data that the user is actively creating, a common backing store is the browser's `localStorage` and `sessionStorage` collections.</span></span> <span data-ttu-id="af3e1-173">Приложению не требуется управлять сохраненным состоянием или очищать его, если канал прерван, что является преимуществом для хранилища на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-173">The app isn't required to manage or clear the stored state if the circuit is abandoned, which is an advantage over server-side storage.</span></span>

> [!NOTE]
> <span data-ttu-id="af3e1-174">"На стороне клиента" в этом разделе относится к сценариям на стороне клиента в браузере, а не к [модели размещения на стороне клиента блазор](xref:blazor/hosting-models#client-side).</span><span class="sxs-lookup"><span data-stu-id="af3e1-174">"Client-side" in this section refers to client-side scenarios in the browser, not the [Blazor client-side hosting model](xref:blazor/hosting-models#client-side).</span></span> <span data-ttu-id="af3e1-175">`localStorage`и `sessionStorage` могут использоваться в клиентских приложениях блазор, но только путем написания пользовательского кода или использования стороннего пакета.</span><span class="sxs-lookup"><span data-stu-id="af3e1-175">`localStorage` and `sessionStorage` can be used in Blazor client-side apps but only by writing custom code or using a 3rd party package.</span></span>

<span data-ttu-id="af3e1-176">`localStorage`и `sessionStorage` различаются следующим образом.</span><span class="sxs-lookup"><span data-stu-id="af3e1-176">`localStorage` and `sessionStorage` differ as follows:</span></span>

* <span data-ttu-id="af3e1-177">`localStorage`входит в область браузера пользователя.</span><span class="sxs-lookup"><span data-stu-id="af3e1-177">`localStorage` is scoped to the user's browser.</span></span> <span data-ttu-id="af3e1-178">Если пользователь перезагружает страницу или закрывает и снова открывает браузер, состояние сохраняется.</span><span class="sxs-lookup"><span data-stu-id="af3e1-178">If the user reloads the page or closes and re-opens the browser, the state persists.</span></span> <span data-ttu-id="af3e1-179">Если пользователь открывает несколько вкладок браузера, это состояние совместно используется на вкладках.</span><span class="sxs-lookup"><span data-stu-id="af3e1-179">If the user opens multiple browser tabs, the state is shared across the tabs.</span></span> <span data-ttu-id="af3e1-180">Данные сохраняются `localStorage` до тех пор, пока не будут явно сняты.</span><span class="sxs-lookup"><span data-stu-id="af3e1-180">Data persists in `localStorage` until explicitly cleared.</span></span>
* <span data-ttu-id="af3e1-181">`sessionStorage`входит в область вкладки браузера пользователя. Если пользователь перезагружает вкладку, состояние сохраняется.</span><span class="sxs-lookup"><span data-stu-id="af3e1-181">`sessionStorage` is scoped to the user's browser tab. If the user reloads the tab, the state persists.</span></span> <span data-ttu-id="af3e1-182">Если пользователь закрывает вкладку или браузер, состояние теряется.</span><span class="sxs-lookup"><span data-stu-id="af3e1-182">If the user closes the tab or the browser, the state is lost.</span></span> <span data-ttu-id="af3e1-183">Если пользователь открывает несколько вкладок браузера, каждая вкладка имеет собственную независимую версию данных.</span><span class="sxs-lookup"><span data-stu-id="af3e1-183">If the user opens multiple browser tabs, each tab has its own independent version of the data.</span></span>

<span data-ttu-id="af3e1-184">Как правило `sessionStorage` , безопаснее использовать.</span><span class="sxs-lookup"><span data-stu-id="af3e1-184">Generally, `sessionStorage` is safer to use.</span></span> <span data-ttu-id="af3e1-185">`sessionStorage`позволяет избежать риска, когда пользователь открывает несколько вкладок и находит следующее:</span><span class="sxs-lookup"><span data-stu-id="af3e1-185">`sessionStorage` avoids the risk that a user opens multiple tabs and encounters the following:</span></span>

* <span data-ttu-id="af3e1-186">Ошибки в хранилище состояний на вкладках.</span><span class="sxs-lookup"><span data-stu-id="af3e1-186">Bugs in state storage across tabs.</span></span>
* <span data-ttu-id="af3e1-187">Путаница в работе, когда на вкладке заменяется состояние других вкладок.</span><span class="sxs-lookup"><span data-stu-id="af3e1-187">Confusing behavior when a tab overwrites the state of other tabs.</span></span>

<span data-ttu-id="af3e1-188">`localStorage`является лучшим выбором, если приложение должно сохранять состояние в случае закрытия и повторного открытия браузера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-188">`localStorage` is the better choice if the app must persist state across closing and re-opening the browser.</span></span>

<span data-ttu-id="af3e1-189">Предостережения при использовании хранилища браузера:</span><span class="sxs-lookup"><span data-stu-id="af3e1-189">Caveats for using browser storage:</span></span>

* <span data-ttu-id="af3e1-190">Аналогично использованию базы данных на стороне сервера, Загрузка и сохранение данных выполняются асинхронно.</span><span class="sxs-lookup"><span data-stu-id="af3e1-190">Similar to the use of a server-side database, loading and saving data are asynchronous.</span></span>
* <span data-ttu-id="af3e1-191">В отличие от базы данных на стороне сервера, хранилище недоступно во время предварительной отрисовки, так как запрошенная страница не существует в браузере во время подготовки к просмотру.</span><span class="sxs-lookup"><span data-stu-id="af3e1-191">Unlike a server-side database, storage isn't available during prerendering because the requested page doesn't exist in the browser during the prerendering stage.</span></span>
* <span data-ttu-id="af3e1-192">Хранение данных в нескольких килобайтах имеет смысл сохранять для приложений Блазор на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-192">Storage of a few kilobytes of data is reasonable to persist for Blazor server-side apps.</span></span> <span data-ttu-id="af3e1-193">Помимо нескольких килобайт, необходимо учитывать последствия производительности, поскольку данные загружаются и сохраняются по сети.</span><span class="sxs-lookup"><span data-stu-id="af3e1-193">Beyond a few kilobytes, you must consider the performance implications because the data is loaded and saved across the network.</span></span>
* <span data-ttu-id="af3e1-194">Пользователи могут просматривать данные или изменять их.</span><span class="sxs-lookup"><span data-stu-id="af3e1-194">Users may view or tamper with the data.</span></span> <span data-ttu-id="af3e1-195">ASP.NET Core [Защита данных](xref:security/data-protection/introduction) может снизить риск.</span><span class="sxs-lookup"><span data-stu-id="af3e1-195">ASP.NET Core [Data Protection](xref:security/data-protection/introduction) can mitigate the risk.</span></span>

## <a name="third-party-browser-storage-solutions"></a><span data-ttu-id="af3e1-196">Сторонние решения для хранения в браузере</span><span class="sxs-lookup"><span data-stu-id="af3e1-196">Third-party browser storage solutions</span></span>

<span data-ttu-id="af3e1-197">Сторонние пакеты NuGet предоставляют интерфейсы API для работы с `localStorage` и. `sessionStorage`</span><span class="sxs-lookup"><span data-stu-id="af3e1-197">Third-party NuGet packages provide APIs for working with `localStorage` and `sessionStorage`.</span></span>

<span data-ttu-id="af3e1-198">Стоит рассмотреть Выбор пакета, который прозрачно использует [защиту данных](xref:security/data-protection/introduction)ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af3e1-198">It's worth considering choosing a package that transparently uses ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="af3e1-199">ASP.NET Core Data Protection шифрует хранимые данные и уменьшает потенциальный риск незаконного изменения хранимых данных.</span><span class="sxs-lookup"><span data-stu-id="af3e1-199">ASP.NET Core Data Protection encrypts stored data and reduces the potential risk of tampering with stored data.</span></span> <span data-ttu-id="af3e1-200">Если сериализованные данные JSON хранятся в виде обычного текста, пользователи могут просматривать данные с помощью средств разработчика браузера, а также изменять сохраненные данные.</span><span class="sxs-lookup"><span data-stu-id="af3e1-200">If JSON-serialized data is stored in plaintext, users can see the data using browser developer tools and also modify the stored data.</span></span> <span data-ttu-id="af3e1-201">Защита данных не всегда является проблемой, так как данные могут быть простыми по своей природе.</span><span class="sxs-lookup"><span data-stu-id="af3e1-201">Securing data isn't always a problem because the data might be trivial in nature.</span></span> <span data-ttu-id="af3e1-202">Например, чтение или изменение сохраненного цвета элемента пользовательского интерфейса не является серьезной угрозой безопасности для пользователя или организации.</span><span class="sxs-lookup"><span data-stu-id="af3e1-202">For example, reading or modifying the stored color of a UI element isn't a significant security risk to the user or the organization.</span></span> <span data-ttu-id="af3e1-203">Не разрешайте пользователям проверять или изменять *конфиденциальные данные*.</span><span class="sxs-lookup"><span data-stu-id="af3e1-203">Avoid allowing users to inspect or tamper with *sensitive data*.</span></span>

## <a name="protected-browser-storage-experimental-package"></a><span data-ttu-id="af3e1-204">Экспериментальный пакет хранилища защищенного браузера</span><span class="sxs-lookup"><span data-stu-id="af3e1-204">Protected Browser Storage experimental package</span></span>

<span data-ttu-id="af3e1-205">Примером пакета NuGet, обеспечивающего [защиту данных](xref:security/data-protection/introduction) для `localStorage` , `sessionStorage` является [Microsoft. AspNetCore. протектедбровсерстораже](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span><span class="sxs-lookup"><span data-stu-id="af3e1-205">An example of a NuGet package that provides [Data Protection](xref:security/data-protection/introduction) for `localStorage` and `sessionStorage` is [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>

> [!WARNING]
> <span data-ttu-id="af3e1-206">`Microsoft.AspNetCore.ProtectedBrowserStorage`является неподдерживаемым экспериментальным пакетом, который в настоящее время не подходит для использования в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="af3e1-206">`Microsoft.AspNetCore.ProtectedBrowserStorage` is an unsupported experimental package unsuitable for production use at this time.</span></span>

### <a name="installation"></a><span data-ttu-id="af3e1-207">Установка</span><span class="sxs-lookup"><span data-stu-id="af3e1-207">Installation</span></span>

<span data-ttu-id="af3e1-208">Чтобы установить пакет `Microsoft.AspNetCore.ProtectedBrowserStorage` , выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="af3e1-208">To install the `Microsoft.AspNetCore.ProtectedBrowserStorage` package:</span></span>

1. <span data-ttu-id="af3e1-209">В проекте приложения на стороне сервера Блазор добавьте ссылку на пакет в [Microsoft. AspNetCore. протектедбровсерстораже](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span><span class="sxs-lookup"><span data-stu-id="af3e1-209">In the Blazor server-side app project, add a package reference to [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>
1. <span data-ttu-id="af3e1-210">В HTML верхнего уровня (например, в файле *pages/_Host. cshtml* в шаблоне проекта по умолчанию) добавьте следующий `<script>` тег:</span><span class="sxs-lookup"><span data-stu-id="af3e1-210">In the top-level HTML (for example, in the *Pages/_Host.cshtml* file in the default project template), add the following `<script>` tag:</span></span>

   ```html
   <script src="_content/Microsoft.AspNetCore.ProtectedBrowserStorage/protectedBrowserStorage.js"></script>
   ```

1. <span data-ttu-id="af3e1-211">В методе вызовите `AddProtectedBrowserStorage` , чтобы `localStorage` добавить `sessionStorage` и службы в коллекцию служб: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="af3e1-211">In the `Startup.ConfigureServices` method, call `AddProtectedBrowserStorage` to add `localStorage` and `sessionStorage` services to the service collection:</span></span>

   ```csharp
   services.AddProtectedBrowserStorage();
   ```

### <a name="save-and-load-data-within-a-component"></a><span data-ttu-id="af3e1-212">Сохранение и загрузка данных в компоненте</span><span class="sxs-lookup"><span data-stu-id="af3e1-212">Save and load data within a component</span></span>

<span data-ttu-id="af3e1-213">В любом компоненте, требующем загрузки или сохранении данных в хранилище браузера [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) , используйте для вставки экземпляра одного из следующих компонентов:</span><span class="sxs-lookup"><span data-stu-id="af3e1-213">In any component that requires loading or saving data to browser storage, use [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) to inject an instance of either of the following:</span></span>

* `ProtectedLocalStorage`
* `ProtectedSessionStorage`

<span data-ttu-id="af3e1-214">Выбор зависит от того, какое резервное хранилище вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="af3e1-214">The choice depends on which backing store you wish to use.</span></span> <span data-ttu-id="af3e1-215">В следующем примере `sessionStorage` используется:</span><span class="sxs-lookup"><span data-stu-id="af3e1-215">In the following example, `sessionStorage` is used:</span></span>

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
```

<span data-ttu-id="af3e1-216">Инструкцию можно поместить в файл *_Imports. Razor* , а не в компонент. `@using`</span><span class="sxs-lookup"><span data-stu-id="af3e1-216">The `@using` statement can be placed into an *_Imports.razor* file instead of in the component.</span></span> <span data-ttu-id="af3e1-217">Использование файла *_Imports. Razor* делает пространство имен доступным для больших сегментов приложения или всего приложения.</span><span class="sxs-lookup"><span data-stu-id="af3e1-217">Use of the *_Imports.razor* file makes the namespace available to larger segments of the app or the whole app.</span></span>

<span data-ttu-id="af3e1-218">Чтобы сохранить `currentCount` значение `Counter` в компоненте шаблона проекта, измените метод так, чтобы `IncrementCount` он использовал: `ProtectedSessionStore.SetAsync`</span><span class="sxs-lookup"><span data-stu-id="af3e1-218">To persist the `currentCount` value in the `Counter` component of the project template, modify the `IncrementCount` method to use `ProtectedSessionStore.SetAsync`:</span></span>

```csharp
private async Task IncrementCount()
{
    currentCount++;
    await ProtectedSessionStore.SetAsync("count", currentCount);
}
```

<span data-ttu-id="af3e1-219">В больших и более реалистичных приложениях хранение отдельных полей является маловероятной ситуацией.</span><span class="sxs-lookup"><span data-stu-id="af3e1-219">In larger, more realistic apps, storage of individual fields is an unlikely scenario.</span></span> <span data-ttu-id="af3e1-220">Приложения, скорее всего, будут хранить все объекты модели, включающие сложное состояние.</span><span class="sxs-lookup"><span data-stu-id="af3e1-220">Apps are more likely to store entire model objects that include complex state.</span></span> <span data-ttu-id="af3e1-221">`ProtectedSessionStore`автоматически сериализует и десериализует данные JSON.</span><span class="sxs-lookup"><span data-stu-id="af3e1-221">`ProtectedSessionStore` automatically serializes and deserializes JSON data.</span></span>

<span data-ttu-id="af3e1-222">В предыдущем примере `currentCount` кода данные хранятся как `sessionStorage['count']` в браузере пользователя.</span><span class="sxs-lookup"><span data-stu-id="af3e1-222">In the preceding code example, the `currentCount` data is stored as `sessionStorage['count']` in the user's browser.</span></span> <span data-ttu-id="af3e1-223">Данные не хранятся в виде обычного текста, а защищены с помощью [защиты данных](xref:security/data-protection/introduction)ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af3e1-223">The data isn't stored in plaintext but rather is protected using ASP.NET Core's [Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="af3e1-224">Зашифрованные данные могут отображаться, если `sessionStorage['count']` вычисляются в консоли разработчика браузера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-224">The encrypted data can be seen if `sessionStorage['count']` is evaluated in the browser's developer console.</span></span>

<span data-ttu-id="af3e1-225">Чтобы восстановить `currentCount` данные, если пользователь снова возвращается `Counter` в компонент (в том числе если они находятся в совершенно новом канале), используйте `ProtectedSessionStore.GetAsync`:</span><span class="sxs-lookup"><span data-stu-id="af3e1-225">To recover the `currentCount` data if the user returns to the `Counter` component later (including if they're on an entirely new circuit), use `ProtectedSessionStore.GetAsync`:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    currentCount = await ProtectedSessionStore.GetAsync<int>("count");
}
```

<span data-ttu-id="af3e1-226">Если параметры компонента включают состояние навигации, вызовите `ProtectedSessionStore.GetAsync` и назначьте результат в, а не `OnInitializedAsync`в `OnParametersSetAsync`.</span><span class="sxs-lookup"><span data-stu-id="af3e1-226">If the component's parameters include navigation state, call `ProtectedSessionStore.GetAsync` and assign the result in `OnParametersSetAsync`, not `OnInitializedAsync`.</span></span> <span data-ttu-id="af3e1-227">`OnInitializedAsync`вызывается только один раз при первом создании экземпляра компонента.</span><span class="sxs-lookup"><span data-stu-id="af3e1-227">`OnInitializedAsync` is only called one time when the component is first instantiated.</span></span> <span data-ttu-id="af3e1-228">`OnInitializedAsync`не вызывается позже, если пользователь переходит на другой URL-адрес, оставшийся на той же странице.</span><span class="sxs-lookup"><span data-stu-id="af3e1-228">`OnInitializedAsync` isn't called again later if the user navigates to a different URL while remaining on the same page.</span></span>

> [!WARNING]
> <span data-ttu-id="af3e1-229">Примеры в этом разделе работают только в том случае, если на сервере не включено предварительное отображение.</span><span class="sxs-lookup"><span data-stu-id="af3e1-229">The examples in this section only work if the server doesn't have prerendering enabled.</span></span> <span data-ttu-id="af3e1-230">При включенной предварительной отрисовке возникает ошибка следующего вида:</span><span class="sxs-lookup"><span data-stu-id="af3e1-230">With prerendering enabled, an error is generated similar to:</span></span>
>
> > <span data-ttu-id="af3e1-231">В настоящее время вызовы взаимодействия JavaScript не могут быть выданы.</span><span class="sxs-lookup"><span data-stu-id="af3e1-231">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="af3e1-232">Это связано с тем, что компонент предварительно готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="af3e1-232">This is because the component is being prerendered.</span></span>
>
> <span data-ttu-id="af3e1-233">Отключите предварительную отрисовку или добавьте дополнительный код для работы с предварительной отрисовкой.</span><span class="sxs-lookup"><span data-stu-id="af3e1-233">Either disable prerendering or add additional code to work with prerendering.</span></span> <span data-ttu-id="af3e1-234">Дополнительные сведения о написании кода, который работает с предварительной отрисовкой, см. в разделе Обработка предварительной [визуализации](#handle-prerendering) .</span><span class="sxs-lookup"><span data-stu-id="af3e1-234">To learn more about writing code that works with prerendering, see the [Handle prerendering](#handle-prerendering) section.</span></span>

### <a name="handle-the-loading-state"></a><span data-ttu-id="af3e1-235">Обработано состояние загрузки</span><span class="sxs-lookup"><span data-stu-id="af3e1-235">Handle the loading state</span></span>

<span data-ttu-id="af3e1-236">Так как хранилище браузера является асинхронным (доступное через сетевое подключение), всегда существует период времени, по истечении которого данные загружаются и доступны для использования компонентом.</span><span class="sxs-lookup"><span data-stu-id="af3e1-236">Since browser storage is asynchronous (accessed over a network connection), there's always a period of time before the data is loaded and available for use by a component.</span></span> <span data-ttu-id="af3e1-237">Для достижения лучших результатов перед отображением пустых или данных по умолчанию выводится сообщение о состоянии загрузки.</span><span class="sxs-lookup"><span data-stu-id="af3e1-237">For the best results, render a loading-state message while loading is in progress instead of displaying blank or default data.</span></span>

<span data-ttu-id="af3e1-238">Один из подходов состоит в том, чтобы `null` определить, являются ли данные (по-прежнему загружаются).</span><span class="sxs-lookup"><span data-stu-id="af3e1-238">One approach is to track whether the data is `null` (still loading) or not.</span></span> <span data-ttu-id="af3e1-239">В компоненте `Counter` по умолчанию счетчик сохраняется `int`в.</span><span class="sxs-lookup"><span data-stu-id="af3e1-239">In the default `Counter` component, the count is held in an `int`.</span></span> <span data-ttu-id="af3e1-240">Добавим`?``int`значение null, добавив вопросительный знак () в тип (): `currentCount`</span><span class="sxs-lookup"><span data-stu-id="af3e1-240">Make `currentCount` nullable by adding a question mark (`?`) to the type (`int`):</span></span>

```csharp
private int? currentCount;
```

<span data-ttu-id="af3e1-241">Вместо безусловного отображения кнопки счетчика и **увеличения** выберите отображение этих элементов только в том случае, если данные загружены:</span><span class="sxs-lookup"><span data-stu-id="af3e1-241">Instead of unconditionally displaying the count and **Increment** button, choose to display these elements only if the data is loaded:</span></span>

```cshtml
@if (currentCount.HasValue)
{
    <p>Current count: <strong>@currentCount</strong></p>

    <button @onclick="IncrementCount">Increment</button>
}
else
{
    <p>Loading...</p>
}
```

### <a name="handle-prerendering"></a><span data-ttu-id="af3e1-242">Обработка предварительной визуализации</span><span class="sxs-lookup"><span data-stu-id="af3e1-242">Handle prerendering</span></span>

<span data-ttu-id="af3e1-243">Во время предварительной подготовки:</span><span class="sxs-lookup"><span data-stu-id="af3e1-243">During prerendering:</span></span>

* <span data-ttu-id="af3e1-244">Интерактивное подключение к браузеру пользователя не существует.</span><span class="sxs-lookup"><span data-stu-id="af3e1-244">An interactive connection to the user's browser doesn't exist.</span></span>
* <span data-ttu-id="af3e1-245">В браузере еще нет страницы, на которой можно запустить код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="af3e1-245">The browser doesn't yet have a page in which it can run JavaScript code.</span></span>

<span data-ttu-id="af3e1-246">`localStorage`или `sessionStorage` недоступно во время предварительной подготовки.</span><span class="sxs-lookup"><span data-stu-id="af3e1-246">`localStorage` or `sessionStorage` aren't available during prerendering.</span></span> <span data-ttu-id="af3e1-247">Если компонент пытается взаимодействовать с хранилищем, возникает ошибка следующего вида:</span><span class="sxs-lookup"><span data-stu-id="af3e1-247">If the component attempts to interact with storage, an error is generated similar to:</span></span>

> <span data-ttu-id="af3e1-248">В настоящее время вызовы взаимодействия JavaScript не могут быть выданы.</span><span class="sxs-lookup"><span data-stu-id="af3e1-248">JavaScript interop calls cannot be issued at this time.</span></span> <span data-ttu-id="af3e1-249">Это связано с тем, что компонент предварительно готовится к просмотру.</span><span class="sxs-lookup"><span data-stu-id="af3e1-249">This is because the component is being prerendered.</span></span>

<span data-ttu-id="af3e1-250">Одним из способов устранения этой ошибки является отключение предварительной визуализации.</span><span class="sxs-lookup"><span data-stu-id="af3e1-250">One way to resolve the error is to disable prerendering.</span></span> <span data-ttu-id="af3e1-251">Обычно это наилучший вариант, если приложение активно использует хранилище на основе браузера.</span><span class="sxs-lookup"><span data-stu-id="af3e1-251">This is usually the best choice if the app makes heavy use of browser-based storage.</span></span> <span data-ttu-id="af3e1-252">Предварительная обработка увеличивает сложность и не дает приложению преимущества, так как приложение не может выдать какое `localStorage` - `sessionStorage` либо полезное содержимое, пока или не станет доступным.</span><span class="sxs-lookup"><span data-stu-id="af3e1-252">Prerendering adds complexity and doesn't benefit the app because the app can't prerender any useful content until `localStorage` or `sessionStorage` are available.</span></span>

<span data-ttu-id="af3e1-253">Чтобы отключить предварительную отрисовку, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="af3e1-253">To disable prerendering:</span></span>

1. <span data-ttu-id="af3e1-254">Откройте файл *pages/_Host. cshtml* и удалите вызов `Html.RenderComponentAsync`.</span><span class="sxs-lookup"><span data-stu-id="af3e1-254">Open the *Pages/_Host.cshtml* file and remove the call to `Html.RenderComponentAsync`.</span></span>
1. <span data-ttu-id="af3e1-255">Откройте файл и замените вызов `endpoints.MapBlazorHub<App>("app")`на `endpoints.MapBlazorHub()`. `Startup.cs`</span><span class="sxs-lookup"><span data-stu-id="af3e1-255">Open the `Startup.cs` file and replace the call to `endpoints.MapBlazorHub()` with `endpoints.MapBlazorHub<App>("app")`.</span></span> <span data-ttu-id="af3e1-256">`App`тип корневого компонента.</span><span class="sxs-lookup"><span data-stu-id="af3e1-256">`App` is the type of the root component.</span></span> <span data-ttu-id="af3e1-257">`"app"`— Это селектор CSS, указывающий расположение для корневого компонента.</span><span class="sxs-lookup"><span data-stu-id="af3e1-257">`"app"` is a CSS selector specifying the location for the root component.</span></span>

<span data-ttu-id="af3e1-258">Предварительная визуализация может быть полезной для других страниц, которые `localStorage` не `sessionStorage`используют или.</span><span class="sxs-lookup"><span data-stu-id="af3e1-258">Prerendering might be useful for other pages that don't use `localStorage` or `sessionStorage`.</span></span> <span data-ttu-id="af3e1-259">Чтобы включить предварительную отрисовку, отложите операцию загрузки до тех пор, пока браузер не подключается к каналу.</span><span class="sxs-lookup"><span data-stu-id="af3e1-259">To keep prerendering enabled, defer the loading operation until the browser is connected to the circuit.</span></span> <span data-ttu-id="af3e1-260">Ниже приведен пример хранения значения счетчика.</span><span class="sxs-lookup"><span data-stu-id="af3e1-260">The following is an example for storing a counter value:</span></span>

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedLocalStorage ProtectedLocalStore
@inject IComponentContext ComponentContext

... rendering code goes here ...

@code {
    private int? currentCount;
    private bool isWaitingForConnection;

    protected override async Task OnInitAsync()
    {
        if (ComponentContext.IsConnected)
        {
            // It looks like the app isn't prerendering, so the data can be
            // immediately loaded from browser storage.
            await LoadStateAsync();
        }
        else
        {
            // Prerendering is in progress, so the app defers the load operation
            // until later.
            isWaitingForConnection = true;
        }
    }

    protected override async Task OnAfterRenderAsync()
    {
        // By this stage, the client has connected back to the server, and
        // browser services are available. If the app didn't load the data earlier,
        // the app should do so now and then trigger a new render.
        if (isWaitingForConnection)
        {
            isWaitingForConnection = false;
            await LoadStateAsync();
            StateHasChanged();
        }
    }

    private async Task LoadStateAsync()
    {
        currentCount = await ProtectedLocalStore.GetAsync<int>("prerenderedCount");
    }

    private async Task IncrementCount()
    {
        currentCount++;
        await ProtectedSessionStore.SetAsync("count", currentCount);
    }
}
```

### <a name="factor-out-the-state-preservation-to-a-common-location"></a><span data-ttu-id="af3e1-261">Разнесите сохранение состояния в общее расположение</span><span class="sxs-lookup"><span data-stu-id="af3e1-261">Factor out the state preservation to a common location</span></span>

<span data-ttu-id="af3e1-262">Если многие компоненты используют хранилище на основе браузера, повторная реализация кода поставщика состояний много раз создает дублирование кода.</span><span class="sxs-lookup"><span data-stu-id="af3e1-262">If many components rely on browser-based storage, re-implementing state provider code many times creates code duplication.</span></span> <span data-ttu-id="af3e1-263">Одним из вариантов предотвращения дублирования кода является создание *родительского компонента поставщика состояний* , который инкапсулирует логику поставщика состояний.</span><span class="sxs-lookup"><span data-stu-id="af3e1-263">One option for avoiding code duplication is to create a *state provider parent component* that encapsulates the state provider logic.</span></span> <span data-ttu-id="af3e1-264">Дочерние компоненты могут работать с сохраненными данными без учета механизма сохранения состояния.</span><span class="sxs-lookup"><span data-stu-id="af3e1-264">Child components can work with persisted data without regard to the state persistence mechanism.</span></span>

<span data-ttu-id="af3e1-265">В следующем примере `CounterStateProvider` компонента данные счетчика сохраняются:</span><span class="sxs-lookup"><span data-stu-id="af3e1-265">In the following example of a `CounterStateProvider` component, counter data is persisted:</span></span>

```cshtml
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore

@if (hasLoaded)
{
    <CascadingValue Value="@this">
        @ChildContent
    </CascadingValue>
}
else
{
    <p>Loading...</p>
}

@code {
    private bool hasLoaded;

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public int CurrentCount { get; set; }

    protected override async Task OnInitAsync()
    {
        CurrentCount = await ProtectedSessionStore.GetAsync<int>("count");
        hasLoaded = true;
    }

    public async Task SaveChangesAsync()
    {
        await ProtectedSessionStore.SetAsync("count", CurrentCount);
    }
}
```

<span data-ttu-id="af3e1-266">`CounterStateProvider` Компонент обрабатывает этап загрузки, не выполняя отрисовку его дочернего содержимого до завершения загрузки.</span><span class="sxs-lookup"><span data-stu-id="af3e1-266">The `CounterStateProvider` component handles the loading phase by not rendering its child content until loading is complete.</span></span>

<span data-ttu-id="af3e1-267">Чтобы использовать `CounterStateProvider` компонент, заключите экземпляр компонента в оболочку для любого другого компонента, который требует доступа к состоянию счетчика.</span><span class="sxs-lookup"><span data-stu-id="af3e1-267">To use the `CounterStateProvider` component, wrap an instance of the component around any other component that requires access to the counter state.</span></span> <span data-ttu-id="af3e1-268">Чтобы сделать состояние доступным для всех компонентов `CounterStateProvider` в приложении, заключите компонент `Router` в `App` компонент (*app. Razor*):</span><span class="sxs-lookup"><span data-stu-id="af3e1-268">To make the state accessible to all components in an app, wrap the `CounterStateProvider` component around the `Router` in the `App` component (*App.razor*):</span></span>

```cshtml
<CounterStateProvider>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CounterStateProvider>
```

<span data-ttu-id="af3e1-269">Упакованные компоненты получают и могут изменять состояние сохраненного счетчика.</span><span class="sxs-lookup"><span data-stu-id="af3e1-269">Wrapped components receive and can modify the persisted counter state.</span></span> <span data-ttu-id="af3e1-270">Следующий `Counter` компонент реализует шаблон:</span><span class="sxs-lookup"><span data-stu-id="af3e1-270">The following `Counter` component implements the pattern:</span></span>

```cshtml
@page "/counter"

<p>Current count: <strong>@CounterStateProvider.CurrentCount</strong></p>

<button @onclick="IncrementCount">Increment</button>

@code {
    [CascadingParameter]
    private CounterStateProvider CounterStateProvider { get; set; }

    private async Task IncrementCount()
    {
        CounterStateProvider.CurrentCount++;
        await CounterStateProvider.SaveChangesAsync();
    }
}
```

<span data-ttu-id="af3e1-271">Предыдущий компонент не обязательно должен взаимодействовать с `ProtectedBrowserStorage`, а также с этапом загрузки.</span><span class="sxs-lookup"><span data-stu-id="af3e1-271">The preceding component isn't required to interact with `ProtectedBrowserStorage`, nor does it deal with a "loading" phase.</span></span>

<span data-ttu-id="af3e1-272">Чтобы обработать предварительную отрисовку, как описано выше `CounterStateProvider` , можно внести изменения, чтобы все компоненты, использующие данные счетчика, автоматически работали с предварительной отрисовкой.</span><span class="sxs-lookup"><span data-stu-id="af3e1-272">To deal with prerendering as described earlier, `CounterStateProvider` can be amended so that all of the components that consume the counter data automatically work with prerendering.</span></span> <span data-ttu-id="af3e1-273">Дополнительные сведения см. в разделе Обработка предварительной [подготовки](#handle-prerendering) к просмотру.</span><span class="sxs-lookup"><span data-stu-id="af3e1-273">See the [Handle prerendering](#handle-prerendering) section for details.</span></span>

<span data-ttu-id="af3e1-274">В общем случае рекомендуется использовать родительский шаблон *компонента поставщика состояний* :</span><span class="sxs-lookup"><span data-stu-id="af3e1-274">In general, *state provider parent component* pattern is recommended:</span></span>

* <span data-ttu-id="af3e1-275">Для использования состояния во многих других компонентах.</span><span class="sxs-lookup"><span data-stu-id="af3e1-275">To consume state in many other components.</span></span>
* <span data-ttu-id="af3e1-276">Если имеется только один объект состояния верхнего уровня для сохранения.</span><span class="sxs-lookup"><span data-stu-id="af3e1-276">If there's just one top-level state object to persist.</span></span>

<span data-ttu-id="af3e1-277">Чтобы сохранить множество различных объектов состояния и использовать разные подмножества объектов в разных местах, лучше избегать обработки загрузки и сохранения состояния глобально.</span><span class="sxs-lookup"><span data-stu-id="af3e1-277">To persist many different state objects and consume different subsets of objects in different places, it's better to avoid handling the loading and saving of state globally.</span></span>
