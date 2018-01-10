---
title: "Общие сведения об учетных данных ASP.NET Core"
author: rick-anderson
description: "Использовать удостоверение с приложением ASP.NET Core"
keywords: "ASP.NET Core, удостоверение, авторизации, безопасность"
ms.author: riande
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: fc8e076af92bd8f9a95e73abb66ce32cae8ab9cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Общие сведения об учетных данных ASP.NET Core

По [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Джон Гэллоуэй [Reitan Эрик](https://github.com/Erikre), и [Стив Смит](https://ardalis.com/)

Удостоверение ASP.NET Core является система членства, в котором можно добавить функциональные возможности входа в приложение. Пользователи могут создавать учетную запись и имя входа с именем пользователя и пароль или их можно использовать поставщик внешней учетной записи, например Facebook, Google, учетной записи Майкрософт, Twitter или другим пользователям.

Вы можете настроить ASP.NET Identity Core использование базы данных SQL Server для хранения имен пользователей, пароли и данные профиля. Кроме того можно использовать собственные постоянное хранилище, например, табличное хранилище Azure. Этот документ содержит инструкции по Visual Studio и с помощью CLI.

[Просмотреть или загрузить образец кода.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Сведения о загрузке)](https://docs.microsoft.com/en-us/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Общие сведения об идентификации

В этом разделе будет использование ASP.NET Core Identity Добавление функциональности для регистрации, вход и выход пользователя. Более подробные инструкции по созданию приложений с помощью ASP.NET Core Identity см. в разделе Дальнейшие действия в конце этой статьи.

1.  Создайте проект веб-приложения ASP.NET Core с отдельными учетными записями пользователей.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    В Visual Studio, выберите **файл** > **New** > **проекта**. Выберите **веб-приложения ASP.NET Core** и нажмите кнопку **ОК**.

    ![Диалоговое окно создания нового проекта](identity/_static/01-new-project.png)

    Выберите ASP.NET Core **веб-приложения (Model-View-Controller)** для ASP.NET Core 2.x, а затем выберите **изменить аутентификацию**.

    ![Диалоговое окно создания нового проекта](identity/_static/02-new-project.png)

    Откроется диалоговое окно предложения вариантов проверки подлинности. Выберите **отдельных учетных записей пользователей** и нажмите кнопку **ОК** для возврата к диалоговому окну предыдущего.

    ![Диалоговое окно создания нового проекта](identity/_static/03-new-project-auth.png)

    При выборе **отдельных учетных записей пользователей** направляет Visual Studio для создания моделей, ViewModels, представления, контроллеры и другие активы, необходимые для проверки подлинности как часть шаблона проекта.

    # <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

    Если используется .NET Core CLI, создайте новый проект с помощью ``dotnet new mvc --auth Individual``. Эта команда создает новый проект с тем же кодом шаблона удостоверений, создаваемых в Visual Studio.

    Созданный проект содержит `Microsoft.AspNetCore.Identity.EntityFrameworkCore` пакет, который сохраняет данные удостоверений и схемы SQL Server с помощью [Entity Framework Core](https://docs.microsoft.com/ef/).

    ---

2.  Настройка службы удостоверений и добавление по промежуточного слоя в `Startup`.

    Службы удостоверений добавляются к приложению в `ConfigureServices` метод `Startup` класса:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).
    
    Путем вызова для приложения включена удостоверения `UseAuthentication` в `Configure` метод. `UseAuthentication`Добавление проверки подлинности [по промежуточного слоя](xref:fundamentals/middleware) к конвейеру запросов.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).
    
    Путем вызова для приложения включена удостоверения `UseIdentity` в `Configure` метод. `UseIdentity`Добавляет файл cookie проверки подлинности с помощью [по промежуточного слоя](xref:fundamentals/middleware) к конвейеру запросов.
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Дополнительные сведения о время загрузки приложения см. в разделе [запуска приложения](xref:fundamentals/startup).

3.  Создайте пользователя.

    Запустите приложение и щелкните на **зарегистрировать** ссылку.

    Если при первом выполнении этого действия может потребоваться для выполнения миграции. Приложение предложит **применить миграции**. При необходимости обновите страницу.
    
    ![Применить миграции веб-страницы](identity/_static/apply-migrations.png)
    
    Кроме того можно проверить с помощью ASP.NET Core Identity вместе с приложением без постоянных баз данных с помощью базы данных в памяти. Чтобы использовать базу данных в памяти, добавьте ``Microsoft.EntityFrameworkCore.InMemory`` пакета приложения и изменить вызов приложения ``AddDbContext`` в ``ConfigureServices`` следующим образом:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Когда пользователь щелкает **зарегистрировать** ссылку, ``Register`` при вызове действия на ``AccountController``. ``Register`` Действие создает пользователя путем вызова `CreateAsync` на `_userManager` объекта (для ``AccountController`` путем внедрения зависимостей):
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Если пользователь успешно создан, пользователь вошел вызове ``_signInManager.SignInAsync``.

    **Примечание:** разделе [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) для меры по предотвращению немедленно входа при регистрации.
 
4.  Войти.
 
    Пользователи могут войти, щелкнув **входа** ссылок в верхней части сайта, или может быть переход на страницу входа, если они пытаются получить доступ к части сайта, требующей авторизации. Когда пользователь отправляет форму на странице входа ``AccountController`` ``Login`` вызова действия.

    ``Login`` Вызывает действие ``PasswordSignInAsync`` на ``_signInManager`` объекта (для ``AccountController`` путем внедрения зависимостей).

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    Базовый ``Controller`` предоставляемых классами ``User`` свойство, которое можно открыть из методов контроллера. Например, можно перечислить `User.Claims` и принимать решения об авторизации. Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).
 
