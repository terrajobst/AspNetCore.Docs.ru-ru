---
title: Использовать проверку подлинности файлов cookie без удостоверения ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать проверку подлинности файлов cookie без удостоверения ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: 62a3d247dade6c83156a8378407d5e3891713fd1
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172119"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="26728-103">Использовать проверку подлинности файлов cookie без удостоверения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26728-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="26728-104">[Рик Андерсон (](https://twitter.com/RickAndMSFT) и [Люк ЛаСаМ](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="26728-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="26728-105">ASP.NET Core удостоверение — это полнофункциональный поставщик проверки подлинности для создания и обслуживания имен входа.</span><span class="sxs-lookup"><span data-stu-id="26728-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="26728-106">Однако можно использовать поставщик проверки подлинности на основе файлов cookie без ASP.NET Core удостоверения.</span><span class="sxs-lookup"><span data-stu-id="26728-106">However, a cookie-based authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="26728-107">Дополнительные сведения см. в разделе <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="26728-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="26728-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="26728-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="26728-109">В демонстрационных целях в примере приложения учетная запись пользователя для гипотетического пользователя, Мария Rodriguez, жестко закодирована в приложении.</span><span class="sxs-lookup"><span data-stu-id="26728-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="26728-110">Используйте адрес **электронной почты** `maria.rodriguez@contoso.com` и любой пароль для входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="26728-110">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="26728-111">Пользователь прошел проверку подлинности в методе `AuthenticateUser` в файле *pages/Account/Login. cshtml. CS* .</span><span class="sxs-lookup"><span data-stu-id="26728-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="26728-112">В реальном примере пользователь будет проходить проверку подлинности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="26728-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="26728-113">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="26728-113">Configuration</span></span>

