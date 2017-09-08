---
title: "Общие сведения об учетных данных ASP.NET Core"
author: rick-anderson
description: "Использование идентификаторов с приложением ASP.NET Core"
keywords: "ASP.NET Core, удостоверение, авторизации, безопасность"
ms.author: riande
manager: wpickett
ms.date: 7/7/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 5718336868f3ee5ab08162ae2bc885c695d19a1d
ms.sourcegitcommit: f3366461010da37981cf7fc092b9b9613eb4ca89
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/18/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="97667-104">Общие сведения об учетных данных ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97667-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="97667-105">По [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Джон Гэллоуэй [Reitan Эрик](https://github.com/Erikre), и [Стив Смит](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="97667-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="97667-106">Удостоверение ASP.NET Core является система членства, в котором можно добавить функциональные возможности входа в приложение.</span><span class="sxs-lookup"><span data-stu-id="97667-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="97667-107">Пользователи могут создавать учетную запись и имя входа с именем пользователя и пароль или их можно использовать поставщик внешней учетной записи, например Facebook, Google, учетной записи Майкрософт, Twitter или другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="97667-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="97667-108">Вы можете настроить ASP.NET Identity Core использование базы данных SQL Server для хранения имен пользователей, пароли и данные профиля.</span><span class="sxs-lookup"><span data-stu-id="97667-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="97667-109">Кроме того можно использовать собственные постоянное хранилище, например табличного хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="97667-109">Alternatively, you can use your own persistent store, for example Azure Table Storage.</span></span> <span data-ttu-id="97667-110">Этот документ содержит инструкции по Visual Studio и с помощью CLI.</span><span class="sxs-lookup"><span data-stu-id="97667-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="97667-111">Общие сведения об идентификации</span><span class="sxs-lookup"><span data-stu-id="97667-111">Overview of Identity</span></span>

<span data-ttu-id="97667-112">В этом разделе будет использование ASP.NET Core Identity Добавление функциональности для регистрации, вход и выход пользователя.</span><span class="sxs-lookup"><span data-stu-id="97667-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="97667-113">Более подробные инструкции по созданию приложений с помощью ASP.NET Core Identity см. в разделе Дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="97667-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="97667-114">Создайте проект веб-приложения ASP.NET Core с отдельными учетными записями пользователей.</span><span class="sxs-lookup"><span data-stu-id="97667-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="97667-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97667-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="97667-116">В Visual Studio, выберите **файл** -> **New** -> **проекта**.</span><span class="sxs-lookup"><span data-stu-id="97667-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="97667-117">Выберите **веб-приложение ASP.NET** из **новый проект** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="97667-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="97667-118">При выборе ASP.NET Core **веб-приложение** с **индивидуальные учетные записи** в качестве метода проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="97667-118">Selecting an ASP.NET Core **Web Application** with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="97667-119">Примечание: Необходимо выбрать **индивидуальные учетные записи**.</span><span class="sxs-lookup"><span data-stu-id="97667-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![Диалоговое окно создания нового проекта](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="97667-121">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="97667-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="97667-122">Если используется .NET Core CLI, создайте новый проект с помощью ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="97667-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="97667-123">Это создаст новый проект с тем же кодом шаблона удостоверений, создаваемых в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97667-123">This will create a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="97667-124">Созданный проект содержит `Microsoft.AspNetCore.Identity.EntityFrameworkCore` пакет, который будет сохраняться данные удостоверений и схемы SQL Server с помощью [Entity Framework Core](https://docs.efproject.net).</span><span class="sxs-lookup"><span data-stu-id="97667-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which will persist the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.efproject.net).</span></span>
    
    ---
 
2.  <span data-ttu-id="97667-125">Настройка службы удостоверений и добавление по промежуточного слоя в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="97667-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="97667-126">Службы удостоверений добавляются к приложению в `ConfigureServices` метод `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="97667-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>
 
    <span data-ttu-id="97667-127">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configureservices&highlight=7-9,13-34)]</span><span class="sxs-lookup"><span data-stu-id="97667-127">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configureservices&highlight=7-9,13-34)]</span></span>
    
    <span data-ttu-id="97667-128">Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="97667-128">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
 
    <span data-ttu-id="97667-129">Путем вызова для приложения включена удостоверения `UseIdentity` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="97667-129">Identity is enabled for the application by calling  `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="97667-130">`UseIdentity`Добавляет файл cookie проверки подлинности с помощью [по промежуточного слоя](xref:fundamentals/middleware) к конвейеру запросов.</span><span class="sxs-lookup"><span data-stu-id="97667-130">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
 
    <span data-ttu-id="97667-131">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configure&highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="97667-131">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configure&highlight=21)]</span></span>
 
    <span data-ttu-id="97667-132">Дополнительные сведения о время загрузки приложения см. в разделе [запуска приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="97667-132">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="97667-133">Создайте пользователя.</span><span class="sxs-lookup"><span data-stu-id="97667-133">Create a user.</span></span>
 
    <span data-ttu-id="97667-134">Запустите приложение и щелкните на **зарегистрировать** ссылку.</span><span class="sxs-lookup"><span data-stu-id="97667-134">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="97667-135">Если при первом выполнении этого действия может потребоваться для выполнения миграции.</span><span class="sxs-lookup"><span data-stu-id="97667-135">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="97667-136">Приложение предложит **применить миграции**:</span><span class="sxs-lookup"><span data-stu-id="97667-136">The application prompts you to **Apply Migrations**:</span></span>
    
    ![Применить миграции веб-страницы](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="97667-138">Кроме того можно проверить с помощью ASP.NET Core Identity вместе с приложением без постоянных баз данных с помощью базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="97667-138">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="97667-139">Чтобы использовать базу данных в памяти, добавьте ``Microsoft.EntityFrameworkCore.InMemory`` пакета приложения и изменить вызов приложения ``AddDbContext`` в ``ConfigureServices`` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="97667-139">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="97667-140">Когда пользователь щелкает **зарегистрировать** ссылку, ``Register`` при вызове действия на ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="97667-140">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="97667-141">``Register`` Действие создает пользователя путем вызова `CreateAsync` на `_userManager` объекта (для ``AccountController`` путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="97667-141">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    <span data-ttu-id="97667-142">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=register&highlight=11)]</span><span class="sxs-lookup"><span data-stu-id="97667-142">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=register&highlight=11)]</span></span>

    <span data-ttu-id="97667-143">Если пользователь успешно создан, пользователь вошел вызове ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="97667-143">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="97667-144">**Примечание:** разделе [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) для меры по предотвращению немедленно входа при регистрации.</span><span class="sxs-lookup"><span data-stu-id="97667-144">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="97667-145">Войти.</span><span class="sxs-lookup"><span data-stu-id="97667-145">Log in.</span></span>
 
    <span data-ttu-id="97667-146">Пользователи могут войти, щелкнув **входа** ссылок в верхней части сайта, или может быть переход на страницу входа, если они пытаются получить доступ к части сайта, требующей авторизации.</span><span class="sxs-lookup"><span data-stu-id="97667-146">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="97667-147">Когда пользователь отправляет форму на странице входа ``AccountController`` ``Login`` вызова действия.</span><span class="sxs-lookup"><span data-stu-id="97667-147">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="97667-148">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=login&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="97667-148">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=login&highlight=13-14)]</span></span>
 
    <span data-ttu-id="97667-149">``Login`` Вызывает действие ``PasswordSignInAsync`` на ``_signInManager`` объекта (для ``AccountController`` путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="97667-149">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="97667-150">Базовый ``Controller`` предоставляемых классами ``User`` свойство, которое можно открыть из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="97667-150">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="97667-151">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="97667-151">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="97667-152">Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="97667-152">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="97667-153">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="97667-153">Log out.</span></span>
 
    <span data-ttu-id="97667-154">Щелкнув **Выход** связывать вызовы `LogOut` действие.</span><span class="sxs-lookup"><span data-stu-id="97667-154">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    <span data-ttu-id="97667-155">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=logout&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="97667-155">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=logout&highlight=7)]</span></span>
 
    <span data-ttu-id="97667-156">Предыдущий код выше вызовы `_signInManager.SignOutAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="97667-156">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="97667-157">`SignOutAsync` Метод очищает утверждения пользователей, хранящихся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="97667-157">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="97667-158">Конфигурация.</span><span class="sxs-lookup"><span data-stu-id="97667-158">Configuration.</span></span>

    <span data-ttu-id="97667-159">Удостоверение имеет некоторые виды поведения по умолчанию, которые можно переопределить в классе при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="97667-159">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="97667-160">Необходимо настроить ``IdentityOptions`` при использовании поведения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="97667-160">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>
 
    <span data-ttu-id="97667-161">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configureservices&highlight=13-34)]</span><span class="sxs-lookup"><span data-stu-id="97667-161">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=configureservices&highlight=13-34)]</span></span>
    
    <span data-ttu-id="97667-162">Дополнительные сведения о настройке удостоверений см. в разделе [Настройка удостоверений](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="97667-162">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="97667-163">Можно также настроить тип данных первичный ключ. в разделе [тип данных первичные ключи Настройка удостоверения](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="97667-163">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="97667-164">Просмотр базы данных.</span><span class="sxs-lookup"><span data-stu-id="97667-164">View the database.</span></span>

    <span data-ttu-id="97667-165">Если приложение использует базу данных SQL Server (по умолчанию в Windows и для пользователей Visual Studio), можно просматривать базы данных с приложением, создаваемым.</span><span class="sxs-lookup"><span data-stu-id="97667-165">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="97667-166">Можно использовать **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="97667-166">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="97667-167">Кроме того, в Visual Studio, выберите **представление** -> **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="97667-167">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="97667-168">Подключиться к **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="97667-168">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="97667-169">База данных с именем, соответствующим  **aspnet - <*имя проекта*>-<*Дата строка*> ** отображается.</span><span class="sxs-lookup"><span data-stu-id="97667-169">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Контекстные меню AspNetUsers таблицу базы данных](identity/_static/04-db.png)
    
    <span data-ttu-id="97667-171">Разверните базу данных и его **таблиц**, щелкните правой кнопкой мыши **dbo. AspNetUsers** таблицы и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="97667-171">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="97667-172">Компоненты идентификаторов</span><span class="sxs-lookup"><span data-stu-id="97667-172">Identity Components</span></span>

<span data-ttu-id="97667-173">Основная ссылка сборки для системы удостоверений `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="97667-173">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="97667-174">Данный пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включается по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="97667-174">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="97667-175">Эти зависимости необходимы для использования в приложениях ASP.NET Core системы удостоверений.</span><span class="sxs-lookup"><span data-stu-id="97667-175">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="97667-176">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Типы, необходимые для использования с Entity Framework Core удостоверений.</span><span class="sxs-lookup"><span data-stu-id="97667-176">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="97667-177">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core — технология доступа к данных, рекомендуемые корпорации Майкрософт для реляционных баз данных, как SQL Server.</span><span class="sxs-lookup"><span data-stu-id="97667-177">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="97667-178">Для тестирования, можно использовать `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="97667-178">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="97667-179">`Microsoft.AspNetCore.Authentication.Cookies`-Промежуточное по, которая позволяет приложению использовать проверку подлинности на основе файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="97667-179">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="97667-180">Переход к удостоверению ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97667-180">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="97667-181">Дополнительные сведения и инструкции по миграции существующего личности магазине см. в разделе [миграции проверку подлинности и удостоверение](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="97667-181">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="97667-182">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="97667-182">Next Steps</span></span>

* [<span data-ttu-id="97667-183">Миграция проверку подлинности и удостоверение</span><span class="sxs-lookup"><span data-stu-id="97667-183">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="97667-184">Подтверждение учетной записи и восстановления пароля.</span><span class="sxs-lookup"><span data-stu-id="97667-184">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="97667-185">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="97667-185">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="97667-186">Включение проверки подлинности с использованием Facebook, Google и других внешних поставщиков</span><span class="sxs-lookup"><span data-stu-id="97667-186">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
