---
title: Общие сведения об Identity в ASP.NET Core
author: rick-anderson
description: Использование удостоверения с приложения ASP.NET Core. Включает параметр паролей (RequireDigit, RequiredLength, RequiredUniqueChars и многое другое).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: 50ddb96000e6a3f9e1762e9bb3e1f215f20d4356
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095643"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Общие сведения об Identity в ASP.NET Core

По [Пранавом Растоги](https://github.com/rustd), [Рик Андерсон](https://twitter.com/RickAndMSFT), [том Дайкстра](https://github.com/tdykstra), Джон Гэллоуэй [Erik Reitan](https://github.com/Erikre), и [Стив Смит](https://ardalis.com/)

Удостоверение ASP.NET Core — это система членства, который позволяет добавить функциональные возможности входа в приложение. Пользователи могут создавать учетную запись и имя входа с именем пользователя и пароль или их можно использовать поставщик внешней учетной записи, например Facebook, Google, учетной записи Майкрософт, Twitter или другим пользователям.

Вы можете настроить удостоверение ASP.NET Core, чтобы использовать базу данных SQL Server для хранения имен пользователей, пароли и данные профиля. Кроме того можно использовать собственные постоянное хранилище, например, хранилище таблиц Azure. Этот документ содержит инструкции по Visual Studio и с помощью интерфейса командной строки.

[Просмотреть или скачать образец кода.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Как для загрузки)](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Общие сведения об удостоверениях

В этом разделе будет использование удостоверения ASP.NET Core для добавления функций, регистрация, вход и выход пользователя. Более подробные инструкции по созданию приложений с помощью удостоверения ASP.NET Core см. в разделе "Дальнейшие действия" в конце этой статьи.

1. Создайте проект веб-приложение ASP.NET Core с учетными записями отдельных пользователей.

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   В Visual Studio выберите **файл** > **New** > **проекта**. Выберите **веб-приложение ASP.NET Core** и нажмите кнопку **ОК**.

   ![Диалоговое окно создания нового проекта](identity/_static/01-new-project.png)

   Выберите ASP.NET Core **веб-приложение (Model-View-Controller)** для ASP.NET Core 2.x, а затем выберите **изменить способ проверки подлинности**.

   ![Диалоговое окно создания нового проекта](identity/_static/02-new-project.png)

   Откроется диалоговое окно, предложить варианты проверки подлинности. Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК** чтобы вернуться в предыдущем диалоговом окне.

   ![Диалоговое окно создания нового проекта](identity/_static/03-new-project-auth.png)

   Выбрав **учетные записи отдельных пользователей** направляет Visual Studio для создания модели, модели ViewModel, представления, контроллеры и другие активы, необходимые для проверки подлинности как часть шаблона проекта.

   # <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

   Если с помощью интерфейса командной строки .NET Core, создайте новый проект с помощью `dotnet new mvc --auth Individual`. Эта команда создает новый проект с тем же кодом шаблона удостоверений, которые создает Visual Studio.

   Созданный проект содержит `Microsoft.AspNetCore.Identity.EntityFrameworkCore` пакет, который сохраняет данные удостоверений и схемы для SQL Server с помощью [Entity Framework Core](https://docs.microsoft.com/ef/).

   ---

2. Настройка службы удостоверений и добавьте по промежуточного слоя в `Startup`.

   Службы удостоверений добавляются к приложению в `ConfigureServices` метод в `Startup` класса:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Эти службы доступны приложению через [внедрения зависимостей](xref:fundamentals/dependency-injection).

   Удостоверение включено для приложения, вызвав `UseAuthentication` в `Configure` метод. `UseAuthentication` Добавляет проверку подлинности [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Эти службы доступны приложению через [внедрения зависимостей](xref:fundamentals/dependency-injection).

   Удостоверение включено для приложения, вызвав `UseIdentity` в `Configure` метод. `UseIdentity` Добавляет проверку подлинности на основе файлов cookie [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   Дополнительные сведения о время загрузки приложения, см. в разделе [запуск приложения](xref:fundamentals/startup).

3. Создание пользователя.

   Запустите приложение и щелкните ссылку **Register** (Регистрация).

   При первом запуске вам может потребоваться выполнить миграцию. Приложение предложит **Apply Migrations** (Выполнить миграцию): Обновите страницу, при необходимости.

   ![Веб-страница применения миграции](identity/_static/apply-migrations.png)

   Кроме того, можно проверить работу удостоверения ASP.NET Core с приложением без постоянной базы данных, а с использованием базы данных в памяти. Для этого добавьте в приложение пакет `Microsoft.EntityFrameworkCore.InMemory`и измените вызов метода `AddDbContext` в `ConfigureServices` следующим образом:

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   Когда пользователь нажимает на ссылку **Register** (Регистрация), вызывается метод действия `Register` контроллера `AccountController`. Действие `Register` создает пользователя путем вызова метода `CreateAsync` объекта`_userManager` (обеспечивается в `AccountController` путем внедрения зависимостей):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.

   **Примечание.** Раздел [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) описывает, как предотвратить автоматический вход пользователя при регистрации.

4. Вход.

   Пользователи могут входить, щелкнув **вход** ссылку в верхней части сайта, или может быть переход на страницу входа, если они пытаются получить доступ к части сайта, требующему проверки подлинности. Когда пользователь отправляет форму на странице входа `AccountController` `Login` вызова действия.

   `Login` Вызовов действия `PasswordSignInAsync` на `_signInManager` объекта (для `AccountController` с помощью внедрения зависимостей).

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   Базовый `Controller` предоставляет `User` свойство, которое можно получить из методов контроллера. Например, можно перечислить `User.Claims` и принимать решения об авторизации. Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).

5. Выход.

   Щелкнув **Выход** связать вызовы `LogOut` действие.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Предыдущий код выше вызовы `_signInManager.SignOutAsync` метод. `SignOutAsync` Метод очищает утверждений пользователя, хранящиеся в файле cookie.

<a name="pw"></a>
6. Конфигурация.

   Удостоверение имеет некоторые поведения по умолчанию, которые могут быть переопределены в классе запуска приложения. `IdentityOptions` не нужно настроить при использовании поведения по умолчанию. В следующем коде задается несколько вариантов стойкость пароля:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   Дополнительные сведения о том, как настроить удостоверение, см. в разделе [настроить удостоверение](xref:security/authentication/identity-configuration).

   Вы также можете настроить тип данных первичного ключа, см. в разделе [тип данных Настройка идентификаторов первичных ключей](xref:security/authentication/identity-primary-key-configuration).

7. Просмотр базы данных.

   Если приложение использует базу данных SQL Server (по умолчанию на Windows и для пользователей Visual Studio), можно просмотреть приложение, созданное для базы данных. Можно использовать **SQL Server Management Studio**. Кроме того, в Visual Studio последовательно выберите **представление** > **обозреватель объектов SQL Server**. Подключение к **(localdb) \MSSQLLocalDB**. Базы данных с именем, соответствующим `aspnet-<name of your project>-<guid>` отображается.

   ![Контекстные меню в таблице AspNetUsers базы данных](identity/_static/04-db.png)

   Разверните базу данных и его **таблиц**, щелкните правой кнопкой мыши **dbo. AspNetUsers** таблицы и выберите **данные представления**.

8. Проверка работы удостоверений

    Значение по умолчанию *веб-приложение ASP.NET Core* шаблон проекта предоставляет пользователям доступ к любое действие в приложении без необходимости с именем входа. Чтобы убедиться, что ASP.NET Identity работает, добавьте`[Authorize]` атрибут `About` действие `Home` контроллера.

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Запустите проект, используя **Ctrl** + **F5** и перейдите к **о** страницы. Только прошедшие проверку подлинности пользователи могут получать доступ к **о** странице, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.

    # <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

    Откройте окно командной строки и перейдите в корневой каталог проекта каталог, содержащий `.csproj` файл. Запустите [запуска dotnet](/dotnet/core/tools/dotnet-run) команду, чтобы запустить приложение:

    ```csharp
    dotnet run 
    ```

    Обзор URL-адрес, указанный в выходных данных из [запуска dotnet](/dotnet/core/tools/dotnet-run) команды. URL-адрес должен указывать `localhost` с созданный номер порта. Перейдите к **о** страницы. Только прошедшие проверку подлинности пользователи могут получать доступ к **о** странице, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.

    ---

## <a name="identity-components"></a>Компонентами системы идентификации

Первичное эталонное сборки для системы идентификации `Microsoft.AspNetCore.Identity`. Этот пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включена в состав по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Эти зависимости необходимы для использования системы идентификации в приложениях ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` — Содержит необходимые типы использовать удостоверение с Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core — технология доступа к данных, рекомендуемые корпорации Майкрософт для реляционных баз данных, таких как SQL Server. Для тестирования, можно использовать `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` — Промежуточный слой, который позволяет приложению использовать проверку подлинности на основе файлов cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Переход на ASP.NET Core Identity

Дополнительные сведения и инструкции по миграции существующее удостоверение в магазине см. в разделе [перенести проверки подлинности и идентификации](xref:migration/identity).

## <a name="setting-password-strength"></a>Стойкость пароля параметр

См. в разделе [конфигурации](#pw) пример, задает минимальное паролей.

## <a name="next-steps"></a>Следующие шаги

* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:security/authentication/social/index>
* <xref:host-and-deploy/web-farm>
