---
title: Пример файла cookie SameSite ASP.NET Core 2,1 MVC
author: rick-anderson
description: Пример файла cookie SameSite ASP.NET Core 2,1 MVC
monikerRange: '>= aspnetcore-2.1 < aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: security/samesite/mvc21
ms.openlocfilehash: 14f3d3d27d5740519e1ba57529d5694b4cdeb9ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654994"
---
# <a name="aspnet-core-21-mvc-samesite-cookie-sample"></a><span data-ttu-id="594e9-103">Пример файла cookie SameSite ASP.NET Core 2,1 MVC</span><span class="sxs-lookup"><span data-stu-id="594e9-103">ASP.NET Core 2.1 MVC SameSite cookie sample</span></span>

<span data-ttu-id="594e9-104">ASP.NET Core 2,1 имеет встроенную поддержку атрибута [SameSite](https://www.owasp.org/index.php/SameSite) , но она была записана в исходный стандарт.</span><span class="sxs-lookup"><span data-stu-id="594e9-104">ASP.NET Core 2.1 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it was written to the original standard.</span></span> <span data-ttu-id="594e9-105">[Исправленное поведение](https://github.com/dotnet/aspnetcore/issues/8212) изменило значение `SameSite.None`, чтобы выдать атрибут sameSite со значением `None`, а не создавать значение вообще.</span><span class="sxs-lookup"><span data-stu-id="594e9-105">The [patched behavior](https://github.com/dotnet/aspnetcore/issues/8212) changed the meaning of `SameSite.None` to emit the sameSite attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="594e9-106">Если вы хотите не выпустить значение, можно установить свойство `SameSite` для файла cookie равным-1.</span><span class="sxs-lookup"><span data-stu-id="594e9-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="594e9-107">Написание атрибута SameSite</span><span class="sxs-lookup"><span data-stu-id="594e9-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="594e9-108">Ниже приведен пример того, как записать атрибут SameSite в файл cookie:</span><span class="sxs-lookup"><span data-stu-id="594e9-108">Following is an example of how to write a SameSite attribute on a cookie:</span></span>

```c#
var cookieOptions = new CookieOptions
{
    // Set the secure flag, which Chrome's changes will require for SameSite none.
    // Note this will also require you to be running on HTTPS
    Secure = true,

    // Set the cookie to HTTP only which is good practice unless you really do need
    // to access it client side in scripts.
    HttpOnly = true,

    // Add the SameSite attribute, this will emit the attribute with a value of none.
    // To not emit the attribute at all set the SameSite property to (SameSiteMode)(-1).
    SameSite = SameSiteMode.None
};

// Add the cookie to the response cookie collection
Response.Cookies.Append(CookieName, "cookieValue", cookieOptions);
```

## <a name="setting-cookie-authentication-and-session-state-cookies"></a><span data-ttu-id="594e9-109">Настройка файлов cookie для проверки подлинности файлов cookie и состояния сеанса</span><span class="sxs-lookup"><span data-stu-id="594e9-109">Setting Cookie Authentication and Session State cookies</span></span>

<span data-ttu-id="594e9-110">Проверка подлинности файлов cookie, состояние сеанса и [различные другие компоненты](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) задают свои параметры sameSite с помощью параметров cookie, например</span><span class="sxs-lookup"><span data-stu-id="594e9-110">Cookie authentication, session state and [various other components](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) set their sameSite options via Cookie options, for example</span></span>

```c#
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.Cookie.SameSite = SameSiteMode.None;
        options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        options.Cookie.IsEssential = true;
    });

services.AddSession(options =>
{
    options.Cookie.SameSite = SameSiteMode.None;
    options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    options.Cookie.IsEssential = true;
});
```

<span data-ttu-id="594e9-111">В приведенном выше коде проверка подлинности файлов cookie и состояние сеанса устанавливают атрибут sameSite в значение `None`, выдает атрибут со значением `None`, а также устанавливает атрибут Secure в значение true.</span><span class="sxs-lookup"><span data-stu-id="594e9-111">In the preceding code, both cookie authentication and session state set their sameSite attribute to `None`, emitting the attribute with a `None` value, and also set the Secure attribute to true.</span></span>

### <a name="run-the-sample"></a><span data-ttu-id="594e9-112">Запуск примера</span><span class="sxs-lookup"><span data-stu-id="594e9-112">Run the sample</span></span>

<span data-ttu-id="594e9-113">При запуске [примера проекта](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)Загрузите отладчик браузера на начальной странице и используйте его для просмотра коллекции файлов cookie для сайта.</span><span class="sxs-lookup"><span data-stu-id="594e9-113">If you run the [sample project](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC), load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span> <span data-ttu-id="594e9-114">Чтобы сделать это на границе и Chrome, нажмите кнопку `F12` затем выберите вкладку `Application` и щелкните URL-адрес сайта в параметре `Cookies` в разделе `Storage`.</span><span class="sxs-lookup"><span data-stu-id="594e9-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Список файлов cookie отладчика браузера](BrowserDebugger.png)

<span data-ttu-id="594e9-116">На рисунке выше показано, что файл cookie, созданный при нажатии кнопки "создать SameSite cookie", имеет значение атрибута SameSite `Lax`, соответствующее значению, заданному в [образце кода](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="594e9-116">You can see from the image above that the cookie created by the sample when you click the "Create SameSite Cookie" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="594e9-117">Перехват файлов cookie</span><span class="sxs-lookup"><span data-stu-id="594e9-117">Intercepting cookies</span></span>

<span data-ttu-id="594e9-118">Чтобы перехватить файлы cookie, чтобы настроить значение None в соответствии с его поддержкой в агенте браузера пользователя, необходимо использовать по промежуточного слоя `CookiePolicy`.</span><span class="sxs-lookup"><span data-stu-id="594e9-118">In order to intercept cookies, to adjust the none value according to its support in the user's browser agent you must use the `CookiePolicy` middleware.</span></span> <span data-ttu-id="594e9-119">Он должен быть помещен в конвейер HTTP-запросов **перед** всеми компонентами, записывающими файлы cookie и настроенными в `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="594e9-119">This must be placed into the http request pipeline **before** any components that write cookies and configured within `ConfigureServices()`.</span></span>

<span data-ttu-id="594e9-120">Чтобы вставить его в конвейер, используйте `app.UseCookiePolicy()` в методе `Configure(IApplicationBuilder, IHostingEnvironment)` в [Startup.CS](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="594e9-120">To insert it into the pipeline use `app.UseCookiePolicy()` in the `Configure(IApplicationBuilder, IHostingEnvironment)` method in [Startup.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs).</span></span> <span data-ttu-id="594e9-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="594e9-121">For example:</span></span>

```c#
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
       app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="594e9-122">Затем в `ConfigureServices(IServiceCollection services)` настроить политику файлов cookie для вызова вспомогательного класса при добавлении или удалении файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="594e9-122">Then in the `ConfigureServices(IServiceCollection services)` configure the cookie policy to call out to a helper class when cookies are appended or deleted.</span></span> <span data-ttu-id="594e9-123">Пример:</span><span class="sxs-lookup"><span data-stu-id="594e9-123">For example:</span></span>

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        options.CheckConsentNeeded = context => true;
        options.MinimumSameSitePolicy = SameSiteMode.None;
        options.OnAppendCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
        options.OnDeleteCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    });
}

private void CheckSameSite(HttpContext httpContext, CookieOptions options)
{
    if (options.SameSite == SameSiteMode.None)
    {
        var userAgent = httpContext.Request.Headers["User-Agent"].ToString();
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            options.SameSite = (SameSiteMode)(-1);
        }
    }
}
```

<span data-ttu-id="594e9-124">Вспомогательная функция `CheckSameSite(HttpContext, CookieOptions)`:</span><span class="sxs-lookup"><span data-stu-id="594e9-124">The helper function `CheckSameSite(HttpContext, CookieOptions)`:</span></span>

* <span data-ttu-id="594e9-125">Вызывается, когда файлы cookie добавляются в запрос или удаляются из запроса.</span><span class="sxs-lookup"><span data-stu-id="594e9-125">Is called when cookies are appended to the request or deleted from the request.</span></span>
* <span data-ttu-id="594e9-126">Проверяет, имеет ли свойство `SameSite` значение `None`.</span><span class="sxs-lookup"><span data-stu-id="594e9-126">Checks to see if the `SameSite` property is set to `None`.</span></span>
* <span data-ttu-id="594e9-127">Если `SameSite` имеет значение `None` и известно, что агент текущего пользователя не поддерживает значение атрибута None.</span><span class="sxs-lookup"><span data-stu-id="594e9-127">If `SameSite` is set to `None` and the current user agent is known to not support the none attribute value.</span></span> <span data-ttu-id="594e9-128">Проверка выполняется с помощью класса [самеситесуппорт](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) :</span><span class="sxs-lookup"><span data-stu-id="594e9-128">The check is done using the [SameSiteSupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) class:</span></span>
  * <span data-ttu-id="594e9-129">Задает `SameSite`, чтобы не выдавало значение, задав для свойства `(SameSiteMode)(-1)`</span><span class="sxs-lookup"><span data-stu-id="594e9-129">Sets `SameSite` to not emit the value by setting the property to `(SameSiteMode)(-1)`</span></span>

## <a name="targeting-net-framework"></a><span data-ttu-id="594e9-130">Нацеливание на .NET Framework</span><span class="sxs-lookup"><span data-stu-id="594e9-130">Targeting .NET Framework</span></span>

<span data-ttu-id="594e9-131">ASP.NET Core и System. Web (классическая модель ASP.NET) имеют независимые реализации SameSite.</span><span class="sxs-lookup"><span data-stu-id="594e9-131">ASP.NET Core and System.Web (ASP.NET Classic) have independent implementations of SameSite.</span></span> <span data-ttu-id="594e9-132">Исправления SameSite KB для .NET Framework не требуются, если используется ASP.NET Core и не является требованием к версии System. Web SameSite минимальной платформы (.NET 4.7.2), применяемым к ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="594e9-132">The SameSite KB patches for .NET Framework are not required if using ASP.NET Core nor does the System.Web SameSite minimum framework version requirement (.NET 4.7.2) apply to ASP.NET Core.</span></span>

<span data-ttu-id="594e9-133">ASP.NET Core в .NET требует обновления зависимостей пакетов NuGet для получения соответствующих исправлений.</span><span class="sxs-lookup"><span data-stu-id="594e9-133">ASP.NET Core on .NET requires updating nuget package dependencies to get the appropriate fixes.</span></span>

<span data-ttu-id="594e9-134">Чтобы получить ASP.NET Core изменения для .NET Framework убедитесь в наличии прямой ссылки на исправленные пакеты и версии (2.1.14 или более поздней версии 2,1).</span><span class="sxs-lookup"><span data-stu-id="594e9-134">To get the ASP.NET Core changes for .NET Framework ensure that you have a direct reference to the patched packages and versions (2.1.14 or later 2.1 versions).</span></span>

```xml
<PackageReference Include="Microsoft.Net.Http.Headers" Version="2.1.14" />
<PackageReference Include="Microsoft.AspNetCore.CookiePolicy" Version="2.1.14" />
```

### <a name="more-information"></a><span data-ttu-id="594e9-135">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="594e9-135">More Information</span></span>
 
<span data-ttu-id="594e9-136">[Обновления Chrome](https://www.chromium.org/updates/same-site)
[ASP.NET Core документация SameSite](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2,1 SameSite объявление об изменениях](https://github.com/dotnet/aspnetcore/issues/8212)</span><span class="sxs-lookup"><span data-stu-id="594e9-136">[Chrome Updates](https://www.chromium.org/updates/same-site)
[ASP.NET Core SameSite Documentation](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2.1 SameSite Change Announcement](https://github.com/dotnet/aspnetcore/issues/8212)</span></span>