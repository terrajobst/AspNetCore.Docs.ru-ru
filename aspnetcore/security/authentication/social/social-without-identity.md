---
title: Facebook, Google и внешних поставщиков проверки подлинности без ASP.NET Core Identity
author: rick-anderson
description: Объяснение с помощью Facebook, Google, Twitter, и т.д. учетная запись проверки подлинности пользователя без ASP.NET Core Identity.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 1e7124e8b07c0faf2d005ec3ef55c0414a697d64
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561566"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Использовать проверку подлинности поставщиков социальных сетей вход без ASP.NET Core Identity

<xref:security/authentication/social/index> Описывает, как разрешить пользователям входить с помощью OAuth 2.0 с учетными данными внешних поставщиков проверки подлинности. Подход, описанный в этом разделе содержит удостоверение ASP.NET Core в качестве поставщика проверки подлинности.

В этом примере показано, как использовать внешний поставщик аутентификации **без** удостоверения ASP.NET Core. Это полезно для приложений, которые не требуются все возможности удостоверения ASP.NET Core, но по-прежнему требуется интеграция с доверенного внешнего поставщика проверки подлинности.

В этом примере используется [проверки подлинности Google](xref:security/authentication/google-logins) для проверки подлинности пользователей. С помощью Google проверки подлинности сдвигает многие из сложностей в управлении процесс входа в Google. Чтобы интегрировать с разных внешнего поставщика проверки подлинности, см. в разделах:

* [Проверка подлинности Facebook](xref:security/authentication/facebook-logins)
* [Проверка подлинности Майкрософт](xref:security/authentication/microsoft-logins)
* [Проверка подлинности Twitter](xref:security/authentication/twitter-logins)
* [Другие поставщики](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Параметр Configuration

В `ConfigureServices` метод, настройте схемы проверки подлинности приложения с `AddAuthentication`, `AddCookie` и `AddGoogle` методов:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

Вызов [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) задает приложения [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme). `DefaultScheme` Представляет собой схему по умолчанию, используемые следующие `HttpContext` методы расширения для проверки подлинности:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Установка приложения `DefaultScheme` для [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) («файлы cookie») настраивает приложение для использования файлов cookie в качестве схемы по умолчанию для этих методов расширения. Установка приложения <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> для [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) («Google») настраивает приложение для использования Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`. `DefaultChallengeScheme` переопределяет `DefaultScheme`. См. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> дополнительных свойств, которые переопределяют `DefaultScheme` при установке.

В `Configure` мы вызываем метод `UseAuthentication` метод, вызываемый по промежуточного слоя проверки подлинности, который задает `HttpContext.User` свойства. Вызовите `UseAuthentication` метод перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

Дополнительные сведения о схемы проверки подлинности и проверки подлинности файла cookie, см. в разделе <xref:security/authentication/cookie>.

## <a name="applying-authorization"></a>Применение авторизации

Проверить конфигурацию проверки подлинности приложения, применяя `AuthorizeAttribute` атрибут контроллера, действия или страницы. Следующий код ограничивает доступ к *конфиденциальности* страницы для пользователей, которые прошли проверку подлинности:

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Выйти

Чтобы выйти из системы текущего пользователя и удалить их файл cookie, вызовите [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0). Следующий код добавляет `Logout` обработчик страниц для *индекс* страницы:

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

Обратите внимание, что вызов `SignOutAsync` не указывает схему проверки подлинности. Приложения `DefaultScheme` из `CookieAuthenticationDefaults.AuthenticationScheme` используется на случай возможного отката обратно.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
