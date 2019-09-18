---
title: Введение в удостоверение на ASP.NET Core
author: rick-anderson
description: Используйте удостоверение с приложением ASP.NET Core. Узнайте, как устанавливать требования к паролю (Рекуиредигит, Рекуиредленгс, Рекуиредуникуечарс и др.).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 979681cfc196aca9fb5097583d99a086e1c597ba
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082452"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Введение в удостоверение на ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

ASP.NET Core удостоверение — это система членства, которая добавляет функции входа в ASP.NET Core приложения. Пользователи могут создать учетную запись с данными для входа, хранящимися в удостоверении, или использовать внешнего поставщика входа. Поддерживаемые внешние поставщики входа включают [Facebook, Google, учетную запись Майкрософт и Twitter](xref:security/authentication/social/index).

Идентификацию можно настроить с помощью SQL Server базы данных для хранения имен пользователей, паролей и данных профилей. Кроме того, можно использовать еще одно постоянное хранилище, например хранилище таблиц Azure.

[Просмотр или скачивание примера кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([как загрузить)](xref:index#how-to-download-a-sample).

В этом разделе вы узнаете, как использовать удостоверение для регистрации, входа и выхода пользователя. Более подробные инструкции по созданию приложений, использующих удостоверение, см. в разделе дальнейшие действия в конце этой статьи.

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>Адддефаултидентити и Аддидентити

[Адддефаултидентити](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) был введен в ASP.NET Core 2,1. Вызов `AddDefaultIdentity` аналогичен вызову следующего:

* [аддидентити](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [адддефаултуи](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [адддефаулттокенпровидерс](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

Дополнительные сведения см. в разделе [адддефаултидентити Source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>Создание веб-приложения с проверкой подлинности

Создание ASP.NET Core проекта веб-приложения с учетными записями отдельных пользователей.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Выберите **Файл** > **Создать** > **Проект**.
* Выберите **Новое веб-приложение ASP.NET Core**. Присвойте проекту имя **APP1** , которое будет иметь то же пространство имен, что и загружаемый проект. Нажмите кнопку **ОК**.
* Выберите ASP.NET Core **веб-приложение**, а затем щелкните **изменить проверку подлинности**.
* Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

Созданный проект предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) в виде [библиотеки классов Razor](xref:razor-pages/ui-class). Библиотека классов Razor Identity предоставляет конечные точки с `Identity` областью. Например:

* /идентити/аккаунт/логин
* /идентити/аккаунт/логаут
* /идентити/аккаунт/манаже

### <a name="apply-migrations"></a>Применение миграции

Примените миграцию, чтобы инициализировать базу данных.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Выполните следующую команду в консоли диспетчера пакетов (PMC):

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>Проверка регистра и вход в систему

Запустите приложение и зарегистрируйте пользователя. В зависимости от размера экрана может потребоваться выбрать переключатель навигации, чтобы просмотреть ссылки **Регистрация** и **Вход** .

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>Настройка служб удостоверений

Службы добавляются в `ConfigureServices`. По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

Приведенный выше код настраивает удостоверение с использованием значений параметров по умолчанию. Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).

   Удостоверение включается путем вызова [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication`добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).

   Удостоверение включается для приложения путем вызова `UseAuthentication` `Configure` метода в методе. `UseAuthentication`добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Эти службы становятся доступными для приложения посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).

   Удостоверение включается для приложения путем вызова `UseIdentity` `Configure` метода в методе. `UseIdentity`добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности на основе файлов cookie в конвейер запросов.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Дополнительные сведения см. в разделе [класс идентитйоптионс](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) и [Запуск приложения](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Регистрация, вход и выход из шаблона формирования шаблонов

Соблюдайте [идентификатор шаблона в проекте Razor с](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) инструкциями по авторизации для создания кода, приведенного в этом разделе.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Добавьте файлы Register, login и LogOut.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Если вы создали проект с именем " **APP1**", выполните следующие команды. В противном случае используйте правильное пространство имен `ApplicationDbContext`для:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

В PowerShell используется точка с запятой в качестве разделителя команд. При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки, как показано в предыдущем примере.

---

### <a name="examine-register"></a>Проверка регистра

::: moniker range=">= aspnetcore-2.1"

   Когда пользователь щелкает ссылку **Register** , `RegisterModel.OnPostAsync` вызывается действие. Пользователь создается с помощью [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) для `_userManager` объекта. `_userManager`предоставляется путем внедрения зависимостей):

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Когда пользователь щелкает ссылку **Register** , `Register` действие вызывается в `AccountController`. Действие `Register` создает пользователя путем вызова метода `CreateAsync` объекта`_userManager` (обеспечивается в `AccountController` путем внедрения зависимостей):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.

   **Примечание.** Инструкции по предотвращению немедленного входа при регистрации см. в статье [Подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) .

### <a name="log-in"></a>Войти

::: moniker range=">= aspnetcore-2.1"

Форма входа отображается в следующих случаях:

* Выбрана ссылка для **входа** .
* Пользователь пытается получить доступ к странице с ограниченным доступом, которая не авторизована для доступа, **или** если система не прошла проверку подлинности.

При отправке `OnPostAsync` формы на странице входа вызывается действие. `PasswordSignInAsync`вызывается для `_signInManager` объекта (предоставляется путем внедрения зависимостей).

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   Базовый `Controller` класс`User` предоставляет свойство, доступ к которому можно получить из методов контроллера. Например, можно перечислять `User.Claims` и принимать решения об авторизации. Дополнительные сведения см. в разделе <xref:security/authorization/introduction>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Форма входа отображается, когда пользователь выбирает ссылку для **входа** или перенаправляется при обращении к странице, требующей проверки подлинности. Когда пользователь отправляет форму на страницу входа, `AccountController` `Login` вызывается действие.

Действие вызывает `PasswordSignInAsync` объект(`AccountController` предоставляется путем внедрения зависимостей). `_signInManager` `Login`

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

Базовый класс (`Controller` или `PageModel`) предоставляет `User` свойство. Например, можно `User.Claims` перечислить, чтобы принимать решения об авторизации.

::: moniker-end

### <a name="log-out"></a>Выход

::: moniker range=">= aspnetcore-2.1"

Ссылка **выхода** вызывает `LogoutModel.OnPost` действие. 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[Сигнаутасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) очищает утверждения пользователя, хранящиеся в файле cookie.

POST указывается в параметре *pages/Shared/_LoginPartial. cshtml*:

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Щелкнув ссылку **выход** , вы вызываете `LogOut` действие.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Приведенный выше код вызывает `_signInManager.SignOutAsync` метод. `SignOutAsync` Метод очищает утверждения пользователя, хранящиеся в файле cookie.

::: moniker-end

## <a name="test-identity"></a>Проверить удостоверение

Шаблоны веб-проектов по умолчанию разрешают анонимный доступ к домашним страницам. Чтобы проверить удостоверение, [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) добавьте на страницу конфиденциальность.

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

Если вы вошли в систему, выйдите из нее. Запустите приложение и щелкните ссылку **Конфиденциальность** . Вы будете перенаправлены на страницу входа.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Просмотр удостоверений

Более подробное изучение удостоверений:

* [Создание полного источника идентификатора пользовательского интерфейса](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Изучите источник каждой страницы и пошаговое выполнение отладчика.

::: moniker-end

## <a name="identity-components"></a>Компоненты идентификации

::: moniker range=">= aspnetcore-2.1"

Все пакеты NuGet, зависящие от удостоверения, включены в пакет [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app).

::: moniker-end

Основным пакетом для удостоверения является [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Этот пакет содержит основной набор интерфейсов для ASP.NET Core удостоверения и включается `Microsoft.AspNetCore.Identity.EntityFrameworkCore`в.

## <a name="migrating-to-aspnet-core-identity"></a>Миграция на ASP.NET Core удостоверение

Дополнительные сведения и рекомендации по переносу существующего хранилища удостоверений см. в статье [Миграция проверки подлинности и удостоверений](xref:migration/identity).

## <a name="setting-password-strength"></a>Установка силы пароля

См. раздел [Конфигурация](#pw) для примера, который устанавливает минимальные требования к паролю.

## <a name="next-steps"></a>Следующие шаги

* [Настройка Identity](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
