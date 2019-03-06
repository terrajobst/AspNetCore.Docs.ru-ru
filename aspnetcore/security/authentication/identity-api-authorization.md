---
title: Общие сведения об аутентификации для одностраничных приложений на ASP.NET Core
author: ''
description: Использование удостоверения с одной страницы приложению, размещенному внутри приложения ASP.NET Core.
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346788"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="17ce6-103">Проверка подлинности и авторизация для одностраничных приложений</span><span class="sxs-lookup"><span data-stu-id="17ce6-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="17ce6-104">В ASP.NET 3.0 мы представляем поддержка проверки подлинности в одностраничных приложений с помощью нашей новой поддержки для авторизации API.</span><span class="sxs-lookup"><span data-stu-id="17ce6-104">In ASP.NET 3.0 we are introducing support for authentication in single page applications using our new support for API authorization.</span></span> <span data-ttu-id="17ce6-105">Эта поддержка основана на комбинации удостоверения ASP.NET Core для проверки подлинности и хранения пользователей и сервера удостоверений для реализации Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="17ce6-105">This support is based on a combination of ASP.NET Core Identity for authenticating and storing users and Identity Server for implementing Open ID Connect.</span></span>

<span data-ttu-id="17ce6-106">Мы добавили новый параметр проверки подлинности в нашем Angular и React шаблонов, аналогично параметру проверки подлинности в наших шаблонов страниц mvc и razor с разрешенными значениями «None» и «Личная».</span><span class="sxs-lookup"><span data-stu-id="17ce6-106">We have added a new authentication parameter to our Angular and React templates that is similar to the authentication parameter in our mvc and razor pages templates with allowed values 'None' and 'Individual'.</span></span>

## <a name="create-an-angular-app-with-api-authorization-support"></a><span data-ttu-id="17ce6-107">Создание приложения Angular с поддержки проверки подлинности API</span><span class="sxs-lookup"><span data-stu-id="17ce6-107">Create an Angular app with API authorization support</span></span>

<span data-ttu-id="17ce6-108">Чтобы создать новое приложение Angular с поддержкой проверки подлинности и авторизация пользователей, откройте командную строку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="17ce6-108">To create a new Angular app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="17ce6-109">Приведенная выше команда создает приложения ASP.NET Core с помощью *ClientApp* каталог, содержащий приложения Angular.</span><span class="sxs-lookup"><span data-stu-id="17ce6-109">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the Angular app.</span></span>

## <a name="create-a-react-app-with-api-authorization-support"></a><span data-ttu-id="17ce6-110">Создание приложения React с поддержки проверки подлинности API</span><span class="sxs-lookup"><span data-stu-id="17ce6-110">Create a React app with API authorization support</span></span>

<span data-ttu-id="17ce6-111">Чтобы создать новое приложение React с поддержкой проверки подлинности и авторизация пользователей, откройте командную строку и выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="17ce6-111">To create a new React app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="17ce6-112">Приведенная выше команда создает приложения ASP.NET Core с помощью *ClientApp* каталога, содержащего приложение React.</span><span class="sxs-lookup"><span data-stu-id="17ce6-112">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the React app.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="17ce6-113">Общее описание компонентов приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17ce6-113">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="17ce6-114">Существует несколько дополнений в проект мы включают поддержку проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="17ce6-114">There are several additions to the project when we include support for authentication:</span></span>

### <a name="startup-class"></a><span data-ttu-id="17ce6-115">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="17ce6-115">Startup class</span></span>

<span data-ttu-id="17ce6-116">Если взглянуть на код в классе Startup ниже Мы ценим следующие включения:</span><span class="sxs-lookup"><span data-stu-id="17ce6-116">If we look at the code in the Startup class below we can appreciate the following inclusions:</span></span>
* <span data-ttu-id="17ce6-117">Внутри `public void ConfigureServices(IServiceCollection services)`:</span><span class="sxs-lookup"><span data-stu-id="17ce6-117">Inside `public void ConfigureServices(IServiceCollection services)`:</span></span>
  * <span data-ttu-id="17ce6-118">Удостоверение с помощью пользовательского интерфейса по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="17ce6-118">Identity with the default UI.</span></span>
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * <span data-ttu-id="17ce6-119">Сервер удостоверений с дополнительным методом вспомогательный AddApiAuthorization этой настройки некоторые по умолчанию соглашения об ASP.NET на основе сервера удостоверений.</span><span class="sxs-lookup"><span data-stu-id="17ce6-119">Identity Server with an additional AddApiAuthorization helper method that setups some default ASP.NET Conventions on top of Identity Server.</span></span>
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * <span data-ttu-id="17ce6-120">Проверка подлинности с помощью AddIdentityServerJwt вспомогательный метод, предназначенный для настройки приложения для проверки токенов Jwt, полученных сервером удостоверений.</span><span class="sxs-lookup"><span data-stu-id="17ce6-120">Authentication with an additional AddIdentityServerJwt helper method that configures the application to validate Jwt tokens produced by Identity Server.</span></span> 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* <span data-ttu-id="17ce6-121">Внутри `public void Configure(IApplicationBuilder app)`:</span><span class="sxs-lookup"><span data-stu-id="17ce6-121">Inside `public void Configure(IApplicationBuilder app)`:</span></span>
  * <span data-ttu-id="17ce6-122">Промежуточного по проверки подлинности, который отвечает за проверку учетных данных во входящем запросе и настройке пользователя на контекст запроса.</span><span class="sxs-lookup"><span data-stu-id="17ce6-122">The authentication middleware that is responsible for validating the credentials in the incoming request and setting the user on the request context.</span></span>
    ```csharp
    app.UseAuthentication();
    ```
  * <span data-ttu-id="17ce6-123">Промежуточный слой сервер удостоверений, который предоставляет конечные точки Openid Connect.</span><span class="sxs-lookup"><span data-stu-id="17ce6-123">The identity server middleware that exposes the Open ID Connect endpoints.</span></span>
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="17ce6-124">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="17ce6-124">AddApiAuthorization</span></span> 
<span data-ttu-id="17ce6-125">Этот вспомогательный метод настраивает сервер удостоверений для использования наших поддерживаемой конфигурации.</span><span class="sxs-lookup"><span data-stu-id="17ce6-125">This helper method configures Identity Server to use our supported configuration.</span></span> <span data-ttu-id="17ce6-126">Сервер удостоверений — это мощный и расширяемый платформа для обработки приложений проблемы безопасности, но в то же время, которое предоставляет массу сложности, нам не нужно знать о для наиболее распространенных сценариев, поэтому мы выбираем набор соглашений и параметры конфигурации, мы рассмотрим являются хорошей отправной точкой.</span><span class="sxs-lookup"><span data-stu-id="17ce6-126">Identity Server is a very powerful and extensible framework for handling application security concerns but at the same time that exposes a lot of complexity that we don't need to know about for the most common scenarios, so we choose a set of conventions and configuration options for you that we consider are a good starting point.</span></span> <span data-ttu-id="17ce6-127">После проверки подлинности потребностей всю мощь Identity Server все еще доступна вам вы можете настроить его в соответствии с потребностями.</span><span class="sxs-lookup"><span data-stu-id="17ce6-127">Once your authentication needs change the full power of Identity Server is still available to you so you can customize it to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="17ce6-128">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="17ce6-128">AddIdentityServerJwt</span></span>
<span data-ttu-id="17ce6-129">Этот вспомогательный метод настраивает схему политики для приложения в качестве обработчика проверки подлинности по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="17ce6-129">This helper method configures a policy scheme for the application as the default authentication handler.</span></span> <span data-ttu-id="17ce6-130">Эта политика настроена для обработки всех запросов, которые отправляются на любой вложенный путь в пространстве URL-адрес удостоверений identity «/ удостоверения» и сообщить JwtBearerHandler обработки всех запросов.</span><span class="sxs-lookup"><span data-stu-id="17ce6-130">The policy is configured to let identity handle all the requests that go to any subpath in the Identity url space "/Identity" and to let the JwtBearerHandler handle all other requests.</span></span>
<span data-ttu-id="17ce6-131">Этот метод регистрирует Addionally `<<ApplicationName>>API` Api ресурса с сервера удостоверений с областью по умолчанию `<<ApplicationName>>API` и настраивает промежуточного слоя маркеров носителей JWT для проверки токенов, выданных сервером удостоверений для приложения.</span><span class="sxs-lookup"><span data-stu-id="17ce6-131">Addionally this method registers an `<<ApplicationName>>API` Api resource with identity server with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by Identity Server for the application.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="17ce6-132">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="17ce6-132">SampleDataController</span></span>
<span data-ttu-id="17ce6-133">Если взглянуть на файл Controllers\SampleDataController.cs можно наблюдать `[Authorize]` атрибут, примененный к классу, который указывает, что пользователь должен быть авторизован исходя из политики по умолчанию для доступа к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="17ce6-133">If we look at the file Controllers\SampleDataController.cs we can observe the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="17ce6-134">Политика авторизации по умолчанию происходит с настроить для использования схему проверки подлинности по умолчанию, который настраивается с `AddIdentityServerJwt` схему политики, который мы упоминали выше, что делает обработчик JwtBearer задаются такие вспомогательный метод обработчика по умолчанию для запросы к приложению.</span><span class="sxs-lookup"><span data-stu-id="17ce6-134">The default authorization policy happens to be configured to use the default authentication scheme which is set up by `AddIdentityServerJwt` to the policy scheme that we mentioned above, making the JwtBearer handler configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="17ce6-135">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="17ce6-135">ApplicationDbContext</span></span>
<span data-ttu-id="17ce6-136">Если взглянуть на файл в Data\ApplicationDbContext.cs мы сможем увидеть одинаковым DbContext, мы используем в удостоверении, за исключением расширяет ApiAuthorizationDbContext (более производный класс от IdentityDbContext) для включения схемы для идентификации сервера.</span><span class="sxs-lookup"><span data-stu-id="17ce6-136">If we look at the file in Data\ApplicationDbContext.cs we can see the same DbContext we use in identity with the exception that it extends ApiAuthorizationDbContext (a more derived class from IdentityDbContext) to include the schema for Identity Server.</span></span>
<span data-ttu-id="17ce6-137">Если требуется полный контроль над схему базы данных можно просто наследовать от одного из доступных классов DbContext удостоверений и настройка контекста для включения схемы удостоверение путем вызова `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` на `OnModelCreating` метод.</span><span class="sxs-lookup"><span data-stu-id="17ce6-137">If we want full control of the database schema we can simply inherit from one of the available Identity DbContext classes and configure the context to include the identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="17ce6-138">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="17ce6-138">OidcConfigurationController</span></span>
<span data-ttu-id="17ce6-139">Если взглянуть на файл Controllers\OidcConfigurationController.cs, мы видим, конечная точка вставать сотрудников для обслуживания OIDC параметры, необходимые клиенту для использования.</span><span class="sxs-lookup"><span data-stu-id="17ce6-139">If we look at the file Controllers\OidcConfigurationController.cs we can see the endpoint that we stand-up to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="17ce6-140">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="17ce6-140">appsettings.json</span></span>
<span data-ttu-id="17ce6-141">Если взглянуть на файл appsettings.json в корневой папке проекта, мы видим, новый `IdentityServer` раздел, описывающий список настройки клиентов, и мы видим, что имеется один клиент.</span><span class="sxs-lookup"><span data-stu-id="17ce6-141">If we look at the appsettings.json file on the root of the project, we can see a new `IdentityServer` section that describes the list of configured clients and we can see that there is a single client.</span></span> <span data-ttu-id="17ce6-142">Имя клиента соответствует имени приложения и сопоставить по соглашению с параметр ClientId oAuth.</span><span class="sxs-lookup"><span data-stu-id="17ce6-142">The name of the client corresponds to the name of the application and is mapped by convention to the oAuth ClientId parameter.</span></span> <span data-ttu-id="17ce6-143">Профиль указывает тип создаваемого приложения, мы настраиваем и используется внутренне для обозначения дисков, которые упрощают процесс настройки для сервера.</span><span class="sxs-lookup"><span data-stu-id="17ce6-143">The profile indicates what type of application we are configuring, and we use it internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="17ce6-144">Существует несколько профилей, доступных как описано в разделе ниже.</span><span class="sxs-lookup"><span data-stu-id="17ce6-144">There are several profiles available explained in the section below.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="17ce6-145">appSettings. Development.JSON</span><span class="sxs-lookup"><span data-stu-id="17ce6-145">appsettings.Development.json</span></span>
<span data-ttu-id="17ce6-146">Если взглянуть на appsettings. Development.JSON файл в корневой папке проекта, можно увидеть новый `IdentityServer` раздел, описывающий ключ, мы используем для подписи маркеров.</span><span class="sxs-lookup"><span data-stu-id="17ce6-146">If we look at the appsettings.Development.json file on the root of the project, we can see a new `IdentityServer` section that describes the key we are using to sign tokens.</span></span> <span data-ttu-id="17ce6-147">При развертывании в рабочей среде ключа необходимо подготовить и развернуть вместе с приложением, как описано ниже.</span><span class="sxs-lookup"><span data-stu-id="17ce6-147">When deploying to production a key needs to be provisioned and deployed alongside the application as explained below.</span></span>

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a><span data-ttu-id="17ce6-148">Общее описание приложения Angular</span><span class="sxs-lookup"><span data-stu-id="17ce6-148">General description of the Angular application</span></span>
<span data-ttu-id="17ce6-149">Поддержка проверки подлинности и авторизации API в шаблоне Angular живет в свой собственный модуль Angular.</span><span class="sxs-lookup"><span data-stu-id="17ce6-149">The support for authentication and API authorization in the Angular template lives in its own Angular module.</span></span> <span data-ttu-id="17ce6-150">В разделе ClientApp\src\api авторизации и он состоит из следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="17ce6-150">Under ClientApp\src\api-authorization and it is composed of the following elements:</span></span>
* <span data-ttu-id="17ce6-151">3 компонента:</span><span class="sxs-lookup"><span data-stu-id="17ce6-151">3 Components:</span></span>
  * <span data-ttu-id="17ce6-152">Компонент имени входа: Обрабатывает поток входа для приложения.</span><span class="sxs-lookup"><span data-stu-id="17ce6-152">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="17ce6-153">Компонент выхода: Обрабатывает поток выхода для приложения.</span><span class="sxs-lookup"><span data-stu-id="17ce6-153">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="17ce6-154">Компонент меню входа: Мини-приложение, отображающая текущего прошедшего проверку подлинности пользователя со ссылками для управления профиль пользователя и выход или ссылки на вход или зарегистрируйтесь, если пользователь не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="17ce6-154">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
