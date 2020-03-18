---
title: Обеспечение безопасности размещенного в ASP.NET Core Blazor приложения с помощью Identity Server
author: guardrex
description: Создание нового Blazor размещенного приложения с проверкой подлинности в Visual Studio, использующей серверную часть [IdentityServer](https://identityserver.io/)
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: a3993bf635e5a7aae408d72796015f2414e13c14
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434477"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="0cd8c-103">Обеспечение безопасности размещенного в ASP.NET Core Blazor приложения с помощью Identity Server</span><span class="sxs-lookup"><span data-stu-id="0cd8c-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="0cd8c-104">[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0cd8c-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="0cd8c-105">Чтобы создать новое Blazor размещенное приложение в Visual Studio, которое использует [IdentityServer](https://identityserver.io/) для проверки подлинности пользователей и вызовов API:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-105">To create a new Blazor hosted app in Visual Studio that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls:</span></span>

1. <span data-ttu-id="0cd8c-106">Используйте Visual Studio для создания нового **Blazor приложения сборки** .</span><span class="sxs-lookup"><span data-stu-id="0cd8c-106">Use Visual Studio to create a new **Blazor WebAssembly** app.</span></span> <span data-ttu-id="0cd8c-107">Дополнительные сведения см. в разделе <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-107">For more information, see <xref:blazor/get-started>.</span></span>
1. <span data-ttu-id="0cd8c-108">В диалоговом окне **Создание нового Blazor приложения** выберите **изменить** в разделе **Проверка подлинности** .</span><span class="sxs-lookup"><span data-stu-id="0cd8c-108">In the **Create a new Blazor app** dialog, select **Change** in the **Authentication** section.</span></span>
1. <span data-ttu-id="0cd8c-109">Выберите **учетные записи отдельных пользователей** , а затем нажмите **кнопку ОК**.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-109">Select **Individual User Accounts** followed by **OK**.</span></span>
1. <span data-ttu-id="0cd8c-110">Установите флажок **ASP.NET Core размещено** в разделе **Дополнительно** .</span><span class="sxs-lookup"><span data-stu-id="0cd8c-110">Select the **ASP.NET Core hosted** checkbox in the **Advanced** section.</span></span>
1. <span data-ttu-id="0cd8c-111">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-111">Select the **Create** button.</span></span>

<span data-ttu-id="0cd8c-112">Чтобы создать приложение в командной оболочке, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-112">To create the app in a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

<span data-ttu-id="0cd8c-113">Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="0cd8c-113">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="0cd8c-114">Имя папки также станет частью имени проекта.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-114">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="0cd8c-115">Конфигурация серверного приложения</span><span class="sxs-lookup"><span data-stu-id="0cd8c-115">Server app configuration</span></span>

<span data-ttu-id="0cd8c-116">В следующих разделах описываются дополнения к проекту при включенной поддержке проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-116">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="0cd8c-117">Класс Startup</span><span class="sxs-lookup"><span data-stu-id="0cd8c-117">Startup class</span></span>

<span data-ttu-id="0cd8c-118">Класс `Startup` имеет следующие дополнения:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="0cd8c-119">В `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-119">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="0cd8c-120">Удостоверение с пользовательским интерфейсом по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="0cd8c-121">IdentityServer с дополнительным вспомогательным методом <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>, который настраивает некоторые соглашения ASP.NET Core по умолчанию поверх IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-121">IdentityServer with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="0cd8c-122">Проверка подлинности с помощью дополнительного вспомогательного метода <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>, который настраивает приложение для проверки маркеров JWT, созданных IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-122">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="0cd8c-123">В `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-123">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="0cd8c-124">По промежуточного слоя для проверки подлинности, которое отвечает за проверку учетных данных запроса и настройку пользователя в контексте запроса:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="0cd8c-125">По промежуточного слоя IdentityServer, которое предоставляет конечные точки Open ID Connect (OIDC):</span><span class="sxs-lookup"><span data-stu-id="0cd8c-125">The IdentityServer middleware that exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="0cd8c-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="0cd8c-126">AddApiAuthorization</span></span>

<span data-ttu-id="0cd8c-127">Вспомогательный метод <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> настраивает [IdentityServer](https://identityserver.io/) для сценариев ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-127">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> <span data-ttu-id="0cd8c-128">IdentityServer — это мощная и расширяемая платформа для обработки проблем безопасности приложений.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="0cd8c-129">IdentityServer предоставляет ненужную сложность для наиболее распространенных сценариев.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-129">IdentityServer exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="0cd8c-130">Следовательно, предусмотрен набор соглашений и параметров конфигурации, которые мы рассмотрим хорошей отправной точкой.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-130">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="0cd8c-131">После изменения требований к проверке подлинности все возможности IdentityServer по-прежнему доступны для настройки проверки подлинности в соответствии с требованиями приложения.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="0cd8c-132">AddIdentityServerJWT</span><span class="sxs-lookup"><span data-stu-id="0cd8c-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="0cd8c-133">Вспомогательный метод <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> настраивает схему политики для приложения в качестве обработчика проверки подлинности по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-133">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="0cd8c-134">Политика настроена таким путем, чтобы разрешить удостоверению обработку всех запросов, направляемых по любому вложенному пути в пространстве URL-адресов удостоверений `/Identity`.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-134">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="0cd8c-135"><xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> обрабатывает все остальные запросы.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-135">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="0cd8c-136">Кроме того, этот метод:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-136">Additionally, this method:</span></span>

* <span data-ttu-id="0cd8c-137">Регистрирует ресурс API `{APPLICATION NAME}API` в IdentityServer с областью действия по умолчанию `{APPLICATION NAME}API`.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-137">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="0cd8c-138">Настраивает промежуточное по для токена носителя JWT для проверки маркеров, выданных IdentityServer для приложения.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-138">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="0cd8c-139">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="0cd8c-139">WeatherForecastController</span></span>

<span data-ttu-id="0cd8c-140">В `WeatherForecastController` (*Controllers/веасерфорекастконтроллер. CS*) атрибут [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) применяется к классу.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-140">In the `WeatherForecastController` (*Controllers/WeatherForecastController.cs*), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="0cd8c-141">Атрибут указывает, что пользователь должен быть санкционирован на основании политики по умолчанию для доступа к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-141">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="0cd8c-142">Политика авторизации по умолчанию настроена для использования схемы проверки подлинности по умолчанию, которая задается <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> схемы политики, упомянутой ранее.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-142">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> to the policy scheme that was mentioned earlier.</span></span> <span data-ttu-id="0cd8c-143">Вспомогательный метод настраивает <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> в качестве обработчика по умолчанию для запросов к приложению.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-143">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="0cd8c-144">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="0cd8c-144">ApplicationDbContext</span></span>

<span data-ttu-id="0cd8c-145">В `ApplicationDbContext` (*Data/ApplicationDbContext. CS*) один и тот же <xref:Microsoft.EntityFrameworkCore.DbContext> используется в удостоверении с тем исключением, которое оно расширяет <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> чтобы включить схему для IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-145">In the `ApplicationDbContext` (*Data/ApplicationDbContext.cs*), the same <xref:Microsoft.EntityFrameworkCore.DbContext> is used in Identity with the exception that it extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="0cd8c-146">Класс <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> является производным от <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="0cd8c-147">Чтобы получить полный контроль над схемой базы данных, наследуйте один из доступных классов удостоверений <xref:Microsoft.EntityFrameworkCore.DbContext> и настройте контекст для включения схемы идентификации путем вызова `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` в методе `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-147">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="0cd8c-148">OIDCConfigurationController</span><span class="sxs-lookup"><span data-stu-id="0cd8c-148">OidcConfigurationController</span></span>

<span data-ttu-id="0cd8c-149">В `OidcConfigurationController` (*Controllers/оидкконфигуратионконтроллер. CS*) конечная точка клиента подготавливается для обслуживания параметров OIDC.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-149">In the `OidcConfigurationController` (*Controllers/OidcConfigurationController.cs*), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="0cd8c-150">Файлы параметров приложения</span><span class="sxs-lookup"><span data-stu-id="0cd8c-150">App settings files</span></span>

<span data-ttu-id="0cd8c-151">В файле параметров приложения (*appSettings. JSON*) в корне проекта в разделе `IdentityServer` описывается список настроенных клиентов.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-151">In the app settings file (*appsettings.json*) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="0cd8c-152">В следующем примере есть один клиент.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-152">In the following example, there's a single client.</span></span> <span data-ttu-id="0cd8c-153">Имя клиента соответствует имени приложения и сопоставляется по соглашению с параметром `ClientId` OAuth.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-153">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="0cd8c-154">Профиль указывает на настраиваемый тип приложения.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-154">The profile indicates the app type being configured.</span></span> <span data-ttu-id="0cd8c-155">Профиль используется внутренне для обозначения соглашений, упрощающих процесс настройки сервера.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-155">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="0cd8c-156">В файле параметров приложения среды разработки (*appSettings. Development. JSON*) в корне проекта в разделе `IdentityServer` описывается ключ, используемый для подписи маркеров.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-156">In the Development environment app settings file (*appsettings.Development.json*) at the project root, the `IdentityServer` section describes the key used to sign tokens.</span></span> <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="0cd8c-157">Конфигурация клиентского приложения</span><span class="sxs-lookup"><span data-stu-id="0cd8c-157">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="0cd8c-158">Пакет проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="0cd8c-158">Authentication package</span></span>

<span data-ttu-id="0cd8c-159">Когда приложение создается для использования отдельных учетных записей пользователей (`Individual`), приложение автоматически получает ссылку на пакет для `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пакета в файле проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-159">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="0cd8c-160">Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-160">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="0cd8c-161">При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-161">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="0cd8c-162">Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-162">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

### <a name="api-authorization-support"></a><span data-ttu-id="0cd8c-163">Поддержка авторизации API</span><span class="sxs-lookup"><span data-stu-id="0cd8c-163">API authorization support</span></span>

<span data-ttu-id="0cd8c-164">Поддержка проверки подлинности пользователей подключается к контейнеру службы с помощью метода расширения, указанного в пакете `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-164">The support for authenticating users is plugged into the service container by the extension method provided inside the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="0cd8c-165">Этот метод настраивает все службы, необходимые для взаимодействия приложения с существующей системой авторизации.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-165">This method sets up all the services needed for the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="0cd8c-166">По умолчанию конфигурация приложения загружается по соглашению из `_configuration/{client-id}`.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-166">By default, it loads the configuration for the app by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="0cd8c-167">По соглашению идентификатор клиента устанавливается в имя сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-167">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="0cd8c-168">Этот URL-адрес можно изменить, чтобы он указывал на отдельную конечную точку, вызвав перегрузку с параметрами.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-168">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="index-page"></a><span data-ttu-id="0cd8c-169">Страница индексации</span><span class="sxs-lookup"><span data-stu-id="0cd8c-169">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="0cd8c-170">Компонент приложения</span><span class="sxs-lookup"><span data-stu-id="0cd8c-170">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="0cd8c-171">Компонент Редиректтологин</span><span class="sxs-lookup"><span data-stu-id="0cd8c-171">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="0cd8c-172">Компонент Логиндисплай</span><span class="sxs-lookup"><span data-stu-id="0cd8c-172">LoginDisplay component</span></span>

<span data-ttu-id="0cd8c-173">Компонент `LoginDisplay` (*Shared/логиндисплай. Razor*) подготавливается к просмотру в компоненте `MainLayout` (*Shared/маинлайаут. Razor*) и управляет следующими поведениями:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-173">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="0cd8c-174">Для пользователей, прошедших проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-174">For authenticated users:</span></span>
  * <span data-ttu-id="0cd8c-175">Отображает имя текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-175">Displays the current user name.</span></span>
  * <span data-ttu-id="0cd8c-176">Содержит ссылку на страницу профиля пользователя в ASP.NET Core удостоверение.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-176">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="0cd8c-177">Предлагает кнопку для выхода из приложения.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-177">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="0cd8c-178">Для анонимных пользователей:</span><span class="sxs-lookup"><span data-stu-id="0cd8c-178">For anonymous users:</span></span>
  * <span data-ttu-id="0cd8c-179">Предлагает возможность регистрации.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-179">Offers the option to register.</span></span>
  * <span data-ttu-id="0cd8c-180">Предоставляет возможность входа в систему.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-180">Offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a><span data-ttu-id="0cd8c-181">Компонент проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="0cd8c-181">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="0cd8c-182">Компонент FetchData</span><span class="sxs-lookup"><span data-stu-id="0cd8c-182">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="0cd8c-183">Запустите приложение</span><span class="sxs-lookup"><span data-stu-id="0cd8c-183">Run the app</span></span>

<span data-ttu-id="0cd8c-184">Запустите приложение из серверного проекта.</span><span class="sxs-lookup"><span data-stu-id="0cd8c-184">Run the app from the Server project.</span></span> <span data-ttu-id="0cd8c-185">При использовании Visual Studio выберите серверный проект в **Обозреватель решений** и нажмите кнопку **выполнить** на панели инструментов или запустите приложение из меню **Отладка** .</span><span class="sxs-lookup"><span data-stu-id="0cd8c-185">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
