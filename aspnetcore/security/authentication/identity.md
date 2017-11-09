---
title: "Общие сведения об учетных данных ASP.NET Core"
author: rick-anderson
description: "Использование идентификаторов с приложением ASP.NET Core"
keywords: "ASP.NET Core, удостоверение, авторизации, безопасность"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 820836eaf3a29c9941e84458f09ac470f8150ba7
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Общие сведения об учетных данных ASP.NET Core

По [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Джон Гэллоуэй [Reitan Эрик](https://github.com/Erikre), и [Стив Смит](https://ardalis.com/)

ASP.NET Core Identity позволяет реализовать проверку подлинности в приложении. Пользователи могут создавать учетную запись с логином и паролем, либо использовать внешнего поставщика, таких как Facebook, Google, Microsoft, Twitter и другие.

Вы можете настроить ASP.NET Identity Core для использования базы данных SQL Server для хранения учетных данных пользователей. Помимо этого, можно использовать собственные постоянные хранилища, например Azure Table Storage. 

Этот документ содержит инструкции по использованию Visual Studio и CLI.

## <a name="overview-of-identity"></a>Общие сведения об идентификации

В этом разделе вы узнаете, как использовать ASP.NET Core Identity для реализации регистрации, входа и выхода пользователя. Более подробные инструкции по созданию приложений с помощью ASP.NET Core Identity см. в разделе «Дальнейшие действия» в конце этой статьи.

1.  Создание проекта «Веб-приложение ASP.NET Core» с использованием проверки подлинности «Индивидуальные учетные записи пользователей».

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)
    В Visual Studio, выберите меню **Файл** -> **Создать** -> **Проект**. Выберите **Веб-приложение ASP.NET Core** в диалоговом окне **Создание проекта**. Затем выберите **Веб-приложение** и в качестве способа проверки пользователя укажите с **Индивидуальные учетные записи пользователей**.

    Примечание: Необходимо выбрать **Индивидуальные учетные записи пользователей**.
 
    ![Диалоговое окно создания нового проекта](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)
    Если используется .NET Core CLI, выполните команду ``dotnet new mvc --auth Individual``. Это создаст новый проект с кодом шаблона проверки подлинности, которые создает Visual Studio.
 
    Созданный проект содержит пакет `Microsoft.AspNetCore.Identity.EntityFrameworkCore`, который будет использовать базу данных SQL Server для хранения удостоверений и создаст схему базы с помощью [Entity Framework Core](https://docs.microsoft.com/ef/).
    
    ---
 
2.  Настройка службы проверки подлинности и добавление промежуточного слоя в классе `Startup`.

    Службы проверки подлинности регистрируются в приложении в методе `ConfigureServices` класса `Startup`:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).
    
    Путем вызова для приложения включена удостоверения `UseAuthentication` в `Configure` метод. `UseAuthentication`Добавление проверки подлинности [по промежуточного слоя](xref:fundamentals/middleware) к конвейеру запросов.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).
    
    Проверка подлинности включается с помощью вызова `UseIdentity` в методе `Configure`. Метод `UseIdentity` добавляет аутентификацию на основе файлов cookie с помощью [промежуточного слоя](xref:fundamentals/middleware) к конвейеру запросов.
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Дополнительные сведения о запуске приложения см. в разделе [запуск приложения](xref:fundamentals/startup).

3.  Создание пользователя.

    Запустите приложение и щелкните по ссылке **Register** (Регистрация).

    При первом запуске, вам может потребоваться выполнить миграцию. Приложение предложит **Apply Migrations** (Выполнить миграцию):
    
    ![Страница миграции](identity/_static/apply-migrations.png)
    
    Кроме того, можно проверить работу ASP.NET Core Identity без использования постоянной баз данных, а применением базы данных в памяти. Чтобы использовать базу данных в памяти, добавьте в приложение пакет ``Microsoft.EntityFrameworkCore.InMemory`` и измените вызов метода ``AddDbContext`` в ``ConfigureServices`` следующим образом:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Когда пользователь нажимает на ссылку **Register** (Регистрация), вызывается метод действия ``Register`` контроллера ``AccountController``. Действие ``Register`` создает пользователя путем вызова метода `CreateAsync` объекта `_userManager` (обеспечивается в ``AccountController`` путем внедрения зависимостей):
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Если пользователь успешно создан, производит вход пользователя с путем вызова метода ``_signInManager.SignInAsync``.

    **Примечание:** См. раздел [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) для  предотвращения автоматического входа пользователя при регистрации.
 
4.  Вход.
 
    Пользователи могут войти, щелкнув **входа** ссылок в верхней части сайта, или может быть переход на страницу входа, если они пытаются получить доступ к части сайта, требующей авторизации. Когда пользователь отправляет форму на странице входа ``AccountController`` ``Login`` вызова действия.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    ``Login`` Вызывает действие ``PasswordSignInAsync`` на ``_signInManager`` объекта (для ``AccountController`` путем внедрения зависимостей).
 
    Базовый ``Controller`` предоставляемых классами ``User`` свойство, которое можно открыть из методов контроллера. Например, можно перечислить `User.Claims` и принимать решения об авторизации. Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).
 
5.  Выход.
 
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

    Если приложение использует базу данных SQL Server (по умолчанию в Windows и для пользователей Visual Studio), можно просматривать базы данных с приложением, создаваемым. Можно использовать **SQL Server Management Studio**. Кроме того, в Visual Studio, выберите **представление** -> **обозреватель объектов SQL Server**. Подключиться к **(localdb) \MSSQLLocalDB**. База данных с именем, соответствующим * *aspnet - <*имя проекта*>-<*Дата строка*> ** отображается.

    ![Контекстные меню AspNetUsers таблицу базы данных](identity/_static/04-db.png)
    
    Разверните базу данных и его **таблиц**, щелкните правой кнопкой мыши **dbo. AspNetUsers** таблицы и выберите **данные представления**.

## <a name="identity-components"></a>Компоненты идентификаторов

Основная ссылка сборки для системы удостоверений `Microsoft.AspNetCore.Identity`. Данный пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включается по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Эти зависимости необходимы для использования в приложениях ASP.NET Core системы удостоверений.

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Типы, необходимые для использования с Entity Framework Core удостоверений.

* `Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core — технология доступа к данных, рекомендуемые корпорации Майкрософт для реляционных баз данных, как SQL Server. Для тестирования, можно использовать `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies`-Промежуточное по, которая позволяет приложению использовать проверку подлинности на основе файлов cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Переход к удостоверению ASP.NET Core

Дополнительные сведения и инструкции по миграции существующего личности магазине см. в разделе [миграции проверку подлинности и удостоверение](xref:migration/identity).

## <a name="next-steps"></a>Дальнейшие действия

* [Миграция на другой метод проверки подлинности и другие удостоверения](xref:migration/identity)
* [Подтверждение учетной записи и восстановление пароля](xref:security/authentication/accconfirm)
* [Двухфакторная проверка подлинности с помощью SMS](xref:security/authentication/2fa)
* [Включение проверки подлинности с помощью Facebook, Google и других внешних поставщиков](xref:security/authentication/social/index)
