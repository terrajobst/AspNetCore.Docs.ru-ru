---
title: Использовать проверку подлинности файлов cookie без ASP.NET Core Identity
author: rick-anderson
description: Узнайте, как использовать проверку подлинности файлов cookie без ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622746"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="6d382-103">Использовать проверку подлинности файлов cookie без ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="6d382-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="6d382-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT) и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6d382-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6d382-105">Удостоверение ASP.NET Core — это поставщик проверки подлинности завершено, полнофункциональную для создания и обслуживания имена входа.</span><span class="sxs-lookup"><span data-stu-id="6d382-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="6d382-106">Тем не менее можно использовать проверку подлинности поставщик проверки подлинности на основе файлов cookie без ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="6d382-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="6d382-107">Дополнительные сведения см. в разделе <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="6d382-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="6d382-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6d382-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6d382-109">Для демонстрационных целей в примере приложения учетной записи пользователя для гипотетической пользователя Родригез Мария встроен в приложение.</span><span class="sxs-lookup"><span data-stu-id="6d382-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="6d382-110">Используйте **электронной почты** имя пользователя `maria.rodriguez@contoso.com` и любой пароль для входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="6d382-110">Use the **Email** user name `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="6d382-111">Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="6d382-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="6d382-112">В реальный пример пользователь может пройти проверку подлинности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6d382-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="6d382-113">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="6d382-113">Configuration</span></span>

<span data-ttu-id="6d382-114">Если приложение не использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), создайте ссылку на пакет в файле проекта для [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) пакета.</span><span class="sxs-lookup"><span data-stu-id="6d382-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="6d382-115">В `Startup.ConfigureServices` метод, создайте службу проверки подлинности по промежуточного слоя с <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> и <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> методов:</span><span class="sxs-lookup"><span data-stu-id="6d382-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="6d382-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> передаваемый `AddAuthentication` задает схему проверки подлинности по умолчанию для приложения.</span><span class="sxs-lookup"><span data-stu-id="6d382-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="6d382-117">`AuthenticationScheme` полезно, если имеется несколько экземпляров файла cookie проверки подлинности, и вы хотите [авторизация с определенной схемой](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="6d382-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="6d382-118">Установка `AuthenticationScheme` для [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) предоставляет значение файлов «cookie» для схемы.</span><span class="sxs-lookup"><span data-stu-id="6d382-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="6d382-119">Вы можете указать любое строковое значение, являющийся отличительным признаком схемы.</span><span class="sxs-lookup"><span data-stu-id="6d382-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="6d382-120">Схема проверки подлинности приложения отличается от схемы проверки подлинности файла cookie для приложения.</span><span class="sxs-lookup"><span data-stu-id="6d382-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="6d382-121">Когда схему проверки подлинности файла cookie не предоставляется для <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, он использует `CookieAuthenticationDefaults.AuthenticationScheme` («файлы cookie»).</span><span class="sxs-lookup"><span data-stu-id="6d382-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="6d382-122">Файл cookie проверки подлинности <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> свойству `true` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6d382-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="6d382-123">Файлы cookie проверки подлинности разрешены, если посетитель не означает согласие на сбор данных.</span><span class="sxs-lookup"><span data-stu-id="6d382-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="6d382-124">Дополнительные сведения см. в разделе <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="6d382-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="6d382-125">В `Startup.Configure` мы вызываем метод `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, который задает `HttpContext.User` свойства.</span><span class="sxs-lookup"><span data-stu-id="6d382-125">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="6d382-126">Вызовите `UseAuthentication` метод перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="6d382-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="6d382-127"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Класс используется для настройки параметров поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="6d382-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="6d382-128">Задайте `CookieAuthenticationOptions` в конфигурации службы для проверки подлинности в `Startup.ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="6d382-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="6d382-129">По промежуточного слоя для файлов cookie политики</span><span class="sxs-lookup"><span data-stu-id="6d382-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="6d382-130">[По промежуточного слоя для файлов cookie политики](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) включает возможности политики файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="6d382-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="6d382-131">Добавление по промежуточного слоя в конвейер обработки приложений является порядок конфиденциальных&mdash;оно влияет только на нижестоящим компонентам, зарегистрированному в конвейере.</span><span class="sxs-lookup"><span data-stu-id="6d382-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="6d382-132">Используйте <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> для промежуточного слоя политики для управления общих характеристик обработки файлов cookie и обработчик в обработчики обработки файлов cookie, если файлы cookie добавляется или удаляется.</span><span class="sxs-lookup"><span data-stu-id="6d382-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="6d382-133">Значение по умолчанию <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> значение `SameSiteMode.Lax` для разрешения проверки подлинности OAuth2.</span><span class="sxs-lookup"><span data-stu-id="6d382-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="6d382-134">Для строго ограничивает политику веб-сайте `SameSiteMode.Strict`, задайте `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="6d382-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="6d382-135">Несмотря на то, что этот параметр приводит к сбою OAuth2 и других схем проверки подлинности независимо от источника, оно повышает уровень безопасности cookie для других типов приложений, которые не полагаются на обработку независимо от источника запроса.</span><span class="sxs-lookup"><span data-stu-id="6d382-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="6d382-136">Параметр политики промежуточного слоя для `MinimumSameSitePolicy` может повлиять на значение `Cookie.SameSite` в `CookieAuthenticationOptions` параметры в соответствии с в таблице ниже.</span><span class="sxs-lookup"><span data-stu-id="6d382-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="6d382-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="6d382-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="6d382-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="6d382-138">Cookie.SameSite</span></span> | <span data-ttu-id="6d382-139">Результирующий параметр Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="6d382-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="6d382-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6d382-140">SameSiteMode.None</span></span>     | <span data-ttu-id="6d382-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6d382-141">SameSiteMode.None</span></span><br><span data-ttu-id="6d382-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6d382-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="6d382-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6d382-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="6d382-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6d382-144">SameSiteMode.None</span></span><br><span data-ttu-id="6d382-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6d382-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="6d382-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6d382-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="6d382-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6d382-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="6d382-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6d382-148">SameSiteMode.None</span></span><br><span data-ttu-id="6d382-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6d382-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="6d382-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6d382-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="6d382-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6d382-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="6d382-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6d382-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="6d382-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6d382-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="6d382-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6d382-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="6d382-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="6d382-155">SameSiteMode.None</span></span><br><span data-ttu-id="6d382-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="6d382-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="6d382-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6d382-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="6d382-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6d382-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="6d382-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6d382-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="6d382-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="6d382-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="6d382-161">Создайте файл cookie проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="6d382-161">Create an authentication cookie</span></span>

