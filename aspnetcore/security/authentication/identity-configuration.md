---
title: "Настройка удостоверения ASP.NET Core"
author: AdrienTorris
description: "Значения по умолчанию ASP.NET Core Identity понять и настройки различных свойств Identity для использования пользовательских значений."
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 9e79e670173952f1e791a0cefba61c41e1ad4437
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="configure-identity"></a><span data-ttu-id="b605e-103">Настройка удостоверения</span><span class="sxs-lookup"><span data-stu-id="b605e-103">Configure Identity</span></span>

<span data-ttu-id="b605e-104">ASP.NET Core Identity имеет общие поведения в приложениях, например политика паролей, время блокировки и параметры файлов cookie, которые можно легко переопределить в вашем приложении `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="b605e-104">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="b605e-105">Политики паролей</span><span class="sxs-lookup"><span data-stu-id="b605e-105">Passwords policy</span></span>

<span data-ttu-id="b605e-106">По умолчанию удостоверение требует, что пароли содержат символ верхнего регистра, строчные буквы, цифры и не алфавитно-цифровой символ.</span><span class="sxs-lookup"><span data-stu-id="b605e-106">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="b605e-107">Существуют также некоторые другие ограничения.</span><span class="sxs-lookup"><span data-stu-id="b605e-107">There are also some other restrictions.</span></span> <span data-ttu-id="b605e-108">Чтобы упростить ограничения для пароля, измените `ConfigureServices` метод `Startup` класса приложения.</span><span class="sxs-lookup"><span data-stu-id="b605e-108">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b605e-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b605e-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b605e-110">ASP.NET Core 2.0 добавлен `RequiredUniqueChars` свойство.</span><span class="sxs-lookup"><span data-stu-id="b605e-110">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="b605e-111">В противном случае параметры совпадают с ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="b605e-111">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b605e-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b605e-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="b605e-113">`IdentityOptions.Password`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="b605e-113">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="b605e-114">Свойство.</span><span class="sxs-lookup"><span data-stu-id="b605e-114">Property</span></span>                | <span data-ttu-id="b605e-115">Описание:</span><span class="sxs-lookup"><span data-stu-id="b605e-115">Description</span></span>                       | <span data-ttu-id="b605e-116">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="b605e-116">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="b605e-117">Необходимо указать число от 0-9 и пароль.</span><span class="sxs-lookup"><span data-stu-id="b605e-117">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="b605e-118">true</span><span class="sxs-lookup"><span data-stu-id="b605e-118">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="b605e-119">Минимальная длина пароля.</span><span class="sxs-lookup"><span data-stu-id="b605e-119">The minimum length of the password.</span></span> | <span data-ttu-id="b605e-120">6</span><span class="sxs-lookup"><span data-stu-id="b605e-120">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="b605e-121">Требуется не буквенно-цифровых символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="b605e-121">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="b605e-122">true</span><span class="sxs-lookup"><span data-stu-id="b605e-122">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="b605e-123">Требуется верхний регистр символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="b605e-123">Requires an upper case character in the password.</span></span> | <span data-ttu-id="b605e-124">true</span><span class="sxs-lookup"><span data-stu-id="b605e-124">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="b605e-125">Должен стоять символ нижнего регистра в пароле.</span><span class="sxs-lookup"><span data-stu-id="b605e-125">Requires a lower case character in the password.</span></span> | <span data-ttu-id="b605e-126">true</span><span class="sxs-lookup"><span data-stu-id="b605e-126">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="b605e-127">Требует количества уникальных символов в пароле.</span><span class="sxs-lookup"><span data-stu-id="b605e-127">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="b605e-128">1</span><span class="sxs-lookup"><span data-stu-id="b605e-128">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="b605e-129">Блокировки пользователя</span><span class="sxs-lookup"><span data-stu-id="b605e-129">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="b605e-130">`IdentityOptions.Lockout`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="b605e-130">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="b605e-131">Свойство.</span><span class="sxs-lookup"><span data-stu-id="b605e-131">Property</span></span>                | <span data-ttu-id="b605e-132">Описание:</span><span class="sxs-lookup"><span data-stu-id="b605e-132">Description</span></span>                       | <span data-ttu-id="b605e-133">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="b605e-133">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="b605e-134">Количество времени, пользователь будет заблокирован при возникновении блокировки.</span><span class="sxs-lookup"><span data-stu-id="b605e-134">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="b605e-135">5 минут</span><span class="sxs-lookup"><span data-stu-id="b605e-135">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="b605e-136">Число неудачных попыток доступа до пользователя будет заблокирована, если включена блокировка.</span><span class="sxs-lookup"><span data-stu-id="b605e-136">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="b605e-137">5</span><span class="sxs-lookup"><span data-stu-id="b605e-137">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="b605e-138">Определяет, если новый пользователь может быть заблокирован.</span><span class="sxs-lookup"><span data-stu-id="b605e-138">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="b605e-139">true</span><span class="sxs-lookup"><span data-stu-id="b605e-139">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="b605e-140">Параметры входа</span><span class="sxs-lookup"><span data-stu-id="b605e-140">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="b605e-141">`IdentityOptions.SignIn`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="b605e-141">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="b605e-142">Свойство.</span><span class="sxs-lookup"><span data-stu-id="b605e-142">Property</span></span>                | <span data-ttu-id="b605e-143">Описание:</span><span class="sxs-lookup"><span data-stu-id="b605e-143">Description</span></span>                       | <span data-ttu-id="b605e-144">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="b605e-144">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="b605e-145">Требует подтверждения электронной почты для входа.</span><span class="sxs-lookup"><span data-stu-id="b605e-145">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="b605e-146">False</span><span class="sxs-lookup"><span data-stu-id="b605e-146">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="b605e-147">Требуется номер телефона, подтвержденной для входа.</span><span class="sxs-lookup"><span data-stu-id="b605e-147">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="b605e-148">False</span><span class="sxs-lookup"><span data-stu-id="b605e-148">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="b605e-149">Параметры проверки пользователя</span><span class="sxs-lookup"><span data-stu-id="b605e-149">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="b605e-150">`IdentityOptions.User`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="b605e-150">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="b605e-151">Свойство.</span><span class="sxs-lookup"><span data-stu-id="b605e-151">Property</span></span>                | <span data-ttu-id="b605e-152">Описание:</span><span class="sxs-lookup"><span data-stu-id="b605e-152">Description</span></span>                       | <span data-ttu-id="b605e-153">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="b605e-153">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="b605e-154">Требуется иметь уникальный адрес электронной почты пользователя.</span><span class="sxs-lookup"><span data-stu-id="b605e-154">Requires each User to have a unique email.</span></span> | <span data-ttu-id="b605e-155">False</span><span class="sxs-lookup"><span data-stu-id="b605e-155">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="b605e-156">Допустимые символы в имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="b605e-156">Allowed characters in the username.</span></span> | <span data-ttu-id="b605e-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="b605e-157">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="b605e-158">Параметры файлов cookie приложения</span><span class="sxs-lookup"><span data-stu-id="b605e-158">Application's cookie settings</span></span>

<span data-ttu-id="b605e-159">Все параметры файла cookie приложения как политика паролей, можно изменить в `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="b605e-159">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b605e-160">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b605e-160">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b605e-161">В разделе `ConfigureServices` в `Startup` класса, можно настроить куки-файл приложения.</span><span class="sxs-lookup"><span data-stu-id="b605e-161">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b605e-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b605e-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="b605e-163">`CookieAuthenticationOptions`имеет следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="b605e-163">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="b605e-164">Свойство.</span><span class="sxs-lookup"><span data-stu-id="b605e-164">Property</span></span>                | <span data-ttu-id="b605e-165">Описание:</span><span class="sxs-lookup"><span data-stu-id="b605e-165">Description</span></span>                       | <span data-ttu-id="b605e-166">По умолчанию</span><span class="sxs-lookup"><span data-stu-id="b605e-166">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="b605e-167">Имя файла cookie.</span><span class="sxs-lookup"><span data-stu-id="b605e-167">The name of the cookie.</span></span>  | <span data-ttu-id="b605e-168">.AspNetCore.Cookies.</span><span class="sxs-lookup"><span data-stu-id="b605e-168">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="b605e-169">Если значение равно true, куки-файл не доступен из скриптов на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="b605e-169">When true, the cookie isn't accessible from client-side scripts.</span></span>  |  <span data-ttu-id="b605e-170">true</span><span class="sxs-lookup"><span data-stu-id="b605e-170">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="b605e-171">Определяет, сколько времени хранится билет проверки подлинности в файл cookie будет оставаться действительным с момента его создания.</span><span class="sxs-lookup"><span data-stu-id="b605e-171">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it's created.</span></span>  | <span data-ttu-id="b605e-172">14 дней</span><span class="sxs-lookup"><span data-stu-id="b605e-172">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="b605e-173">Если пользователь не авторизован, они будут перенаправляться по этому пути, имени входа.</span><span class="sxs-lookup"><span data-stu-id="b605e-173">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="b605e-174">/Account/Login</span><span class="sxs-lookup"><span data-stu-id="b605e-174">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="b605e-175">При выходе пользователя он будет перенаправлен по этому пути.</span><span class="sxs-lookup"><span data-stu-id="b605e-175">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="b605e-176">/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="b605e-176">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="b605e-177">При сбое проверка авторизации пользователя они будут перенаправляться к этому пути.</span><span class="sxs-lookup"><span data-stu-id="b605e-177">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="b605e-178">Если значение равно true, новый файл cookie будет выдан с новым окончанием срока действия текущего файла cookie при более половины срока.</span><span class="sxs-lookup"><span data-stu-id="b605e-178">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="b605e-179">/Account/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="b605e-179">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="b605e-180">Определяет имя параметра строки запроса, который добавляется по промежуточного слоя, когда код состояния 401 несанкционированный изменяется на перенаправление 302 пути входа.</span><span class="sxs-lookup"><span data-stu-id="b605e-180">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="b605e-181">true</span><span class="sxs-lookup"><span data-stu-id="b605e-181">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="b605e-182">Это относится только к ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="b605e-182">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="b605e-183">Логическое имя для проверки подлинности конкретной схемы.</span><span class="sxs-lookup"><span data-stu-id="b605e-183">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="b605e-184">Этот флаг относится только к ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="b605e-184">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="b605e-185">Если значение равно true, файл cookie проверки подлинности запустите при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана.</span><span class="sxs-lookup"><span data-stu-id="b605e-185">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |
