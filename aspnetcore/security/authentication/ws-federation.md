---
title: Проверка подлинности пользователей с помощью WS-Federation в ASP.NET Core
author: chlowell
description: В этом руководстве показано, как использовать WS-Federation в приложении ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: d82421a14ede6cb6b01ef59f233bb2eba6b56aec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651334"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a><span data-ttu-id="6adc0-103">Проверка подлинности пользователей с помощью WS-Federation в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6adc0-103">Authenticate users with WS-Federation in ASP.NET Core</span></span>

<span data-ttu-id="6adc0-104">В этом руководстве показано, как разрешить пользователям выполнять вход с помощью поставщика проверки подлинности WS-Federation, например службы федерации Active Directory (AD FS) (ADFS) или [Azure Active Directory](/azure/active-directory/) (AAD).</span><span class="sxs-lookup"><span data-stu-id="6adc0-104">This tutorial demonstrates how to enable users to sign in with a WS-Federation authentication provider like Active Directory Federation Services (ADFS) or [Azure Active Directory](/azure/active-directory/) (AAD).</span></span> <span data-ttu-id="6adc0-105">В нем используется пример приложения ASP.NET Core 2,0, описанный в статье [Аутентификация Facebook, Google и внешнего поставщика](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="6adc0-105">It uses the ASP.NET Core 2.0 sample app described in [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="6adc0-106">Для приложений ASP.NET Core 2,0 поддержка WS-Federation обеспечивается [Microsoft. AspNetCore. Authentication. WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span><span class="sxs-lookup"><span data-stu-id="6adc0-106">For ASP.NET Core 2.0 apps, WS-Federation support is provided by [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation).</span></span> <span data-ttu-id="6adc0-107">Этот компонент переносится из [Microsoft. Owin. Security. WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) и разделяет многие механики этого компонента.</span><span class="sxs-lookup"><span data-stu-id="6adc0-107">This component is ported from [Microsoft.Owin.Security.WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) and shares many of that component's mechanics.</span></span> <span data-ttu-id="6adc0-108">Однако эти компоненты отличаются друг от друга несколькими важными способами.</span><span class="sxs-lookup"><span data-stu-id="6adc0-108">However, the components differ in a couple of important ways.</span></span>

<span data-ttu-id="6adc0-109">По умолчанию новое по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="6adc0-109">By default, the new middleware:</span></span>

* <span data-ttu-id="6adc0-110">Не разрешает незапрошенные имена входа.</span><span class="sxs-lookup"><span data-stu-id="6adc0-110">Doesn't allow unsolicited logins.</span></span> <span data-ttu-id="6adc0-111">Эта функция протокола WS-Federation уязвима для атак XSRF.</span><span class="sxs-lookup"><span data-stu-id="6adc0-111">This feature of the WS-Federation protocol is vulnerable to XSRF attacks.</span></span> <span data-ttu-id="6adc0-112">Однако его можно включить с помощью параметра `AllowUnsolicitedLogins`.</span><span class="sxs-lookup"><span data-stu-id="6adc0-112">However, it can be enabled with the `AllowUnsolicitedLogins` option.</span></span>
* <span data-ttu-id="6adc0-113">Не проверяет каждую форму POST для сообщений входа.</span><span class="sxs-lookup"><span data-stu-id="6adc0-113">Doesn't check every form post for sign-in messages.</span></span> <span data-ttu-id="6adc0-114">Для входа проверяются только запросы к `CallbackPath`. `CallbackPath` по умолчанию имеет значение `/signin-wsfed` но может быть изменено с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [всфедератионоптионс](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) .</span><span class="sxs-lookup"><span data-stu-id="6adc0-114">Only requests to the `CallbackPath` are checked for sign-ins. `CallbackPath` defaults to `/signin-wsfed` but can be changed via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [WsFederationOptions](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) class.</span></span> <span data-ttu-id="6adc0-115">Этот путь можно использовать совместно с другими поставщиками проверки подлинности, включив параметр [скипунрекогнизедрекуестс](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) .</span><span class="sxs-lookup"><span data-stu-id="6adc0-115">This path can be shared with other authentication providers by enabling the [SkipUnrecognizedRequests](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) option.</span></span>

## <a name="register-the-app-with-active-directory"></a><span data-ttu-id="6adc0-116">Регистрация приложения в Active Directory</span><span class="sxs-lookup"><span data-stu-id="6adc0-116">Register the app with Active Directory</span></span>

### <a name="active-directory-federation-services"></a><span data-ttu-id="6adc0-117">службы федерации Active Directory;</span><span class="sxs-lookup"><span data-stu-id="6adc0-117">Active Directory Federation Services</span></span>

* <span data-ttu-id="6adc0-118">Откройте **Мастер добавления отношения доверия с проверяющей стороной** на сервере из консоли управления ADFS:</span><span class="sxs-lookup"><span data-stu-id="6adc0-118">Open the server's **Add Relying Party Trust Wizard** from the ADFS Management console:</span></span>

![Мастер добавления отношения доверия с проверяющей стороной: Добро пожаловать](ws-federation/_static/AdfsAddTrust.png)

* <span data-ttu-id="6adc0-120">Выберите Ввод данных вручную:</span><span class="sxs-lookup"><span data-stu-id="6adc0-120">Choose to enter data manually:</span></span>

![Мастер добавления отношения доверия с проверяющей стороной: Выбор источника данных](ws-federation/_static/AdfsSelectDataSource.png)

* <span data-ttu-id="6adc0-122">Введите отображаемое имя проверяющей стороны.</span><span class="sxs-lookup"><span data-stu-id="6adc0-122">Enter a display name for the relying party.</span></span> <span data-ttu-id="6adc0-123">Это имя не имеет значения для приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6adc0-123">The name isn't important to the ASP.NET Core app.</span></span>

* <span data-ttu-id="6adc0-124">В [Microsoft. AspNetCore. Authentication. WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) отсутствует поддержка шифрования маркеров, поэтому не настраивайте сертификат шифрования маркеров:</span><span class="sxs-lookup"><span data-stu-id="6adc0-124">[Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) lacks support for token encryption, so don't configure a token encryption certificate:</span></span>

![Мастер добавления отношения доверия с проверяющей стороной: Настройка сертификата](ws-federation/_static/AdfsConfigureCert.png)

* <span data-ttu-id="6adc0-126">Включите поддержку пассивного протокола WS-Federation с помощью URL-адреса приложения.</span><span class="sxs-lookup"><span data-stu-id="6adc0-126">Enable support for WS-Federation Passive protocol, using the app's URL.</span></span> <span data-ttu-id="6adc0-127">Проверьте правильность порта для приложения:</span><span class="sxs-lookup"><span data-stu-id="6adc0-127">Verify the port is correct for the app:</span></span>

![Мастер добавления отношения доверия с проверяющей стороной: Настройка URL-адреса](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> <span data-ttu-id="6adc0-129">Это должен быть URL-адрес HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6adc0-129">This must be an HTTPS URL.</span></span> <span data-ttu-id="6adc0-130">IIS Express может предоставить самозаверяющий сертификат при размещении приложения во время разработки.</span><span class="sxs-lookup"><span data-stu-id="6adc0-130">IIS Express can provide a self-signed certificate when hosting the app during development.</span></span> <span data-ttu-id="6adc0-131">Для Kestrel требуется ручная настройка сертификата.</span><span class="sxs-lookup"><span data-stu-id="6adc0-131">Kestrel requires manual certificate configuration.</span></span> <span data-ttu-id="6adc0-132">Дополнительные сведения см. в [документации по Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="6adc0-132">See the [Kestrel documentation](xref:fundamentals/servers/kestrel) for more details.</span></span>

* <span data-ttu-id="6adc0-133">Нажмите кнопку **Далее** в мастере и **закройте** в конце.</span><span class="sxs-lookup"><span data-stu-id="6adc0-133">Click **Next** through the rest of the wizard and **Close** at the end.</span></span>

* <span data-ttu-id="6adc0-134">Для удостоверения ASP.NET Core требуется утверждение **идентификатора имени** .</span><span class="sxs-lookup"><span data-stu-id="6adc0-134">ASP.NET Core Identity requires a **Name ID** claim.</span></span> <span data-ttu-id="6adc0-135">Добавьте его в диалоговом окне **изменение правил утверждений** :</span><span class="sxs-lookup"><span data-stu-id="6adc0-135">Add one from the **Edit Claim Rules** dialog:</span></span>

![Изменение правил утверждения](ws-federation/_static/EditClaimRules.png)

* <span data-ttu-id="6adc0-137">В **мастере добавления правила преобразования утверждений**Оставьте выбранный по умолчанию шаблон **отправки атрибутов LDAP в качестве шаблона утверждений** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="6adc0-137">In the **Add Transform Claim Rule Wizard**, leave the default **Send LDAP Attributes as Claims** template selected, and click **Next**.</span></span> <span data-ttu-id="6adc0-138">Добавьте правило, сопоставленное с атрибутом **SAM-Account-Name** LDAP к исходящему утверждению **ID** :</span><span class="sxs-lookup"><span data-stu-id="6adc0-138">Add a rule mapping the **SAM-Account-Name** LDAP attribute to the **Name ID** outgoing claim:</span></span>

![Мастер добавления правила преобразования утверждений: Настройка правила для утверждений](ws-federation/_static/AddTransformClaimRule.png)

* <span data-ttu-id="6adc0-140">Нажмите кнопку **готово** > **ОК** в окне **изменение правил утверждений** .</span><span class="sxs-lookup"><span data-stu-id="6adc0-140">Click **Finish** > **OK** in the **Edit Claim Rules** window.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="6adc0-141">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6adc0-141">Azure Active Directory</span></span>

* <span data-ttu-id="6adc0-142">Перейдите в колонку регистрации приложений клиента AAD.</span><span class="sxs-lookup"><span data-stu-id="6adc0-142">Navigate to the AAD tenant's app registrations blade.</span></span> <span data-ttu-id="6adc0-143">Щелкните **Регистрация нового приложения**:</span><span class="sxs-lookup"><span data-stu-id="6adc0-143">Click **New application registration**:</span></span>

![Azure Active Directory: Регистрация приложений](ws-federation/_static/AadNewAppRegistration.png)

* <span data-ttu-id="6adc0-145">Введите имя для регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="6adc0-145">Enter a name for the app registration.</span></span> <span data-ttu-id="6adc0-146">Это не имеет значения для ASP.NET Core приложения.</span><span class="sxs-lookup"><span data-stu-id="6adc0-146">This isn't important to the ASP.NET Core app.</span></span>
* <span data-ttu-id="6adc0-147">Введите URL-адрес, который прослушивает приложение в качестве **URL-адреса входа**:</span><span class="sxs-lookup"><span data-stu-id="6adc0-147">Enter the URL the app listens on as the **Sign-on URL**:</span></span>

![Azure Active Directory: создание регистрации приложения](ws-federation/_static/AadCreateAppRegistration.png)

* <span data-ttu-id="6adc0-149">Щелкните **конечные точки** и запишите URL-адрес **документа метаданных федерации** .</span><span class="sxs-lookup"><span data-stu-id="6adc0-149">Click **Endpoints** and note the **Federation Metadata Document** URL.</span></span> <span data-ttu-id="6adc0-150">Это `MetadataAddress`по промежуточного слоя WS-Federation:</span><span class="sxs-lookup"><span data-stu-id="6adc0-150">This is the WS-Federation middleware's `MetadataAddress`:</span></span>

![Azure Active Directory: конечные точки](ws-federation/_static/AadFederationMetadataDocument.png)

* <span data-ttu-id="6adc0-152">Перейдите к новой регистрации приложения.</span><span class="sxs-lookup"><span data-stu-id="6adc0-152">Navigate to the new app registration.</span></span> <span data-ttu-id="6adc0-153">Щелкните **параметры** > **Свойства** и запишите **URI идентификатора приложения**.</span><span class="sxs-lookup"><span data-stu-id="6adc0-153">Click **Settings** > **Properties** and make note of the **App ID URI**.</span></span> <span data-ttu-id="6adc0-154">Это `Wtrealm`по промежуточного слоя WS-Federation:</span><span class="sxs-lookup"><span data-stu-id="6adc0-154">This is the WS-Federation middleware's `Wtrealm`:</span></span>

![Azure Active Directory: свойства регистрации приложения](ws-federation/_static/AadAppIdUri.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a><span data-ttu-id="6adc0-156">Использовать WS-Federation без удостоверения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6adc0-156">Use WS-Federation without ASP.NET Core Identity</span></span>

<span data-ttu-id="6adc0-157">По промежуточного слоя WS-Federation можно использовать без удостоверения.</span><span class="sxs-lookup"><span data-stu-id="6adc0-157">The WS-Federation middleware can be used without Identity.</span></span> <span data-ttu-id="6adc0-158">Пример:</span><span class="sxs-lookup"><span data-stu-id="6adc0-158">For example:</span></span>
::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon21.cs?name=snippet)]
::: moniker-end

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a><span data-ttu-id="6adc0-159">Добавление WS-Federation в качестве внешнего поставщика входа для ASP.NET Core удостоверения</span><span class="sxs-lookup"><span data-stu-id="6adc0-159">Add WS-Federation as an external login provider for ASP.NET Core Identity</span></span>

* <span data-ttu-id="6adc0-160">Добавьте зависимость от [Microsoft. AspNetCore. Authentication. WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) в проект.</span><span class="sxs-lookup"><span data-stu-id="6adc0-160">Add a dependency on [Microsoft.AspNetCore.Authentication.WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) to the project.</span></span>
* <span data-ttu-id="6adc0-161">Добавление WS-Federation в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6adc0-161">Add WS-Federation to `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup21.cs?name=snippet)]
::: moniker-end

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a><span data-ttu-id="6adc0-162">Вход с помощью WS-Federation</span><span class="sxs-lookup"><span data-stu-id="6adc0-162">Log in with WS-Federation</span></span>

<span data-ttu-id="6adc0-163">Перейдите к приложению и щелкните ссылку **Вход** в заголовке навигации.</span><span class="sxs-lookup"><span data-stu-id="6adc0-163">Browse to the app and click the **Log in** link in the nav header.</span></span> <span data-ttu-id="6adc0-164">Есть возможность войти в систему с помощью WsFederation: ![войти на страницу](ws-federation/_static/WsFederationButton.png)</span><span class="sxs-lookup"><span data-stu-id="6adc0-164">There's an option to log in with WsFederation: ![Log in page](ws-federation/_static/WsFederationButton.png)</span></span>

<span data-ttu-id="6adc0-165">При использовании ADFS в качестве поставщика кнопка перенаправляется на страницу входа в ADFS: ![страницу входа ADFS](ws-federation/_static/AdfsLoginPage.png)</span><span class="sxs-lookup"><span data-stu-id="6adc0-165">With ADFS as the provider, the button redirects to an ADFS sign-in page: ![ADFS sign-in page](ws-federation/_static/AdfsLoginPage.png)</span></span>

<span data-ttu-id="6adc0-166">При Azure Active Directory в качестве поставщика кнопка перенаправляется на страницу входа AAD: ![страницу входа AAD](ws-federation/_static/AadSignIn.png)</span><span class="sxs-lookup"><span data-stu-id="6adc0-166">With Azure Active Directory as the provider, the button redirects to an AAD sign-in page: ![AAD sign-in page](ws-federation/_static/AadSignIn.png)</span></span>

<span data-ttu-id="6adc0-167">Успешный вход нового пользователя приводит к перенаправлению на страницу регистрации пользователей приложения: ![](ws-federation/_static/Register.png) страницы регистрации</span><span class="sxs-lookup"><span data-stu-id="6adc0-167">A successful sign-in for a new user redirects to the app's user registration page: ![Register page](ws-federation/_static/Register.png)</span></span>