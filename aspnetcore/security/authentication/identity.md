---
title: Общие сведения об Identity в ASP.NET Core
author: rick-anderson
description: Использование удостоверения с приложения ASP.NET Core. Узнайте, как задать требования к паролю (RequireDigit, RequiredLength, RequiredUniqueChars и многое другое).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 6a23dd4ad78c0695b5724a78204abf6752dfe67d
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655314"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Общие сведения об Identity в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Удостоверение ASP.NET Core — это система членства, который расширяет функциональные возможности входа для приложений ASP.NET Core. Пользователи могут создавать учетную запись входа сведений, хранящихся в удостоверении или они могут использовать внешнего поставщика входа. Поставщики поддерживаемых внешней учетной записи включают [Facebook, Google, учетной записи Майкрософт и Twitter](xref:security/authentication/social/index).

Удостоверений можно настроить с помощью базы данных SQL Server для хранения имен пользователей, пароли и данные профиля. Кроме того других постоянных хранилищах можно использовать, например, хранилище таблиц Azure.

[Просмотреть или скачать образец кода.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Как для загрузки)](xref:tutorials/index#how-to-download-a-sample)

В этом разделе сведения об использовании идентификаторов регистрация, вход и выход пользователя. Более подробные инструкции по созданию приложений, использующих удостоверения см. в разделе "Дальнейшие действия" в конце этой статьи.

## <a name="create-a-web-app-with-authentication"></a>Создание веб-приложения с проверкой подлинности

Создайте проект веб-приложение ASP.NET Core с учетными записями отдельных пользователей.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Выберите **Файл** > **Создать** > **Проект**. 
* Выберите **Новое веб-приложение ASP.NET Core**. Назовите проект **WebApp1** иметь то же пространство имен, как загрузка проекта. Нажмите кнопку **ОК**.
* Выберите ASP.NET Core **веб-приложение** ASP.NET Core 2.1, затем выберите **изменить способ проверки подлинности**.
* Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

Созданный проект предоставляет [удостоверения ASP.NET Core](xref:security/authentication/identity) как [библиотеки классов Razor](xref:razor-pages/ui-class).

### <a name="test-register-and-login"></a>Тест регистра и имени входа

Запустите приложение и регистрации пользователя. Зависимости от размера экрана, может потребоваться выбрать кнопки-переключателя навигации для просмотра **зарегистрировать** и **входа** ссылки.

![выключатель переходов.](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a>Настройка служб удостоверений

Службы добавляются в `ConfigureServices`.

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

Приведенный выше код настраивает удостоверений со значениями по умолчанию параметр. Доступ к службе предоставляется в приложение с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).

   Удостоверение включено, вызвав [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication` Добавляет проверку подлинности [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Доступ к службе предоставляется приложению через [внедрения зависимостей](xref:fundamentals/dependency-injection).

   Удостоверение включено для приложения, вызвав `UseAuthentication` в `Configure` метод. `UseAuthentication` Добавляет проверку подлинности [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Эти службы доступны приложению через [внедрения зависимостей](xref:fundamentals/dependency-injection).

   Удостоверение включено для приложения, вызвав `UseIdentity` в `Configure` метод. `UseIdentity` Добавляет проверку подлинности на основе файлов cookie [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Дополнительные сведения см. в разделе [класс IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) и [запуск приложения](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Сформировать шаблон регистрации, входа и выхода

Выполните [позволяет формировать удостоверений в проект Razor без авторизации](xref:security/authentication/scaffold-identity#) инструкции.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Добавьте файлы регистрации, входа и выхода.


# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Если вы создали проект с именем **WebApp1**, выполните следующие команды. В противном случае использовать правильное пространство имен для `ApplicationDbContext`:


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

PowerShell используется точка с запятой в качестве разделителя команды. При использовании PowerShell экранировать точку с запятой в списке файлов или поместить в список файлов в двойные кавычки, как показано в предыдущем примере.

---

### <a name="examine-register"></a>Проверьте регистр

::: moniker range=">= aspnetcore-2.1"

   Когда пользователь щелкает **зарегистрировать** ссылку, `RegisterModel.OnPostAsync` вызова действия. Пользователь создается путем [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) на `_userManager` объекта. `_userManager` предоставляется с помощью внедрения зависимостей):

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   Когда пользователь щелкает **зарегистрировать** ссылку, `Register` действие вызывается в `AccountController`. Действие `Register` создает пользователя путем вызова метода `CreateAsync` объекта`_userManager` (обеспечивается в `AccountController` путем внедрения зависимостей):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.

   **Примечание.** Раздел [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) описывает, как предотвратить автоматический вход пользователя при регистрации.

### <a name="log-in"></a>Войти

::: moniker range=">= aspnetcore-2.1"

Форма входа отображается при:

* **Вход** выбора ссылки.
* Когда пользователь обращается к странице, где они не проходят проверку подлинности **или** авторизован, они перенаправляются на страницу входа. 

При отправке формы на странице входа, `OnPostAsync` вызова действия. `PasswordSignInAsync` вызывается для `_signInManager` объекта (предоставляется с помощью внедрения зависимостей).

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   Базовый `Controller` предоставляет `User` свойство, которое можно получить из методов контроллера. Например, можно перечислить `User.Claims` и принимать решения об авторизации. Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).

::: moniker-end
::: moniker range="= aspnetcore-2.0"

Форма входа отображается в том случае, когда пользователь выбирает **вход** связывание или перенаправляются, когда доступ к странице, который требует проверки подлинности. Когда пользователь отправляет форму на странице входа `AccountController` `Login` вызова действия.

`Login` Вызовов действия `PasswordSignInAsync` на `_signInManager` объекта (для `AccountController` с помощью внедрения зависимостей).

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

Базовый (`Controller` или `PageModel`) предоставляет `User` свойство. Например `User.Claims` может быть перечислимым для принятия решений об авторизации.

::: moniker-end

### <a name="log-out"></a>Выход

::: moniker range=">= aspnetcore-2.1"

**Выход** открывается `LogoutModel.OnPost` действие. 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) удаляет файл cookie утверждения пользователя. Не перенаправления после вызова метода `SignOutAsync` или пользователь будет **не** выполнен выход.

POST указывается в *Pages/Shared/_LoginPartial.cshtml*:

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   Щелкнув **Выход** связать вызовы `LogOut` действие.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Предыдущий код вызывает `_signInManager.SignOutAsync` метод. `SignOutAsync` Метод очищает утверждений пользователя, хранящиеся в файле cookie.
::: moniker-end

## <a name="test-identity"></a>Проверить идентификатор

Шаблоны веб-проектов по умолчанию разрешает анонимный доступ к домашней страницы. Чтобы проверить тождество, добавьте [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) на страницу About.

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

Если вы вошли, выйдите из системы. Запустите приложение и выберите **о** ссылку. Вы будете перенаправлены на страницу входа.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Изучите удостоверений

Для просмотра идентификации, более подробно:

* [Создать источник полное удостоверение пользовательского интерфейса](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Проверьте источник каждой страницы и пошагового выполнения отладчика.

::: moniker-end

## <a name="identity-components"></a>Компонентами системы идентификации

::: moniker range=">= aspnetcore-2.1"

Все удостоверения зависимые пакеты NuGet, включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
::: moniker-end

Основной пакет для удостоверения [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Этот пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включена в состав по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

## <a name="migrating-to-aspnet-core-identity"></a>Переход на ASP.NET Core Identity

Дополнительные сведения и инструкции по миграции существующего хранилища удостоверений, см. в разделе [перенести проверки подлинности и идентификации](xref:migration/identity).

## <a name="setting-password-strength"></a>Стойкость пароля параметр

См. в разделе [конфигурации](#pw) пример, задает минимальное паролей.

## <a name="next-steps"></a>Следующие шаги

* [Настройка Identity](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* [Настроить тип данных идентификаторов первичных ключей](xref:security/authentication/identity-primary-key-configuration).
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
