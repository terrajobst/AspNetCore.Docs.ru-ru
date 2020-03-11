---
title: Проверка подлинности пользователей с помощью WS-Federation в ASP.NET Core
author: chlowell
description: В этом руководстве показано, как использовать WS-Federation в приложении ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/16/2019
uid: security/authentication/ws-federation
ms.openlocfilehash: d82421a14ede6cb6b01ef59f233bb2eba6b56aec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651334"
---
# <a name="authenticate-users-with-ws-federation-in-aspnet-core"></a>Проверка подлинности пользователей с помощью WS-Federation в ASP.NET Core

В этом руководстве показано, как разрешить пользователям выполнять вход с помощью поставщика проверки подлинности WS-Federation, например службы федерации Active Directory (AD FS) (ADFS) или [Azure Active Directory](/azure/active-directory/) (AAD). В нем используется пример приложения ASP.NET Core 2,0, описанный в статье [Аутентификация Facebook, Google и внешнего поставщика](xref:security/authentication/social/index).

Для приложений ASP.NET Core 2,0 поддержка WS-Federation обеспечивается [Microsoft. AspNetCore. Authentication. WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation). Этот компонент переносится из [Microsoft. Owin. Security. WsFederation](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) и разделяет многие механики этого компонента. Однако эти компоненты отличаются друг от друга несколькими важными способами.

По умолчанию новое по промежуточного слоя:

* Не разрешает незапрошенные имена входа. Эта функция протокола WS-Federation уязвима для атак XSRF. Однако его можно включить с помощью параметра `AllowUnsolicitedLogins`.
* Не проверяет каждую форму POST для сообщений входа. Для входа проверяются только запросы к `CallbackPath`. `CallbackPath` по умолчанию имеет значение `/signin-wsfed` но может быть изменено с помощью унаследованного свойства [ремотеаусентикатионоптионс. каллбаккпас](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) класса [всфедератионоптионс](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions) . Этот путь можно использовать совместно с другими поставщиками проверки подлинности, включив параметр [скипунрекогнизедрекуестс](/dotnet/api/microsoft.aspnetcore.authentication.wsfederation.wsfederationoptions.skipunrecognizedrequests) .

## <a name="register-the-app-with-active-directory"></a>Регистрация приложения в Active Directory

### <a name="active-directory-federation-services"></a>службы федерации Active Directory;

* Откройте **Мастер добавления отношения доверия с проверяющей стороной** на сервере из консоли управления ADFS:

![Мастер добавления отношения доверия с проверяющей стороной: Добро пожаловать](ws-federation/_static/AdfsAddTrust.png)

* Выберите Ввод данных вручную:

![Мастер добавления отношения доверия с проверяющей стороной: Выбор источника данных](ws-federation/_static/AdfsSelectDataSource.png)

* Введите отображаемое имя проверяющей стороны. Это имя не имеет значения для приложения ASP.NET Core.

* В [Microsoft. AspNetCore. Authentication. WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) отсутствует поддержка шифрования маркеров, поэтому не настраивайте сертификат шифрования маркеров:

![Мастер добавления отношения доверия с проверяющей стороной: Настройка сертификата](ws-federation/_static/AdfsConfigureCert.png)

* Включите поддержку пассивного протокола WS-Federation с помощью URL-адреса приложения. Проверьте правильность порта для приложения:

![Мастер добавления отношения доверия с проверяющей стороной: Настройка URL-адреса](ws-federation/_static/AdfsConfigureUrl.png)

> [!NOTE]
> Это должен быть URL-адрес HTTPS. IIS Express может предоставить самозаверяющий сертификат при размещении приложения во время разработки. Для Kestrel требуется ручная настройка сертификата. Дополнительные сведения см. в [документации по Kestrel](xref:fundamentals/servers/kestrel) .

* Нажмите кнопку **Далее** в мастере и **закройте** в конце.

* Для удостоверения ASP.NET Core требуется утверждение **идентификатора имени** . Добавьте его в диалоговом окне **изменение правил утверждений** :

![Изменение правил утверждения](ws-federation/_static/EditClaimRules.png)

* В **мастере добавления правила преобразования утверждений**Оставьте выбранный по умолчанию шаблон **отправки атрибутов LDAP в качестве шаблона утверждений** и нажмите кнопку **Далее**. Добавьте правило, сопоставленное с атрибутом **SAM-Account-Name** LDAP к исходящему утверждению **ID** :

![Мастер добавления правила преобразования утверждений: Настройка правила для утверждений](ws-federation/_static/AddTransformClaimRule.png)

* Нажмите кнопку **готово** > **ОК** в окне **изменение правил утверждений** .

### <a name="azure-active-directory"></a>Azure Active Directory

* Перейдите в колонку регистрации приложений клиента AAD. Щелкните **Регистрация нового приложения**:

![Azure Active Directory: Регистрация приложений](ws-federation/_static/AadNewAppRegistration.png)

* Введите имя для регистрации приложения. Это не имеет значения для ASP.NET Core приложения.
* Введите URL-адрес, который прослушивает приложение в качестве **URL-адреса входа**:

![Azure Active Directory: создание регистрации приложения](ws-federation/_static/AadCreateAppRegistration.png)

* Щелкните **конечные точки** и запишите URL-адрес **документа метаданных федерации** . Это `MetadataAddress`по промежуточного слоя WS-Federation:

![Azure Active Directory: конечные точки](ws-federation/_static/AadFederationMetadataDocument.png)

* Перейдите к новой регистрации приложения. Щелкните **параметры** > **Свойства** и запишите **URI идентификатора приложения**. Это `Wtrealm`по промежуточного слоя WS-Federation:

![Azure Active Directory: свойства регистрации приложения](ws-federation/_static/AadAppIdUri.png)

## <a name="use-ws-federation-without-aspnet-core-identity"></a>Использовать WS-Federation без удостоверения ASP.NET Core

По промежуточного слоя WS-Federation можно использовать без удостоверения. Пример:
::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/StartupNon21.cs?name=snippet)]
::: moniker-end

## <a name="add-ws-federation-as-an-external-login-provider-for-aspnet-core-identity"></a>Добавление WS-Federation в качестве внешнего поставщика входа для ASP.NET Core удостоверения

* Добавьте зависимость от [Microsoft. AspNetCore. Authentication. WsFederation](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.WsFederation) в проект.
* Добавление WS-Federation в `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup31.cs?name=snippet)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"
[!code-csharp[](ws-federation/samples/Startup21.cs?name=snippet)]
::: moniker-end

[!INCLUDE [default settings configuration](social/includes/default-settings.md)]

### <a name="log-in-with-ws-federation"></a>Вход с помощью WS-Federation

Перейдите к приложению и щелкните ссылку **Вход** в заголовке навигации. Есть возможность войти в систему с помощью WsFederation: ![войти на страницу](ws-federation/_static/WsFederationButton.png)

При использовании ADFS в качестве поставщика кнопка перенаправляется на страницу входа в ADFS: ![страницу входа ADFS](ws-federation/_static/AdfsLoginPage.png)

При Azure Active Directory в качестве поставщика кнопка перенаправляется на страницу входа AAD: ![страницу входа AAD](ws-federation/_static/AadSignIn.png)

Успешный вход нового пользователя приводит к перенаправлению на страницу регистрации пользователей приложения: ![](ws-federation/_static/Register.png) страницы регистрации