* <span data-ttu-id="17ce6-155">Условие маршрута `AuthorizeGuard` , можно добавить маршруты и пользователь должен пройти проверку подлинности перед посещением маршрута.</span><span class="sxs-lookup"><span data-stu-id="17ce6-155">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="17ce6-156">Перехватчик http `AuthorizeInterceptor` маркер доступа, присоединяет к исходящим запросам HTTP, предназначенных для API, когда пользователь проходит проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="17ce6-156">An http interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="17ce6-157">Услуга `AuthorizeService` , обрабатывает подробности более низкого уровня процесса проверки подлинности и предоставляет информацию об аутентифицированном пользователе к остальной части приложения для использования.</span><span class="sxs-lookup"><span data-stu-id="17ce6-157">A service `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>
* <span data-ttu-id="17ce6-158">Angular модуль, который определяет маршруты, связанные с частями приложения проверки подлинности и предоставляет компонент меню для имени входа, перехватчик, условие и службы для потребления от остальной части приложения.</span><span class="sxs-lookup"><span data-stu-id="17ce6-158">An angular module that defines routes associated with the authentication parts of the application and exposes the login menu component, the interceptor, the guard and the service for consumption from the rest of the application.</span></span>

## <a name="general-description-of-the-react-application"></a><span data-ttu-id="17ce6-159">Общее описание приложение React</span><span class="sxs-lookup"><span data-stu-id="17ce6-159">General description of the React application</span></span>
<span data-ttu-id="17ce6-160">Поддержка проверки подлинности и авторизации API находится шаблон React в ClientApp\src\components\api authorization\ и он состоит из следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="17ce6-160">The support for authentication and API authorization in the React template lives under ClientApp\src\components\api-authorization\ and it is composed of the following elements:</span></span>
* <span data-ttu-id="17ce6-161">4 компонентами:</span><span class="sxs-lookup"><span data-stu-id="17ce6-161">4 Components:</span></span>
  * <span data-ttu-id="17ce6-162">Компонент имени входа: Обрабатывает поток входа для приложения.</span><span class="sxs-lookup"><span data-stu-id="17ce6-162">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="17ce6-163">Компонент выхода: Обрабатывает поток выхода для приложения.</span><span class="sxs-lookup"><span data-stu-id="17ce6-163">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="17ce6-164">Компонент меню входа: Мини-приложение, отображающая текущего прошедшего проверку подлинности пользователя со ссылками для управления профиль пользователя и выход или ссылки на вход или зарегистрируйтесь, если пользователь не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="17ce6-164">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
  * <span data-ttu-id="17ce6-165">AuthorizeRoute: Компонент маршрутизации, пользователь должен пройти проверку подлинности перед отображением компонента, указанного в параметре компонента.</span><span class="sxs-lookup"><span data-stu-id="17ce6-165">AuthorizeRoute: A route component that requires a user to be authenticated before rendering the component indicated in the Component parameter.</span></span>
  * <span data-ttu-id="17ce6-166">Экспортированное `authService` экземпляр класса `AuthorizeService` , обрабатывает подробности более низкого уровня процесса проверки подлинности и предоставляет информацию об аутентифицированном пользователе к остальной части приложения для использования.</span><span class="sxs-lookup"><span data-stu-id="17ce6-166">An exported `authService` instance of class `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>

