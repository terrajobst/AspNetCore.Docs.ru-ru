---
title: Настройка удостоверения ASP.NET Core
author: AdrienTorris
description: Сведения о значениях по умолчанию для удостоверений ASP.NET Core и о настройке свойств удостоверений для использования пользовательских значений.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652696"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="212ea-103">Настройка удостоверения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="212ea-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="212ea-104">ASP.NET Core удостоверение использует значения по умолчанию для таких параметров, как политика паролей, блокировка и конфигурация файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="212ea-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="212ea-105">Эти параметры можно переопределить в классе `Startup`.</span><span class="sxs-lookup"><span data-stu-id="212ea-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="212ea-106">Параметры идентификации</span><span class="sxs-lookup"><span data-stu-id="212ea-106">Identity options</span></span>

<span data-ttu-id="212ea-107">Класс [идентитйоптионс](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) представляет параметры, которые можно использовать для настройки системы удостоверений.</span><span class="sxs-lookup"><span data-stu-id="212ea-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="212ea-108">`IdentityOptions` должны быть заданы **после** вызова `AddIdentity` или `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="212ea-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="212ea-109">Удостоверение утверждений</span><span class="sxs-lookup"><span data-stu-id="212ea-109">Claims Identity</span></span>

<span data-ttu-id="212ea-110">[Идентитйоптионс. ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) указывает [клаимсидентитйоптионс](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) со свойствами, показанными в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="212ea-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="212ea-111">Свойство</span><span class="sxs-lookup"><span data-stu-id="212ea-111">Property</span></span> | <span data-ttu-id="212ea-112">Description</span><span class="sxs-lookup"><span data-stu-id="212ea-112">Description</span></span> | <span data-ttu-id="212ea-113">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="212ea-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="212ea-114">ролеклаимтипе</span><span class="sxs-lookup"><span data-stu-id="212ea-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="212ea-115">Возвращает или задает тип утверждения, используемого для утверждения роли.</span><span class="sxs-lookup"><span data-stu-id="212ea-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="212ea-116">ClaimTypes. Role</span><span class="sxs-lookup"><span data-stu-id="212ea-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="212ea-117">секуритистампклаимтипе</span><span class="sxs-lookup"><span data-stu-id="212ea-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="212ea-118">Возвращает или задает тип утверждения, используемого для утверждения метки безопасности.</span><span class="sxs-lookup"><span data-stu-id="212ea-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="212ea-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="212ea-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="212ea-120">Возвращает или задает тип утверждения, используемого для утверждения идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="212ea-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="212ea-121">ClaimTypes. NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="212ea-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="212ea-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="212ea-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="212ea-123">Возвращает или задает тип утверждения, используемого для утверждения имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="212ea-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="212ea-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="212ea-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="212ea-125">Блокирован</span><span class="sxs-lookup"><span data-stu-id="212ea-125">Lockout</span></span>