<span data-ttu-id="26728-114">Если приложение не использует [метапакет Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для пакета [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="26728-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="26728-115">В методе `Startup.ConfigureServices` создайте службы промежуточного слоя проверки подлинности с помощью методов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> и <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:</span><span class="sxs-lookup"><span data-stu-id="26728-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware services with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="26728-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>, передаваемые в `AddAuthentication`, задает схему проверки подлинности по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="26728-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="26728-117">`AuthenticationScheme` полезна при наличии нескольких экземпляров проверки подлинности файлов cookie и необходимости [авторизации с определенной схемой](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="26728-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="26728-118">Установка `AuthenticationScheme` [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) предоставляет значение "cookies" для схемы.</span><span class="sxs-lookup"><span data-stu-id="26728-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="26728-119">Можно указать любое строковое значение, которое различает схему.</span><span class="sxs-lookup"><span data-stu-id="26728-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="26728-120">Схема проверки подлинности приложения отличается от схемы проверки подлинности файлов cookie приложения.</span><span class="sxs-lookup"><span data-stu-id="26728-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="26728-121">Если для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>не указана схема проверки подлинности файлов cookie, она использует `CookieAuthenticationDefaults.AuthenticationScheme` ("cookies").</span><span class="sxs-lookup"><span data-stu-id="26728-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="26728-122">Для свойства <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> файла cookie проверки подлинности по умолчанию установлено значение `true`.</span><span class="sxs-lookup"><span data-stu-id="26728-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="26728-123">Файлы cookie проверки подлинности разрешены, когда посетитель сайта не был передан в сбор данных.</span><span class="sxs-lookup"><span data-stu-id="26728-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="26728-124">Дополнительные сведения см. в разделе <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="26728-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="26728-125">В `Startup.Configure`вызовите метод `UseAuthentication` и `UseAuthorization`, чтобы задать свойство `HttpContext.User` и запустить по промежуточного слоя авторизации для запросов.</span><span class="sxs-lookup"><span data-stu-id="26728-125">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="26728-126">Перед вызовом `UseEndpoints`вызовите методы `UseAuthentication` и `UseAuthorization`:</span><span class="sxs-lookup"><span data-stu-id="26728-126">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="26728-127">Класс <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="26728-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="26728-128">Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в методе `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="26728-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="26728-129">По промежуточного слоя политики файлов cookie</span><span class="sxs-lookup"><span data-stu-id="26728-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="26728-130">По [промежуточного слоя политики файлов cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) включает возможности политики файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="26728-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="26728-131">Добавление по промежуточного слоя к конвейеру обработки приложений выполняется с учетом порядка&mdash;он влияет только на нисходящие компоненты, зарегистрированные в конвейере.</span><span class="sxs-lookup"><span data-stu-id="26728-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="26728-132">Используйте <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>, предоставляемые по промежуточного слоя политики cookie для управления глобальными характеристиками обработки файлов cookie и подключения к обработчикам обработки файлов cookie при добавлении или удалении файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="26728-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="26728-133">Значение <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> по умолчанию — `SameSiteMode.Lax`, чтобы обеспечить проверку подлинности OAuth2.</span><span class="sxs-lookup"><span data-stu-id="26728-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="26728-134">Чтобы строго применить политику того же сайта `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="26728-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="26728-135">Хотя этот параметр нарушает OAuth2 и другие схемы проверки подлинности в разных источниках, он повышает уровень безопасности файлов cookie для других типов приложений, которые не полагаются на обработку запросов между источниками.</span><span class="sxs-lookup"><span data-stu-id="26728-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="26728-136">Параметр по промежуточного слоя политики cookie для `MinimumSameSitePolicy` может влиять на настройку `Cookie.SameSite` в параметрах `CookieAuthenticationOptions` в соответствии с приведенной ниже таблицей.</span><span class="sxs-lookup"><span data-stu-id="26728-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="26728-137">минимумсамеситеполици</span><span class="sxs-lookup"><span data-stu-id="26728-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="26728-138">Файл cookie. SameSite</span><span class="sxs-lookup"><span data-stu-id="26728-138">Cookie.SameSite</span></span> | <span data-ttu-id="26728-139">Результирующий параметр cookie. SameSite</span><span class="sxs-lookup"><span data-stu-id="26728-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="26728-140">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-140">SameSiteMode.None</span></span>     | <span data-ttu-id="26728-141">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-141">SameSiteMode.None</span></span><br><span data-ttu-id="26728-142">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-143">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="26728-144">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-144">SameSiteMode.None</span></span><br><span data-ttu-id="26728-145">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-146">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="26728-147">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="26728-148">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-148">SameSiteMode.None</span></span><br><span data-ttu-id="26728-149">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-150">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="26728-151">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-152">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-153">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="26728-154">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="26728-155">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-155">SameSiteMode.None</span></span><br><span data-ttu-id="26728-156">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-157">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="26728-158">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="26728-159">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="26728-160">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="26728-161">Создание файла cookie для проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="26728-161">Create an authentication cookie</span></span>

<span data-ttu-id="26728-162">Чтобы создать файл cookie, содержащий сведения о пользователе, создайте <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="26728-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="26728-163">Сведения о пользователе сериализуются и хранятся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="26728-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="26728-164">Создайте <xref:System.Security.Claims.ClaimsIdentity> с любыми необходимыми <xref:System.Security.Claims.Claim>s и вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> для входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="26728-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="26728-165">`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ.</span><span class="sxs-lookup"><span data-stu-id="26728-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="26728-166">Если `AuthenticationScheme` не указан, используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="26728-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="26728-167">Для шифрования используется система [защиты данных](xref:security/data-protection/using-data-protection) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="26728-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="26728-168">Для приложения, размещенного на нескольких компьютерах, балансировки нагрузки между приложениями или с помощью веб-фермы, [Настройте защиту данных](xref:security/data-protection/configuration/overview) для использования одного и того же типа звонка и идентификатора приложения.</span><span class="sxs-lookup"><span data-stu-id="26728-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="26728-169">Выход</span><span class="sxs-lookup"><span data-stu-id="26728-169">Sign out</span></span>

<span data-ttu-id="26728-170">Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="26728-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="26728-171">Если `CookieAuthenticationDefaults.AuthenticationScheme` (или "cookie") не используется в качестве схемы (например, "Контосокукие"), укажите схему, используемую при настройке поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="26728-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="26728-172">В противном случае используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="26728-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="26728-173">Реагирование на изменения серверной части</span><span class="sxs-lookup"><span data-stu-id="26728-173">React to back-end changes</span></span>

<span data-ttu-id="26728-174">После создания файла cookie файл cookie является единственным источником удостоверения.</span><span class="sxs-lookup"><span data-stu-id="26728-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="26728-175">Если учетная запись пользователя отключена в серверных системах:</span><span class="sxs-lookup"><span data-stu-id="26728-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="26728-176">Система проверки подлинности файлов cookie приложения продолжит обрабатывать запросы на основе файла cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="26728-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="26728-177">Пользователь остается в приложении, если файл cookie проверки подлинности является допустимым.</span><span class="sxs-lookup"><span data-stu-id="26728-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="26728-178">Событие <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> может использоваться для перехвата и переопределения проверки удостоверения файла cookie.</span><span class="sxs-lookup"><span data-stu-id="26728-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="26728-179">Проверка файла cookie при каждом запросе снижает риск отзыва пользователей, обращающихся к приложению.</span><span class="sxs-lookup"><span data-stu-id="26728-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="26728-180">Один из подходов к проверке файлов cookie основан на отслеживании времени изменения пользовательской базы данных.</span><span class="sxs-lookup"><span data-stu-id="26728-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="26728-181">Если база данных не была изменена с момента выдачи файла cookie пользователя, нет необходимости повторно пройти проверку подлинности пользователя, если его файл cookie остается действительным.</span><span class="sxs-lookup"><span data-stu-id="26728-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="26728-182">В примере приложения база данных реализуется в `IUserRepository` и сохраняет значение `LastChanged`.</span><span class="sxs-lookup"><span data-stu-id="26728-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="26728-183">Когда пользователь обновляется в базе данных, `LastChanged` значение устанавливается в текущее время.</span><span class="sxs-lookup"><span data-stu-id="26728-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="26728-184">Чтобы сделать файл cookie недействительным при изменении базы данных на основе значения `LastChanged`, создайте файл cookie с утверждением `LastChanged`, содержащим текущее значение `LastChanged` из базы данных:</span><span class="sxs-lookup"><span data-stu-id="26728-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

<span data-ttu-id="26728-185">Чтобы реализовать переопределение для события `ValidatePrincipal`, напишите метод со следующей сигнатурой в классе, производном от <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="26728-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="26728-186">Ниже приведен пример реализации `CookieAuthenticationEvents`.</span><span class="sxs-lookup"><span data-stu-id="26728-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="26728-187">Зарегистрируйте экземпляр событий во время регистрации службы файлов cookie в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="26728-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="26728-188">Укажите регистрацию службы с заданной [областью](xref:fundamentals/dependency-injection#service-lifetimes) для класса `CustomCookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="26728-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="26728-189">Рассмотрим ситуацию, в которой имя пользователя обновляется&mdash;принятия решения, которое не влияет на безопасность каким-либо образом.</span><span class="sxs-lookup"><span data-stu-id="26728-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="26728-190">Если вы хотите без разрушения обновить субъект-пользователя, вызовите `context.ReplacePrincipal` и задайте для свойства `context.ShouldRenew` значение `true`.</span><span class="sxs-lookup"><span data-stu-id="26728-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="26728-191">Описанный здесь подход срабатывает при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="26728-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="26728-192">Проверка файлов cookie проверки подлинности для всех пользователей при каждом запросе может привести к значительному снижению производительности приложения.</span><span class="sxs-lookup"><span data-stu-id="26728-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="26728-193">Постоянные файлы cookie</span><span class="sxs-lookup"><span data-stu-id="26728-193">Persistent cookies</span></span>

<span data-ttu-id="26728-194">Возможно, вы хотите, чтобы файл cookie сохранялся в сеансах браузера.</span><span class="sxs-lookup"><span data-stu-id="26728-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="26728-195">Это сохраняемость следует включать только при явном согласии пользователей с флажком "Запомнить меня" при входе или аналогичном механизме.</span><span class="sxs-lookup"><span data-stu-id="26728-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="26728-196">В следующем фрагменте кода создается удостоверение и соответствующий файл cookie, который остается недействительным через замыкания браузера.</span><span class="sxs-lookup"><span data-stu-id="26728-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="26728-197">Учитываются все настроенные ранее параметры срока действия.</span><span class="sxs-lookup"><span data-stu-id="26728-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="26728-198">Если срок действия файла cookie истекает при закрытии браузера, браузер очищает файл cookie после его перезапуска.</span><span class="sxs-lookup"><span data-stu-id="26728-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="26728-199">Задайте для <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> значение `true` в <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="26728-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="26728-200">Абсолютный срок действия файла cookie</span><span class="sxs-lookup"><span data-stu-id="26728-200">Absolute cookie expiration</span></span>

<span data-ttu-id="26728-201">Абсолютный срок действия можно задать с помощью <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="26728-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="26728-202">Чтобы создать постоянный файл cookie, необходимо также задать `IsPersistent`.</span><span class="sxs-lookup"><span data-stu-id="26728-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="26728-203">В противном случае файл cookie создается с временем существования на основе сеанса и может истечь до или после полученного билета проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="26728-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="26728-204">Если параметр `ExpiresUtc` установлен, он переопределяет значение параметра <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> в <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, если оно задано.</span><span class="sxs-lookup"><span data-stu-id="26728-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="26728-205">В следующем фрагменте кода создается удостоверение и соответствующий файл cookie, который длится 20 минут.</span><span class="sxs-lookup"><span data-stu-id="26728-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="26728-206">При этом игнорируются все параметры скользящего срока действия, настроенные ранее.</span><span class="sxs-lookup"><span data-stu-id="26728-206">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="26728-207">ASP.NET Core удостоверение — это полнофункциональный поставщик проверки подлинности для создания и обслуживания имен входа.</span><span class="sxs-lookup"><span data-stu-id="26728-207">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="26728-208">Однако можно использовать поставщик проверки подлинности на основе файлов cookie без ASP.NET Core удостоверения.</span><span class="sxs-lookup"><span data-stu-id="26728-208">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="26728-209">Дополнительные сведения см. в разделе <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="26728-209">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="26728-210">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="26728-210">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="26728-211">В демонстрационных целях в примере приложения учетная запись пользователя для гипотетического пользователя, Мария Rodriguez, жестко закодирована в приложении.</span><span class="sxs-lookup"><span data-stu-id="26728-211">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="26728-212">Используйте адрес **электронной почты** `maria.rodriguez@contoso.com` и любой пароль для входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="26728-212">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="26728-213">Пользователь прошел проверку подлинности в методе `AuthenticateUser` в файле *pages/Account/Login. cshtml. CS* .</span><span class="sxs-lookup"><span data-stu-id="26728-213">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="26728-214">В реальном примере пользователь будет проходить проверку подлинности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="26728-214">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="26728-215">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="26728-215">Configuration</span></span>

<span data-ttu-id="26728-216">Если приложение не использует [метапакет Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для пакета [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .</span><span class="sxs-lookup"><span data-stu-id="26728-216">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="26728-217">В методе `Startup.ConfigureServices` Создайте службу промежуточного слоя проверки подлинности с помощью методов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> и <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:</span><span class="sxs-lookup"><span data-stu-id="26728-217">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="26728-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>, передаваемые в `AddAuthentication`, задает схему проверки подлинности по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="26728-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="26728-219">`AuthenticationScheme` полезна при наличии нескольких экземпляров проверки подлинности файлов cookie и необходимости [авторизации с определенной схемой](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="26728-219">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="26728-220">Установка `AuthenticationScheme` [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) предоставляет значение "cookies" для схемы.</span><span class="sxs-lookup"><span data-stu-id="26728-220">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="26728-221">Можно указать любое строковое значение, которое различает схему.</span><span class="sxs-lookup"><span data-stu-id="26728-221">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="26728-222">Схема проверки подлинности приложения отличается от схемы проверки подлинности файлов cookie приложения.</span><span class="sxs-lookup"><span data-stu-id="26728-222">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="26728-223">Если для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>не указана схема проверки подлинности файлов cookie, она использует `CookieAuthenticationDefaults.AuthenticationScheme` ("cookies").</span><span class="sxs-lookup"><span data-stu-id="26728-223">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="26728-224">Для свойства <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> файла cookie проверки подлинности по умолчанию установлено значение `true`.</span><span class="sxs-lookup"><span data-stu-id="26728-224">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="26728-225">Файлы cookie проверки подлинности разрешены, когда посетитель сайта не был передан в сбор данных.</span><span class="sxs-lookup"><span data-stu-id="26728-225">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="26728-226">Дополнительные сведения см. в разделе <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="26728-226">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="26728-227">В методе `Startup.Configure` вызовите метод `UseAuthentication` для вызова по промежуточного слоя проверки подлинности, устанавливающего свойство `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="26728-227">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="26728-228">Вызовите метод `UseAuthentication` перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="26728-228">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="26728-229">Класс <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="26728-229">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="26728-230">Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в методе `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="26728-230">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="26728-231">По промежуточного слоя политики файлов cookie</span><span class="sxs-lookup"><span data-stu-id="26728-231">Cookie Policy Middleware</span></span>

<span data-ttu-id="26728-232">По [промежуточного слоя политики файлов cookie](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) включает возможности политики файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="26728-232">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="26728-233">Добавление по промежуточного слоя к конвейеру обработки приложений выполняется с учетом порядка&mdash;он влияет только на нисходящие компоненты, зарегистрированные в конвейере.</span><span class="sxs-lookup"><span data-stu-id="26728-233">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="26728-234">Используйте <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>, предоставляемые по промежуточного слоя политики cookie для управления глобальными характеристиками обработки файлов cookie и подключения к обработчикам обработки файлов cookie при добавлении или удалении файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="26728-234">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="26728-235">Значение <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> по умолчанию — `SameSiteMode.Lax`, чтобы обеспечить проверку подлинности OAuth2.</span><span class="sxs-lookup"><span data-stu-id="26728-235">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="26728-236">Чтобы строго применить политику того же сайта `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="26728-236">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="26728-237">Хотя этот параметр нарушает OAuth2 и другие схемы проверки подлинности в разных источниках, он повышает уровень безопасности файлов cookie для других типов приложений, которые не полагаются на обработку запросов между источниками.</span><span class="sxs-lookup"><span data-stu-id="26728-237">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="26728-238">Параметр по промежуточного слоя политики cookie для `MinimumSameSitePolicy` может влиять на настройку `Cookie.SameSite` в параметрах `CookieAuthenticationOptions` в соответствии с приведенной ниже таблицей.</span><span class="sxs-lookup"><span data-stu-id="26728-238">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="26728-239">минимумсамеситеполици</span><span class="sxs-lookup"><span data-stu-id="26728-239">MinimumSameSitePolicy</span></span> | <span data-ttu-id="26728-240">Файл cookie. SameSite</span><span class="sxs-lookup"><span data-stu-id="26728-240">Cookie.SameSite</span></span> | <span data-ttu-id="26728-241">Результирующий параметр cookie. SameSite</span><span class="sxs-lookup"><span data-stu-id="26728-241">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="26728-242">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-242">SameSiteMode.None</span></span>     | <span data-ttu-id="26728-243">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-243">SameSiteMode.None</span></span><br><span data-ttu-id="26728-244">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-244">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-245">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-245">SameSiteMode.Strict</span></span> | <span data-ttu-id="26728-246">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-246">SameSiteMode.None</span></span><br><span data-ttu-id="26728-247">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-247">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-248">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-248">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="26728-249">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-249">SameSiteMode.Lax</span></span>      | <span data-ttu-id="26728-250">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-250">SameSiteMode.None</span></span><br><span data-ttu-id="26728-251">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-251">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-252">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-252">SameSiteMode.Strict</span></span> | <span data-ttu-id="26728-253">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-253">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-254">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-254">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-255">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-255">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="26728-256">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-256">SameSiteMode.Strict</span></span>   | <span data-ttu-id="26728-257">Самеситемоде. None</span><span class="sxs-lookup"><span data-stu-id="26728-257">SameSiteMode.None</span></span><br><span data-ttu-id="26728-258">Самеситемоде. нестрогий</span><span class="sxs-lookup"><span data-stu-id="26728-258">SameSiteMode.Lax</span></span><br><span data-ttu-id="26728-259">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-259">SameSiteMode.Strict</span></span> | <span data-ttu-id="26728-260">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-260">SameSiteMode.Strict</span></span><br><span data-ttu-id="26728-261">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-261">SameSiteMode.Strict</span></span><br><span data-ttu-id="26728-262">Самеситемоде.</span><span class="sxs-lookup"><span data-stu-id="26728-262">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="26728-263">Создание файла cookie для проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="26728-263">Create an authentication cookie</span></span>

<span data-ttu-id="26728-264">Чтобы создать файл cookie, содержащий сведения о пользователе, создайте <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="26728-264">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="26728-265">Сведения о пользователе сериализуются и хранятся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="26728-265">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="26728-266">Создайте <xref:System.Security.Claims.ClaimsIdentity> с любыми необходимыми <xref:System.Security.Claims.Claim>s и вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> для входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="26728-266">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="26728-267">`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ.</span><span class="sxs-lookup"><span data-stu-id="26728-267">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="26728-268">Если `AuthenticationScheme` не указан, используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="26728-268">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="26728-269">Для шифрования используется система [защиты данных](xref:security/data-protection/using-data-protection) ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="26728-269">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="26728-270">Для приложения, размещенного на нескольких компьютерах, балансировки нагрузки между приложениями или с помощью веб-фермы, [Настройте защиту данных](xref:security/data-protection/configuration/overview) для использования одного и того же типа звонка и идентификатора приложения.</span><span class="sxs-lookup"><span data-stu-id="26728-270">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="26728-271">Выход</span><span class="sxs-lookup"><span data-stu-id="26728-271">Sign out</span></span>

<span data-ttu-id="26728-272">Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="26728-272">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="26728-273">Если `CookieAuthenticationDefaults.AuthenticationScheme` (или "cookie") не используется в качестве схемы (например, "Контосокукие"), укажите схему, используемую при настройке поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="26728-273">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="26728-274">В противном случае используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="26728-274">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="26728-275">Реагирование на изменения серверной части</span><span class="sxs-lookup"><span data-stu-id="26728-275">React to back-end changes</span></span>

<span data-ttu-id="26728-276">После создания файла cookie файл cookie является единственным источником удостоверения.</span><span class="sxs-lookup"><span data-stu-id="26728-276">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="26728-277">Если учетная запись пользователя отключена в серверных системах:</span><span class="sxs-lookup"><span data-stu-id="26728-277">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="26728-278">Система проверки подлинности файлов cookie приложения продолжит обрабатывать запросы на основе файла cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="26728-278">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="26728-279">Пользователь остается в приложении, если файл cookie проверки подлинности является допустимым.</span><span class="sxs-lookup"><span data-stu-id="26728-279">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="26728-280">Событие <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> может использоваться для перехвата и переопределения проверки удостоверения файла cookie.</span><span class="sxs-lookup"><span data-stu-id="26728-280">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="26728-281">Проверка файла cookie при каждом запросе снижает риск отзыва пользователей, обращающихся к приложению.</span><span class="sxs-lookup"><span data-stu-id="26728-281">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="26728-282">Один из подходов к проверке файлов cookie основан на отслеживании времени изменения пользовательской базы данных.</span><span class="sxs-lookup"><span data-stu-id="26728-282">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="26728-283">Если база данных не была изменена с момента выдачи файла cookie пользователя, нет необходимости повторно пройти проверку подлинности пользователя, если его файл cookie остается действительным.</span><span class="sxs-lookup"><span data-stu-id="26728-283">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="26728-284">В примере приложения база данных реализуется в `IUserRepository` и сохраняет значение `LastChanged`.</span><span class="sxs-lookup"><span data-stu-id="26728-284">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="26728-285">Когда пользователь обновляется в базе данных, `LastChanged` значение устанавливается в текущее время.</span><span class="sxs-lookup"><span data-stu-id="26728-285">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="26728-286">Чтобы сделать файл cookie недействительным при изменении базы данных на основе значения `LastChanged`, создайте файл cookie с утверждением `LastChanged`, содержащим текущее значение `LastChanged` из базы данных:</span><span class="sxs-lookup"><span data-stu-id="26728-286">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

<span data-ttu-id="26728-287">Чтобы реализовать переопределение для события `ValidatePrincipal`, напишите метод со следующей сигнатурой в классе, производном от <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="26728-287">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="26728-288">Ниже приведен пример реализации `CookieAuthenticationEvents`.</span><span class="sxs-lookup"><span data-stu-id="26728-288">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="26728-289">Зарегистрируйте экземпляр событий во время регистрации службы файлов cookie в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="26728-289">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="26728-290">Укажите регистрацию службы с заданной [областью](xref:fundamentals/dependency-injection#service-lifetimes) для класса `CustomCookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="26728-290">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="26728-291">Рассмотрим ситуацию, в которой имя пользователя обновляется&mdash;принятия решения, которое не влияет на безопасность каким-либо образом.</span><span class="sxs-lookup"><span data-stu-id="26728-291">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="26728-292">Если вы хотите без разрушения обновить субъект-пользователя, вызовите `context.ReplacePrincipal` и задайте для свойства `context.ShouldRenew` значение `true`.</span><span class="sxs-lookup"><span data-stu-id="26728-292">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="26728-293">Описанный здесь подход срабатывает при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="26728-293">The approach described here is triggered on every request.</span></span> <span data-ttu-id="26728-294">Проверка файлов cookie проверки подлинности для всех пользователей при каждом запросе может привести к значительному снижению производительности приложения.</span><span class="sxs-lookup"><span data-stu-id="26728-294">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="26728-295">Постоянные файлы cookie</span><span class="sxs-lookup"><span data-stu-id="26728-295">Persistent cookies</span></span>

<span data-ttu-id="26728-296">Возможно, вы хотите, чтобы файл cookie сохранялся в сеансах браузера.</span><span class="sxs-lookup"><span data-stu-id="26728-296">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="26728-297">Это сохраняемость следует включать только при явном согласии пользователей с флажком "Запомнить меня" при входе или аналогичном механизме.</span><span class="sxs-lookup"><span data-stu-id="26728-297">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="26728-298">В следующем фрагменте кода создается удостоверение и соответствующий файл cookie, который остается недействительным через замыкания браузера.</span><span class="sxs-lookup"><span data-stu-id="26728-298">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="26728-299">Учитываются все настроенные ранее параметры срока действия.</span><span class="sxs-lookup"><span data-stu-id="26728-299">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="26728-300">Если срок действия файла cookie истекает при закрытии браузера, браузер очищает файл cookie после его перезапуска.</span><span class="sxs-lookup"><span data-stu-id="26728-300">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="26728-301">Задайте для <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> значение `true` в <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="26728-301">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="26728-302">Абсолютный срок действия файла cookie</span><span class="sxs-lookup"><span data-stu-id="26728-302">Absolute cookie expiration</span></span>

<span data-ttu-id="26728-303">Абсолютный срок действия можно задать с помощью <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="26728-303">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="26728-304">Чтобы создать постоянный файл cookie, необходимо также задать `IsPersistent`.</span><span class="sxs-lookup"><span data-stu-id="26728-304">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="26728-305">В противном случае файл cookie создается с временем существования на основе сеанса и может истечь до или после полученного билета проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="26728-305">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="26728-306">Если параметр `ExpiresUtc` установлен, он переопределяет значение параметра <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> в <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, если оно задано.</span><span class="sxs-lookup"><span data-stu-id="26728-306">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="26728-307">В следующем фрагменте кода создается удостоверение и соответствующий файл cookie, который длится 20 минут.</span><span class="sxs-lookup"><span data-stu-id="26728-307">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="26728-308">При этом игнорируются все параметры скользящего срока действия, настроенные ранее.</span><span class="sxs-lookup"><span data-stu-id="26728-308">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="26728-309">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="26728-309">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="26728-310">Проверки ролей на основе политик</span><span class="sxs-lookup"><span data-stu-id="26728-310">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
