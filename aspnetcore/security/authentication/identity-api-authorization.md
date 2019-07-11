---
title: Общие сведения об аутентификации для одностраничных приложений на ASP.NET Core
author: javiercn
description: Использование идентификаторов с одностраничное приложение, в рамках приложения ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 302a5e10a70e40e75ab9fe4b3e5a98c4e847b822
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815223"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="21535-103">Проверка подлинности и авторизация для одностраничных приложений</span><span class="sxs-lookup"><span data-stu-id="21535-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="21535-104">ASP.NET Core 3.0 или более поздней предлагает проверки подлинности в одностраничных приложений (SPA), используя поддержку API авторизации.</span><span class="sxs-lookup"><span data-stu-id="21535-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="21535-105">Удостоверение ASP.NET Core для проверки подлинности и хранение данных пользователей в сочетании с [IdentityServer](https://identityserver.io/) для реализации Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="21535-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="21535-106">Был добавлен параметр проверки подлинности **Angular** и **React** , аналогично параметру проверки подлинности в шаблоны проектов **веб-приложение (Model-View-Controller)**  (MVC) и **веб-приложение** шаблоны проектов (Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="21535-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="21535-107">Допустимых значениях параметров являются **None** и **отдельных**.</span><span class="sxs-lookup"><span data-stu-id="21535-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="21535-108">**React.js и Redux** шаблон проекта не поддерживает параметр проверки подлинности в данный момент.</span><span class="sxs-lookup"><span data-stu-id="21535-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="21535-109">Создать приложение с помощью поддержки проверки подлинности API</span><span class="sxs-lookup"><span data-stu-id="21535-109">Create an app with API authorization support</span></span>

<span data-ttu-id="21535-110">Проверки подлинности пользователя можно с помощью Angular и React одностраничные приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="21535-111">Откройте командную строку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="21535-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="21535-112">**Angular**:</span><span class="sxs-lookup"><span data-stu-id="21535-112">**Angular**:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="21535-113">**REACT**:</span><span class="sxs-lookup"><span data-stu-id="21535-113">**React**:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="21535-114">Приведенная выше команда создает приложения ASP.NET Core с помощью *ClientApp* каталог, содержащий SPA.</span><span class="sxs-lookup"><span data-stu-id="21535-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="21535-115">Общее описание компонентов приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="21535-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="21535-116">В следующих разделах описаны дополнения к проекту, когда включается поддержка проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="21535-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="21535-117">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="21535-117">Startup class</span></span>

<span data-ttu-id="21535-118">`Startup` Класс имеет следующими дополнениями:</span><span class="sxs-lookup"><span data-stu-id="21535-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="21535-119">Внутри `Startup.ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="21535-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="21535-120">Удостоверение с помощью пользовательского интерфейса по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="21535-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="21535-121">Сервер с помощью дополнительного `AddApiAuthorization` вспомогательный метод этой настройки некоторые по умолчанию соглашения об ASP.NET Core поверх IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="21535-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="21535-122">Проверка подлинности с помощью дополнительного `AddIdentityServerJwt` вспомогательный метод, который настраивает приложение для проверки маркеров JWT, полученных при IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="21535-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="21535-123">Внутри `Startup.Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="21535-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="21535-124">Промежуточного по проверки подлинности, который отвечает за проверку учетных данных запроса и настройке пользователя на контекст запроса:</span><span class="sxs-lookup"><span data-stu-id="21535-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="21535-125">IdentityServer промежуточного слоя, которое предоставляет конечные точки Open ID Connect:</span><span class="sxs-lookup"><span data-stu-id="21535-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="21535-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="21535-126">AddApiAuthorization</span></span>

<span data-ttu-id="21535-127">Этот вспомогательный метод настраивает сервер для использования нашей поддерживаемые конфигурации.</span><span class="sxs-lookup"><span data-stu-id="21535-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="21535-128">IdentityServer — это мощная и расширяемая платформа для обработки обеспечения безопасности приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="21535-129">В то же время, предоставляющий ненужные сложности для наиболее распространенных сценариев.</span><span class="sxs-lookup"><span data-stu-id="21535-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="21535-130">Следовательно набор соглашений и параметры конфигурации вам предоставляется, которые считаются хорошей отправной точкой.</span><span class="sxs-lookup"><span data-stu-id="21535-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="21535-131">После изменения потребностей проверки подлинности, всю мощь identityserver должно по-прежнему доступен для настройки проверки подлинности в соответствии с потребностями.</span><span class="sxs-lookup"><span data-stu-id="21535-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="21535-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="21535-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="21535-133">Этот вспомогательный метод настраивает схему политики для приложения в качестве обработчика проверки подлинности по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="21535-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="21535-134">Эта политика настроена для удостоверения, которые обрабатывают все запросы направляются все вложенный путь в пространстве URL-адрес удостоверения «/ Identity».</span><span class="sxs-lookup"><span data-stu-id="21535-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="21535-135">`JwtBearerHandler` Обрабатывает все запросы.</span><span class="sxs-lookup"><span data-stu-id="21535-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="21535-136">Кроме того, этот метод регистрирует `<<ApplicationName>>API` API ресурсов с identityserver должно с областью по умолчанию `<<ApplicationName>>API` и настраивает промежуточного слоя маркеров носителей JWT для проверки токенов, выданных IdentityServer для приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="21535-137">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="21535-137">SampleDataController</span></span>

<span data-ttu-id="21535-138">В *Controllers\SampleDataController.cs* файл, обратите внимание, что `[Authorize]` атрибут, примененный к классу, который указывает, что пользователь должен быть авторизован исходя из политики по умолчанию для доступа к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="21535-138">In the *Controllers\SampleDataController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="21535-139">Политика авторизации по умолчанию происходит с настроить для использования схему проверки подлинности по умолчанию, который настраивается с `AddIdentityServerJwt` схему политики, которая упоминалась выше, делая `JwtBearerHandler` задаются обработчик по умолчанию для таких вспомогательный метод запросы к приложению.</span><span class="sxs-lookup"><span data-stu-id="21535-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="21535-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="21535-140">ApplicationDbContext</span></span>

<span data-ttu-id="21535-141">В *Data\ApplicationDbContext.cs* файл, обратите внимание, что же `DbContext` используется удостоверение с исключением, который он расширяет `ApiAuthorizationDbContext` (более производный класс от `IdentityDbContext`) для включения схемы для IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="21535-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="21535-142">Чтобы получить полный контроль схемы базы данных, унаследованные от одного из доступных удостоверений `DbContext` классов и настроить контекст включать схему удостоверений, вызвав `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` на `OnModelCreating` метод.</span><span class="sxs-lookup"><span data-stu-id="21535-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="21535-143">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="21535-143">OidcConfigurationController</span></span>

<span data-ttu-id="21535-144">В *Controllers\OidcConfigurationController.cs* файл, обратите внимание, что конечную точку, которая подготавливается для обслуживания OIDC параметры, необходимые клиенту для использования.</span><span class="sxs-lookup"><span data-stu-id="21535-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="21535-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="21535-145">appsettings.json</span></span>

<span data-ttu-id="21535-146">В *appsettings.json* файл из корневого каталога проекта, доступна новая `IdentityServer` раздел, описывающий список настроить клиенты.</span><span class="sxs-lookup"><span data-stu-id="21535-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="21535-147">В следующем примере имеется один клиент.</span><span class="sxs-lookup"><span data-stu-id="21535-147">In the following example, there's a single client.</span></span> <span data-ttu-id="21535-148">Имя клиента соответствует имени приложения и сопоставить по соглашению с OAuth `ClientId` параметра.</span><span class="sxs-lookup"><span data-stu-id="21535-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="21535-149">Профиль указывает настраиваемый тип приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="21535-150">Он используется системой для обозначения дисков, которые упрощают процесс настройки для сервера.</span><span class="sxs-lookup"><span data-stu-id="21535-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="21535-151">Существует несколько профилей, как описано в [профили приложений](#application-profiles) раздел.</span><span class="sxs-lookup"><span data-stu-id="21535-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="21535-152">appSettings. Development.JSON</span><span class="sxs-lookup"><span data-stu-id="21535-152">appsettings.Development.json</span></span>

<span data-ttu-id="21535-153">В *appsettings. Development.JSON* файл из корневой папки проекта есть `IdentityServer` раздел, описывающий ключ, используемый для подписи маркеров.</span><span class="sxs-lookup"><span data-stu-id="21535-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="21535-154">При развертывании в рабочей среде, ключ необходимо подготовить и развернуть вместе с приложениями, как описано в [развертывание в рабочей среде](#deploy-to-production) раздел.</span><span class="sxs-lookup"><span data-stu-id="21535-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="21535-155">Общее описание приложения Angular</span><span class="sxs-lookup"><span data-stu-id="21535-155">General description of the Angular app</span></span>

<span data-ttu-id="21535-156">Аутентификацию и авторизацию API поддерживает в Angular шаблон находится в свой собственный модуль Angular в *ClientApp\src\api авторизации* каталога.</span><span class="sxs-lookup"><span data-stu-id="21535-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="21535-157">Модуль состоит из следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="21535-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="21535-158">3 компонента:</span><span class="sxs-lookup"><span data-stu-id="21535-158">3 components:</span></span>
  * <span data-ttu-id="21535-159">*Login.Component.TS*: Обрабатывает поток входа приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="21535-160">*Logout.Component.TS*: Обрабатывает поток выхода приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="21535-161">*login-menu.component.ts*: Мини-приложение, отображает одно из следующих ссылок:</span><span class="sxs-lookup"><span data-stu-id="21535-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="21535-162">Управление профилями пользователей и выход связи, когда пользователь проходит проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="21535-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="21535-163">Регистрация и вход связи, когда пользователь не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="21535-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="21535-164">Условие маршрута `AuthorizeGuard` , можно добавить маршруты и пользователь должен пройти проверку подлинности перед посещением маршрута.</span><span class="sxs-lookup"><span data-stu-id="21535-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="21535-165">Перехватчик HTTP `AuthorizeInterceptor` маркер доступа, присоединяет к исходящим запросам HTTP, предназначенных для API, когда пользователь проходит проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="21535-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="21535-166">Услуга `AuthorizeService` , обрабатывает сведения низкого уровня процесса проверки подлинности и предоставляет информацию об аутентифицированном пользователе к остальной части приложения для использования.</span><span class="sxs-lookup"><span data-stu-id="21535-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="21535-167">Angular модуль, который определяет маршруты, связанные с частями проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="21535-168">Он предоставляет компонент меню для имени входа, перехватчик, условие и службы для потребления от остальной части приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="21535-169">Общее описание приложение React</span><span class="sxs-lookup"><span data-stu-id="21535-169">General description of the React app</span></span>

<span data-ttu-id="21535-170">Поддержка проверки подлинности и авторизации API в шаблоне React находится в *ClientApp\src\components\api авторизации* каталога.</span><span class="sxs-lookup"><span data-stu-id="21535-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="21535-171">Он состоит из следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="21535-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="21535-172">4 компонентами:</span><span class="sxs-lookup"><span data-stu-id="21535-172">4 components:</span></span>
  * <span data-ttu-id="21535-173">*Login.js*: Обрабатывает поток входа приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="21535-174">*Logout.js*: Обрабатывает поток выхода приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="21535-175">*LoginMenu.js*: Мини-приложение, отображает одно из следующих ссылок:</span><span class="sxs-lookup"><span data-stu-id="21535-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="21535-176">Управление профилями пользователей и выход связи, когда пользователь проходит проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="21535-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="21535-177">Регистрация и вход связи, когда пользователь не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="21535-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="21535-178">*AuthorizeRoute.js*: Указанный компонент маршрута, который пользователь должен пройти проверку подлинности перед отображением компонента в `Component` параметра.</span><span class="sxs-lookup"><span data-stu-id="21535-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="21535-179">Экспортированное `authService` экземпляр класса `AuthorizeService` , обрабатывает сведения низкого уровня процесса проверки подлинности и предоставляет информацию об аутентифицированном пользователе к остальной части приложения для использования.</span><span class="sxs-lookup"><span data-stu-id="21535-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="21535-180">Теперь, когда вы узнали основные компоненты решения, может занять более глубокое рассмотрение отдельные сценарии для приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="21535-181">Требовать авторизации на новый интерфейс API</span><span class="sxs-lookup"><span data-stu-id="21535-181">Require authorization on a new API</span></span>

<span data-ttu-id="21535-182">По умолчанию система настроена для легко требуют наличия авторизации для новых интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="21535-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="21535-183">Чтобы сделать это, создайте новый контроллер и добавьте `[Authorize]` атрибут в класс контроллера или к любому действию контроллера.</span><span class="sxs-lookup"><span data-stu-id="21535-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="21535-184">Защитить клиентские маршрут (Angular)</span><span class="sxs-lookup"><span data-stu-id="21535-184">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="21535-185">Защита маршрут на стороне клиента можно сделать, добавив guard авторизовать в список условий, который будет выполняться при настройке маршрута.</span><span class="sxs-lookup"><span data-stu-id="21535-185">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="21535-186">Например, можно увидеть как `fetch-data` маршрут настроен в модуле Angular основного приложения:</span><span class="sxs-lookup"><span data-stu-id="21535-186">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="21535-187">Важно отметить, что защита маршрут не защищает фактической конечной точки (по-прежнему требуется `[Authorize]` атрибут, примененный к нему), но, что он только ограничивает пользователя перейти на заданный маршрут со стороны клиента, когда он не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="21535-187">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="21535-188">Проверка подлинности запросов API (Angular)</span><span class="sxs-lookup"><span data-stu-id="21535-188">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="21535-189">Проверка подлинности запросов к API, размещенного вместе с приложениями выполняется автоматически при помощи перехватчик клиента HTTP, определенная приложением.</span><span class="sxs-lookup"><span data-stu-id="21535-189">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="21535-190">Защитить клиентские маршрут (React)</span><span class="sxs-lookup"><span data-stu-id="21535-190">Protect a client-side route (React)</span></span>

<span data-ttu-id="21535-191">Защитить клиентские маршрут с помощью `AuthorizeRoute` компонента вместо обычного `Route` компонента.</span><span class="sxs-lookup"><span data-stu-id="21535-191">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="21535-192">Например, обратите внимание, как `fetch-data` маршрут настроен в `App` компонента:</span><span class="sxs-lookup"><span data-stu-id="21535-192">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="21535-193">Защита маршрут:</span><span class="sxs-lookup"><span data-stu-id="21535-193">Protecting a route:</span></span>

* <span data-ttu-id="21535-194">Не защищает фактической конечной точки (по-прежнему требуется `[Authorize]` атрибут, примененный к нему).</span><span class="sxs-lookup"><span data-stu-id="21535-194">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="21535-195">Только пользователю перейти на заданный маршрут со стороны клиента, когда он не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="21535-195">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="21535-196">Проверка подлинности запросов API (React)</span><span class="sxs-lookup"><span data-stu-id="21535-196">Authenticate API requests (React)</span></span>

<span data-ttu-id="21535-197">Проверка подлинности запросов с помощью React выполняется путем импорта `authService` экземпляра из `AuthorizeService`.</span><span class="sxs-lookup"><span data-stu-id="21535-197">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="21535-198">Извлечение маркера доступа из `authService` и присоединенного к запросу, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="21535-198">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="21535-199">В компонентах React, эта работа обычно выполняется `componentDidMount` метод жизненного цикла, так и в результате взаимодействия пользователей.</span><span class="sxs-lookup"><span data-stu-id="21535-199">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="21535-200">Импортировать authService в компоненте</span><span class="sxs-lookup"><span data-stu-id="21535-200">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="21535-201">Получить и присоединить маркер доступа в ответ</span><span class="sxs-lookup"><span data-stu-id="21535-201">Retrieve and attach the access token to the response</span></span>

```javascript
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-to-production"></a><span data-ttu-id="21535-202">Развертывание в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="21535-202">Deploy to production</span></span>

<span data-ttu-id="21535-203">Чтобы развернуть приложение в рабочей среде, необходимо подготовить следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="21535-203">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="21535-204">База данных для хранения идентификаторов учетных записей пользователей и предоставление разрешений IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="21535-204">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="21535-205">Рабочий сертификат для подписи маркеров.</span><span class="sxs-lookup"><span data-stu-id="21535-205">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="21535-206">Требования к этому сертификату; Это может быть, самозаверяющий сертификат или сертификат, предоставленными через Центр сертификации.</span><span class="sxs-lookup"><span data-stu-id="21535-206">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="21535-207">Его можно формировать с помощью стандартных средств, таких как PowerShell или OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="21535-207">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="21535-208">Его можно устанавливать в хранилище сертификатов на целевых компьютерах или развернуть в качестве *.pfx* файл с надежным паролем.</span><span class="sxs-lookup"><span data-stu-id="21535-208">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="21535-209">Пример Развертывание на веб-сайты Azure</span><span class="sxs-lookup"><span data-stu-id="21535-209">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="21535-210">В этом разделе описывается развертывание приложения для веб-сайты Azure с помощью сертификата, хранящегося в хранилище сертификатов.</span><span class="sxs-lookup"><span data-stu-id="21535-210">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="21535-211">Чтобы изменить приложение, чтобы загрузить сертификат из хранилища сертификатов, план службы приложений нужно размещать на по крайней мере уровень "стандартный", при настройке на более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="21535-211">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="21535-212">В приложении *appsettings.json* файл, измените `IdentityServer` раздел, посвященный приведены основные сведения:</span><span class="sxs-lookup"><span data-stu-id="21535-212">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Store",
    "StoreName": "My",
    "StoreLocation": "CurrentUser",
    "Name": "CN=MyApplication"
  }
}
```

* <span data-ttu-id="21535-213">Свойство name на сертификат соответствует различающееся субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="21535-213">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="21535-214">Расположение хранилища представляет расположение для загрузки сертификата из (`CurrentUser` или `LocalMachine`).</span><span class="sxs-lookup"><span data-stu-id="21535-214">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="21535-215">Имя хранилища представляет имя хранилища сертификатов, где хранится сертификат.</span><span class="sxs-lookup"><span data-stu-id="21535-215">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="21535-216">В этом случае она указывает на хранилище пользователя.</span><span class="sxs-lookup"><span data-stu-id="21535-216">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="21535-217">Чтобы развернуть веб-сайтах Azure, развернуть приложение, описанные в [развертывание приложения в Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) создать необходимые ресурсы Azure и развернуть приложение в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="21535-217">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="21535-218">После выполнения приведенных выше инструкций, приложение развертывается в Azure, но еще не функциональной.</span><span class="sxs-lookup"><span data-stu-id="21535-218">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="21535-219">Сертификат, используемый приложением по-прежнему необходимо настроить.</span><span class="sxs-lookup"><span data-stu-id="21535-219">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="21535-220">Найти отпечаток сертификата для использования и выполните действия, описанные в [загрузка сертификатов](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span><span class="sxs-lookup"><span data-stu-id="21535-220">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span></span>

<span data-ttu-id="21535-221">Хотя эти шаги упомянуть SSL, то **закрытые сертификаты** раздела на портале, где можно загрузить подготовленную сертификат для использования с приложением.</span><span class="sxs-lookup"><span data-stu-id="21535-221">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="21535-222">После выполнения этого шага, перезапустите приложение и должно работать.</span><span class="sxs-lookup"><span data-stu-id="21535-222">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="21535-223">Другие параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="21535-223">Other configuration options</span></span>

<span data-ttu-id="21535-224">Поддержка API авторизации подключает IdentityServer с набором соглашений, значения по умолчанию и усовершенствования для упрощения работы для одностраничных приложений.</span><span class="sxs-lookup"><span data-stu-id="21535-224">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="21535-225">Нечего и говорить всю мощь IdentityServer доступна за кулисами, если интеграции ASP.NET Core не занимают вашего сценария.</span><span class="sxs-lookup"><span data-stu-id="21535-225">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="21535-226">Поддержка ASP.NET Core ориентирован на «» приложениях, где все приложения создаваемых и развертываемых решением нашей организации.</span><span class="sxs-lookup"><span data-stu-id="21535-226">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="21535-227">Таким образом поддержка не предоставляется для таких вещей, как разрешение или федерации.</span><span class="sxs-lookup"><span data-stu-id="21535-227">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="21535-228">Для этих сценариев используйте IdentityServer и следуйте его документации.</span><span class="sxs-lookup"><span data-stu-id="21535-228">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="21535-229">Профили приложений</span><span class="sxs-lookup"><span data-stu-id="21535-229">Application profiles</span></span>

<span data-ttu-id="21535-230">Профили приложений являются стандартных конфигураций для приложений, уточнить свои параметры.</span><span class="sxs-lookup"><span data-stu-id="21535-230">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="21535-231">В настоящее время поддерживаются следующие профили:</span><span class="sxs-lookup"><span data-stu-id="21535-231">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="21535-232">`IdentityServerSPA`: Представляет одностраничного приложения, размещенного вместе с IdentityServer как единое целое.</span><span class="sxs-lookup"><span data-stu-id="21535-232">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="21535-233">`redirect_uri` По умолчанию используется `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="21535-233">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="21535-234">`post_logout_redirect_uri` По умолчанию используется `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="21535-234">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="21535-235">Включает в себя набор областей `openid`, `profile`и всех областях, определенных для интерфейсов API в приложение.</span><span class="sxs-lookup"><span data-stu-id="21535-235">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="21535-236">Набор разрешенных типов ответов OIDC, `id_token token` или каждого из них по отдельности (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="21535-236">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="21535-237">Режим допустимый ответ — `fragment`.</span><span class="sxs-lookup"><span data-stu-id="21535-237">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="21535-238">`SPA`: Представляет SPA, которая не размещена с identityserver должно.</span><span class="sxs-lookup"><span data-stu-id="21535-238">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="21535-239">Включает в себя набор областей `openid`, `profile`и всех областях, определенных для интерфейсов API в приложение.</span><span class="sxs-lookup"><span data-stu-id="21535-239">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="21535-240">Набор разрешенных типов ответов OIDC, `id_token token` или каждого из них по отдельности (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="21535-240">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="21535-241">Режим допустимый ответ — `fragment`.</span><span class="sxs-lookup"><span data-stu-id="21535-241">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="21535-242">`IdentityServerJwt`: Представляет API, размещенного вместе с с IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="21535-242">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="21535-243">Приложение настроено для одной области, по умолчанию используется имя приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-243">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="21535-244">`API`: Представляет интерфейс API, которая не размещена с identityserver должно.</span><span class="sxs-lookup"><span data-stu-id="21535-244">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="21535-245">Приложение настроено для одной области, по умолчанию используется имя приложения.</span><span class="sxs-lookup"><span data-stu-id="21535-245">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="21535-246">Настройка с помощью AppSettings</span><span class="sxs-lookup"><span data-stu-id="21535-246">Configuration through AppSettings</span></span>

<span data-ttu-id="21535-247">Настройка приложений с помощью системы конфигурации, добавив их в список `Clients` или `Resources`.</span><span class="sxs-lookup"><span data-stu-id="21535-247">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="21535-248">Настройка каждого клиента `redirect_uri` и `post_logout_redirect_uri` свойства, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="21535-248">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

```json
"IdentityServer": {
  "Clients": {
    "MySPA": {
      "Profile": "SPA",
      "RedirectUri": "https://www.example.com/authentication/login-callback",
      "LogoutUri": "https://www.example.com/authentication/logout-callback"
    }
  }
}
```

<span data-ttu-id="21535-249">При настройке ресурсов, можно настроить области для ресурса, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="21535-249">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c"
    }
  }
}
```

### <a name="configuration-through-code"></a><span data-ttu-id="21535-250">Настройка с помощью кода</span><span class="sxs-lookup"><span data-stu-id="21535-250">Configuration through code</span></span>

<span data-ttu-id="21535-251">Можно также настроить клиентов и ресурсов с помощью кода, используя перегрузку `AddApiAuthorization` , выполняет действие для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="21535-251">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA", spa =>
        spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
           .WithLogoutRedirectUri(
               "http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource =>
        resource.WithScopes("a", "b", "c"));
});
```

## <a name="additional-resources"></a><span data-ttu-id="21535-252">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="21535-252">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