<span data-ttu-id="6d382-162">Чтобы создать файл cookie, содержащий сведения о пользователе, сконструировать <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="6d382-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="6d382-163">Сведения о пользователе сериализуется и сохраняется в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="6d382-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="6d382-164">Создание <xref:System.Security.Claims.ClaimsIdentity> с любыми обязательными <xref:System.Security.Claims.Claim>s и вызвать метод <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> для входа пользователя:</span><span class="sxs-lookup"><span data-stu-id="6d382-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="6d382-165">`SignInAsync` создает зашифрованный файл cookie и добавляет его в текущий ответ.</span><span class="sxs-lookup"><span data-stu-id="6d382-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="6d382-166">Если `AuthenticationScheme` не указано, используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6d382-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="6d382-167">ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection) системы используется для шифрования.</span><span class="sxs-lookup"><span data-stu-id="6d382-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="6d382-168">Для приложения, размещенного на нескольких компьютерах, загрузить балансировку между приложениями, или с помощью веб-фермы, [настроить защиту данных](xref:security/data-protection/configuration/overview) использование одного набора ключей и идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="6d382-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="6d382-169">Выйти</span><span class="sxs-lookup"><span data-stu-id="6d382-169">Sign out</span></span>

<span data-ttu-id="6d382-170">Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="6d382-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="6d382-171">Если `CookieAuthenticationDefaults.AuthenticationScheme` (или файлы «cookie») не используется как схемы (например, «ContosoCookie»), укажите схему, используемую при настройке поставщика проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="6d382-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="6d382-172">В противном случае используется схема по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6d382-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="6d382-173">Реагировать на изменения серверной части</span><span class="sxs-lookup"><span data-stu-id="6d382-173">React to back-end changes</span></span>