<span data-ttu-id="17ce6-167">Теперь, когда мы видели основные компоненты решения, мы можем создавать особый вид в отдельных сценариях приложения:</span><span class="sxs-lookup"><span data-stu-id="17ce6-167">Now that we've seen the main components of the solution, we can take a specific look at individual scenarios for the application:</span></span>

## <a name="requiring-authorization-on-a-new-api"></a><span data-ttu-id="17ce6-168">Требуется выполнять авторизацию на новый интерфейс API</span><span class="sxs-lookup"><span data-stu-id="17ce6-168">Requiring authorization on a new API</span></span>
<span data-ttu-id="17ce6-169">Система настроена по умолчанию, чтобы сделать тривиальным, чтобы требовать авторизации для новых интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="17ce6-169">The system is configured out of the box to make it trivial to require authorization for new APIs.</span></span> <span data-ttu-id="17ce6-170">Чтобы сделать это, просто создайте новый контроллер и добавьте `[Authorize]` атрибут в класс контроллера или к любому действию контроллера.</span><span class="sxs-lookup"><span data-stu-id="17ce6-170">In order to do so, simply create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protecting-a-client-side-route-angular"></a><span data-ttu-id="17ce6-171">Защита маршрут на стороне клиента (Angular)</span><span class="sxs-lookup"><span data-stu-id="17ce6-171">Protecting a client-side route (Angular)</span></span>
<span data-ttu-id="17ce6-172">Защита маршрут на стороне клиента можно сделать, добавив условие авторизовать в список условий, который будет выполняться при настройке маршрута.</span><span class="sxs-lookup"><span data-stu-id="17ce6-172">Protecting a client side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="17ce6-173">В качестве примера можно увидеть настройку выборки данных маршрута в модуле angular основного приложения:</span><span class="sxs-lookup"><span data-stu-id="17ce6-173">As an example you can see how the fetch-data route is configured within the main app angular module:</span></span>

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="17ce6-174">Важно отметить, что защита маршрут не защищает фактической конечной точки (по-прежнему требуется `[Authorize]` атрибут, примененный к нему), но, что он только запрещает пользователю переход к маршрута на стороне клиента, если он не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="17ce6-174">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="17ce6-175">Проверка подлинности запросов API (Angular)</span><span class="sxs-lookup"><span data-stu-id="17ce6-175">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="17ce6-176">Проверка подлинности запросов к API, размещенных вдоль стороны, что приложение выполняется автоматически при помощи перехватчик клиента HTTP, определяется приложением.</span><span class="sxs-lookup"><span data-stu-id="17ce6-176">Authenticating requests to APIs hosted along side the application is done automatically through the use of the HTTP client interceptor defined by the application.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="17ce6-177">Защитить клиентские маршрут (React)</span><span class="sxs-lookup"><span data-stu-id="17ce6-177">Protect a client-side route (React)</span></span>