<span data-ttu-id="212ea-126">Блокировка задается в методе [пассвордсигнинасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) :</span><span class="sxs-lookup"><span data-stu-id="212ea-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="212ea-127">Приведенный выше код основан на шаблоне удостоверения `Login`.</span><span class="sxs-lookup"><span data-stu-id="212ea-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="212ea-128">Параметры блокировки задаются в `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="212ea-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="212ea-129">Приведенный выше код задает [идентитйоптионс](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [локкаутоптионс](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) со значениями по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="212ea-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="212ea-130">Сбрасывает счетчик попыток неудачные попытки доступа успешной проверки подлинности и сбрасывает часы.</span><span class="sxs-lookup"><span data-stu-id="212ea-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="212ea-131">[Идентитйоптионс. Блокировка](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) задает [локкаутоптионс](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) со свойствами, показанными в таблице.</span><span class="sxs-lookup"><span data-stu-id="212ea-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="212ea-132">Свойство</span><span class="sxs-lookup"><span data-stu-id="212ea-132">Property</span></span> | <span data-ttu-id="212ea-133">Description</span><span class="sxs-lookup"><span data-stu-id="212ea-133">Description</span></span> | <span data-ttu-id="212ea-134">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="212ea-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="212ea-135">алловедфорневусерс</span><span class="sxs-lookup"><span data-stu-id="212ea-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="212ea-136">Определяет, можно ли заблокировать нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="212ea-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="212ea-137">дефаултлоккауттимеспан</span><span class="sxs-lookup"><span data-stu-id="212ea-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="212ea-138">Количество времени, в течение которого пользователь блокируется в случае блокировки.</span><span class="sxs-lookup"><span data-stu-id="212ea-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="212ea-139">5 мин</span><span class="sxs-lookup"><span data-stu-id="212ea-139">5 minutes</span></span> |
| [<span data-ttu-id="212ea-140">максфаиледакцессаттемптс</span><span class="sxs-lookup"><span data-stu-id="212ea-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="212ea-141">Число неудачных попыток доступа, пока пользователь не будет заблокирован, если блокировка включена.</span><span class="sxs-lookup"><span data-stu-id="212ea-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="212ea-142">5</span><span class="sxs-lookup"><span data-stu-id="212ea-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="212ea-143">Пароль</span><span class="sxs-lookup"><span data-stu-id="212ea-143">Password</span></span>

<span data-ttu-id="212ea-144">По умолчанию для удостоверения требуется, чтобы пароли содержали символы в верхнем регистре, символы нижнего регистра, цифры и символы, отличные от буквенно-цифровых.</span><span class="sxs-lookup"><span data-stu-id="212ea-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="212ea-145">Пароли должны иметь длину не менее шести символов.</span><span class="sxs-lookup"><span data-stu-id="212ea-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="212ea-146">[Пассвордоптионс](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) можно задать в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="212ea-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="212ea-147">[Идентитйоптионс. Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) указывает [пассвордоптионс](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) с свойствами, показанными в таблице.</span><span class="sxs-lookup"><span data-stu-id="212ea-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="212ea-148">Свойство</span><span class="sxs-lookup"><span data-stu-id="212ea-148">Property</span></span> | <span data-ttu-id="212ea-149">Description</span><span class="sxs-lookup"><span data-stu-id="212ea-149">Description</span></span> | <span data-ttu-id="212ea-150">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="212ea-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="212ea-151">рекуиредигит</span><span class="sxs-lookup"><span data-stu-id="212ea-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="212ea-152">Требуется число от 0-9 в пароле.</span><span class="sxs-lookup"><span data-stu-id="212ea-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="212ea-153">рекуиредленгс</span><span class="sxs-lookup"><span data-stu-id="212ea-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="212ea-154">Минимальная длина пароля.</span><span class="sxs-lookup"><span data-stu-id="212ea-154">The minimum length of the password.</span></span> | <span data-ttu-id="212ea-155">6</span><span class="sxs-lookup"><span data-stu-id="212ea-155">6</span></span> |
| [<span data-ttu-id="212ea-156">рекуиреловеркасе</span><span class="sxs-lookup"><span data-stu-id="212ea-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="212ea-157">В пароле требуется символ нижнего регистра.</span><span class="sxs-lookup"><span data-stu-id="212ea-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="212ea-158">рекуиреноналфанумерик</span><span class="sxs-lookup"><span data-stu-id="212ea-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="212ea-159">В пароле требуется символ, отличный от буквенно-цифрового.</span><span class="sxs-lookup"><span data-stu-id="212ea-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="212ea-160">рекуиредуникуечарс</span><span class="sxs-lookup"><span data-stu-id="212ea-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="212ea-161">Применяется только к ASP.NET Core 2,0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="212ea-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="212ea-162">Требуется число уникальных символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="212ea-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="212ea-163">1</span><span class="sxs-lookup"><span data-stu-id="212ea-163">1</span></span> |
| [<span data-ttu-id="212ea-164">рекуиреупперкасе</span><span class="sxs-lookup"><span data-stu-id="212ea-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="212ea-165">В пароле требуется символ верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="212ea-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="212ea-166">Свойство</span><span class="sxs-lookup"><span data-stu-id="212ea-166">Property</span></span> | <span data-ttu-id="212ea-167">Description</span><span class="sxs-lookup"><span data-stu-id="212ea-167">Description</span></span> | <span data-ttu-id="212ea-168">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="212ea-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="212ea-169">рекуиредигит</span><span class="sxs-lookup"><span data-stu-id="212ea-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="212ea-170">Требуется число от 0-9 в пароле.</span><span class="sxs-lookup"><span data-stu-id="212ea-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="212ea-171">рекуиредленгс</span><span class="sxs-lookup"><span data-stu-id="212ea-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="212ea-172">Минимальная длина пароля.</span><span class="sxs-lookup"><span data-stu-id="212ea-172">The minimum length of the password.</span></span> | <span data-ttu-id="212ea-173">6</span><span class="sxs-lookup"><span data-stu-id="212ea-173">6</span></span> |
| [<span data-ttu-id="212ea-174">рекуиреловеркасе</span><span class="sxs-lookup"><span data-stu-id="212ea-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="212ea-175">В пароле требуется символ нижнего регистра.</span><span class="sxs-lookup"><span data-stu-id="212ea-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="212ea-176">рекуиреноналфанумерик</span><span class="sxs-lookup"><span data-stu-id="212ea-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="212ea-177">В пароле требуется символ, отличный от буквенно-цифрового.</span><span class="sxs-lookup"><span data-stu-id="212ea-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="212ea-178">рекуиреупперкасе</span><span class="sxs-lookup"><span data-stu-id="212ea-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="212ea-179">В пароле требуется символ верхнего регистра.</span><span class="sxs-lookup"><span data-stu-id="212ea-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="212ea-180">Вход</span><span class="sxs-lookup"><span data-stu-id="212ea-180">Sign-in</span></span>

<span data-ttu-id="212ea-181">Следующий код задает параметры `SignIn` (значения по умолчанию):</span><span class="sxs-lookup"><span data-stu-id="212ea-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="212ea-182">[Идентитйоптионс. Signing](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) указывает [сигниноптионс](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) со свойствами, показанными в таблице.</span><span class="sxs-lookup"><span data-stu-id="212ea-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="212ea-183">Свойство</span><span class="sxs-lookup"><span data-stu-id="212ea-183">Property</span></span> | <span data-ttu-id="212ea-184">Description</span><span class="sxs-lookup"><span data-stu-id="212ea-184">Description</span></span> | <span data-ttu-id="212ea-185">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="212ea-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="212ea-186">рекуиреконфирмедемаил</span><span class="sxs-lookup"><span data-stu-id="212ea-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="212ea-187">Для входа требуется подтвержденное электронное письмо.</span><span class="sxs-lookup"><span data-stu-id="212ea-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="212ea-188">рекуиреконфирмедфоненумбер</span><span class="sxs-lookup"><span data-stu-id="212ea-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="212ea-189">Для входа требуется подтвержденный номер телефона.</span><span class="sxs-lookup"><span data-stu-id="212ea-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="212ea-190">Токены</span><span class="sxs-lookup"><span data-stu-id="212ea-190">Tokens</span></span>

<span data-ttu-id="212ea-191">[Идентитйоптионс. Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) указывает [токеноптионс](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) с помощью свойств, показанных в таблице.</span><span class="sxs-lookup"><span data-stu-id="212ea-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

|                                                        <span data-ttu-id="212ea-192">Свойство</span><span class="sxs-lookup"><span data-stu-id="212ea-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="212ea-193">Description</span><span class="sxs-lookup"><span data-stu-id="212ea-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="212ea-194">аусентикатортокенпровидер</span><span class="sxs-lookup"><span data-stu-id="212ea-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="212ea-195">Возвращает или задает `AuthenticatorTokenProvider`, используемую для проверки двухфакторной регистрации с помощью средства проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="212ea-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="212ea-196">чанжеемаилтокенпровидер</span><span class="sxs-lookup"><span data-stu-id="212ea-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="212ea-197">Возвращает или задает `ChangeEmailTokenProvider`, используемую для создания токенов, используемых в сообщениях электронной почты с подтверждением изменений.</span><span class="sxs-lookup"><span data-stu-id="212ea-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="212ea-198">чанжефоненумбертокенпровидер</span><span class="sxs-lookup"><span data-stu-id="212ea-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="212ea-199">Возвращает или задает `ChangePhoneNumberTokenProvider`, используемую для создания токенов, используемых при изменении номеров телефонов.</span><span class="sxs-lookup"><span data-stu-id="212ea-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="212ea-200">емаилконфирматионтокенпровидер</span><span class="sxs-lookup"><span data-stu-id="212ea-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="212ea-201">Возвращает или задает поставщик токенов, используемый для создания токенов, используемых в сообщениях электронной почты с подтверждением учетной записи.</span><span class="sxs-lookup"><span data-stu-id="212ea-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="212ea-202">пассвордресеттокенпровидер</span><span class="sxs-lookup"><span data-stu-id="212ea-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="212ea-203">Возвращает или задает [иусертвофактортокенпровидер\<тусер >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) , используемый для создания маркеров, используемых при сбросе пароля.</span><span class="sxs-lookup"><span data-stu-id="212ea-203">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="212ea-204">провидермап</span><span class="sxs-lookup"><span data-stu-id="212ea-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="212ea-205">Используется для создания [поставщика пользовательского токена](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) с ключом, используемым в качестве имени поставщика.</span><span class="sxs-lookup"><span data-stu-id="212ea-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="212ea-206">Пользователь</span><span class="sxs-lookup"><span data-stu-id="212ea-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="212ea-207">[Идентитйоптионс. User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) указывает [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) с свойствами, показанными в таблице.</span><span class="sxs-lookup"><span data-stu-id="212ea-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="212ea-208">Свойство</span><span class="sxs-lookup"><span data-stu-id="212ea-208">Property</span></span> | <span data-ttu-id="212ea-209">Description</span><span class="sxs-lookup"><span data-stu-id="212ea-209">Description</span></span> | <span data-ttu-id="212ea-210">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="212ea-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="212ea-211">алловедусернамечарактерс</span><span class="sxs-lookup"><span data-stu-id="212ea-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="212ea-212">Допустимые символы в имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="212ea-212">Allowed characters in the username.</span></span> | <span data-ttu-id="212ea-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="212ea-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="212ea-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="212ea-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="212ea-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="212ea-215">0123456789</span></span><br><span data-ttu-id="212ea-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="212ea-216">-.\_@+</span></span> |
| [<span data-ttu-id="212ea-217">рекуиреуникуимаил</span><span class="sxs-lookup"><span data-stu-id="212ea-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="212ea-218">Требует, чтобы каждый пользователь имел уникальный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="212ea-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="212ea-219">Параметры файлов cookie</span><span class="sxs-lookup"><span data-stu-id="212ea-219">Cookie settings</span></span>

<span data-ttu-id="212ea-220">Настройте файл cookie приложения в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="212ea-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="212ea-221">[Конфигуреаппликатионкукие](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) должен вызываться **после** вызова `AddIdentity` или `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="212ea-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="212ea-222">Дополнительные сведения см. в разделе [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="212ea-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="212ea-223">Параметры хэша пароля</span><span class="sxs-lookup"><span data-stu-id="212ea-223">Password Hasher options</span></span>

