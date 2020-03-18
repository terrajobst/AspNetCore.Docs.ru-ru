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
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a>Обеспечение безопасности размещенного в ASP.NET Core Blazor приложения с помощью Identity Server

[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Чтобы создать новое Blazor размещенное приложение в Visual Studio, которое использует [IdentityServer](https://identityserver.io/) для проверки подлинности пользователей и вызовов API:

1. Используйте Visual Studio для создания нового **Blazor приложения сборки** . Дополнительные сведения см. в разделе <xref:blazor/get-started>.
1. В диалоговом окне **Создание нового Blazor приложения** выберите **изменить** в разделе **Проверка подлинности** .
1. Выберите **учетные записи отдельных пользователей** , а затем нажмите **кнопку ОК**.
1. Установите флажок **ASP.NET Core размещено** в разделе **Дополнительно** .
1. Нажмите кнопку **Создать**.

Чтобы создать приложение в командной оболочке, выполните следующую команду:

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`). Имя папки также станет частью имени проекта.

## <a name="server-app-configuration"></a>Конфигурация серверного приложения

В следующих разделах описываются дополнения к проекту при включенной поддержке проверки подлинности.

### <a name="startup-class"></a>Класс Startup

Класс `Startup` имеет следующие дополнения:

* В `Startup.ConfigureServices`:

  * Удостоверение с пользовательским интерфейсом по умолчанию:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer с дополнительным вспомогательным методом <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>, который настраивает некоторые соглашения ASP.NET Core по умолчанию поверх IdentityServer:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * Проверка подлинности с помощью дополнительного вспомогательного метода <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>, который настраивает приложение для проверки маркеров JWT, созданных IdentityServer:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* В `Startup.Configure`:

  * По промежуточного слоя для проверки подлинности, которое отвечает за проверку учетных данных запроса и настройку пользователя в контексте запроса:

    ```csharp
    app.UseAuthentication();
    ```

  * По промежуточного слоя IdentityServer, которое предоставляет конечные точки Open ID Connect (OIDC):

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

Вспомогательный метод <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> настраивает [IdentityServer](https://identityserver.io/) для сценариев ASP.NET Core. IdentityServer — это мощная и расширяемая платформа для обработки проблем безопасности приложений. IdentityServer предоставляет ненужную сложность для наиболее распространенных сценариев. Следовательно, предусмотрен набор соглашений и параметров конфигурации, которые мы рассмотрим хорошей отправной точкой. После изменения требований к проверке подлинности все возможности IdentityServer по-прежнему доступны для настройки проверки подлинности в соответствии с требованиями приложения.

### <a name="addidentityserverjwt"></a>AddIdentityServerJWT

Вспомогательный метод <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> настраивает схему политики для приложения в качестве обработчика проверки подлинности по умолчанию. Политика настроена таким путем, чтобы разрешить удостоверению обработку всех запросов, направляемых по любому вложенному пути в пространстве URL-адресов удостоверений `/Identity`. <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> обрабатывает все остальные запросы. Кроме того, этот метод:

* Регистрирует ресурс API `{APPLICATION NAME}API` в IdentityServer с областью действия по умолчанию `{APPLICATION NAME}API`.
* Настраивает промежуточное по для токена носителя JWT для проверки маркеров, выданных IdentityServer для приложения.

### <a name="weatherforecastcontroller"></a>WeatherForecastController

В `WeatherForecastController` (*Controllers/веасерфорекастконтроллер. CS*) атрибут [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) применяется к классу. Атрибут указывает, что пользователь должен быть санкционирован на основании политики по умолчанию для доступа к ресурсу. Политика авторизации по умолчанию настроена для использования схемы проверки подлинности по умолчанию, которая задается <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> схемы политики, упомянутой ранее. Вспомогательный метод настраивает <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> в качестве обработчика по умолчанию для запросов к приложению.

### <a name="applicationdbcontext"></a>ApplicationDbContext

В `ApplicationDbContext` (*Data/ApplicationDbContext. CS*) один и тот же <xref:Microsoft.EntityFrameworkCore.DbContext> используется в удостоверении с тем исключением, которое оно расширяет <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> чтобы включить схему для IdentityServer. Класс <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> является производным от <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.

Чтобы получить полный контроль над схемой базы данных, наследуйте один из доступных классов удостоверений <xref:Microsoft.EntityFrameworkCore.DbContext> и настройте контекст для включения схемы идентификации путем вызова `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` в методе `OnModelCreating`.

### <a name="oidcconfigurationcontroller"></a>OIDCConfigurationController

В `OidcConfigurationController` (*Controllers/оидкконфигуратионконтроллер. CS*) конечная точка клиента подготавливается для обслуживания параметров OIDC.

### <a name="app-settings-files"></a>Файлы параметров приложения

В файле параметров приложения (*appSettings. JSON*) в корне проекта в разделе `IdentityServer` описывается список настроенных клиентов. В следующем примере есть один клиент. Имя клиента соответствует имени приложения и сопоставляется по соглашению с параметром `ClientId` OAuth. Профиль указывает на настраиваемый тип приложения. Профиль используется внутренне для обозначения соглашений, упрощающих процесс настройки сервера. <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

В файле параметров приложения среды разработки (*appSettings. Development. JSON*) в корне проекта в разделе `IdentityServer` описывается ключ, используемый для подписи маркеров. <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a>Конфигурация клиентского приложения

### <a name="authentication-package"></a>Пакет проверки подлинности

Когда приложение создается для использования отдельных учетных записей пользователей (`Individual`), приложение автоматически получает ссылку на пакет для `Microsoft.AspNetCore.Components.WebAssembly.Authentication` пакета в файле проекта приложения. Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.

При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.

### <a name="api-authorization-support"></a>Поддержка авторизации API

Поддержка проверки подлинности пользователей подключается к контейнеру службы с помощью метода расширения, указанного в пакете `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. Этот метод настраивает все службы, необходимые для взаимодействия приложения с существующей системой авторизации.

```csharp
builder.Services.AddApiAuthorization();
```

По умолчанию конфигурация приложения загружается по соглашению из `_configuration/{client-id}`. По соглашению идентификатор клиента устанавливается в имя сборки приложения. Этот URL-адрес можно изменить, чтобы он указывал на отдельную конечную точку, вызвав перегрузку с параметрами.

### <a name="index-page"></a>Страница индексации

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a>Компонент приложения

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>Компонент Редиректтологин

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>Компонент Логиндисплай

Компонент `LoginDisplay` (*Shared/логиндисплай. Razor*) подготавливается к просмотру в компоненте `MainLayout` (*Shared/маинлайаут. Razor*) и управляет следующими поведениями:

* Для пользователей, прошедших проверку подлинности:
  * Отображает имя текущего пользователя.
  * Содержит ссылку на страницу профиля пользователя в ASP.NET Core удостоверение.
  * Предлагает кнопку для выхода из приложения.
* Для анонимных пользователей:
  * Предлагает возможность регистрации.
  * Предоставляет возможность входа в систему.

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

### <a name="authentication-component"></a>Компонент проверки подлинности

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>Компонент FetchData

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>Запустите приложение

Запустите приложение из серверного проекта. При использовании Visual Studio выберите серверный проект в **Обозреватель решений** и нажмите кнопку **выполнить** на панели инструментов или запустите приложение из меню **Отладка** .

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
