---
title: "Настройка удостоверения ASP.NET Core"
author: AdrienTorris
description: "Значения по умолчанию ASP.NET Core Identity понять и настройки различных свойств Identity для использования пользовательских значений."
keywords: "Проверку подлинности ASP.NET Core, удостоверение,"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="bb1f2-104">Настройка удостоверения</span><span class="sxs-lookup"><span data-stu-id="bb1f2-104">Configure Identity</span></span>

<span data-ttu-id="bb1f2-105">ASP.NET Core Identity имеет общие поведения в приложениях, например политика паролей, время блокировки и параметры файлов cookie, которые можно легко переопределить в вашем приложении `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="bb1f2-106">Политики паролей</span><span class="sxs-lookup"><span data-stu-id="bb1f2-106">Passwords policy</span></span>

<span data-ttu-id="bb1f2-107">По умолчанию удостоверение требует, что пароли содержат символ верхнего регистра, строчные буквы, цифры и не алфавитно-цифровой символ.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="bb1f2-108">Существуют также некоторые другие ограничения.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-108">There are also some other restrictions.</span></span> <span data-ttu-id="bb1f2-109">Чтобы упростить ограничения для пароля, измените `ConfigureServices` метод `Startup` класса приложения.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb1f2-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb1f2-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bb1f2-111">ASP.NET Core 2.0 добавлен `RequiredUniqueChars` свойство.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="bb1f2-112">В противном случае параметры совпадают с ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb1f2-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb1f2-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="bb1f2-114">`IdentityOptions.Password`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="bb1f2-115">Свойство.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-115">Property</span></span>                | <span data-ttu-id="bb1f2-116">Описание:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-116">Description</span></span>                       | <span data-ttu-id="bb1f2-117">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="bb1f2-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="bb1f2-118">Необходимо указать число от 0-9 и пароль.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="bb1f2-119">true</span><span class="sxs-lookup"><span data-stu-id="bb1f2-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="bb1f2-120">Минимальная длина пароля.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-120">The minimum length of the password.</span></span> | <span data-ttu-id="bb1f2-121">6</span><span class="sxs-lookup"><span data-stu-id="bb1f2-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="bb1f2-122">Требуется не буквенно-цифровых символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="bb1f2-123">true</span><span class="sxs-lookup"><span data-stu-id="bb1f2-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="bb1f2-124">Требуется верхний регистр символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="bb1f2-125">true</span><span class="sxs-lookup"><span data-stu-id="bb1f2-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="bb1f2-126">Должен стоять символ нижнего регистра в пароле.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="bb1f2-127">true</span><span class="sxs-lookup"><span data-stu-id="bb1f2-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="bb1f2-128">Требует количества уникальных символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="bb1f2-129">1</span><span class="sxs-lookup"><span data-stu-id="bb1f2-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="bb1f2-130">Блокировки пользователя</span><span class="sxs-lookup"><span data-stu-id="bb1f2-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="bb1f2-131">`IdentityOptions.Lockout`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="bb1f2-132">Свойство.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-132">Property</span></span>                | <span data-ttu-id="bb1f2-133">Описание:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-133">Description</span></span>                       | <span data-ttu-id="bb1f2-134">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="bb1f2-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="bb1f2-135">Количество времени, пользователь будет заблокирован при возникновении блокировки.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="bb1f2-136">5 минут</span><span class="sxs-lookup"><span data-stu-id="bb1f2-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="bb1f2-137">Число неудачных попыток доступа до пользователя будет заблокирована, если включена блокировка.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="bb1f2-138">5</span><span class="sxs-lookup"><span data-stu-id="bb1f2-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="bb1f2-139">Определяет, если новый пользователь может быть заблокирован.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="bb1f2-140">true</span><span class="sxs-lookup"><span data-stu-id="bb1f2-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="bb1f2-141">Параметры входа</span><span class="sxs-lookup"><span data-stu-id="bb1f2-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="bb1f2-142">`IdentityOptions.SignIn`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="bb1f2-143">Свойство.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-143">Property</span></span>                | <span data-ttu-id="bb1f2-144">Описание:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-144">Description</span></span>                       | <span data-ttu-id="bb1f2-145">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="bb1f2-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="bb1f2-146">Требует подтверждения электронной почты для входа.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="bb1f2-147">False</span><span class="sxs-lookup"><span data-stu-id="bb1f2-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="bb1f2-148">Требуется номер телефона, подтвержденной для входа.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="bb1f2-149">False</span><span class="sxs-lookup"><span data-stu-id="bb1f2-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="bb1f2-150">Параметры проверки пользователя</span><span class="sxs-lookup"><span data-stu-id="bb1f2-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="bb1f2-151">`IdentityOptions.User`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="bb1f2-152">Свойство.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-152">Property</span></span>                | <span data-ttu-id="bb1f2-153">Описание:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-153">Description</span></span>                       | <span data-ttu-id="bb1f2-154">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="bb1f2-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="bb1f2-155">Требуется иметь уникальный адрес электронной почты пользователя.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="bb1f2-156">False</span><span class="sxs-lookup"><span data-stu-id="bb1f2-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="bb1f2-157">Допустимые символы в имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-157">Allowed characters in the username.</span></span> | <span data-ttu-id="bb1f2-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="bb1f2-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="bb1f2-159">Параметры файлов cookie приложения</span><span class="sxs-lookup"><span data-stu-id="bb1f2-159">Application's cookie settings</span></span>

<span data-ttu-id="bb1f2-160">Все параметры файла cookie приложения как политика паролей, можно изменить в `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bb1f2-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bb1f2-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bb1f2-162">В разделе `ConfigureServices` в `Startup` класса, можно настроить куки-файл приложения.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bb1f2-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bb1f2-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="bb1f2-164">`CookieAuthenticationOptions`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="bb1f2-165">Свойство.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-165">Property</span></span>                | <span data-ttu-id="bb1f2-166">Описание:</span><span class="sxs-lookup"><span data-stu-id="bb1f2-166">Description</span></span>                       | <span data-ttu-id="bb1f2-167">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="bb1f2-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="bb1f2-168">Имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-168">The name of the cookie.</span></span>  | <span data-ttu-id="bb1f2-169">. AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="bb1f2-170">Если значение равно true, куки-файл недоступен из скриптов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="bb1f2-171">true</span><span class="sxs-lookup"><span data-stu-id="bb1f2-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="bb1f2-172">Определяет, сколько времени хранится билет проверки подлинности в файл cookie будет оставаться действительным с момента его создания.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="bb1f2-173">14 дней</span><span class="sxs-lookup"><span data-stu-id="bb1f2-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="bb1f2-174">Если пользователь не авторизован, они будут перенаправляться по этому пути, имени входа.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="bb1f2-175">/ Учетной записи или имени входа</span><span class="sxs-lookup"><span data-stu-id="bb1f2-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="bb1f2-176">При выходе пользователя он будет перенаправлен по этому пути.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="bb1f2-177">/ Учетной записи и выхода из системы</span><span class="sxs-lookup"><span data-stu-id="bb1f2-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="bb1f2-178">При сбое проверка авторизации пользователя они будут перенаправляться к этому пути.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="bb1f2-179">Если значение равно true, новый файл cookie будет выдан с новым окончанием срока действия текущего файла cookie при более половины срока.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="bb1f2-180">/ Учетной записи или доступ запрещен</span><span class="sxs-lookup"><span data-stu-id="bb1f2-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="bb1f2-181">Определяет имя параметра строки запроса, который добавляется по промежуточного слоя, когда код состояния 401 несанкционированный изменяется на перенаправление 302 пути входа.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="bb1f2-182">true</span><span class="sxs-lookup"><span data-stu-id="bb1f2-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="bb1f2-183">Это относится только к ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="bb1f2-184">Логическое имя для проверки подлинности конкретной схемы.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="bb1f2-185">Этот флаг относится только к ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="bb1f2-186">Если значение равно true, файл cookie проверки подлинности запустите при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана.</span><span class="sxs-lookup"><span data-stu-id="bb1f2-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |