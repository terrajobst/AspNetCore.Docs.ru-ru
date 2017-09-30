---
title: "С помощью файла Cookie проверки подлинности без удостоверения ASP.NET Core"
author: rick-anderson
description: "Описание использования файлов cookie проверки подлинности без ASP.NET Core Identity"
keywords: "ASP.NET Core, файлы cookie"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: af3ffe418521d5d97f5d14ca9c904c21b4d4ff89
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="ce46c-104">С помощью файла Cookie проверки подлинности без удостоверения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce46c-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<a name="security-authentication-cookie-middleware"></a>

<span data-ttu-id="ce46c-105">ASP.NET Core 1.x указывает куки-файл [по промежуточного слоя](../../fundamentals/middleware.md#fundamentals-middleware) которого сериализует участника-пользователя в зашифрованном файле cookie, а затем, при последующих запросах проводится проверка куки-файл, повторно создает основной и присваивает его `HttpContext.User` свойство .</span><span class="sxs-lookup"><span data-stu-id="ce46c-105">ASP.NET Core 1.x provides cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal, and assigns it to the `HttpContext.User` property.</span></span> <span data-ttu-id="ce46c-106">Если вы хотите предоставить экраны входа и пользовательские базы данных, можно использовать файл cookie по промежуточного слоя изолированно.</span><span class="sxs-lookup"><span data-stu-id="ce46c-106">If you want to provide your own login screens and user databases, you can use the cookie middleware as a standalone feature.</span></span>

<span data-ttu-id="ce46c-107">Значительное изменение в ASP.NET Core 2.x — что по промежуточного слоя куки-файл не существует.</span><span class="sxs-lookup"><span data-stu-id="ce46c-107">A major change in ASP.NET Core 2.x is that the cookie middleware is absent.</span></span> <span data-ttu-id="ce46c-108">Вместо этого `UseAuthentication` вызов метода в `Configure` метод *файла Startup.cs* добавляет AuthenticationMiddleware, который задает `HttpContext.User` свойства.</span><span class="sxs-lookup"><span data-stu-id="ce46c-108">Instead, the `UseAuthentication` method invocation in the `Configure` method of *Startup.cs* adds the AuthenticationMiddleware which sets the `HttpContext.User` property.</span></span>

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a><span data-ttu-id="ce46c-109">Добавление и настройка</span><span class="sxs-lookup"><span data-stu-id="ce46c-109">Adding and configuring</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce46c-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ce46c-111">Выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="ce46c-111">Complete the following steps:</span></span>

- <span data-ttu-id="ce46c-112">Если не используется `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), установите версию 2.0 + `Microsoft.AspNetCore.Authentication.Cookies` пакета NuGet в проекте.</span><span class="sxs-lookup"><span data-stu-id="ce46c-112">If not using the `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), install version 2.0+ of the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span>

- <span data-ttu-id="ce46c-113">Вызвать `UseAuthentication` метод в `Configure` метод *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="ce46c-113">Invoke the `UseAuthentication` method in the `Configure` method of the *Startup.cs* file:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ce46c-114">Вызвать `AddAuthentication` и `AddCookie` методы в `ConfigureServices` метод *файла Startup.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="ce46c-114">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method of the *Startup.cs* file:</span></span>

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie("MyCookieAuthenticationScheme", options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce46c-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ce46c-116">Выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="ce46c-116">Complete the following steps:</span></span>

- <span data-ttu-id="ce46c-117">Установить `Microsoft.AspNetCore.Authentication.Cookies` пакет NuGet в проекте.</span><span class="sxs-lookup"><span data-stu-id="ce46c-117">Install the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span> <span data-ttu-id="ce46c-118">Этот пакет содержит файл cookie по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="ce46c-118">This package contains the cookie middleware.</span></span>

- <span data-ttu-id="ce46c-119">Добавьте следующие строки для `Configure` метод в вашей *файла Startup.cs* файла перед `app.UseMvc()` инструкции:</span><span class="sxs-lookup"><span data-stu-id="ce46c-119">Add the following lines to the `Configure` method in your *Startup.cs* file before the `app.UseMvc()` statement:</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

<span data-ttu-id="ce46c-120">Фрагменты кода выше настроить некоторые или все из следующих вариантов:</span><span class="sxs-lookup"><span data-stu-id="ce46c-120">The code snippets above configure some or all of the following options:</span></span>

* <span data-ttu-id="ce46c-121">`AccessDeniedPath`— Это относительный путь, к которому перенаправления запросов, если пользователь пытается получить доступ к ресурсу, но не передает [политики авторизации](xref:security/authorization/policies#security-authorization-policies-based) для этого ресурса.</span><span class="sxs-lookup"><span data-stu-id="ce46c-121">`AccessDeniedPath` - This is the relative path to which requests redirect when a user attempts to access a resource but does not pass any [authorization policies](xref:security/authorization/policies#security-authorization-policies-based) for that resource.</span></span>

* <span data-ttu-id="ce46c-122">`AuthenticationScheme`-Это значение, по которой происходит обращение схему определенного файла cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="ce46c-122">`AuthenticationScheme` - This is a value by which a particular cookie authentication scheme is known.</span></span> <span data-ttu-id="ce46c-123">Это полезно при наличии нескольких экземпляров файла cookie проверки подлинности, и вы хотите [применять проверку подлинности только один экземпляр](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).</span><span class="sxs-lookup"><span data-stu-id="ce46c-123">This is useful when there are multiple instances of cookie authentication and you want to [limit authorization to one instance](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).</span></span>

* <span data-ttu-id="ce46c-124">`AutomaticAuthenticate`-Этот флаг действителен только для ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="ce46c-124">`AutomaticAuthenticate` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="ce46c-125">Указывает на то файла cookie проверки подлинности следует запустить при каждом запросе и попытка проверки и воссоздания любому участнику сериализованный она создана.</span><span class="sxs-lookup"><span data-stu-id="ce46c-125">It indicates that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

* <span data-ttu-id="ce46c-126">`AutomaticChallenge`-Этот флаг действителен только для ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="ce46c-126">`AutomaticChallenge` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="ce46c-127">Указывает, что файл cookie проверки подлинности 1.x следует перенаправить браузер `LoginPath` или `AccessDeniedPath` при сбое авторизации.</span><span class="sxs-lookup"><span data-stu-id="ce46c-127">It indicates that the 1.x cookie authentication should redirect the browser to the `LoginPath` or `AccessDeniedPath` when authorization fails.</span></span>

* <span data-ttu-id="ce46c-128">`LoginPath`— Это относительный путь, по которому перенаправить запросы, когда пользователь пытается получить доступ к ресурсу, но не прошел проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="ce46c-128">`LoginPath` - This is the relative path to which requests redirect when a user attempts to access a resource but has not been authenticated.</span></span>

<span data-ttu-id="ce46c-129">[Другие параметры](xref:security/authentication/cookie#security-authentication-cookie-options) включает возможность устанавливать издатель все утверждения, создает файл cookie проверки подлинности, имя файла cookie проверки подлинности удаляет домен для файла cookie и различные свойства безопасности в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-129">[Other options](xref:security/authentication/cookie#security-authentication-cookie-options) include the ability to set the issuer for any claims the cookie authentication creates, the name of the cookie the authentication drops, the domain for the cookie and various security properties on the cookie.</span></span> <span data-ttu-id="ce46c-130">По умолчанию файл cookie проверки подлинности использует соответствующие параметры защиты для все файлы cookie, который создается, такие как:</span><span class="sxs-lookup"><span data-stu-id="ce46c-130">By default, the cookie authentication uses appropriate security options for any cookies it creates, such as:</span></span>
- <span data-ttu-id="ce46c-131">Установка флажка HttpOnly, чтобы предотвратить доступ к файлам cookie в JavaScript на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="ce46c-131">Setting the HttpOnly flag to prevent cookie access in client-side JavaScript</span></span>
- <span data-ttu-id="ce46c-132">Ограничение куки-файл для HTTPS, если запрос прошел по протоколу HTTPS</span><span class="sxs-lookup"><span data-stu-id="ce46c-132">Limiting the cookie to HTTPS if a request has traveled over HTTPS</span></span>

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a><span data-ttu-id="ce46c-133">Создание файла cookie удостоверений</span><span class="sxs-lookup"><span data-stu-id="ce46c-133">Creating an Identity cookie</span></span>

<span data-ttu-id="ce46c-134">Чтобы создать файл cookie, содержащий сведения о пользователе, следует создать [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) с информацией, нужно сериализовать в файл cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-134">To create a cookie holding your user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) holding the information you wish to be serialized in the cookie.</span></span> <span data-ttu-id="ce46c-135">При наличии подходящего `ClaimsPrincipal` , следует вызвать следующий внутри метода контроллера:</span><span class="sxs-lookup"><span data-stu-id="ce46c-135">Once you have a suitable `ClaimsPrincipal` object, call the following inside your controller method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce46c-136">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce46c-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

<span data-ttu-id="ce46c-138">Это создает зашифрованный файл cookie и добавляет его в текущий ответ.</span><span class="sxs-lookup"><span data-stu-id="ce46c-138">This creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="ce46c-139">`AuthenticationScheme` Указано при [конфигурации](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) должен использоваться при вызове `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-139">The `AuthenticationScheme` specified during [configuration](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) must be used when calling `SignInAsync`.</span></span>

<span data-ttu-id="ce46c-140">На самом деле шифрования, используемый — ASP.NET Core [защиты данных](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) системы.</span><span class="sxs-lookup"><span data-stu-id="ce46c-140">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="ce46c-141">Если имеются на нескольких компьютерах, нагрузки балансировки или с помощью веб-фермы, то необходимо [настроить защиту данных](xref:security/data-protection/configuration/overview#data-protection-configuring) использовать же ключей и идентификатор приложения.</span><span class="sxs-lookup"><span data-stu-id="ce46c-141">If you are hosting on multiple machines, load balancing, or using a web farm, then you need to [configure data protection](xref:security/data-protection/configuration/overview#data-protection-configuring) to use the same key ring and application identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="ce46c-142">Выход</span><span class="sxs-lookup"><span data-stu-id="ce46c-142">Signing out</span></span>

<span data-ttu-id="ce46c-143">Чтобы выйти из системы текущего пользователя и удалить их куки-файл, следует вызвать следующий в свой контроллер:</span><span class="sxs-lookup"><span data-stu-id="ce46c-143">To sign out the current user and delete their cookie, call the following inside your controller:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce46c-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce46c-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="ce46c-146">Отклик на изменения в серверной части</span><span class="sxs-lookup"><span data-stu-id="ce46c-146">Reacting to back-end changes</span></span>

>[!WARNING]
> <span data-ttu-id="ce46c-147">После создания основного файла cookie становится единственным источником удостоверения.</span><span class="sxs-lookup"><span data-stu-id="ce46c-147">Once a principal cookie has been created, it becomes the single source of identity.</span></span> <span data-ttu-id="ce46c-148">Даже при отключении пользователя в серверной части системы проверки подлинности файла cookie не имеет сведений о это и пользователь остается вошедший в систему, при условии, что их файл cookie является допустимым.</span><span class="sxs-lookup"><span data-stu-id="ce46c-148">Even if you disable a user in your back-end systems, the cookie authentication has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="ce46c-149">Файл cookie проверки подлинности предоставляет ряд событий в классе параметра.</span><span class="sxs-lookup"><span data-stu-id="ce46c-149">The cookie authentication provides a series of events in its option class.</span></span> <span data-ttu-id="ce46c-150">`ValidateAsync()` Событий можно использовать для перехвата и переопределение проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-150">The `ValidateAsync()` event can be used to intercept and override validation of the cookie identity.</span></span>

<span data-ttu-id="ce46c-151">Рассмотрим базу данных пользователя серверной части, возможно, столбец «LastChanged».</span><span class="sxs-lookup"><span data-stu-id="ce46c-151">Consider a back-end user database that may have a "LastChanged" column.</span></span> <span data-ttu-id="ce46c-152">Чтобы сделать недействительными файла cookie при изменениях базы данных, следует сначала, если [Создание куки-файл](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), добавить утверждение «LastChanged», содержащую текущее значение.</span><span class="sxs-lookup"><span data-stu-id="ce46c-152">In order to invalidate a cookie when the database changes, you should first, when [creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), add a "LastChanged" claim containing the current value.</span></span> <span data-ttu-id="ce46c-153">При изменении базы данных, значение «LastChanged» должен быть обновлен.</span><span class="sxs-lookup"><span data-stu-id="ce46c-153">When the database changes, the "LastChanged" value should be updated.</span></span>

<span data-ttu-id="ce46c-154">Чтобы реализовать переопределение для `ValidateAsync()` событий, необходимо написать метод со следующей сигнатурой:</span><span class="sxs-lookup"><span data-stu-id="ce46c-154">To implement an override for the `ValidateAsync()` event, you must write a method with the following signature:</span></span>

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

<span data-ttu-id="ce46c-155">ASP.NET Core Identity реализует эту проверку как часть его `SecurityStampValidator`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-155">ASP.NET Core Identity implements this check as part of its `SecurityStampValidator`.</span></span> <span data-ttu-id="ce46c-156">Пример выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ce46c-156">An example looks like the following:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce46c-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="ce46c-158">Это может затем быть реализовано во время регистрации службы куки-файл в `ConfigureServices` метод *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce46c-158">This would then be wired up during cookie service registration in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce46c-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="ce46c-160">Это может затем быть реализовано во время настройки файла cookie проверки подлинности в `Configure` метод *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce46c-160">This would then be wired up during cookie authentication configuration in the `Configure` method of *Startup.cs*:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="ce46c-161">Рассмотрим пример, в котором был обновлен их имя &mdash; принятия решений, которая не влияет на безопасность системы никаким образом.</span><span class="sxs-lookup"><span data-stu-id="ce46c-161">Consider the example in which their name has been updated &mdash; a decision which doesn't affect security in any way.</span></span> <span data-ttu-id="ce46c-162">Если вы хотите безболезненно обновить участника-пользователя, можно вызвать `context.ReplacePrincipal()` и задайте `context.ShouldRenew` свойства `true`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-162">If you want to non-destructively update the user principal, you can call `context.ReplacePrincipal()` and set the `context.ShouldRenew` property to `true`.</span></span>

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a><span data-ttu-id="ce46c-163">Управление параметрами куки-файл</span><span class="sxs-lookup"><span data-stu-id="ce46c-163">Controlling cookie options</span></span>

<span data-ttu-id="ce46c-164">[CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) класс включает различные параметры конфигурации для точной настройки создаваемого файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-164">The [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) class comes with various configuration options to fine-tune the cookies being created.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce46c-165">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ce46c-166">ASP.NET Core 2.x объединяет API, используемые для настройки файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-166">ASP.NET Core 2.x unifies the APIs used for configuring cookies.</span></span> <span data-ttu-id="ce46c-167">1.x, API-интерфейсы были помечены как устаревшие, а новые `Cookie` свойство типа `CookieBuilder` была введена в `CookieAuthenticationOptions` класса.</span><span class="sxs-lookup"><span data-stu-id="ce46c-167">The 1.x APIs have been marked as obsolete, and a new `Cookie` property of type `CookieBuilder` has been introduced in the `CookieAuthenticationOptions` class.</span></span> <span data-ttu-id="ce46c-168">Рекомендуется выполнить миграцию 2.x API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="ce46c-168">It's recommended that you migrate to the 2.x APIs.</span></span>

* <span data-ttu-id="ce46c-169">`ClaimsIssuer`является издателем для [издателя](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) свойство все утверждения, созданные с помощью проверки подлинности файла cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-169">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication.</span></span>

* <span data-ttu-id="ce46c-170">`CookieBuilder.Domain`— Это имя домена, на который обслуживается куки-файл.</span><span class="sxs-lookup"><span data-stu-id="ce46c-170">`CookieBuilder.Domain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="ce46c-171">По умолчанию это имя узла, которому отправляется запрос.</span><span class="sxs-lookup"><span data-stu-id="ce46c-171">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="ce46c-172">Браузер служит только файла cookie для сопоставления имени узла.</span><span class="sxs-lookup"><span data-stu-id="ce46c-172">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="ce46c-173">Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="ce46c-173">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="ce46c-174">Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`и т. д.</span><span class="sxs-lookup"><span data-stu-id="ce46c-174">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="ce46c-175">`CookieBuilder.HttpOnly`флаг, указывающий, куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="ce46c-175">`CookieBuilder.HttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="ce46c-176">По умолчанию используется `true`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-176">This defaults to `true`.</span></span> <span data-ttu-id="ce46c-177">Изменение этого значения может открыть приложение для кражи cookie приложения должны иметь ошибки межсайтовых сценариев.</span><span class="sxs-lookup"><span data-stu-id="ce46c-177">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="ce46c-178">`CookieBuilder.Path`можно использовать для изоляции приложений, выполняющихся на то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="ce46c-178">`CookieBuilder.Path` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="ce46c-179">Если у вас есть приложение, работающее в `/app1` ограничить файлы cookie, выданный для только что отправлен на это приложение, затем следует установить `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-179">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="ce46c-180">Таким образом, куки-файл доступен только для запросов к `/app1` или каком-либо под ним.</span><span class="sxs-lookup"><span data-stu-id="ce46c-180">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="ce46c-181">`CookieBuilder.SameSite`Указывает, является ли браузер должен позволять куки-файл для подключения к веб-сайте или межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="ce46c-181">`CookieBuilder.SameSite` indicates whether the browser should allow the cookie to be attached to same-site or cross-site requests.</span></span> <span data-ttu-id="ce46c-182">По умолчанию используется `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-182">This defaults to `SameSiteMode.Lax`.</span></span>

* <span data-ttu-id="ce46c-183">`CookieBuilder.SecurePolicy`флаг, указывающий, если созданный файл cookie был ограничен HTTPS, HTTP или HTTPS или тот же протокол, что и при запросе.</span><span class="sxs-lookup"><span data-stu-id="ce46c-183">`CookieBuilder.SecurePolicy` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="ce46c-184">По умолчанию используется `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-184">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="ce46c-185">`ExpireTimeSpan`— `TimeSpan` после которого истекает срок действия cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-185">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="ce46c-186">Он добавляется в текущую дату и время создания срока действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-186">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="ce46c-187">`SlidingExpiration`флаг, указывающий, сбрасывается ли дата окончания срока действия файла cookie при более половины является `ExpireTimeSpan` истечет интервал.</span><span class="sxs-lookup"><span data-stu-id="ce46c-187">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="ce46c-188">Новую дату окончания срока действия перемещается вперед для текущей даты, а также `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-188">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="ce46c-189">[Абсолютный срок действия](xref:security/authentication/cookie#security-authentication-absolute-expiry) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-189">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="ce46c-190">Абсолютного срока действия может повысить безопасность ваших приложений, ограничивая количество времени, для которого действителен cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="ce46c-190">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="ce46c-191">Пример использования `CookieAuthenticationOptions` в `ConfigureServices` метод *файла Startup.cs* ниже:</span><span class="sxs-lookup"><span data-stu-id="ce46c-191">An example of using `CookieAuthenticationOptions` in the `ConfigureServices` method of *Startup.cs* follows:</span></span>

```csharp
services.AddAuthentication()
        .AddCookie(options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce46c-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="ce46c-193">`ClaimsIssuer`является издателем для [издателя](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) свойство все утверждения, созданные по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="ce46c-193">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the middleware.</span></span>

* <span data-ttu-id="ce46c-194">`CookieDomain`— Это имя домена, на который обслуживается куки-файл.</span><span class="sxs-lookup"><span data-stu-id="ce46c-194">`CookieDomain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="ce46c-195">По умолчанию это имя узла, которому отправляется запрос.</span><span class="sxs-lookup"><span data-stu-id="ce46c-195">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="ce46c-196">Браузер служит только файла cookie для сопоставления имени узла.</span><span class="sxs-lookup"><span data-stu-id="ce46c-196">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="ce46c-197">Вы можете настроить этот параметр для файлов cookie, доступных любому узлу в вашем домене.</span><span class="sxs-lookup"><span data-stu-id="ce46c-197">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="ce46c-198">Например, установите значение домена файла cookie `.contoso.com` делает ее доступной для `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`и т. д.</span><span class="sxs-lookup"><span data-stu-id="ce46c-198">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="ce46c-199">`CookieHttpOnly`флаг, указывающий, куки-файл должен быть доступен только на серверы.</span><span class="sxs-lookup"><span data-stu-id="ce46c-199">`CookieHttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="ce46c-200">По умолчанию используется `true`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-200">This defaults to `true`.</span></span> <span data-ttu-id="ce46c-201">Изменение этого значения может открыть приложение для кражи cookie приложения должны иметь ошибки межсайтовых сценариев.</span><span class="sxs-lookup"><span data-stu-id="ce46c-201">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="ce46c-202">`CookiePath`можно использовать для изоляции приложений, выполняющихся на то же имя узла.</span><span class="sxs-lookup"><span data-stu-id="ce46c-202">`CookiePath` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="ce46c-203">Если у вас есть приложение, работающее в `/app1` ограничить файлы cookie, выданный для только что отправлен на это приложение, затем следует установить `CookiePath` свойства `/app1`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-203">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="ce46c-204">Таким образом, куки-файл доступен только для запросов к `/app1` или каком-либо под ним.</span><span class="sxs-lookup"><span data-stu-id="ce46c-204">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="ce46c-205">`CookieSecure`флаг, указывающий, если созданный файл cookie был ограничен HTTPS, HTTP или HTTPS или тот же протокол, что и при запросе.</span><span class="sxs-lookup"><span data-stu-id="ce46c-205">`CookieSecure` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="ce46c-206">По умолчанию используется `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-206">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="ce46c-207">`ExpireTimeSpan`— `TimeSpan` после которого истекает срок действия cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-207">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="ce46c-208">Он добавляется в текущую дату и время создания срока действия файла cookie.</span><span class="sxs-lookup"><span data-stu-id="ce46c-208">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="ce46c-209">`SlidingExpiration`флаг, указывающий, сбрасывается ли дата окончания срока действия файла cookie при более половины является `ExpireTimeSpan` истечет интервал.</span><span class="sxs-lookup"><span data-stu-id="ce46c-209">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="ce46c-210">Новую дату окончания срока действия перемещается вперед для текущей даты, а также `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-210">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="ce46c-211">[Абсолютный срок действия](xref:security/authentication/cookie#security-authentication-absolute-expiry) можно задать с помощью `AuthenticationProperties` класса при вызове `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="ce46c-211">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="ce46c-212">Абсолютного срока действия может повысить безопасность ваших приложений, ограничивая количество времени, для которого действителен cookie проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="ce46c-212">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="ce46c-213">Пример использования `CookieAuthenticationOptions` в `Configure` метод *файла Startup.cs* ниже:</span><span class="sxs-lookup"><span data-stu-id="ce46c-213">An example of using `CookieAuthenticationOptions` in the `Configure` method of *Startup.cs* follows:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a><span data-ttu-id="ce46c-214">Сохраняемые файлы cookie и время абсолютного срока действия</span><span class="sxs-lookup"><span data-stu-id="ce46c-214">Persistent cookies and absolute expiry times</span></span>

<span data-ttu-id="ce46c-215">Вы можете истечения срока действия файла cookie, и сохраняются между сеансами браузера абсолютного срока действия для удостоверения и передает его куки-файл.</span><span class="sxs-lookup"><span data-stu-id="ce46c-215">You may want the cookie expiry to persist across browser sessions and want an absolute expiry to the identity and the cookie transporting it.</span></span> <span data-ttu-id="ce46c-216">Это сохраняемости должны включаться только с согласия пользователя, через флажка «Запомнить мои» на имя входа или аналогичного механизма.</span><span class="sxs-lookup"><span data-stu-id="ce46c-216">This persistence should only be enabled with explicit user consent, via a "Remember Me" checkbox on login or a similar mechanism.</span></span> <span data-ttu-id="ce46c-217">Это можно сделать с помощью `AuthenticationProperties` параметр на `SignInAsync` метод вызывается, когда [подписи в удостоверении и создание файла cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span><span class="sxs-lookup"><span data-stu-id="ce46c-217">You can do these things by using the `AuthenticationProperties` parameter on the `SignInAsync` method called when [signing in an identity and creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span></span> <span data-ttu-id="ce46c-218">Пример:</span><span class="sxs-lookup"><span data-stu-id="ce46c-218">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce46c-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="ce46c-220">`AuthenticationProperties` Класс, используемый в предыдущем фрагменте кода находится в `Microsoft.AspNetCore.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="ce46c-220">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce46c-221">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-221">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="ce46c-222">`AuthenticationProperties` Класс, используемый в предыдущем фрагменте кода находится в `Microsoft.AspNetCore.Http.Authentication` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="ce46c-222">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

<span data-ttu-id="ce46c-223">В предыдущем фрагменте кода создает удостоверение и соответствующий файл cookie, который сохраняется до закрытия браузера.</span><span class="sxs-lookup"><span data-stu-id="ce46c-223">The preceding code snippet creates an identity and corresponding cookie which survives through browser closures.</span></span> <span data-ttu-id="ce46c-224">Параметры скользящего срока действия ранее настроены с помощью [параметры cookie](xref:security/authentication/cookie#security-authentication-cookie-options) по-прежнему соблюдаются.</span><span class="sxs-lookup"><span data-stu-id="ce46c-224">Any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options) are still honored.</span></span> <span data-ttu-id="ce46c-225">После истечения срока действия файла cookie, хотя браузер будет закрыт, браузер удаляет его, после его перезапуска.</span><span class="sxs-lookup"><span data-stu-id="ce46c-225">If the cookie expires whilst the browser is closed, the browser clears it once it is restarted.</span></span>

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce46c-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce46c-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce46c-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

<span data-ttu-id="ce46c-228">В предыдущем фрагменте кода создает удостоверение и соответствующий файл cookie, который сохраняется в течение 20 минут.</span><span class="sxs-lookup"><span data-stu-id="ce46c-228">The preceding code snippet creates an identity and corresponding cookie which lasts for 20 minutes.</span></span> <span data-ttu-id="ce46c-229">Это не учитывает параметры скользящего срока действия ранее настроены с помощью [параметры cookie](xref:security/authentication/cookie#security-authentication-cookie-options).</span><span class="sxs-lookup"><span data-stu-id="ce46c-229">This ignores any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options).</span></span>

<span data-ttu-id="ce46c-230">`ExpiresUtc` И `IsPersistent` свойства являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="ce46c-230">The `ExpiresUtc` and `IsPersistent` properties are mutually exclusive.</span></span>
