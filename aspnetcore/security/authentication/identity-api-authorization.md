---
title: Общие сведения о проверке подлинности для одностраничных приложений на ASP.NET Core
author: javiercn
description: Используйте удостоверение с одностраничным приложением, размещенным в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/08/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 31a5e47d772e7416646c4d83c3209d7d2b254199
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829170"
---
# <a name="authentication-and-authorization-for-spas"></a>Проверка подлинности и авторизация для одностраничные приложения

ASP.NET Core 3,0 или более поздней версии обеспечивает проверку подлинности в одностраничных приложениях (одностраничные приложения) с помощью поддержки авторизации API. ASP.NET Core удостоверения для проверки подлинности и хранения пользователей, объединяется с [IdentityServer](https://identityserver.io/) для реализации Open ID Connect.

В шаблоны проектов **Angular** и **React** добавлен параметр проверки подлинности, аналогичный такому параметру в шаблонах **Веб-приложение (модель-представление-контроллер)** (MVC) и **Веб-приложение** (Razor Pages). Допустимые значения параметра: **None** (нет) и **Individual** (индивидуально). Шаблон проекта **React.js и Redux** сейчас не поддерживает параметр проверки подлинности.

## <a name="create-an-app-with-api-authorization-support"></a>Создание приложения с поддержкой авторизации API

Проверку подлинности и авторизацию пользователя можно использовать как в радиальном, так и на одностраничные приложения. Откройте командную строку и выполните следующую команду:

**Angular**:

```dotnetcli
dotnet new angular -o <output_directory_name> -au Individual
```

**React**:

```dotnetcli
dotnet new react -o <output_directory_name> -au Individual
```

Предыдущая команда создает приложение ASP.NET Core с каталогом *ClientApp* , СОДЕРЖАЩИМ значение SPA.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>Общее описание компонентов ASP.NET Core приложения

В следующих разделах описываются дополнения к проекту при включенной поддержке проверки подлинности.

### <a name="startup-class"></a>Класс Startup

Класс `Startup` имеет следующие дополнения:

* Внутри метода `Startup.ConfigureServices`:
  * Удостоверение с пользовательским интерфейсом по умолчанию:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer с дополнительным вспомогательным методом `AddApiAuthorization`, который настраивает некоторые соглашения ASP.NET Core по умолчанию поверх IdentityServer:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * Проверка подлинности с помощью дополнительного вспомогательного метода `AddIdentityServerJwt`, который настраивает приложение для проверки маркеров JWT, созданных IdentityServer:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* Внутри метода `Startup.Configure`:
  * По промежуточного слоя для проверки подлинности, которое отвечает за проверку учетных данных запроса и настройку пользователя в контексте запроса:

    ```csharp
    app.UseAuthentication();
    ```

  * По промежуточного слоя IdentityServer, предоставляющее конечные точки подключения Open ID:

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

Этот вспомогательный метод настраивает IdentityServer для использования нашей поддерживаемой конфигурации. IdentityServer — это мощная и расширяемая платформа для обработки проблем безопасности приложений. В то же время это делает ненужную сложность для наиболее распространенных сценариев. Следовательно, для вас предоставляется набор соглашений и параметров конфигурации, которые считаются хорошей отправной точкой. После изменения требований к проверке подлинности все возможности IdentityServer по-прежнему доступны для настройки проверки подлинности в соответствии с вашими потребностями.

### <a name="addidentityserverjwt"></a>AddIdentityServerJWT

Этот вспомогательный метод настраивает схему политики для приложения в качестве обработчика проверки подлинности по умолчанию. Политика настроена таким путем, чтобы разрешить всем запросам идентифицировать все запросы, направляемые по любому вложенному пути в пространстве URL-адресов удостоверений "/Identity". `JwtBearerHandler` обрабатывает все остальные запросы. Кроме того, этот метод регистрирует ресурс API `<<ApplicationName>>API` с IdentityServer с областью действия по умолчанию `<<ApplicationName>>API` и настраивает по промежуточного слоя маркера носителя JWT для проверки маркеров, выпущенных IdentityServer для приложения.

### <a name="weatherforecastcontroller"></a>WeatherForecastController

В файле *Controllers\WeatherForecastController.cs* обратите внимание на применяемый к классу атрибут `[Authorize]`, который указывает, что пользователь должен иметь разрешение на доступ к ресурсу согласно политике по умолчанию. Политика авторизации по умолчанию в нашем случае настроена на использование схемы проверки подлинности по умолчанию, которая соответствует вышеупомянутой схеме политики, задаваемой `AddIdentityServerJwt`. За счет этого настроенный таким вспомогательным методом обработчик `JwtBearerHandler` становится обработчиком по умолчанию для запросов к приложению.

### <a name="applicationdbcontext"></a>ApplicationDbContext

В файле *Data\ApplicationDbContext.cs* обратите внимание на использование в Identity того же `DbContext` с тем исключением, что он расширяет `ApiAuthorizationDbContext` (более производный класс от `IdentityDbContext`) и включает схему для IdentityServer.

Чтобы получить полный контроль над схемой базы данных, наследуйте один из доступных классов удостоверений `DbContext` и настройте контекст для включения схемы идентификации путем вызова метода `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` в методе `OnModelCreating`.

### <a name="oidcconfigurationcontroller"></a>OIDCConfigurationController

В файле *Controllers\OidcConfigurationController.cs* обратите внимание на конечную точку, подготовленную для выдачи параметров OIDC, которые необходимо использовать клиенту.

### <a name="appsettingsjson"></a>appsettings.json

В файле *appSettings. JSON* корневого каталога проекта имеется новый `IdentityServer` раздел, описывающий список настроенных клиентов. В следующем примере есть один клиент. Имя клиента соответствует имени приложения и сопоставляется по соглашению с параметром `ClientId` OAuth. Профиль указывает на настраиваемый тип приложения. Он используется внутренне для обозначения соглашений, упрощающих процесс настройки сервера. Существует несколько доступных профилей, как описано в разделе [Профили приложений](#application-profiles) .

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a>appSettings. Development. JSON

В списке *appSettings. Файл Development. JSON* корневого проекта, существует `IdentityServer` раздел, описывающий ключ, используемый для подписи маркеров. При развертывании в рабочей среде ключ должен быть подготовлен и развернут вместе с приложением, как описано в разделе [развертывание в производство](#deploy-to-production) .

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a>Общее описание приложения Angular

Поддержка проверки подлинности и API авторизации в шаблоне Angular реализуется в собственном модуле Angular в каталоге *ClientApp\src\api-authorization*. Модуль состоит из следующих элементов: Модуль состоит из следующих элементов:

* 3 компонента:
  * *Login. Component. TS*: обрабатывает поток входа в приложение.
  * *logout. Component. TS*: обрабатывает поток выхода из приложения.
  * *имя входа. Component. TS*: мини-приложение, которое отображает один из следующих наборов ссылок:
    * Управление профилями пользователей и ссылки для выхода, когда пользователь прошел проверку подлинности.
    * Регистрация и вход в систему, если пользователь не прошел проверку подлинности.
* `AuthorizeGuard` охранника маршрутов, который можно добавить к маршрутам и требует проверки подлинности пользователя перед посещением маршрута.
* `AuthorizeInterceptor` перехватчика HTTP, который прикрепляет маркер доступа к исходящим HTTP-запросам, направленным на API, когда пользователь прошел проверку подлинности.
* `AuthorizeService` службы, которая обрабатывает сведения о процессе проверки подлинности на более низком уровне и предоставляет сведения о пользователе, прошедшем проверку подлинности, в оставшейся части приложения для использования.
* Угловой модуль, который определяет маршруты, связанные с элементами проверки подлинности приложения. Он предоставляет компонент меню входа, перехватчик, условие и службу для использования в остальной части приложения.

## <a name="general-description-of-the-react-app"></a>Общее описание приложения React

Поддержка проверки подлинности и авторизации API в шаблоне отклика находится в каталоге *клиентапп\срк\компонентс\апи-аусоризатион* . Он состоит из следующих элементов:

* 4 компонента:
  * *Login. js*: обрабатывает поток входа в приложение.
  * *Logout. js*: обрабатывает поток выхода из приложения.
  * *Логинмену. js*— мини-приложение, которое отображает один из следующих наборов ссылок:
    * Управление профилями пользователей и ссылки для выхода, когда пользователь прошел проверку подлинности.
    * Регистрация и вход в систему, если пользователь не прошел проверку подлинности.
  * *Аусоризерауте. js*— компонент маршрута, для которого требуется проверка подлинности пользователя перед отрисовкой компонента, указанного в параметре `Component`.
* Экспортированный экземпляр `authService` класса `AuthorizeService`, который обрабатывает сведения о процессе проверки подлинности на более низком уровне и предоставляет сведения о пользователе, прошедшем проверку подлинности, в оставшейся части приложения для использования.

Теперь, когда вы видели основные компоненты решения, вы можете более подробно изучить отдельные сценарии для приложения.

## <a name="require-authorization-on-a-new-api"></a>Требовать авторизацию для нового API

По умолчанию система настроена для простоты авторизации новых API-интерфейсов. Для этого создайте новый контроллер и добавьте атрибут `[Authorize]` в класс Controller или в любое действие в контроллере.

## <a name="customize-the-api-authentication-handler"></a>Настройка обработчика проверки подлинности API

Чтобы настроить конфигурацию обработчика JWT API, настройте его экземпляр <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions>.

```csharp
services.AddAuthentication()
    .AddIdentityServerJwt();

services.Configure<JwtBearerOptions>(
    IdentityServerJwtConstants.IdentityServerJwtBearerScheme,
    options =>
    {
        ...
    });
```

Обработчик JWT API создает события, которые обеспечивают контроль над процессом проверки подлинности с помощью `JwtBearerEvents`. Чтобы обеспечить поддержку авторизации API, `AddIdentityServerJwt` регистрирует собственные обработчики событий.

Чтобы настроить обработку события, заключите существующий обработчик событий в требуемую дополнительную логику. Например:

```csharp
services.Configure<JwtBearerOptions>(
    IdentityServerJwtConstants.IdentityServerJwtBearerScheme,
    options =>
    {
        var onTokenValidated = options.Events.OnTokenValidated;       
        
        options.Events.OnTokenValidated = async context =>
        {
            await onTokenValidated(context);
            ...
        }
    });
```

В приведенном выше коде обработчик событий `OnTokenValidated` заменяется пользовательской реализацией. Эта реализация:

1. Вызывает исходную реализацию, предоставляемую службой поддержки авторизации API.
1. Запустите собственную пользовательскую логику.

## <a name="protect-a-client-side-route-angular"></a>Защита маршрута на стороне клиента (угловой)

Защита маршрута на стороне клиента выполняется путем добавления защиты авторизовать в список условий, выполняемых при настройке маршрута. В качестве примера можно увидеть, как настраивается `fetch-data` маршрут в главном модуле приложения.

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

Важно упомянуть, что защита маршрута не защищает фактическую конечную точку (к которой по-прежнему требуется примененный к нему атрибут `[Authorize]`), но он только не позволяет пользователю перейти к данному маршруту на стороне клиента, если он не прошел проверку подлинности.

## <a name="authenticate-api-requests-angular"></a>Проверка подлинности запросов API (Angular)

Проверка подлинности запросов к API, размещенным вместе с приложением, выполняется автоматически с помощью перехватчика клиента HTTP, определенного приложением.

## <a name="protect-a-client-side-route-react"></a>Защита маршрута на стороне клиента (React)

Защитите клиентский маршрут с помощью компонента `AuthorizeRoute`, а не обычного `Route` компонента. Например, обратите внимание, что `fetch-data` маршрут настроен в компоненте `App`:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

Защита маршрута:

* Не защищает фактическую конечную точку (к которой по-прежнему требуется примененный атрибут `[Authorize]`).
* Не позволяет пользователю перейти к данному маршруту на стороне клиента, если он не прошел проверку подлинности.

## <a name="authenticate-api-requests-react"></a>Проверка подлинности запросов API (React)

Проверка подлинности запросов с реагированием выполняется путем первоначального импорта `authService` экземпляра из `AuthorizeService`. Маркер доступа извлекается из `authService` и прикрепляется к запросу, как показано ниже. В ответ на компоненты эта работа обычно выполняется в методе `componentDidMount` жизненного цикла или в результате какого-либо взаимодействия с пользователем.

### <a name="import-the-authservice-into-your-component"></a>Импорт authService в компонент

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Получение и присоединение маркера доступа к ответу

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

## <a name="deploy-to-production"></a>Развертывание в рабочую среду

Чтобы развернуть приложение в рабочей среде, необходимо подготовить следующие ресурсы:

* База данных для хранения учетных записей пользователей удостоверений и IdentityServer.
* Рабочий сертификат, используемый для подписи маркеров.
  * Для этого сертификата нет особых требований; Это может быть самозаверяющий сертификат или сертификат, подготовленный через центр сертификации.
  * Его можно создать с помощью стандартных средств, таких как PowerShell или OpenSSL.
  * Его можно установить в хранилище сертификатов на целевых компьютерах или развернуть в виде *PFX* -файла с надежным паролем.

### <a name="example-deploy-to-azure-websites"></a>Пример. развертывание на веб-сайтах Azure

В этом разделе описывается развертывание приложения на веб-сайтах Azure с помощью сертификата, хранящегося в хранилище сертификатов. Чтобы изменить приложение для загрузки сертификата из хранилища сертификатов, при настройке на более позднем этапе план службы приложений должен быть по крайней мере на уровне "Стандартный". В файле *appSettings. JSON* приложения измените раздел `IdentityServer`, включив в него ключевые сведения:

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

* Свойство Name в сертификате соответствует отличительной теме сертификата.
* Расположение хранилища представляет место загрузки сертификата (`CurrentUser` или `LocalMachine`).
* Имя хранилища представляет имя хранилища сертификатов, в котором хранится сертификат. В этом случае он указывает на личное хранилище пользователей.

Чтобы выполнить развертывание на веб-сайтах Azure, разверните приложение, выполнив действия, описанные в разделе [развертывание приложения в Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) , чтобы создать необходимые ресурсы Azure и развернуть приложение в рабочей среде.

После выполнения указанных выше инструкций приложение развертывается в Azure, но еще не работает. Сертификат, используемый приложением, по-прежнему должен быть настроен. Выберите отпечаток для используемого сертификата и выполните действия, описанные в разделе [Загрузка сертификатов](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).

Хотя эти действия упоминают SSL **, на** портале можно отправить подготовленный сертификат для использования с приложением.

После выполнения этого шага перезапустите приложение, и оно должно быть функциональным.

## <a name="other-configuration-options"></a>Другие параметры конфигурации

Поддержка авторизации API основана на IdentityServer с набором соглашений, значениями по умолчанию и улучшениями для упрощения работы одностраничные приложения. Нет нужды говорить, что все возможности IdentityServer доступны в фоновом режиме, если ASP.NET Core интеграции не охватывают ваш сценарий. Поддержка ASP.NET Coreов сосредоточена на «первых» приложениях, где все приложения создаются и развертываются в нашей Организации. Таким образом, поддержка не предоставляется для таких вещей, как согласие или Федерация. Для этих сценариев используйте IdentityServer и следуйте их документации.

### <a name="application-profiles"></a>Профили приложений.

Профили приложений — это предопределенные конфигурации для приложений, которые дополнительно определяют свои параметры. В настоящее время поддерживаются следующие профили:

* `IdentityServerSPA`: представляет собой SPA, размещенная вместе с IdentityServer, как единое целое.
  * Значение `redirect_uri` по умолчанию равно `/authentication/login-callback`.
  * Значение `post_logout_redirect_uri` по умолчанию равно `/authentication/logout-callback`.
  * Набор областей включает `openid`, `profile`и все области, определенные для API-интерфейсов в приложении.
  * Набор разрешенных типов отклика OIDC `id_token token` или каждый из них по отдельности (`id_token`, `token`).
  * Допустимый режим ответа — `fragment`.
* `SPA`: представляет SPA, который не размещается с помощью IdentityServer.
  * Набор областей включает `openid`, `profile`и все области, определенные для API-интерфейсов в приложении.
  * Набор разрешенных типов отклика OIDC `id_token token` или каждый из них по отдельности (`id_token`, `token`).
  * Допустимый режим ответа — `fragment`.
* `IdentityServerJwt`: представляет API, размещенный вместе с IdentityServer.
  * В приложении настроена одна область, которая по умолчанию имеет имя приложения.
* `API`: представляет API, который не размещается с помощью IdentityServer.
  * В приложении настроена одна область, которая по умолчанию имеет имя приложения.

### <a name="configuration-through-appsettings"></a>Настройка с помощью AppSettings

Настройте приложения в системе конфигурации, добавив их в список `Clients` или `Resources`.

Настройте свойства `redirect_uri` и `post_logout_redirect_uri` каждого клиента, как показано в следующем примере:

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

При настройке ресурсов можно настроить области для ресурса, как показано ниже.

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

### <a name="configuration-through-code"></a>Настройка с помощью кода

Можно также настроить клиенты и ресурсы с помощью кода, используя перегрузку `AddApiAuthorization`, которая принимает действия для настройки параметров.

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:spa/angular>
* <xref:spa/react>
* <xref:security/authentication/scaffold-identity>