<span data-ttu-id="6d382-174">Создав файл cookie, файл cookie является единственным источником удостоверения.</span><span class="sxs-lookup"><span data-stu-id="6d382-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="6d382-175">Если учетная запись пользователя отключена в обслуживающих систем:</span><span class="sxs-lookup"><span data-stu-id="6d382-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="6d382-176">В системе приложения-файла cookie проверки подлинности продолжает обрабатывать запросы на основании файла cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="6d382-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="6d382-177">Пользователь остается в приложение до тех пор, пока файл cookie проверки подлинности является допустимым.</span><span class="sxs-lookup"><span data-stu-id="6d382-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="6d382-178"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Событий можно использовать для перехвата и переопределение проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="6d382-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="6d382-179">Проверка файла cookie при каждом запросе уменьшает риск появления отозванных пользователей, обращающихся к приложению.</span><span class="sxs-lookup"><span data-stu-id="6d382-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="6d382-180">Один из способов проверки cookie основан на слежении за при изменениях базы данных пользователя.</span><span class="sxs-lookup"><span data-stu-id="6d382-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="6d382-181">Если базы данных не был изменен с момента выпуска файла cookie пользователя, нет необходимости пройти повторную аутентификацию пользователя, если их куки-файл все еще действителен.</span><span class="sxs-lookup"><span data-stu-id="6d382-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="6d382-182">В примере приложения базы данных была реализована в `IUserRepository` и сохраняет `LastChanged` значение.</span><span class="sxs-lookup"><span data-stu-id="6d382-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="6d382-183">При обновлении пользователя в базе данных, `LastChanged` имеет значение текущего времени.</span><span class="sxs-lookup"><span data-stu-id="6d382-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="6d382-184">Чтобы сделать недействительным файл cookie, когда база данных изменяется в зависимости от `LastChanged` создайте файл cookie с `LastChanged` утверждение, содержащее текущий `LastChanged` значение из базы данных:</span><span class="sxs-lookup"><span data-stu-id="6d382-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="6d382-185">Для реализации переопределения для `ValidatePrincipal` события, написание метод со следующей сигнатурой в классе, который является производным от <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="6d382-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="6d382-186">Ниже приведен пример реализации `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="6d382-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="6d382-187">Зарегистрируйте экземпляр события во время регистрации службы файл cookie в `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="6d382-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="6d382-188">Укажите [областью действия регистрации службы](xref:fundamentals/dependency-injection#service-lifetimes) для вашей `CustomCookieAuthenticationEvents` класса:</span><span class="sxs-lookup"><span data-stu-id="6d382-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="6d382-189">Рассмотрим ситуацию, в котором имя пользователя будет обновлено&mdash;решение, которое не влияет на безопасность любым способом.</span><span class="sxs-lookup"><span data-stu-id="6d382-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="6d382-190">Если вы хотите безболезненно обновить участника-пользователя, вызовите `context.ReplacePrincipal` и задайте `context.ShouldRenew` свойства `true`.</span><span class="sxs-lookup"><span data-stu-id="6d382-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="6d382-191">Подход, описанный здесь активируется при каждом запросе.</span><span class="sxs-lookup"><span data-stu-id="6d382-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="6d382-192">Проверка файлов cookie проверки подлинности для всех пользователей при каждом запросе может привести к снижению производительности для приложения.</span><span class="sxs-lookup"><span data-stu-id="6d382-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="6d382-193">Постоянные файлы cookie</span><span class="sxs-lookup"><span data-stu-id="6d382-193">Persistent cookies</span></span>

<span data-ttu-id="6d382-194">Требуется файл cookie для сохранения между сеансами браузера.</span><span class="sxs-lookup"><span data-stu-id="6d382-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="6d382-195">Такое сохранение должны включаться только с согласия пользователя с помощью флажка «Запомнить мои» при входе или похожий механизм.</span><span class="sxs-lookup"><span data-stu-id="6d382-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="6d382-196">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выдерживает его через браузер замыкания.</span><span class="sxs-lookup"><span data-stu-id="6d382-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="6d382-197">Учитываются параметры скользящего срока действия ранее была настроена.</span><span class="sxs-lookup"><span data-stu-id="6d382-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="6d382-198">Если срок действия файла cookie, при закрытии браузера, браузер удаляет файл cookie после перезагрузки.</span><span class="sxs-lookup"><span data-stu-id="6d382-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="6d382-199">Задайте <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> для `true` в <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="6d382-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="6d382-200">Срок действия файла cookie абсолютный</span><span class="sxs-lookup"><span data-stu-id="6d382-200">Absolute cookie expiration</span></span>

<span data-ttu-id="6d382-201">Можно задать абсолютный срок действия <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="6d382-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="6d382-202">Чтобы создать постоянный файл cookie, `IsPersistent` также должно быть задано.</span><span class="sxs-lookup"><span data-stu-id="6d382-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="6d382-203">В противном случае файл cookie создается с временем жизни на основе сеансов и срок действия может истечь до или после билет проверки подлинности, который его содержит.</span><span class="sxs-lookup"><span data-stu-id="6d382-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="6d382-204">Когда `ExpiresUtc` не установлен, этот параметр переопределяет значение <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> параметр <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, если задать.</span><span class="sxs-lookup"><span data-stu-id="6d382-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="6d382-205">Следующий фрагмент кода создает удостоверение и соответствующий файл cookie, который выполняется в течение 20 минут.</span><span class="sxs-lookup"><span data-stu-id="6d382-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="6d382-206">Это не учитывает параметры скользящего срока действия ранее была настроена.</span><span class="sxs-lookup"><span data-stu-id="6d382-206">This ignores any sliding expiration settings previously configured.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="6d382-207">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6d382-207">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="6d382-208">Проверок роли на основе политик</span><span class="sxs-lookup"><span data-stu-id="6d382-208">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
