---
title: Общие сведения о проверке подлинности для одностраничных приложений на ASP.NET Core
author: javiercn
description: Используйте удостоверение с одностраничным приложением, размещенным в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/05/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 4f6e3a4922c0a8a74b0e13edf1f00fe5f7bb76ba
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082327"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="6e2f6-103">Проверка подлинности и авторизация для одностраничные приложения</span><span class="sxs-lookup"><span data-stu-id="6e2f6-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="6e2f6-104">ASP.NET Core 3,0 или более поздней версии обеспечивает проверку подлинности в одностраничных приложениях (одностраничные приложения) с помощью поддержки авторизации API.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="6e2f6-105">ASP.NET Core удостоверения для проверки подлинности и хранения пользователей, объединяется с [IdentityServer](https://identityserver.io/) для реализации Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="6e2f6-106">В шаблоны проектов **Angular** и **React** добавлен параметр проверки подлинности, аналогичный такому параметру в шаблонах **Веб-приложение (модель-представление-контроллер)** (MVC) и **Веб-приложение** (Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="6e2f6-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="6e2f6-107">Допустимые значения параметра: **None** (нет) и **Individual** (индивидуально).</span><span class="sxs-lookup"><span data-stu-id="6e2f6-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="6e2f6-108">Шаблон проекта **React.js и Redux** сейчас не поддерживает параметр проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="6e2f6-109">Создание приложения с поддержкой авторизации API</span><span class="sxs-lookup"><span data-stu-id="6e2f6-109">Create an app with API authorization support</span></span>

<span data-ttu-id="6e2f6-110">Проверку подлинности и авторизацию пользователя можно использовать как в радиальном, так и на одностраничные приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="6e2f6-111">Откройте командную строку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="6e2f6-112">**Angular**:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-112">**Angular**:</span></span>

```dotnetcli
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="6e2f6-113">**React**:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-113">**React**:</span></span>

```dotnetcli
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="6e2f6-114">Предыдущая команда создает приложение ASP.NET Core с каталогом *ClientApp* , СОДЕРЖАЩИМ значение SPA.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="6e2f6-115">Общее описание компонентов ASP.NET Core приложения</span><span class="sxs-lookup"><span data-stu-id="6e2f6-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="6e2f6-116">В следующих разделах описываются дополнения к проекту при включенной поддержке проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="6e2f6-117">Класс запуска</span><span class="sxs-lookup"><span data-stu-id="6e2f6-117">Startup class</span></span>

<span data-ttu-id="6e2f6-118">`Startup` Класс имеет следующие дополнения:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="6e2f6-119">`Startup.ConfigureServices` Внутри метода:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="6e2f6-120">Удостоверение с пользовательским интерфейсом по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="6e2f6-121">IdentityServer с дополнительным `AddApiAuthorization` вспомогательным методом, который настраивает некоторые соглашения ASP.NET Core по умолчанию поверх IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="6e2f6-122">Проверка подлинности `AddIdentityServerJwt` с помощью дополнительного вспомогательного метода, который настраивает приложение для проверки маркеров JWT, созданных IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="6e2f6-123">`Startup.Configure` Внутри метода:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="6e2f6-124">По промежуточного слоя для проверки подлинности, которое отвечает за проверку учетных данных запроса и настройку пользователя в контексте запроса:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="6e2f6-125">По промежуточного слоя IdentityServer, предоставляющее конечные точки подключения Open ID:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="6e2f6-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="6e2f6-126">AddApiAuthorization</span></span>

<span data-ttu-id="6e2f6-127">Этот вспомогательный метод настраивает IdentityServer для использования нашей поддерживаемой конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="6e2f6-128">IdentityServer — это мощная и расширяемая платформа для обработки проблем безопасности приложений.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="6e2f6-129">В то же время это делает ненужную сложность для наиболее распространенных сценариев.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="6e2f6-130">Следовательно, для вас предоставляется набор соглашений и параметров конфигурации, которые считаются хорошей отправной точкой.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="6e2f6-131">После изменения требований к проверке подлинности все возможности IdentityServer по-прежнему доступны для настройки проверки подлинности в соответствии с вашими потребностями.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="6e2f6-132">AddIdentityServerJWT</span><span class="sxs-lookup"><span data-stu-id="6e2f6-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="6e2f6-133">Этот вспомогательный метод настраивает схему политики для приложения в качестве обработчика проверки подлинности по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="6e2f6-134">Политика настроена таким путем, чтобы разрешить всем запросам идентифицировать все запросы, направляемые по любому вложенному пути в пространстве URL-адресов удостоверений "/Identity".</span><span class="sxs-lookup"><span data-stu-id="6e2f6-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="6e2f6-135">`JwtBearerHandler` Обрабатывает все остальные запросы.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="6e2f6-136">Кроме того, этот метод регистрирует `<<ApplicationName>>API` ресурс API в IdentityServer с `<<ApplicationName>>API` областью по умолчанию и настраивает по промежуточного слоя маркера носителя JWT для проверки маркеров, выданных IdentityServer для приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="6e2f6-137">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="6e2f6-137">WeatherForecastController</span></span>

<span data-ttu-id="6e2f6-138">В файле *Controllers\WeatherForecastController.cs* обратите внимание на применяемый к классу атрибут `[Authorize]`, который указывает, что пользователь должен иметь разрешение на доступ к ресурсу согласно политике по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-138">In the *Controllers\WeatherForecastController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="6e2f6-139">Политика авторизации по умолчанию в нашем случае настроена на использование схемы проверки подлинности по умолчанию, которая соответствует вышеупомянутой схеме политики, задаваемой `AddIdentityServerJwt`. За счет этого настроенный таким вспомогательным методом обработчик `JwtBearerHandler` становится обработчиком по умолчанию для запросов к приложению.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="6e2f6-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="6e2f6-140">ApplicationDbContext</span></span>

<span data-ttu-id="6e2f6-141">В файле *Data\ApplicationDbContext.cs* обратите внимание на использование в Identity того же `DbContext` с тем исключением, что он расширяет `ApiAuthorizationDbContext` (более производный класс от `IdentityDbContext`) и включает схему для IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="6e2f6-142">Чтобы получить полный контроль над схемой базы данных, наследуйте один из доступных классов `DbContext` удостоверений и настройте контекст для включения схемы идентификации путем вызова `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` `OnModelCreating` метода в методе.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="6e2f6-143">OIDCConfigurationController</span><span class="sxs-lookup"><span data-stu-id="6e2f6-143">OidcConfigurationController</span></span>

<span data-ttu-id="6e2f6-144">В файле *Controllers\OidcConfigurationController.cs* обратите внимание на конечную точку, подготовленную для выдачи параметров OIDC, которые необходимо использовать клиенту.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="6e2f6-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="6e2f6-145">appsettings.json</span></span>

<span data-ttu-id="6e2f6-146">В файле *appSettings. JSON* корневого каталога проекта имеется новый `IdentityServer` раздел, описывающий список настроенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="6e2f6-147">В следующем примере есть один клиент.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-147">In the following example, there's a single client.</span></span> <span data-ttu-id="6e2f6-148">Имя клиента соответствует имени приложения и сопоставляется по соглашению с параметром OAuth `ClientId` .</span><span class="sxs-lookup"><span data-stu-id="6e2f6-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="6e2f6-149">Профиль указывает на настраиваемый тип приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="6e2f6-150">Он используется внутренне для обозначения соглашений, упрощающих процесс настройки сервера.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="6e2f6-151">Существует несколько доступных профилей, как описано в разделе [Профили приложений](#application-profiles) .</span><span class="sxs-lookup"><span data-stu-id="6e2f6-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="6e2f6-152">appSettings. Development. JSON</span><span class="sxs-lookup"><span data-stu-id="6e2f6-152">appsettings.Development.json</span></span>

<span data-ttu-id="6e2f6-153">В списке *appSettings. Файл Development. JSON* корневого проекта, есть `IdentityServer` раздел, описывающий ключ, используемый для подписи маркеров.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="6e2f6-154">При развертывании в рабочей среде ключ должен быть подготовлен и развернут вместе с приложением, как описано в разделе [развертывание в производство](#deploy-to-production) .</span><span class="sxs-lookup"><span data-stu-id="6e2f6-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="6e2f6-155">Общее описание приложения Angular</span><span class="sxs-lookup"><span data-stu-id="6e2f6-155">General description of the Angular app</span></span>

<span data-ttu-id="6e2f6-156">Поддержка проверки подлинности и API авторизации в шаблоне Angular реализуется в собственном модуле Angular в каталоге *ClientApp\src\api-authorization*. Модуль состоит из следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="6e2f6-157">Модуль состоит из следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="6e2f6-158">3 компонента:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-158">3 components:</span></span>
  * <span data-ttu-id="6e2f6-159">*имя входа. Component. TS*: Обрабатывает поток входа в приложение.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="6e2f6-160">*logout. Component. TS*: Обрабатывает поток выхода из приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="6e2f6-161">*имя входа — Menu. Component. TS*: Мини-приложение, отображающее один из следующих наборов ссылок:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="6e2f6-162">Управление профилями пользователей и ссылки для выхода, когда пользователь прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="6e2f6-163">Регистрация и вход в систему, если пользователь не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="6e2f6-164">Предохранитель `AuthorizeGuard` маршрута, который можно добавить к маршрутам и требует проверки подлинности пользователя перед посещением маршрута.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="6e2f6-165">Перехватчик `AuthorizeInterceptor` HTTP, который прикрепляет маркер доступа к исходящим HTTP-запросам, направленным на API, когда пользователь прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="6e2f6-166">Служба `AuthorizeService` , которая обрабатывает сведения о процессе проверки подлинности на более низком уровне и предоставляет сведения о пользователе, прошедшем проверку подлинности, в оставшейся части приложения для использования.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="6e2f6-167">Угловой модуль, который определяет маршруты, связанные с элементами проверки подлинности приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="6e2f6-168">Он предоставляет компонент меню входа, перехватчик, условие и службу для использования в остальной части приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="6e2f6-169">Общее описание приложения React</span><span class="sxs-lookup"><span data-stu-id="6e2f6-169">General description of the React app</span></span>

<span data-ttu-id="6e2f6-170">Поддержка проверки подлинности и авторизации API в шаблоне отклика находится в каталоге *клиентапп\срк\компонентс\апи-аусоризатион* .</span><span class="sxs-lookup"><span data-stu-id="6e2f6-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="6e2f6-171">Он состоит из следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="6e2f6-172">4 компонента:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-172">4 components:</span></span>
  * <span data-ttu-id="6e2f6-173">*Имя входа. js*: Обрабатывает поток входа в приложение.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="6e2f6-174">*Logout. js*: Обрабатывает поток выхода из приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="6e2f6-175">*Логинмену. js*: Мини-приложение, отображающее один из следующих наборов ссылок:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="6e2f6-176">Управление профилями пользователей и ссылки для выхода, когда пользователь прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="6e2f6-177">Регистрация и вход в систему, если пользователь не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="6e2f6-178">*Аусоризерауте. js*: Компонент маршрута, требующий проверки подлинности пользователя перед отрисовкой компонента, указанного в `Component` параметре.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="6e2f6-179">Экспортированный `authService` экземпляр класса `AuthorizeService` , который обрабатывает сведения о процессе проверки подлинности на более низком уровне и предоставляет сведения о пользователе, прошедшем проверку подлинности, в оставшейся части приложения для использования.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="6e2f6-180">Теперь, когда вы видели основные компоненты решения, вы можете более подробно изучить отдельные сценарии для приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="6e2f6-181">Требовать авторизацию для нового API</span><span class="sxs-lookup"><span data-stu-id="6e2f6-181">Require authorization on a new API</span></span>

<span data-ttu-id="6e2f6-182">По умолчанию система настроена для простоты авторизации новых API-интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="6e2f6-183">Для этого создайте новый контроллер и добавьте `[Authorize]` атрибут в класс Controller или в любое действие в контроллере.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="6e2f6-184">Защита маршрута на стороне клиента (угловой)</span><span class="sxs-lookup"><span data-stu-id="6e2f6-184">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="6e2f6-185">Защита маршрута на стороне клиента выполняется путем добавления защиты авторизовать в список условий, выполняемых при настройке маршрута.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-185">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="6e2f6-186">В качестве примера можно увидеть, как `fetch-data` настраивается маршрут в главном модуле приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-186">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="6e2f6-187">Важно упомянуть, что защита маршрута не защищает фактическую конечную точку (к которой по- `[Authorize]` прежнему требуется примененный атрибут), но она только запрещает пользователю переходить к конкретному маршруту на стороне клиента, если он не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-187">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="6e2f6-188">Проверка подлинности запросов API (Angular)</span><span class="sxs-lookup"><span data-stu-id="6e2f6-188">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="6e2f6-189">Проверка подлинности запросов к API, размещенным вместе с приложением, выполняется автоматически с помощью перехватчика клиента HTTP, определенного приложением.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-189">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="6e2f6-190">Защита маршрута на стороне клиента (React)</span><span class="sxs-lookup"><span data-stu-id="6e2f6-190">Protect a client-side route (React)</span></span>

<span data-ttu-id="6e2f6-191">Защитите клиентский маршрут, используя компонент, `AuthorizeRoute` а не обычный `Route` компонент.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-191">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="6e2f6-192">Например, обратите внимание, `fetch-data` что маршрут настроен `App` в компоненте:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-192">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="6e2f6-193">Защита маршрута:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-193">Protecting a route:</span></span>

* <span data-ttu-id="6e2f6-194">Не защищает фактическую конечную точку (к которой `[Authorize]` по-прежнему требуется примененный атрибут).</span><span class="sxs-lookup"><span data-stu-id="6e2f6-194">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="6e2f6-195">Не позволяет пользователю перейти к данному маршруту на стороне клиента, если он не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-195">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="6e2f6-196">Проверка подлинности запросов API (React)</span><span class="sxs-lookup"><span data-stu-id="6e2f6-196">Authenticate API requests (React)</span></span>

<span data-ttu-id="6e2f6-197">Проверка подлинности запросов с реагированием выполняется путем первоначального импорта `authService` экземпляра `AuthorizeService`из.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-197">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="6e2f6-198">Маркер доступа извлекается из `authService` и присоединяется к запросу, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-198">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="6e2f6-199">В окне «реагирующие компоненты» эта работа обычно выполняется `componentDidMount` в методе жизненного цикла или в результате какого-либо взаимодействия с пользователем.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-199">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="6e2f6-200">Импорт authService в компонент</span><span class="sxs-lookup"><span data-stu-id="6e2f6-200">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="6e2f6-201">Получение и присоединение маркера доступа к ответу</span><span class="sxs-lookup"><span data-stu-id="6e2f6-201">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-to-production"></a><span data-ttu-id="6e2f6-202">Развертывание в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="6e2f6-202">Deploy to production</span></span>

<span data-ttu-id="6e2f6-203">Чтобы развернуть приложение в рабочей среде, необходимо подготовить следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-203">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="6e2f6-204">База данных для хранения учетных записей пользователей удостоверений и IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-204">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="6e2f6-205">Рабочий сертификат, используемый для подписи маркеров.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-205">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="6e2f6-206">Для этого сертификата нет особых требований; Это может быть самозаверяющий сертификат или сертификат, подготовленный через центр сертификации.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-206">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="6e2f6-207">Его можно создать с помощью стандартных средств, таких как PowerShell или OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-207">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="6e2f6-208">Его можно установить в хранилище сертификатов на целевых компьютерах или развернуть в виде *PFX* -файла с надежным паролем.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-208">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="6e2f6-209">Пример Развертывание на веб-сайтах Azure</span><span class="sxs-lookup"><span data-stu-id="6e2f6-209">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="6e2f6-210">В этом разделе описывается развертывание приложения на веб-сайтах Azure с помощью сертификата, хранящегося в хранилище сертификатов.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-210">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="6e2f6-211">Чтобы изменить приложение для загрузки сертификата из хранилища сертификатов, при настройке на более позднем этапе план службы приложений должен быть по крайней мере на уровне "Стандартный".</span><span class="sxs-lookup"><span data-stu-id="6e2f6-211">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="6e2f6-212">В файле *appSettings. JSON* приложения измените `IdentityServer` раздел, включив в него ключевые сведения:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-212">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

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

* <span data-ttu-id="6e2f6-213">Свойство Name в сертификате соответствует отличительной теме сертификата.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-213">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="6e2f6-214">Расположение хранилища представляет место загрузки сертификата из (`CurrentUser` или `LocalMachine`).</span><span class="sxs-lookup"><span data-stu-id="6e2f6-214">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="6e2f6-215">Имя хранилища представляет имя хранилища сертификатов, в котором хранится сертификат.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-215">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="6e2f6-216">В этом случае он указывает на личное хранилище пользователей.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-216">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="6e2f6-217">Чтобы выполнить развертывание на веб-сайтах Azure, разверните приложение, выполнив действия, описанные в разделе [развертывание приложения в Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) , чтобы создать необходимые ресурсы Azure и развернуть приложение в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-217">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="6e2f6-218">После выполнения указанных выше инструкций приложение развертывается в Azure, но еще не работает.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-218">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="6e2f6-219">Сертификат, используемый приложением, по-прежнему должен быть настроен.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-219">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="6e2f6-220">Выберите отпечаток для используемого сертификата и выполните действия, описанные в разделе [Загрузка сертификатов](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span><span class="sxs-lookup"><span data-stu-id="6e2f6-220">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span></span>

<span data-ttu-id="6e2f6-221">Хотя эти действия упоминают SSL **, на** портале можно отправить подготовленный сертификат для использования с приложением.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-221">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="6e2f6-222">После выполнения этого шага перезапустите приложение, и оно должно быть функциональным.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-222">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="6e2f6-223">Другие параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="6e2f6-223">Other configuration options</span></span>

<span data-ttu-id="6e2f6-224">Поддержка авторизации API основана на IdentityServer с набором соглашений, значениями по умолчанию и улучшениями для упрощения работы одностраничные приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-224">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="6e2f6-225">Нет нужды говорить, что все возможности IdentityServer доступны в фоновом режиме, если ASP.NET Core интеграции не охватывают ваш сценарий.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-225">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="6e2f6-226">Поддержка ASP.NET Coreов сосредоточена на «первых» приложениях, где все приложения создаются и развертываются в нашей Организации.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-226">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="6e2f6-227">Таким образом, поддержка не предоставляется для таких вещей, как согласие или Федерация.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-227">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="6e2f6-228">Для этих сценариев используйте IdentityServer и следуйте их документации.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-228">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="6e2f6-229">Профили приложений</span><span class="sxs-lookup"><span data-stu-id="6e2f6-229">Application profiles</span></span>

<span data-ttu-id="6e2f6-230">Профили приложений — это предопределенные конфигурации для приложений, которые дополнительно определяют свои параметры.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-230">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="6e2f6-231">В настоящее время поддерживаются следующие профили:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-231">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="6e2f6-232">`IdentityServerSPA`: Представляет SPA, размещенный вместе с IdentityServer, как единое целое.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-232">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="6e2f6-233">Значение по умолчанию `/authentication/login-callback`—. `redirect_uri`</span><span class="sxs-lookup"><span data-stu-id="6e2f6-233">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="6e2f6-234">Значение по умолчанию `/authentication/logout-callback`—. `post_logout_redirect_uri`</span><span class="sxs-lookup"><span data-stu-id="6e2f6-234">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="6e2f6-235">Набор областей включает `openid`, и все области, `profile`определенные для API-интерфейсов в приложении.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-235">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="6e2f6-236">Набор разрешенных типов ответов OIDC — `id_token token` или каждый из них по отдельности ( `token``id_token`,).</span><span class="sxs-lookup"><span data-stu-id="6e2f6-236">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="6e2f6-237">Допустимый режим ответа — `fragment`.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-237">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="6e2f6-238">`SPA`: Представляет SPA, который не размещается с помощью IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-238">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="6e2f6-239">Набор областей включает `openid`, и все области, `profile`определенные для API-интерфейсов в приложении.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-239">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="6e2f6-240">Набор разрешенных типов ответов OIDC — `id_token token` или каждый из них по отдельности ( `token``id_token`,).</span><span class="sxs-lookup"><span data-stu-id="6e2f6-240">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="6e2f6-241">Допустимый режим ответа — `fragment`.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-241">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="6e2f6-242">`IdentityServerJwt`: Представляет API, размещенный вместе с IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-242">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="6e2f6-243">В приложении настроена одна область, которая по умолчанию имеет имя приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-243">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="6e2f6-244">`API`: Представляет API, который не размещается с помощью IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-244">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="6e2f6-245">В приложении настроена одна область, которая по умолчанию имеет имя приложения.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-245">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="6e2f6-246">Настройка с помощью AppSettings</span><span class="sxs-lookup"><span data-stu-id="6e2f6-246">Configuration through AppSettings</span></span>

<span data-ttu-id="6e2f6-247">Настройте приложения в системе конфигурации, добавив их в список `Clients` или. `Resources`</span><span class="sxs-lookup"><span data-stu-id="6e2f6-247">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="6e2f6-248">Настройте каждое клиентское `redirect_uri` свойство `post_logout_redirect_uri` и, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="6e2f6-248">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

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

<span data-ttu-id="6e2f6-249">При настройке ресурсов можно настроить области для ресурса, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-249">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

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

### <a name="configuration-through-code"></a><span data-ttu-id="6e2f6-250">Настройка с помощью кода</span><span class="sxs-lookup"><span data-stu-id="6e2f6-250">Configuration through code</span></span>

<span data-ttu-id="6e2f6-251">Можно также настроить клиенты и ресурсы с помощью кода `AddApiAuthorization` , используя перегрузку, которая принимает действие для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="6e2f6-251">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="6e2f6-252">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6e2f6-252">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:security/authentication/scaffold-identity>