<span data-ttu-id="212ea-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> получает и задает параметры хэширования паролей.</span><span class="sxs-lookup"><span data-stu-id="212ea-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="212ea-225">Параметр</span><span class="sxs-lookup"><span data-stu-id="212ea-225">Option</span></span> | <span data-ttu-id="212ea-226">Description</span><span class="sxs-lookup"><span data-stu-id="212ea-226">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="212ea-227">Режим совместимости, используемый при хэшировании новых паролей.</span><span class="sxs-lookup"><span data-stu-id="212ea-227">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="212ea-228">По умолчанию равен <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="212ea-228">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="212ea-229">Первый байт хэшированного пароля, называемый *маркером формата*, указывает версию алгоритма хэширования, используемого для хэширования пароля.</span><span class="sxs-lookup"><span data-stu-id="212ea-229">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="212ea-230">При проверке пароля по хэшу метод <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> выбирает правильный алгоритм на основе первого байта.</span><span class="sxs-lookup"><span data-stu-id="212ea-230">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="212ea-231">Клиент может проходить проверку подлинности независимо от того, какая версия алгоритма использовалась для хэширования пароля.</span><span class="sxs-lookup"><span data-stu-id="212ea-231">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="212ea-232">Установка режима совместимости влияет на хэширование *новых паролей*.</span><span class="sxs-lookup"><span data-stu-id="212ea-232">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="212ea-233">Число итераций, используемых при хэшировании паролей с помощью PBKDF2.</span><span class="sxs-lookup"><span data-stu-id="212ea-233">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="212ea-234">Это значение используется только в том случае, если <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> имеет значение <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="212ea-234">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="212ea-235">Значение должно быть положительным целым числом, а по умолчанию — `10000`.</span><span class="sxs-lookup"><span data-stu-id="212ea-235">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="212ea-236">В следующем примере <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> устанавливается в `12000` в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="212ea-236">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
