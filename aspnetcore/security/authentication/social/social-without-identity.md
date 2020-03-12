---
title: Аутентификация Facebook, Google и внешнего поставщика без удостоверения ASP.NET Core
author: rick-anderson
description: Объяснение использования проверки подлинности пользователей Facebook, Google, Twitter и т. д. без ASP.NET Core удостоверения.
ms.author: riande
ms.date: 12/10/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: b30ce7055b35b721c7fb83b61a328200d6a136b1
ms.sourcegitcommit: 3ca4a2235a8129def9e480d0a6ad54cc856920ec
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2020
ms.locfileid: "79025398"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Использовать проверку подлинности поставщика социальных сетей без удостоверения ASP.NET Core

[Kirk Ларкин](https://twitter.com/serpent5) и [Рик Андерсон (](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

<xref:security/authentication/social/index> описано, как разрешить пользователям входить в систему с помощью OAuth 2,0 с учетными данными от внешних поставщиков проверки подлинности. Подход, описанный в этом разделе, включает ASP.NET Core удостоверение в качестве поставщика проверки подлинности.

В этом примере демонстрируется использование внешнего поставщика проверки подлинности **без** ASP.NET Core удостоверения. Это полезно для приложений, которые не занимают все функции ASP.NET Core удостоверения, но по-прежнему нуждаются в интеграции с доверенным внешним поставщиком проверки подлинности.

В этом примере для проверки подлинности пользователей используется [Проверка подлинности Google](xref:security/authentication/google-logins) . Использование аутентификации Google сдвигает многие сложности управления процессом входа в Google. Чтобы выполнить интеграцию с другим внешним поставщиком проверки подлинности, см. следующие разделы:

* [Проверка подлинности Facebook](xref:security/authentication/facebook-logins)
* [Проверка подлинности Майкрософт](xref:security/authentication/microsoft-logins)
* [Проверка подлинности Twitter](xref:security/authentication/twitter-logins)
* [Другие поставщики](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Конфигурация

В методе `ConfigureServices` настройте схемы проверки подлинности приложения с помощью методов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>и <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>.

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

Вызов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> задает <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>приложения. `DefaultScheme` является схемой по умолчанию, используемой следующими методами расширения проверки подлинности `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Настройка `DefaultScheme` приложения в [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies") настраивает приложение на использование файлов cookie в качестве схемы по умолчанию для этих методов расширения. Настройка <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> приложения в [гугледефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") настраивает приложение на использование Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`. `DefaultChallengeScheme` переопределяет `DefaultScheme`. Дополнительные свойства, переопределяющие `DefaultScheme` при установке, см. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.

В `Startup.Configure`вызывайте `UseAuthentication` и `UseAuthorization` между вызовами `UseRouting` и `UseEndpoints`. При этом задается свойство `HttpContext.User` и выполняется по промежуточного слоя авторизации для запросов:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

Дополнительные сведения о схемах проверки подлинности см. в разделе [Основные понятия проверки подлинности](xref:security/authentication/index#authentication-concepts). Дополнительные сведения о проверке подлинности файлов cookie см. в разделе <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Применить авторизацию

Проверьте конфигурацию проверки подлинности приложения, применив атрибут `AuthorizeAttribute` к контроллеру, действию или странице. Следующий код ограничивает доступ к странице *конфиденциальности* для пользователей, которые прошли проверку подлинности:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Выйти

Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите [сигнаутасинк](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Следующий код добавляет обработчик `Logout` страницы на страницу *индекса* :

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

Обратите внимание, что в вызове `SignOutAsync` не указана схема проверки подлинности. `DefaultScheme` приложения `CookieAuthenticationDefaults.AuthenticationScheme` используется для возврата.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<xref:security/authentication/social/index> описано, как разрешить пользователям входить в систему с помощью OAuth 2,0 с учетными данными от внешних поставщиков проверки подлинности. Подход, описанный в этом разделе, включает ASP.NET Core удостоверение в качестве поставщика проверки подлинности.

В этом примере демонстрируется использование внешнего поставщика проверки подлинности **без** ASP.NET Core удостоверения. Это полезно для приложений, которые не занимают все функции ASP.NET Core удостоверения, но по-прежнему нуждаются в интеграции с доверенным внешним поставщиком проверки подлинности.

В этом примере для проверки подлинности пользователей используется [Проверка подлинности Google](xref:security/authentication/google-logins) . Использование аутентификации Google сдвигает многие сложности управления процессом входа в Google. Чтобы выполнить интеграцию с другим внешним поставщиком проверки подлинности, см. следующие разделы:

* [Проверка подлинности Facebook](xref:security/authentication/facebook-logins)
* [Проверка подлинности Майкрософт](xref:security/authentication/microsoft-logins)
* [Проверка подлинности Twitter](xref:security/authentication/twitter-logins)
* [Другие поставщики](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Конфигурация

В методе `ConfigureServices` настройте схемы проверки подлинности приложения с помощью методов `AddAuthentication`, `AddCookie`и `AddGoogle`.

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

Вызов [аддаусентикатион](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) задает [дефаултсчеме](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)приложения. `DefaultScheme` является схемой по умолчанию, используемой следующими методами расширения проверки подлинности `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Настройка `DefaultScheme` приложения в [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies") настраивает приложение на использование файлов cookie в качестве схемы по умолчанию для этих методов расширения. Настройка <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> приложения в [гугледефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") настраивает приложение на использование Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`. `DefaultChallengeScheme` переопределяет `DefaultScheme`. Дополнительные свойства, переопределяющие `DefaultScheme` при установке, см. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.

В методе `Configure` вызовите метод `UseAuthentication` для вызова по промежуточного слоя проверки подлинности, устанавливающего свойство `HttpContext.User`. Вызовите метод `UseAuthentication` перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

Дополнительные сведения о схемах проверки подлинности см. в разделе [Основные понятия проверки подлинности](xref:security/authentication/index#authentication-concepts). Дополнительные сведения о проверке подлинности файлов cookie см. в разделе <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Применить авторизацию

Проверьте конфигурацию проверки подлинности приложения, применив атрибут `AuthorizeAttribute` к контроллеру, действию или странице. Следующий код ограничивает доступ к странице *конфиденциальности* для пользователей, которые прошли проверку подлинности:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Выйти

Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите [сигнаутасинк](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Следующий код добавляет обработчик `Logout` страницы на страницу *индекса* :

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

Обратите внимание, что в вызове `SignOutAsync` не указана схема проверки подлинности. `DefaultScheme` приложения `CookieAuthenticationDefaults.AuthenticationScheme` используется для возврата.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
