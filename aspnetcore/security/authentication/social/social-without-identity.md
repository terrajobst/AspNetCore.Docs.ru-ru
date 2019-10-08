---
title: Аутентификация Facebook, Google и внешнего поставщика без удостоверения ASP.NET Core
author: rick-anderson
description: Объяснение использования проверки подлинности пользователей Facebook, Google, Twitter и т. д. без ASP.NET Core удостоверения.
ms.author: riande
ms.date: 09/25/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 54dd93a13b2f7ed09c2c305f529d5f4610567184
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999895"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>Использовать проверку подлинности поставщика социальных сетей без удостоверения ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

<xref:security/authentication/social/index> описывает, как разрешить пользователям входить в систему с помощью OAuth 2,0 с учетными данными от внешних поставщиков проверки подлинности. Подход, описанный в этом разделе, включает ASP.NET Core удостоверение в качестве поставщика проверки подлинности.

В этом примере демонстрируется использование внешнего поставщика проверки подлинности **без** ASP.NET Core удостоверения. Это полезно для приложений, которые не занимают все функции ASP.NET Core удостоверения, но по-прежнему нуждаются в интеграции с доверенным внешним поставщиком проверки подлинности.

В этом примере для проверки подлинности пользователей используется [Проверка подлинности Google](xref:security/authentication/google-logins) . Использование аутентификации Google сдвигает многие сложности управления процессом входа в Google. Чтобы выполнить интеграцию с другим внешним поставщиком проверки подлинности, см. следующие разделы:

* [Проверка подлинности Facebook](xref:security/authentication/facebook-logins)
* [Проверка подлинности Майкрософт](xref:security/authentication/microsoft-logins)
* [Проверка подлинности Twitter](xref:security/authentication/twitter-logins)
* [Другие поставщики](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Конфигурация

В методе `ConfigureServices` настройте схемы проверки подлинности приложения с помощью методов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> и <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>:

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet1)]

Вызов <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> задает <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme> приложения. @No__t-0 — это схема по умолчанию, используемая следующими методами расширения проверки подлинности `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Если задать для приложения `DefaultScheme` значение [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), приложение будет использовать файлы cookie в качестве схемы по умолчанию для этих методов расширения. Если задать для приложения значение @no__t от 0 до [гугледефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), приложение будет использовать Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`. `DefaultChallengeScheme` переопределяет `DefaultScheme`. Дополнительные свойства, которые переопределяют `DefaultScheme` при установке, см. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.

В `Startup.Configure` вызовите `UseAuthentication` и `UseAuthorization`, чтобы задать свойство `HttpContext.User` и запустить по промежуточного слоя авторизации для запросов. Вызовите методы `UseAuthentication` и `UseAuthorization` перед вызовом `UseEndpoints`:

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet2)]

Дополнительные сведения о схемах проверки подлинности и проверке подлинности файлов cookie см. в разделе <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Применить авторизацию

Проверьте конфигурацию проверки подлинности приложения, применив атрибут `AuthorizeAttribute` к контроллеру, действию или странице. Следующий код ограничивает доступ к странице *конфиденциальности* для пользователей, которые прошли проверку подлинности:

[!code-csharp[](social-without-identity/3.0sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Выйти

Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите [сигнаутасинк](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Следующий код добавляет обработчик страницы `Logout` на страницу *индекса* :

[!code-csharp[](social-without-identity/3.0sample/Pages/Index.cshtml.cs?name=snippet&highlight=14-18)]

Обратите внимание, что при вызове `SignOutAsync` не указана схема проверки подлинности. Значение @no__t (0) приложения `CookieAuthenticationDefaults.AuthenticationScheme` используется для возврата.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<xref:security/authentication/social/index> описывает, как разрешить пользователям входить в систему с помощью OAuth 2,0 с учетными данными от внешних поставщиков проверки подлинности. Подход, описанный в этом разделе, включает ASP.NET Core удостоверение в качестве поставщика проверки подлинности.

В этом примере демонстрируется использование внешнего поставщика проверки подлинности **без** ASP.NET Core удостоверения. Это полезно для приложений, которые не занимают все функции ASP.NET Core удостоверения, но по-прежнему нуждаются в интеграции с доверенным внешним поставщиком проверки подлинности.

В этом примере для проверки подлинности пользователей используется [Проверка подлинности Google](xref:security/authentication/google-logins) . Использование аутентификации Google сдвигает многие сложности управления процессом входа в Google. Чтобы выполнить интеграцию с другим внешним поставщиком проверки подлинности, см. следующие разделы:

* [Проверка подлинности Facebook](xref:security/authentication/facebook-logins)
* [Проверка подлинности Майкрософт](xref:security/authentication/microsoft-logins)
* [Проверка подлинности Twitter](xref:security/authentication/twitter-logins)
* [Другие поставщики](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Конфигурация

В методе `ConfigureServices` настройте схемы проверки подлинности приложения с помощью методов `AddAuthentication`, `AddCookie` и `AddGoogle`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

Вызов [аддаусентикатион](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) задает [дефаултсчеме](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)приложения. @No__t-0 — это схема по умолчанию, используемая следующими методами расширения проверки подлинности `HttpContext`:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Если задать для приложения `DefaultScheme` значение [кукиеаусентикатиондефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookies"), приложение будет использовать файлы cookie в качестве схемы по умолчанию для этих методов расширения. Если задать для приложения значение @no__t от 0 до [гугледефаултс. аусентикатионсчеме](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), приложение будет использовать Google в качестве схемы по умолчанию для вызовов `ChallengeAsync`. `DefaultChallengeScheme` переопределяет `DefaultScheme`. Дополнительные свойства, которые переопределяют `DefaultScheme` при установке, см. в разделе <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.

В методе `Configure` вызовите метод `UseAuthentication` для вызова по промежуточного слоя проверки подлинности, устанавливающего свойство `HttpContext.User`. Вызовите метод `UseAuthentication` перед вызовом `UseMvcWithDefaultRoute` или `UseMvc`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

Дополнительные сведения о схемах проверки подлинности и проверке подлинности файлов cookie см. в разделе <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Применить авторизацию

Проверьте конфигурацию проверки подлинности приложения, применив атрибут `AuthorizeAttribute` к контроллеру, действию или странице. Следующий код ограничивает доступ к странице *конфиденциальности* для пользователей, которые прошли проверку подлинности:

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Выйти

Чтобы выйти из текущего пользователя и удалить его файл cookie, вызовите [сигнаутасинк](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*). Следующий код добавляет обработчик страницы `Logout` на страницу *индекса* :

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

Обратите внимание, что при вызове `SignOutAsync` не указана схема проверки подлинности. Значение @no__t (0) приложения `CookieAuthenticationDefaults.AuthenticationScheme` используется для возврата.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
