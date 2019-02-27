---
title: Проверка подлинности пользователей с помощью WS-Federation в ASP.NET Core
author: chlowell
description: Этот учебник демонстрирует использование WS-Federation в приложении ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: 7967410686da0e59742b721c0154e143bf19ba01
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833544"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Проверка подлинности пользователей с помощью WS-Federation в ASP.NET Core

Этом руководстве показано, как разрешить пользователям выполнить вход с использованием поставщика проверки подлинности WS-Federation как службы федерации Active Directory (ADFS) или [Azure Active Directory](/azure/active-directory/) (AAD). Он использует пример приложения ASP.NET Core 2.0, описанные в [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).

Для приложений ASP.NET Core 2.0, обеспечивается поддержка WS-Federation [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Этот компонент переносится из [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) и обладает многими механики этого компонента. Тем не менее компоненты отличаются в ряде важных аспектов.

По умолчанию новый по промежуточного слоя:

* Не Разрешать незапрошенные имена входа. Эта функция протокола WS-Federation становится уязвимым для атак XSRF. Тем не менее, его можно включить с помощью `AllowUnsolicitedLogins` параметр.
* Не выполняется для каждой формы post, для входа в систему сообщений. Только запросы к `CallbackPath` проверяются на наличие входа автоматически `CallbackPath` по умолчанию используется `/signin-wsfed` , но можно изменить с помощью наследуемого [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) свойство [ WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) класса. Этот путь можно использовать совместно с другими поставщиками проверки подлинности, включив [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) параметр.

## <a name="register-the-app-with-active-directory"></a>Регистрация приложения в Active Directory

### <a name="active-directory-federation-services"></a>Службы федерации Active Directory

* Откройте страницу сервера **мастером Add Relying Party Trust** из консоли управления AD FS:

![Мастер установки проверяющей стороны отношения доверия: Приветствие](ws-federation/_static/AdfsAddTrust.png)

* Выберите для ввода данных вручную.

![Мастер установки проверяющей стороны отношения доверия: Выберите источник данных](ws-federation/_static/AdfsSelectDataSource.png)

* Введите отображаемое имя для проверяющей стороны. Имя, не имеет значения для приложения ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) не предоставляет поддержку для шифрования маркеров, поэтому не настраивайте сертификат шифрования маркеров:

![Мастер установки проверяющей стороны отношения доверия: Настройка сертификата](ws-federation/_static/AdfsConfigureCert.png)

* Включите поддержку протокола WS-Federation Passive, используя URL-адрес приложения. Убедитесь, что порт является правильным для приложения:

![Мастер установки проверяющей стороны отношения доверия: Настроить URL-адрес](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Это должен быть URL-адрес HTTPS. IIS Express можно предоставить самозаверяющий сертификат, при размещении приложения во время разработки. Kestrel требуется настройка сертификат вручную. См. в разделе [Kestrel документации](xref:fundamentals/servers/kestrel) для получения дополнительных сведений.

* Нажмите кнопку **Далее** Пройдите оставшуюся часть мастера и **закрыть** в конце.

* Требуется ASP.NET Core Identity **идентификатор имени** утверждения. Добавьте один из **изменение правил для утверждений** диалоговое окно:

![Изменение правил утверждения](ws-federation/_static/EditClaimRules.png)

* В **утверждения мастере добавления правила преобразования**, оставьте значение по умолчанию **отправлять атрибуты LDAP как утверждений** выбранным шаблоном и нажмите кнопку **Далее**. Добавление сопоставления правил **SAM-Account-Name** атрибут LDAP для **идентификатор имени** исходящего утверждения:

![Утверждение мастер добавления правила преобразования: Настроить правила для утверждений](ws-federation/_static/AddTransformClaimRule.png)

* Нажмите кнопку **Готово** > **ОК** в **изменение правил для утверждений** окна.

### <a name="azure-active-directory"></a>Azure Active Directory

* Перейдите к колонке регистрации клиента AAD приложения. Нажмите кнопку **Регистрация нового приложения**:

![Azure Active Directory: Регистрация приложений](ws-federation/_static/AadNewAppRegistration.png)

* Введите имя для регистрации приложения. Это не имеет значения для приложения ASP.NET Core.
* Введите URL-адрес, прослушиваемый приложением как **URL-адрес входа**:

![Azure Active Directory: Создание регистрации приложения](ws-federation/_static/AadCreateAppRegistration.png)

* Нажмите кнопку **конечные точки** и обратите внимание, **документа метаданных федерации** URL-адрес. Это по промежуточного слоя WS-Federation `MetadataAddress`:

![Azure Active Directory: Конечные точки](ws-federation/_static/AadFederationMetadataDocument.png)

* Перейдите к новой регистрации приложения. Нажмите кнопку **параметры** > **свойства** и запишите **URI идентификатора приложения**. Это по промежуточного слоя WS-Federation `Wtrealm`:

![Azure Active Directory: Свойства регистрации приложения](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Добавить WS-Federation в качестве внешнего поставщика входа для ASP.NET Core Identity

* Добавить зависимость от [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) в проект.
* WS-Federation, чтобы добавить `Startup.ConfigureServices`:

    ```csharp
    services.AddIdentity<IdentityUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddAuthentication()
        .AddWsFederation(options =>
        {
            // MetadataAddress represents the Active Directory instance used to authenticate users.
            options.MetadataAddress = "https://<ADFS FQDN or AAD tenant>/FederationMetadata/2007-06/FederationMetadata.xml";

            // Wtrealm is the app's identifier in the Active Directory instance.
            // For ADFS, use the relying party's identifier, its WS-Federation Passive protocol URL:
            options.Wtrealm = "https://localhost:44307/";

            // For AAD, use the App ID URI from the app registration's Properties blade:
            options.Wtrealm = "https://wsfedsample.onmicrosoft.com/bf0e7e6d-056e-4e37-b9a6-2c36797b9f01";
        });

    services.AddMvc()
     // ...
    ```

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Войдите с помощью WS-Federation

Обзор приложения и нажмите кнопку **вход** ссылку в заголовке навигации. Наличие возможности войти, используя WsFederation: ![Страница входа](ws-federation/_static/WsFederationButton.png)

С помощью служб федерации Active Directory как поставщик кнопку, перенаправляется на страницу входа служб федерации Active Directory: ![Страницы входа служб федерации Active Directory](ws-federation/_static/AdfsLoginPage.png)

С помощью Azure Active Directory в качестве поставщика кнопку, перенаправляется на страницу входа в AAD: ![Страница входа AAD](ws-federation/_static/AadSignIn.png)

Успешного входа в систему для нового пользователя перенаправляет на страницу регистрации пользователя приложения: ![Страница «Регистрация»](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Использование WS-Federation, без ASP.NET Core Identity

По промежуточного слоя WS-Federation может использоваться без Identity. Пример:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = WsFederationDefaults.AuthenticationScheme;
    })
    .AddWsFederation(options =>
    {
        options.Wtrealm = Configuration["wsfed:realm"];
        options.MetadataAddress = Configuration["wsfed:metadata"];
    })
    .AddCookie();
}

public void Configure(IApplicationBuilder app)
{
    app.UseAuthentication();
        // …
}
```
