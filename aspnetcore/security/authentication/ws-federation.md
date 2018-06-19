---
title: Проверять подлинность пользователей с WS-Federation в ASP.NET Core
author: chlowell
description: Демонстрирует использование WS-Federation в приложении ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/ws-federation
ms.openlocfilehash: d4621c7b97678903b9f2562e353da3883334b599
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898808"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="80f7f-103">Проверять подлинность пользователей с WS-Federation в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="80f7f-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="80f7f-104">Этот учебник демонстрирует включение пользователям входить с использованием поставщика проверки подлинности WS-Federation как службы федерации Active Directory (ADFS) или [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="80f7f-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="80f7f-105">Он использует пример приложения ASP.NET Core 2.0, описанные в [Facebook, Google и внешнего поставщика проверки подлинности](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="80f7f-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="80f7f-106">Для приложений ASP.NET Core 2.0, обеспечивается поддержка WS-Federation [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="80f7f-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="80f7f-107">Этот компонент будет перенесен из [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) и обладает многими механизм соответствующего компонента.</span><span class="sxs-lookup"><span data-stu-id="80f7f-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="80f7f-108">Однако компоненты отличаются в нескольких важных аспектах.</span><span class="sxs-lookup"><span data-stu-id="80f7f-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="80f7f-109">По умолчанию новый по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="80f7f-109">By default, the new middleware:</span></span>

* <span data-ttu-id="80f7f-110">Не Разрешать незапрошенные имена входа.</span><span class="sxs-lookup"><span data-stu-id="80f7f-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="80f7f-111">Этот компонент протокола WS-Federation подвержена XSRF-атак.</span><span class="sxs-lookup"><span data-stu-id="80f7f-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="80f7f-112">Тем не менее, он может быть включен с `AllowUnsolicitedLogins` параметр.</span><span class="sxs-lookup"><span data-stu-id="80f7f-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="80f7f-113">Не выполняется для каждой формы post для входа в систему сообщений.</span><span class="sxs-lookup"><span data-stu-id="80f7f-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="80f7f-114">Только запросы к `CallbackPath` проверяются на наличие знака модули `CallbackPath` по умолчанию используется значение `/signin-wsfed` , но может быть изменено.</span><span class="sxs-lookup"><span data-stu-id="80f7f-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed.</span></span> <span data-ttu-id="80f7f-115">Этот путь можно использовать совместно с других поставщиков проверки подлинности, включив `SkipUnrecognizedRequests` параметр.</span><span class="sxs-lookup"><span data-stu-id="80f7f-115">This path can be shared with other authentication providers by enabling the `SkipUnrecognizedRequests` option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="80f7f-116">Регистрация приложения в Active Directory</span><span class="sxs-lookup"><span data-stu-id="80f7f-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="80f7f-117">Службы федерации Active Directory</span><span class="sxs-lookup"><span data-stu-id="80f7f-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="80f7f-118">Открыть сервер **проверяющей стороной мастер добавления отношений доверия** из консоли управления служб федерации Active Directory:</span><span class="sxs-lookup"><span data-stu-id="80f7f-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Добавление отношения доверия с проверяющей стороной мастера: приветствие](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="80f7f-120">Выберите ввести данные вручную:</span><span class="sxs-lookup"><span data-stu-id="80f7f-120">Choose to enter data manually:</span></span>

![Мастер добавления проверяющей стороной доверия: Выбор источника данных](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="80f7f-122">Введите отображаемое имя для проверяющей стороны.</span><span class="sxs-lookup"><span data-stu-id="80f7f-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="80f7f-123">Имя не имеет значения для приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="80f7f-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="80f7f-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) не предоставляет поддержку для шифрования маркера, поэтому не настраивайте сертификат шифрования маркера:</span><span class="sxs-lookup"><span data-stu-id="80f7f-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Мастер добавления проверяющей стороной доверия: Настройка сертификата](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="80f7f-126">Включите поддержку протокола WS-Federation Passive, используя URL-адрес приложения.</span><span class="sxs-lookup"><span data-stu-id="80f7f-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="80f7f-127">Убедитесь, что порт является правильным для приложения:</span><span class="sxs-lookup"><span data-stu-id="80f7f-127">Verify the port is correct for the app:</span></span>

![Мастер добавления проверяющей стороной доверия: Настройка URL-адреса](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="80f7f-129">Это должен быть URL-адрес HTTPS.</span><span class="sxs-lookup"><span data-stu-id="80f7f-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="80f7f-130">IIS Express можно предоставить самозаверяющий сертификат при размещении приложения во время разработки.</span><span class="sxs-lookup"><span data-stu-id="80f7f-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="80f7f-131">Kestrel требует настройки сертификат вручную.</span><span class="sxs-lookup"><span data-stu-id="80f7f-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="80f7f-132">В разделе [Kestrel документации](xref:fundamentals/servers/kestrel) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="80f7f-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="80f7f-133">Нажмите кнопку **Далее** через остальные инструкции мастера и **закрыть** в конце.</span><span class="sxs-lookup"><span data-stu-id="80f7f-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="80f7f-134">Требуется ASP.NET Core Identity **идентификатор имени** утверждения.</span><span class="sxs-lookup"><span data-stu-id="80f7f-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="80f7f-135">Добавьте один из **изменение правил для утверждений** диалогового окна:</span><span class="sxs-lookup"><span data-stu-id="80f7f-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Изменение правил утверждения](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="80f7f-137">В **преобразования мастера добавления правила утверждения**, оставьте значение по умолчанию **отправлять атрибуты LDAP как утверждения** выбранный шаблон и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="80f7f-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="80f7f-138">Добавление правила сопоставления **имя учетной записи SAM** атрибут LDAP для **идентификатор имени** исходящего утверждения:</span><span class="sxs-lookup"><span data-stu-id="80f7f-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Мастер добавления преобразования утверждений правил: Настройка правила для утверждений](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="80f7f-140">Нажмите кнопку **Готово** > **ОК** в **изменение правил для утверждений** окна.</span><span class="sxs-lookup"><span data-stu-id="80f7f-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="80f7f-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="80f7f-141">Azure Active Directory</span></span>

* <span data-ttu-id="80f7f-142">Перейдите к колонке регистрации клиента AAD приложения.</span><span class="sxs-lookup"><span data-stu-id="80f7f-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="80f7f-143">Нажмите кнопку **Регистрация нового приложения**:</span><span class="sxs-lookup"><span data-stu-id="80f7f-143">Click **New application registration**:</span></span>

![Azure Active Directory: Регистрация приложения](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="80f7f-145">Введите имя для регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="80f7f-145">Enter a name for the app registration.</span></span> <span data-ttu-id="80f7f-146">Это не имеет значения для приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="80f7f-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="80f7f-147">Введите URL-адрес как приложение прослушивает **URL-адрес входа**:</span><span class="sxs-lookup"><span data-stu-id="80f7f-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: Регистрация приложения создайте](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="80f7f-149">Нажмите кнопку **конечные точки** и обратите внимание, **документа метаданных федерации** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="80f7f-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="80f7f-150">Это по промежуточного слоя WS-Federation `MetadataAddress`:</span><span class="sxs-lookup"><span data-stu-id="80f7f-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: конечные точки](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="80f7f-152">Перейдите к Регистрация нового приложения.</span><span class="sxs-lookup"><span data-stu-id="80f7f-152">Navigate to the new app registration.</span></span> <span data-ttu-id="80f7f-153">Нажмите кнопку **параметры** > **свойства** и запишите **URI идентификатора приложения**.</span><span class="sxs-lookup"><span data-stu-id="80f7f-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="80f7f-154">Это по промежуточного слоя WS-Federation `Wtrealm`:</span><span class="sxs-lookup"><span data-stu-id="80f7f-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: Свойства регистрации приложения](ws-federation/_static/AadAppIdUri.png)

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="80f7f-156">Добавить WS-Federation как поставщик внешней учетной записи для ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="80f7f-156">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="80f7f-157">Добавить зависимость для [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) в проект.</span><span class="sxs-lookup"><span data-stu-id="80f7f-157">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="80f7f-158">Добавить WS-Federation для `Configure` метод в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="80f7f-158">Add WS-Federation to the `Configure` method in *Startup.cs*:</span></span>

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

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="80f7f-159">Войдите с помощью WS-Federation</span><span class="sxs-lookup"><span data-stu-id="80f7f-159">Log in with WS-Federation</span></span>

<span data-ttu-id="80f7f-160">Перейдите в приложение и нажмите **входа** ссылку в заголовке панель навигации.</span><span class="sxs-lookup"><span data-stu-id="80f7f-160">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="80f7f-161">Имеется возможность войти с использованием WsFederation: ![страница входа в](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="80f7f-161">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="80f7f-162">С помощью служб федерации Active Directory как поставщик кнопки перенаправляет на страницу входа служб федерации Active Directory: ![страницу входа служб федерации Active Directory](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="80f7f-162">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="80f7f-163">В Azure Active Directory как поставщик, кнопки перенаправляет на страницу входа в AAD: ![страница входа в AAD](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="80f7f-163">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="80f7f-164">Успешно вход для нового пользователя перенаправляет на страницу регистрации пользователя приложения: ![страница регистрации](ws-federation/_static/Register.png)</span><span class="sxs-lookup"><span data-stu-id="80f7f-164">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="80f7f-165">Использование WS-Federation без удостоверения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="80f7f-165">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="80f7f-166">По промежуточного слоя WS-Federation может использоваться без идентификатора.</span><span class="sxs-lookup"><span data-stu-id="80f7f-166">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="80f7f-167">Пример:</span><span class="sxs-lookup"><span data-stu-id="80f7f-167">For example:</span></span>

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
