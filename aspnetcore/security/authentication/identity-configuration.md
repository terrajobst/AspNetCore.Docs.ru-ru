---
title: Настройка удостоверения ASP.NET Core
author: AdrienTorris
description: Понять значения по умолчанию ASP.NET Core Identity и узнайте, как настроить свойства удостоверения для использования пользовательских значений.
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210123"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="f3bb1-103">Настройка удостоверения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3bb1-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="f3bb1-104">Удостоверение ASP.NET Core использует значения по умолчанию для параметров, таких как политики паролей, блокировки и куки-файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="f3bb1-105">Эти параметры можно переопределить в `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="f3bb1-106">Параметры удостоверений</span><span class="sxs-lookup"><span data-stu-id="f3bb1-106">Identity options</span></span>

<span data-ttu-id="f3bb1-107">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) класс представляет параметры, которые могут использоваться для настройки системы удостоверений.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="f3bb1-108">`IdentityOptions` необходимо задать **после** вызова `AddIdentity` или `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="f3bb1-109">Идентификатор утверждений</span><span class="sxs-lookup"><span data-stu-id="f3bb1-109">Claims Identity</span></span>

<span data-ttu-id="f3bb1-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) указывает [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) с помощью свойства, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="f3bb1-111">Свойство</span><span class="sxs-lookup"><span data-stu-id="f3bb1-111">Property</span></span> | <span data-ttu-id="f3bb1-112">Описание</span><span class="sxs-lookup"><span data-stu-id="f3bb1-112">Description</span></span> | <span data-ttu-id="f3bb1-113">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f3bb1-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="f3bb1-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="f3bb1-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="f3bb1-115">Возвращает или задает тип утверждения, используемый для утверждения роли.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="f3bb1-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="f3bb1-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="f3bb1-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="f3bb1-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="f3bb1-118">Возвращает или задает тип утверждения, используемый для отметки утверждения безопасности.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="f3bb1-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="f3bb1-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="f3bb1-120">Возвращает или задает тип утверждения, используемый для утверждение идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="f3bb1-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="f3bb1-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="f3bb1-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="f3bb1-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="f3bb1-123">Возвращает или задает тип утверждения, используемый для утверждения с именем пользователя.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="f3bb1-124">Содержащееся в ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="f3bb1-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="f3bb1-125">Блокировки</span><span class="sxs-lookup"><span data-stu-id="f3bb1-125">Lockout</span></span>

<span data-ttu-id="f3bb1-126">Блокировки задается в [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) метод:</span><span class="sxs-lookup"><span data-stu-id="f3bb1-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="f3bb1-127">Приведенный выше код основан на `Login` удостоверение шаблона.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="f3bb1-128">Задаются параметры блокировки `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f3bb1-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="f3bb1-129">Указанный выше код задает [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) со значениями по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="f3bb1-130">Сбрасывает счетчик попыток неудачные попытки доступа успешной проверки подлинности и сбрасывает часы.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="f3bb1-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) указывает [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) со свойствами, приведенных в таблице.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="f3bb1-132">Свойство</span><span class="sxs-lookup"><span data-stu-id="f3bb1-132">Property</span></span> | <span data-ttu-id="f3bb1-133">Описание</span><span class="sxs-lookup"><span data-stu-id="f3bb1-133">Description</span></span> | <span data-ttu-id="f3bb1-134">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f3bb1-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="f3bb1-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="f3bb1-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="f3bb1-136">Определяет, если новый пользователь может быть заблокирован.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="f3bb1-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="f3bb1-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="f3bb1-138">Количество времени, пользователь будет заблокирован при возникновении блокировки.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="f3bb1-139">5 минут</span><span class="sxs-lookup"><span data-stu-id="f3bb1-139">5 minutes</span></span> |
| [<span data-ttu-id="f3bb1-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="f3bb1-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="f3bb1-141">Число неудачных попыток доступа до пользователя будет заблокирована, если включена блокировка.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="f3bb1-142">5</span><span class="sxs-lookup"><span data-stu-id="f3bb1-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="f3bb1-143">Пароль</span><span class="sxs-lookup"><span data-stu-id="f3bb1-143">Password</span></span>

<span data-ttu-id="f3bb1-144">По умолчанию удостоверение требует, что пароли содержат символ верхнего регистра, строчную букву, цифру и специальный символ.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="f3bb1-145">Пароли должны быть по крайней мере шесть символов.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="f3bb1-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) можно задать в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="f3bb1-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) указывает [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) со свойствами, приведенных в таблице.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="f3bb1-148">Свойство</span><span class="sxs-lookup"><span data-stu-id="f3bb1-148">Property</span></span> | <span data-ttu-id="f3bb1-149">Описание</span><span class="sxs-lookup"><span data-stu-id="f3bb1-149">Description</span></span> | <span data-ttu-id="f3bb1-150">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f3bb1-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="f3bb1-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="f3bb1-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="f3bb1-152">Требуется число в диапазоне от 0 до 9, в пароле.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="f3bb1-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="f3bb1-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="f3bb1-154">Минимальная длина пароля.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-154">The minimum length of the password.</span></span> | <span data-ttu-id="f3bb1-155">6</span><span class="sxs-lookup"><span data-stu-id="f3bb1-155">6</span></span> |
| [<span data-ttu-id="f3bb1-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="f3bb1-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="f3bb1-157">Должен стоять символ нижнего регистра в пароле.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="f3bb1-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="f3bb1-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="f3bb1-159">Требуется не буквенно-цифровых символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="f3bb1-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="f3bb1-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="f3bb1-161">Применяется только к ASP.NET Core 2.0 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="f3bb1-162">Необходимо число уникальных символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="f3bb1-163">1</span><span class="sxs-lookup"><span data-stu-id="f3bb1-163">1</span></span> |
| [<span data-ttu-id="f3bb1-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="f3bb1-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="f3bb1-165">Требуется символ верхнего регистра в пароле.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="f3bb1-166">Свойство</span><span class="sxs-lookup"><span data-stu-id="f3bb1-166">Property</span></span> | <span data-ttu-id="f3bb1-167">Описание</span><span class="sxs-lookup"><span data-stu-id="f3bb1-167">Description</span></span> | <span data-ttu-id="f3bb1-168">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f3bb1-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="f3bb1-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="f3bb1-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="f3bb1-170">Требуется число в диапазоне от 0 до 9, в пароле.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="f3bb1-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="f3bb1-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="f3bb1-172">Минимальная длина пароля.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-172">The minimum length of the password.</span></span> | <span data-ttu-id="f3bb1-173">6</span><span class="sxs-lookup"><span data-stu-id="f3bb1-173">6</span></span> |
| [<span data-ttu-id="f3bb1-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="f3bb1-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="f3bb1-175">Должен стоять символ нижнего регистра в пароле.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="f3bb1-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="f3bb1-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="f3bb1-177">Требуется не буквенно-цифровых символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="f3bb1-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="f3bb1-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="f3bb1-179">Требуется символ верхнего регистра в пароле.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="f3bb1-180">Вход</span><span class="sxs-lookup"><span data-stu-id="f3bb1-180">Sign-in</span></span>

<span data-ttu-id="f3bb1-181">В следующем коде наборы `SignIn` (по умолчанию значения параметров):</span><span class="sxs-lookup"><span data-stu-id="f3bb1-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="f3bb1-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) указывает [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) со свойствами, приведенных в таблице.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="f3bb1-183">Свойство</span><span class="sxs-lookup"><span data-stu-id="f3bb1-183">Property</span></span> | <span data-ttu-id="f3bb1-184">Описание</span><span class="sxs-lookup"><span data-stu-id="f3bb1-184">Description</span></span> | <span data-ttu-id="f3bb1-185">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f3bb1-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="f3bb1-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="f3bb1-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="f3bb1-187">Требуется подтверждена электронной почты для входа.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="f3bb1-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="f3bb1-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="f3bb1-189">Требуется номер подтверждена телефона для входа.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="f3bb1-190">Токены</span><span class="sxs-lookup"><span data-stu-id="f3bb1-190">Tokens</span></span>

<span data-ttu-id="f3bb1-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) указывает [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) со свойствами, приведенных в таблице.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

|                                                        <span data-ttu-id="f3bb1-192">Свойство</span><span class="sxs-lookup"><span data-stu-id="f3bb1-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="f3bb1-193">Описание</span><span class="sxs-lookup"><span data-stu-id="f3bb1-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="f3bb1-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="f3bb1-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="f3bb1-195">Возвращает или задает `AuthenticatorTokenProvider` используется для проверки двухфакторной входы в систему с помощью средства проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="f3bb1-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="f3bb1-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="f3bb1-197">Возвращает или задает `ChangeEmailTokenProvider` используется для создания токенов, используемых в сообщениях электронной почты подтверждением изменение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="f3bb1-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="f3bb1-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="f3bb1-199">Возвращает или задает `ChangePhoneNumberTokenProvider` использовать для создания маркеров, используемых для изменения номера телефонов.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="f3bb1-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="f3bb1-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="f3bb1-201">Возвращает или задает поставщик маркеров, используемый для создания токенов, используемых в сообщениях электронной почты для подтверждения учетной записи.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="f3bb1-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="f3bb1-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="f3bb1-203">Возвращает или задает [IUserTwoFactorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) используется для создания токенов, используемых в сообщениях электронной почты для сброса пароля.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-203">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="f3bb1-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="f3bb1-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="f3bb1-205">Используется для конструирования [Поставщик маркера пользователя](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) с ключом, используемым в качестве имени поставщика.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="f3bb1-206">Пользователь</span><span class="sxs-lookup"><span data-stu-id="f3bb1-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="f3bb1-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) указывает [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) со свойствами, приведенных в таблице.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="f3bb1-208">Свойство</span><span class="sxs-lookup"><span data-stu-id="f3bb1-208">Property</span></span> | <span data-ttu-id="f3bb1-209">Описание</span><span class="sxs-lookup"><span data-stu-id="f3bb1-209">Description</span></span> | <span data-ttu-id="f3bb1-210">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f3bb1-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="f3bb1-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="f3bb1-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="f3bb1-212">Допустимые символы в имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-212">Allowed characters in the username.</span></span> | <span data-ttu-id="f3bb1-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="f3bb1-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="f3bb1-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="f3bb1-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="f3bb1-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="f3bb1-215">0123456789</span></span><br><span data-ttu-id="f3bb1-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="f3bb1-216">-.\_@+</span></span> |
| [<span data-ttu-id="f3bb1-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="f3bb1-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="f3bb1-218">Требуется уникальный адрес электронной почты для каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="f3bb1-219">Параметры файлов cookie</span><span class="sxs-lookup"><span data-stu-id="f3bb1-219">Cookie settings</span></span>

<span data-ttu-id="f3bb1-220">Настройка приложения в файле cookie `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f3bb1-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) должен вызываться **после** вызова `AddIdentity` или `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="f3bb1-222">Дополнительные сведения см. в разделе [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span><span class="sxs-lookup"><span data-stu-id="f3bb1-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="f3bb1-223">Модуль хэширования паролей</span><span class="sxs-lookup"><span data-stu-id="f3bb1-223">Password Hasher options</span></span>

<span data-ttu-id="f3bb1-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> Возвращает и задает параметры для хэширования пароля.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="f3bb1-225">Параметр</span><span class="sxs-lookup"><span data-stu-id="f3bb1-225">Option</span></span> | <span data-ttu-id="f3bb1-226">Описание:</span><span class="sxs-lookup"><span data-stu-id="f3bb1-226">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="f3bb1-227">Режим совместимости, используемый при хэшировании новые пароли.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-227">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="f3bb1-228">По умолчанию — <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-228">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="f3bb1-229">Первый байт хэшированный пароль, который называется *формат маркера*, указывает версию алгоритм хэширования, используемый для хэширования пароля.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-229">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="f3bb1-230">При проверке пароля с хэшем, <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> метод Выбор правильного алгоритма, в зависимости от первого байта.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-230">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="f3bb1-231">Клиент способен для проверки подлинности независимо от того, из которых версией алгоритма, использованный для хэширования пароля.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-231">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="f3bb1-232">Настройка режима совместимости влияет на хэширования *новые пароли*.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-232">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="f3bb1-233">Число итераций, используемых при хэшировании пароли с помощью PBKDF2.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-233">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="f3bb1-234">Это значение используется, только если <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> присваивается <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-234">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="f3bb1-235">Значение должно быть целым положительным числом, значение по умолчанию — `10000`.</span><span class="sxs-lookup"><span data-stu-id="f3bb1-235">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="f3bb1-236">В следующем примере <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> присваивается `12000` в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f3bb1-236">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
