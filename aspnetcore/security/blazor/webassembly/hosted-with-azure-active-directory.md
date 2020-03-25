---
title: Обеспечение безопасности размещенного в ASP.NET Core Blazor приложения сборки Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: fc16a7212254e73efd4cea8155975f293e5d9ebb
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219289"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a>Обеспечение безопасности размещенного в ASP.NET Core Blazor приложения сборки Azure Active Directory

[Хавьер Калварро Воронков](https://github.com/javiercn) и [Люк ЛаСаМ](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



В этой статье описывается создание [размещенного вBlazor приложения](xref:blazor/hosting-models#blazor-webassembly) , использующего [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) для проверки подлинности.

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>Регистрация приложений в AAD B2C и создание решения

### <a name="create-a-tenant"></a>Создание клиента

Следуйте указаниям в [кратком руководстве по настройке клиента](/azure/active-directory/develop/quickstart-create-new-tenant) для создания клиента в AAD.

### <a name="register-a-server-api-app"></a>Регистрация приложения API сервера

Следуйте указаниям в [кратком руководстве: регистрация приложения с помощью платформы удостоверений Майкрософт](/azure/active-directory/develop/quickstart-register-app) и последующих разделов Azure AAD, чтобы зарегистрировать приложение AAD для *приложения API сервера* в **Azure Active Directory** > **Регистрация приложений** области портал Azure:

1. Выберите **Новая регистрация**.
1. Укажите **имя** приложения (например, **Blazor Server AAD**).
1. Выберите **Поддерживаемые типы учетных записей**. Для этого интерфейса можно выбрать **учетные записи только в этом каталоге Организации** (один клиент).
1. В этом сценарии *приложению API сервера* не требуется **универсальный код ресурса (URI) перенаправления** , поэтому оставьте в раскрывающемся списке значение **Web** и не вводите URI перенаправления.
1. Отключите **разрешения** > установите флажок **предоставить доступ к администратору для OpenID Connect и offline_access разрешений** .
1. Выберите **Зарегистрировать**.

В окне **разрешения API**удалите разрешение **Microsoft Graph** > **пользователь. чтение** , так как приложению не требуется доступ для входа или профиля уер.

В **предоставление API**:

1. Нажмите **Добавить группу**.
1. Выберите **Сохранить и продолжить**.
1. Укажите **имя области** (например, `API.Access`).
1. Укажите **Отображаемое имя согласия администратора** (например, `Access API`).
1. Введите **Описание согласия администратора** (например, `Allows the app to access server app API endpoints.`).
1. Убедитесь, что для **состояния** задано значение **включено**.
1. Выберите **Добавить область**.

Запишите следующие сведения:

* *Приложение API сервера* Идентификатор приложения (идентификатор клиента) (например, `11111111-1111-1111-1111-111111111111`)
* Идентификатор каталога (идентификатор клиента) (например, `222222222-2222-2222-2222-222222222222`)
* Домен клиента AAD (например, `contoso.onmicrosoft.com`)
* Область по умолчанию (например, `API.Access`)

### <a name="register-a-client-app"></a>Регистрация клиентского приложения

Следуйте указаниям в [кратком руководстве: регистрация приложения с помощью платформы удостоверений Майкрософт](/azure/active-directory/develop/quickstart-register-app) и последующих разделов Azure AAD для регистрации приложения AAD для *клиентского приложения* в **Azure Active Directory** > **Регистрация приложений** области портал Azure:

1. Выберите **Новая регистрация**.
1. Укажите **имя** приложения (например, **Blazor клиента AAD**).
1. Выберите **Поддерживаемые типы учетных записей**. Для этого интерфейса можно выбрать **учетные записи только в этом каталоге Организации** (один клиент).
1. Оставьте в раскрывающемся списке **URI перенаправления** значение **веб**и укажите универсальный код ресурса (uri) перенаправления `https://localhost:5001/authentication/login-callback`.
1. Отключите **разрешения** > установите флажок **предоставить доступ к администратору для OpenID Connect и offline_access разрешений** .
1. Выберите **Зарегистрировать**.

При **проверке Подлинности** > **конфигурации платформы** > **Web**:

1. Убедитесь, что **URI перенаправления** для `https://localhost:5001/authentication/login-callback` имеется.
1. Для **неявного предоставления**установите флажки для **маркеров доступа** и **маркеров идентификации**.
1. Остальные значения по умолчанию для приложения приемлемы для этого интерфейса.
1. Нажмите кнопку **Сохранить**.

В **разрешениях API**:

1. Убедитесь, что приложение имеет **Microsoft Graph** > **пользователь.** разрешение на чтение.
1. Выберите **Добавить разрешение** , а затем — **Мои API**.
1. Выберите *приложение API сервера* из столбца **имя** (например, **Blazor Server AAD**).
1. Откройте список **API** .
1. Разрешение доступа к API (например, `API.Access`).
1. Выберите **Добавить разрешения**.
1. Нажмите кнопку **предоставить содержимое администратора для {имя клиента}** . Выберите **Да** для подтверждения.

Запишите идентификатор приложения *клиентского приложения* (идентификатор клиента) (например, `33333333-3333-3333-3333-333333333333`).

### <a name="create-the-app"></a>Создайте приложение

Замените заполнители в следующей команде на записанные ранее сведения и выполните команду в командной оболочке:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

Чтобы указать расположение выходных данных, которое создает папку проекта, если она не существует, включите параметр OUTPUT в команду с путем (например, `-o BlazorSample`). Имя папки также станет частью имени проекта.

> [!NOTE]
> Важные изменения конфигурации для области маркера доступа по умолчанию см. в разделе [Поддержка службы проверки подлинности](#Authentication service support) . Значение, предоставленное шаблоном Blazor сборки, необходимо изменить вручную после того, как *клиентское приложение* будет создано из шаблона.

## <a name="server-app-configuration"></a>Конфигурация серверного приложения

*Этот раздел относится к **серверному** приложению решения.*

### <a name="authentication-package"></a>Пакет проверки подлинности

Поддержка проверки подлинности и авторизации вызовов ASP.NET Core веб-интерфейсов API обеспечивается `Microsoft.AspNetCore.Authentication.AzureAD.UI`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a>Поддержка службы проверки подлинности

Метод `AddAuthentication` настраивает службы проверки подлинности в приложении и настраивает обработчик носителя JWT в качестве метода проверки подлинности по умолчанию. Метод `AddAzureADBearer` настраивает определенные параметры в обработчике носителя JWT, который требуется для проверки маркеров, создаваемых Azure Active Directory:

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

`UseAuthentication` и `UseAuthorization` убедитесь, что:

* Приложение пытается проанализировать и проверить маркеры в входящих запросах.
* Любой запрос, пытающийся получить доступ к защищенному ресурсу без соответствующих учетных данных, завершится ошибкой.

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a>Параметры приложения

Файл *appSettings. JSON* содержит параметры для настройки обработчика носителя JWT, используемого для проверки маркеров доступа.

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a>Контроллер Веасерфорекаст

Контроллер Веасерфорекаст (*Controllers/веасерфорекастконтроллер. CS*) предоставляет защищенный API с атрибутом `[Authorize]`, применяемым к контроллеру. **Важно** понимать, что:

* Атрибут `[Authorize]` в этом контроллере API является единственным, который защищает этот API от несанкционированного доступа.
* Атрибут `[Authorize]`, используемый в приложении Blazor сборки, служит указанием для приложения, которое должно быть проверено для правильной работы приложения.

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a>Конфигурация клиентского приложения

*Этот раздел относится к **клиентскому** приложению решения.*

### <a name="authentication-package"></a>Пакет проверки подлинности

Когда приложение создается для использования рабочих или учебных учетных записей (`SingleOrg`), приложение автоматически получает ссылку на пакет для [библиотеки проверки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Пакет предоставляет набор примитивов, которые помогают приложению проверять подлинность пользователей и получать маркеры для вызова защищенных интерфейсов API.

При добавлении проверки подлинности в приложение вручную добавьте пакет в файл проекта приложения:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Замените `{VERSION}` в предыдущей ссылке на пакет версией пакета `Microsoft.AspNetCore.Blazor.Templates`, показанного в статье <xref:blazor/get-started>.

Пакет `Microsoft.Authentication.WebAssembly.Msal` транзитивно добавляет пакет `Microsoft.AspNetCore.Components.WebAssembly.Authentication` в приложение.

### <a name="authentication-service-support"></a>Поддержка службы проверки подлинности

Поддержка проверки подлинности пользователей регистрируется в контейнере службы с помощью метода расширения `AddMsalAuthentication`, предоставленного пакетом `Microsoft.Authentication.WebAssembly.Msal`. Этот метод настраивает все службы, необходимые для взаимодействия приложения с поставщиком удостоверений (IP).

*Program.cs*:

При создании *клиентского приложения* область маркера доступа по умолчанию имеет формат `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`. **Удалите `api://`ную часть значения области.** Эта проблема будет устранена в будущих выпусках.

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> Область маркера доступа по умолчанию должна иметь формат `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (например, `11111111-1111-1111-1111-111111111111/API.Access`). Если для параметра области (как показано на портале Azure) предоставлена схема или схема и узел, то *клиентское приложение* создает необработанное исключение при получении от *приложения API сервера*ответа *401 с несанкционированным* доступом.

Метод `AddMsalAuthentication` принимает обратный вызов для настройки параметров, необходимых для проверки подлинности приложения. Значения, необходимые для настройки приложения, можно получить из конфигурации AAD на портале Azure при регистрации приложения.

Области токена доступа по умолчанию представляют собой список областей токенов доступа.

* Включается по умолчанию в запросе на вход.
* Используется для предоставления маркера доступа сразу после проверки подлинности.

Все области должны принадлежать одному и тому же приложению для правил Azure Active Directory. При необходимости можно добавить дополнительные области для дополнительных приложений API:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a>Страница индексации

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a>Компонент приложения

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>Компонент Редиректтологин

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>Компонент Логиндисплай

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>Компонент проверки подлинности

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>Компонент FetchData

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>Запустите приложение

Запустите приложение из серверного проекта. При использовании Visual Studio выберите серверный проект в **Обозреватель решений** и нажмите кнопку **выполнить** на панели инструментов или запустите приложение из меню **Отладка** .

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authentication/azure-active-directory/index>