5.  Выйдите из системы.
 
    Щелкнув **Выход** связывать вызовы `LogOut` действие.
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    Предыдущий код выше вызовы `_signInManager.SignOutAsync` метод. `SignOutAsync` Метод очищает утверждения пользователей, хранящихся в файле cookie.
 
6.  Конфигурация.

    Удостоверение имеет некоторые виды поведения по умолчанию, которые можно переопределить в классе при запуске приложения. Необходимо настроить ``IdentityOptions`` при использовании поведения по умолчанию.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    Дополнительные сведения о настройке удостоверений см. в разделе [Настройка удостоверений](xref:security/authentication/identity-configuration).
    
    Можно также настроить тип данных первичный ключ. в разделе [тип данных первичные ключи Настройка удостоверения](xref:security/authentication/identity-primary-key-configuration).
 
7.  Просмотр базы данных.

    Если приложение использует базу данных SQL Server (по умолчанию в Windows и для пользователей Visual Studio), можно просматривать базы данных с приложением, создаваемым. Можно использовать **SQL Server Management Studio**. Кроме того, в Visual Studio, выберите **представление** > **обозреватель объектов SQL Server**. Подключиться к **(localdb) \MSSQLLocalDB**. База данных с именем, соответствующим  **aspnet - <*имя проекта*>-<*Дата строка*> ** отображается.

    ![Контекстные меню AspNetUsers таблицу базы данных](identity/_static/04-db.png)
    
    Разверните базу данных и его **таблиц**, щелкните правой кнопкой мыши **dbo. AspNetUsers** таблицы и выберите **данные представления**.

8. Проверки удостоверения работы

    Значение по умолчанию *веб-приложения ASP.NET Core* шаблон проекта предоставляет пользователям доступ к любое действие в приложении без повторного входа. Чтобы проверить работоспособность ASP.NET Identity, добавьте`[Authorize]` атрибут `About` действие `Home` контроллера.
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Запустите проект с помощью **Ctrl** + **F5** и перейдите к **о** страницы. Только прошедшие проверку подлинности пользователи могут получить доступ к **о** страницы, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.

    # <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

    Откройте окно командной строки и перейдите в корневой каталог проекта в каталог, содержащий `.csproj` файла. Запустите `dotnet run` команду, чтобы запустить приложение:

    ```cs
    dotnet run 
    ```

    Обзор URL-адрес, указанный в выходные данные `dotnet run` команды. URL-адрес должен указывать на `localhost` с созданный номер порта. Перейдите к **о** страницы. Только прошедшие проверку подлинности пользователи могут получить доступ к **о** страницы, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.

    ---

## <a name="identity-components"></a>Компоненты идентификаторов

Основная ссылка сборки для системы удостоверений `Microsoft.AspNetCore.Identity`. Данный пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включается по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Эти зависимости необходимы для использования в приложениях ASP.NET Core системы удостоверений.

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Типы, необходимые для использования с Entity Framework Core удостоверений.

* `Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core — технология доступа к данных, рекомендуемые корпорации Майкрософт для реляционных баз данных, как SQL Server. Для тестирования, можно использовать `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies`-Промежуточное по, которая позволяет приложению использовать проверку подлинности на основе файлов cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Переход к удостоверению ASP.NET Core

Дополнительные сведения и инструкции по миграции существующего личности магазине см. в разделе [миграции проверку подлинности и удостоверение](xref:migration/identity).

## <a name="next-steps"></a>Следующие шаги

* [Миграция на другой метод проверки подлинности и другие удостоверения](xref:migration/identity)
* [Подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm)
* [Двухфакторная проверка подлинности с помощью SMS](xref:security/authentication/2fa)
* [Включение проверки подлинности с помощью Facebook, Google и других внешних поставщиков](xref:security/authentication/social/index)