<span data-ttu-id="17ce6-178">Защита маршрут на стороне клиента выполняется с помощью компонента AuthorizeRoute вместо обычного маршрута компонента.</span><span class="sxs-lookup"><span data-stu-id="17ce6-178">Protecting a client side route is done by using the AuthorizeRoute component instead of the plain Route component.</span></span> <span data-ttu-id="17ce6-179">В качестве примера можно увидеть настройку выборки данных маршрута в компоненте приложения:</span><span class="sxs-lookup"><span data-stu-id="17ce6-179">As an example you can see how the fetch-data route is configured within the App component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="17ce6-180">Важно отметить, что защита маршрут не защищает фактической конечной точки (по-прежнему требуется `[Authorize]` атрибут, примененный к нему), но, что он только запрещает пользователю переход к маршрута на стороне клиента, если он не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="17ce6-180">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="17ce6-181">Проверка подлинности запросов API (React)</span><span class="sxs-lookup"><span data-stu-id="17ce6-181">Authenticate API requests (React)</span></span>

<span data-ttu-id="17ce6-182">Проверка подлинности запросов с помощью react выполняется путем импорта `authService` экземпляра из `AuthorizeService` получения маркера доступа из authService и подключив его к запросу, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="17ce6-182">Authenticating requests with react is done by first importing the `authService` instance from the `AuthorizeService` and then retrieving the access token from the authService and attaching it to the request as shown below.</span></span> <span data-ttu-id="17ce6-183">В компонентах react обычно это делается в метод жизненного цикла componentDidMount либо в виде результата из взаимодействия пользователей.</span><span class="sxs-lookup"><span data-stu-id="17ce6-183">In react components this is typically done in the componentDidMount lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="17ce6-184">Импортировать authService в компоненте</span><span class="sxs-lookup"><span data-stu-id="17ce6-184">Import the authService into your component</span></span>

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="17ce6-185">Получить и присоединить маркер доступа в ответ</span><span class="sxs-lookup"><span data-stu-id="17ce6-185">Retrieve and attach the access token to the response</span></span>

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a><span data-ttu-id="17ce6-186">Развертывание в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="17ce6-186">Deploy into production</span></span>

