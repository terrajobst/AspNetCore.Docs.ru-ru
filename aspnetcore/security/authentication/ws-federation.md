---
title: "Проверять подлинность пользователей с WS-Federation в ASP.NET Core"
author: chlowell
description: "Демонстрирует использование WS-Federation в приложении ASP.NET Core."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: 0532f866e9c58b2e45623f522f62438e15017e54
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Проверять подлинность пользователей с WS-Federation в ASP.NET Core

Этот учебник демонстрирует включение пользователям входить с использованием поставщика проверки подлинности WS-Federation как службы федерации Active Directory (ADFS) или [Azure Active Directory](/azure/active-directory/) (AAD). Он использует пример приложения ASP.NET Core 2.0, описанные в [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).

Для приложений ASP.NET Core 2.0, обеспечивается поддержка WS-Federation [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Этот компонент будет перенесен из [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) и обладает многими механизм соответствующего компонента. Однако компоненты отличаются в нескольких важных аспектах.

По умолчанию новый по промежуточного слоя:

* Не Разрешать незапрошенные имена входа. Этот компонент протокола WS-Federation подвержена XSRF-атак. Тем не менее, он может быть включен с `AllowUnsolicitedLogins` параметр.
* Не выполняется для каждой формы post для входа в систему сообщений. Только запросы к `CallbackPath` проверяются на наличие знака модули `CallbackPath` по умолчанию используется значение `/signin-wsfed` , но может быть изменено. Этот путь можно использовать совместно с других поставщиков проверки подлинности, включив `SkipUnrecognizedRequests` параметр.

## <a name="register-the-app-with-active-directory"></a>Регистрация приложения в Active Directory

### <a name="active-directory-federation-services"></a>Службы федерации Active Directory

* Открыть сервер **проверяющей стороной мастер добавления отношений доверия** из консоли управления служб федерации Active Directory:

![Добавление отношения доверия с проверяющей стороной мастера: приветствие](ws-federation/_static/AdfsAddTrust.png)

* Выберите ввести данные вручную:

![Мастер добавления проверяющей стороной доверия: Выбор источника данных](ws-federation/_static/AdfsSelectDataSource.png)

* Введите отображаемое имя для проверяющей стороны. Имя не имеет значения для приложения ASP.NET Core.

* [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) не предоставляет поддержку для шифрования маркера, поэтому не настраивайте сертификат шифрования маркера:

![Мастер добавления проверяющей стороной доверия: Настройка сертификата](ws-federation/_static/AdfsConfigureCert.png)

* Включите поддержку протокола WS-Federation Passive, используя URL-адрес приложения. Убедитесь, что порт является правильным для приложения:

![Мастер добавления проверяющей стороной доверия: Настройка URL-адреса](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Это должен быть URL-адрес HTTPS. IIS Express можно предоставить самозаверяющий сертификат при размещении приложения во время разработки. Kestrel требует настройки сертификат вручную. В разделе [Kestrel документации](xref:fundamentals/servers/kestrel) для получения дополнительных сведений.

* Нажмите кнопку **Далее** через остальные инструкции мастера и **закрыть** в конце.

* Требуется ASP.NET Core Identity **идентификатор имени** утверждения. Добавьте один из **изменение правил для утверждений** диалогового окна:

![Изменение правил утверждения](ws-federation/_static/EditClaimRules.png)

* В **преобразования мастера добавления правила утверждения**, оставьте значение по умолчанию **отправлять атрибуты LDAP как утверждения** выбранный шаблон и нажмите кнопку **Далее**. Добавление правила сопоставления **имя учетной записи SAM** атрибут LDAP для **идентификатор имени** исходящего утверждения:

![Мастер добавления преобразования утверждений правил: Настройка правила для утверждений](ws-federation/_static/AddTransformClaimRule.png)

* Нажмите кнопку **Готово** > **ОК** в **изменение правил для утверждений** окна.

### <a name="azure-active-directory"></a>Azure Active Directory

* Перейдите к колонке регистрации клиента AAD приложения. Нажмите кнопку **Регистрация нового приложения**:

![Azure Active Directory: Регистрация приложения](ws-federation/_static/AadNewAppRegistration.png)

* Введите имя для регистрации приложения. Это не имеет значения для приложения ASP.NET Core.
* Введите URL-адрес как приложение прослушивает **URL-адрес входа**:

![Azure Active Directory: Регистрация приложения создайте](ws-federation/_static/AadCreateAppRegistration.png)

* Нажмите кнопку **конечные точки** и обратите внимание, **документа метаданных федерации** URL-адрес. Это по промежуточного слоя WS-Federation `MetadataAddress`:

![Azure Active Directory: конечные точки](ws-federation/_static/AadFederationMetadataDocument.png)

* Перейдите к Регистрация нового приложения. Нажмите кнопку **параметры** > **свойства** и запишите **URI идентификатора приложения**. Это по промежуточного слоя WS-Federation `Wtrealm`:

![Azure Active Directory: Свойства регистрации приложения](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Добавить WS-Federation как поставщик внешней учетной записи для ASP.NET Core Identity

* Добавить зависимость для [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) в проект.
* Добавить WS-Federation для `Configure` метод в *файла Startup.cs*:

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
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

[!INCLUDE[default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Войдите с помощью WS-Federation

Перейдите в приложение и нажмите **входа** ссылку в заголовке панель навигации. Имеется возможность войти с использованием WsFederation: ![страница входа в](ws-federation/_static/WsFederationButton.png)

С помощью служб федерации Active Directory как поставщик кнопки перенаправляет на страницу входа служб федерации Active Directory: ![страницу входа служб федерации Active Directory](ws-federation/_static/AdfsLoginPage.png)

В Azure Active Directory как поставщик, кнопки перенаправляет на страницу входа в AAD: ![страница входа в AAD](ws-federation/_static/AadSignIn.png)

Успешно вход для нового пользователя перенаправляет на страницу регистрации пользователя приложения: ![страница регистрации](ws-federation/_static/Register.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Использование WS-Federation без удостоверения ASP.NET Core

По промежуточного слоя WS-Federation может использоваться без идентификатора. Пример:

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
