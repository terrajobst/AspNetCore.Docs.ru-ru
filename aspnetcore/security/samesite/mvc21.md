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
# <a name="aspnet-core-21-mvc-samesite-cookie-sample"></a>Пример файла cookie SameSite ASP.NET Core 2,1 MVC

ASP.NET Core 2,1 имеет встроенную поддержку атрибута [SameSite](https://www.owasp.org/index.php/SameSite) , но она была записана в исходный стандарт. [Исправленное поведение](https://github.com/dotnet/aspnetcore/issues/8212) изменило значение `SameSite.None`, чтобы выдать атрибут sameSite со значением `None`, а не создавать значение вообще. Если вы хотите не выпустить значение, можно установить свойство `SameSite` для файла cookie равным-1.

## <a name="sampleCode"></a>Написание атрибута SameSite

Ниже приведен пример того, как записать атрибут SameSite в файл cookie:

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

## <a name="setting-cookie-authentication-and-session-state-cookies"></a>Настройка файлов cookie для проверки подлинности файлов cookie и состояния сеанса

Проверка подлинности файлов cookie, состояние сеанса и [различные другие компоненты](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) задают свои параметры sameSite с помощью параметров cookie, например

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

В приведенном выше коде проверка подлинности файлов cookie и состояние сеанса устанавливают атрибут sameSite в значение `None`, выдает атрибут со значением `None`, а также устанавливает атрибут Secure в значение true.

### <a name="run-the-sample"></a>Запуск примера

При запуске [примера проекта](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)Загрузите отладчик браузера на начальной странице и используйте его для просмотра коллекции файлов cookie для сайта. Чтобы сделать это на границе и Chrome, нажмите кнопку `F12` затем выберите вкладку `Application` и щелкните URL-адрес сайта в параметре `Cookies` в разделе `Storage`.

![Список файлов cookie отладчика браузера](BrowserDebugger.png)

На рисунке выше показано, что файл cookie, созданный при нажатии кнопки "создать SameSite cookie", имеет значение атрибута SameSite `Lax`, соответствующее значению, заданному в [образце кода](#sampleCode).

## <a name="interception"></a>Перехват файлов cookie

Чтобы перехватить файлы cookie, чтобы настроить значение None в соответствии с его поддержкой в агенте браузера пользователя, необходимо использовать по промежуточного слоя `CookiePolicy`. Он должен быть помещен в конвейер HTTP-запросов **перед** всеми компонентами, записывающими файлы cookie и настроенными в `ConfigureServices()`.

Чтобы вставить его в конвейер, используйте `app.UseCookiePolicy()` в методе `Configure(IApplicationBuilder, IHostingEnvironment)` в [Startup.CS](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs). Пример:

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

Затем в `ConfigureServices(IServiceCollection services)` настроить политику файлов cookie для вызова вспомогательного класса при добавлении или удалении файлов cookie. Пример:

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

Вспомогательная функция `CheckSameSite(HttpContext, CookieOptions)`:

* Вызывается, когда файлы cookie добавляются в запрос или удаляются из запроса.
* Проверяет, имеет ли свойство `SameSite` значение `None`.
* Если `SameSite` имеет значение `None` и известно, что агент текущего пользователя не поддерживает значение атрибута None. Проверка выполняется с помощью класса [самеситесуппорт](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) :
  * Задает `SameSite`, чтобы не выдавало значение, задав для свойства `(SameSiteMode)(-1)`

## <a name="targeting-net-framework"></a>Нацеливание на .NET Framework

ASP.NET Core и System. Web (классическая модель ASP.NET) имеют независимые реализации SameSite. Исправления SameSite KB для .NET Framework не требуются, если используется ASP.NET Core и не является требованием к версии System. Web SameSite минимальной платформы (.NET 4.7.2), применяемым к ASP.NET Core.

ASP.NET Core в .NET требует обновления зависимостей пакетов NuGet для получения соответствующих исправлений.

Чтобы получить ASP.NET Core изменения для .NET Framework убедитесь в наличии прямой ссылки на исправленные пакеты и версии (2.1.14 или более поздней версии 2,1).

```xml
<PackageReference Include="Microsoft.Net.Http.Headers" Version="2.1.14" />
<PackageReference Include="Microsoft.AspNetCore.CookiePolicy" Version="2.1.14" />
```

### <a name="more-information"></a>Дополнительные сведения
 
[Обновления Chrome](https://www.chromium.org/updates/same-site)
[ASP.NET Core документация SameSite](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2,1 SameSite объявление об изменениях](https://github.com/dotnet/aspnetcore/issues/8212)