<span data-ttu-id="17ce6-187">Чтобы развернуть приложение в рабочей среде, нам нужно подготовить несколько ресурсов:</span><span class="sxs-lookup"><span data-stu-id="17ce6-187">In order to deploy the application into production we need to provision several resources:</span></span>
* <span data-ttu-id="17ce6-188">Предоставляет базу для хранения идентификаторов учетных записей пользователей и сервера удостоверений.</span><span class="sxs-lookup"><span data-stu-id="17ce6-188">A database to store the Identity user accounts and the identity server grants.</span></span>
* <span data-ttu-id="17ce6-189">Рабочий сертификат для подписи маркеров.</span><span class="sxs-lookup"><span data-stu-id="17ce6-189">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="17ce6-190">Требования к этому сертификату; Это может быть, самозаверяющий сертификат или сертификат, предоставленными через Центр сертификации.</span><span class="sxs-lookup"><span data-stu-id="17ce6-190">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="17ce6-191">Его можно формировать с помощью стандартных средств, таких как powershell или openssl.</span><span class="sxs-lookup"><span data-stu-id="17ce6-191">It can be generated through standard tools like powershell or openssl.</span></span>
  * <span data-ttu-id="17ce6-192">Он может быть установлен в хранилище сертификатов на целевых компьютерах или развернут как PFX-файл с надежным паролем.</span><span class="sxs-lookup"><span data-stu-id="17ce6-192">It can be installed into the certificate store on the target machines or deployed as a pfx file with a strong password.</span></span>

### <a name="example-deploy-into-azure-websites"></a><span data-ttu-id="17ce6-193">Пример Развертывание в веб-сайты Azure</span><span class="sxs-lookup"><span data-stu-id="17ce6-193">Example: Deploy into Azure Websites</span></span>

<span data-ttu-id="17ce6-194">В этом разделе мы собираемся развернуть приложение на веб-сайты Azure с помощью сертификата, хранящегося в хранилище сертификатов.</span><span class="sxs-lookup"><span data-stu-id="17ce6-194">In this section we are going to deploy the application to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="17ce6-195">Нам нужно изменить приложения, чтобы загрузить сертификат из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="17ce6-195">We need to modify the application to load a certicate from the certificate store.</span></span> <span data-ttu-id="17ce6-196">Чтобы сделать это, наш план службы приложений должен иметь по крайней мере на уровень "стандартный", если мы настроим в более позднем этапе.</span><span class="sxs-lookup"><span data-stu-id="17ce6-196">To do so, our app service plan needs to be at least on the standard tier when we configure in a later step.</span></span> <span data-ttu-id="17ce6-197">В этом примере нам просто нужно изменить раздел IdentityServer на файл appsettings.json, включают основные сведения:</span><span class="sxs-lookup"><span data-stu-id="17ce6-197">In our application we simply need to modify the IdentityServer section on appsettings.json to include the key details:</span></span>
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* <span data-ttu-id="17ce6-198">Свойство name на сертификат соответствует различающееся субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="17ce6-198">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="17ce6-199">Расположение хранилища представляет расположение для загрузки сертификата из (CurrentUrser или LocalMachine).</span><span class="sxs-lookup"><span data-stu-id="17ce6-199">The store location represents where to load the certificate from (CurrentUrser or LocalMachine).</span></span>
* <span data-ttu-id="17ce6-200">Имя хранилища представляет имя хранилища сертификатов, где хранится сертификат, в данном случае, на который указывает хранилище пользователя.</span><span class="sxs-lookup"><span data-stu-id="17ce6-200">The store name represents the name of the certificate store where the certificate is stored, in this case it points to the personal user store.</span></span>

<span data-ttu-id="17ce6-201">Чтобы развернуть веб-сайтах Azure, развернуть приложение, описанные в [развертывание приложения в Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) создать необходимые ресурсы Azure и развернуть приложение в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="17ce6-201">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="17ce6-202">После этого приложение развертывается в Azure, но еще не полностью работает как мы по-прежнему должен быть настроен сертификат, используемый приложением.</span><span class="sxs-lookup"><span data-stu-id="17ce6-202">After doing this, the application is deployed into Azure but is not yet completely functional as we still need to setup the certificate to be used by the application.</span></span> <span data-ttu-id="17ce6-203">Чтобы сделать это, необходимо иметь отпечаток для сертификата, мы собираемся использовать и выполните действия, описанные в [загрузка сертификатов](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="17ce6-203">To do so, we need to have the thumbprint for the certificate we are going to use and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="17ce6-204">Хотя эти шаги упомянуть SSL, имеется раздел «Закрытые сертификаты» на портале, где мы можем передать наших подготовленной сертификат, используемый вместе с приложением.</span><span class="sxs-lookup"><span data-stu-id="17ce6-204">While these steps mention SSL, there is a "Private certificates" section on the portal where we can upload our provisioned certificate to use with our app.</span></span>

<span data-ttu-id="17ce6-205">После выполнения этого шага мы должны получить возможность перезапуска наше приложение и должно быть полностью работоспособным.</span><span class="sxs-lookup"><span data-stu-id="17ce6-205">After this step, we should be able to restart our application and it should be completely functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="17ce6-206">Другие параметры конфигурации</span><span class="sxs-lookup"><span data-stu-id="17ce6-206">Other configuration options</span></span>
<span data-ttu-id="17ce6-207">Наши поддержка авторизации API строится поверх Identity Server с набором соглашений, значения по умолчанию и усовершенствования для упрощения работы для одностраничных приложений.</span><span class="sxs-lookup"><span data-stu-id="17ce6-207">Our support for API authorization builds on top of Identity Server with a set of conventions, default values and enhancements to simplify the experience for Single Page Applications.</span></span> <span data-ttu-id="17ce6-208">Нечего и говорить всю мощь Identity Server доступна в фоновом, если интеграций, которые мы предлагаем не занимают вашего сценария.</span><span class="sxs-lookup"><span data-stu-id="17ce6-208">Needless to say, the full power of Identity Server is available behind the scenes if the integrations that we offer don't cover your scenario.</span></span> <span data-ttu-id="17ce6-209">Получение поддержки сосредоточена на так называемых «основных» приложения, где все приложения создаваемых и развертываемых решением нашей организации.</span><span class="sxs-lookup"><span data-stu-id="17ce6-209">Our support is focused on what we call "first-party" applications, where all the applications are created and deployed by our organization.</span></span> <span data-ttu-id="17ce6-210">Таким образом, мы не предоставляем поддержку согласия или федерации.</span><span class="sxs-lookup"><span data-stu-id="17ce6-210">As such we don't offer support for things like consent or federation.</span></span> <span data-ttu-id="17ce6-211">Для этих сценариев мы рекомендуем использовать сервер удостоверений и выполните его документации.</span><span class="sxs-lookup"><span data-stu-id="17ce6-211">For those scenarios our recommendation is to use Identity Server and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="17ce6-212">Профили приложений</span><span class="sxs-lookup"><span data-stu-id="17ce6-212">Application profiles</span></span>
<span data-ttu-id="17ce6-213">Профили приложений являются стандартных конфигураций для приложений, уточнить свои параметры.</span><span class="sxs-lookup"><span data-stu-id="17ce6-213">Application profiles are predefined configurations for applications that further define their parameters.</span></span> <span data-ttu-id="17ce6-214">В настоящее время мы поддерживаем два профиля:</span><span class="sxs-lookup"><span data-stu-id="17ce6-214">At this time we support two profiles:</span></span>
* <span data-ttu-id="17ce6-215">IdentityServerSPA: Представляет одностраничного приложения, размещенного вместе с Identity Server как единое целое.</span><span class="sxs-lookup"><span data-stu-id="17ce6-215">IdentityServerSPA: Represents a single page application hosted alongside Identity Server as a single unit.</span></span>
  * <span data-ttu-id="17ce6-216">По умолчанию используется URI перенаправления `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="17ce6-216">The redirect_uri defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="17ce6-217">По умолчанию используется post_logout_redirect_uri `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="17ce6-217">The post_logout_redirect_uri defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="17ce6-218">Включает в себя набор областей `openid`, `profile`и всех областях, определенных для интерфейсов API в приложение.</span><span class="sxs-lookup"><span data-stu-id="17ce6-218">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="17ce6-219">Набор разрешенных типов ответов OIDC, `id_token token` или каждого из них по отдельности (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="17ce6-219">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="17ce6-220">Режим допустимый ответ — `fragment`.</span><span class="sxs-lookup"><span data-stu-id="17ce6-220">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="17ce6-221">SPA: Представляет одностраничного приложения, которая не размещена с сервера удостоверений.</span><span class="sxs-lookup"><span data-stu-id="17ce6-221">SPA: Represents a single page application that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="17ce6-222">Включает в себя набор областей `openid`, `profile`и всех областях, определенных для интерфейсов API в приложение.</span><span class="sxs-lookup"><span data-stu-id="17ce6-222">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="17ce6-223">Набор разрешенных типов ответов OIDC, `id_token token` или каждого из них по отдельности (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="17ce6-223">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="17ce6-224">Режим допустимый ответ — `fragment`.</span><span class="sxs-lookup"><span data-stu-id="17ce6-224">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="17ce6-225">IdentityServerJwt: Представляет API, размещенного вместе с с помощью сервера удостоверений.</span><span class="sxs-lookup"><span data-stu-id="17ce6-225">IdentityServerJwt: Represents an API that is hosted alongside with Identity Server.</span></span>
  * <span data-ttu-id="17ce6-226">Приложение настроено для одной области, по умолчанию используется имя приложения.</span><span class="sxs-lookup"><span data-stu-id="17ce6-226">The application is configured to have a single scope that defaults to the application name.</span></span>
* <span data-ttu-id="17ce6-227">API: Представляет API, который не будет размещено с сервера удостоверений.</span><span class="sxs-lookup"><span data-stu-id="17ce6-227">API: Represents an API that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="17ce6-228">Приложение настроено для одной области, по умолчанию используется имя приложения.</span><span class="sxs-lookup"><span data-stu-id="17ce6-228">The application is configured to have a single scope that defaults to the application name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="17ce6-229">Настройка с помощью AppSettings</span><span class="sxs-lookup"><span data-stu-id="17ce6-229">Configuration through AppSettings</span></span>
<span data-ttu-id="17ce6-230">Приложения можно настроить через наша система конфигурации, добавив их в список клиентам или ресурсов, соответственно.</span><span class="sxs-lookup"><span data-stu-id="17ce6-230">We can configure the applications through our configuration system by adding them to the list of Clients or Resources respectively.</span></span> 

<span data-ttu-id="17ce6-231">При настройке клиентов можно настроить `redirect_uri` и `post_logout_redirect_uri` как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="17ce6-231">When configuring clients we can configure the `redirect_uri` and the `post_logout_redirect_uri` as shown below:</span></span>
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

<span data-ttu-id="17ce6-232">При настройке ресурсов мы можно настроить области для ресурса, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="17ce6-232">When configuring resources we can configure the scopes for the resource as shown below:</span></span>
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a><span data-ttu-id="17ce6-233">Настройка с помощью кода</span><span class="sxs-lookup"><span data-stu-id="17ce6-233">Configuration through code</span></span>
<span data-ttu-id="17ce6-234">Можно также настроить клиентов и ресурсов с помощью кода, с помощью перегрузки AddApiAuthorization, который принимает действие для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="17ce6-234">We can also configure the clients and resources through code using an overload of AddApiAuthorization that takes an action to configure options.</span></span>
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
