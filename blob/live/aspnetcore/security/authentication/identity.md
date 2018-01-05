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
ms.openlocfilehash: 7af53bfad2b77558a06003cbc6534236235054c4
ms.sourcegitcommit: 677986b3a39817b712e2432cce85ad1685326b75
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/04/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="4fb8e-104">Общие сведения об учетных данных ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4fb8e-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="4fb8e-105">По [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Джон Гэллоуэй [Reitan Эрик](https://github.com/Erikre), и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4fb8e-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4fb8e-106">Удостоверение ASP.NET Core является система членства, в котором можно добавить функциональные возможности входа в приложение.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="4fb8e-107">Пользователи могут создавать учетную запись и имя входа с именем пользователя и пароль или их можно использовать поставщик внешней учетной записи, например Facebook, Google, учетной записи Майкрософт, Twitter или другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="4fb8e-108">Вы можете настроить ASP.NET Identity Core использование базы данных SQL Server для хранения имен пользователей, пароли и данные профиля.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="4fb8e-109">Кроме того можно использовать собственные постоянное хранилище, например, табличное хранилище Azure.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="4fb8e-110">Этот документ содержит инструкции по Visual Studio и с помощью CLI.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="4fb8e-111">Общие сведения об идентификации</span><span class="sxs-lookup"><span data-stu-id="4fb8e-111">Overview of Identity</span></span>

<span data-ttu-id="4fb8e-112">В этом разделе будет использование ASP.NET Core Identity Добавление функциональности для регистрации, вход и выход пользователя.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="4fb8e-113">Более подробные инструкции по созданию приложений с помощью ASP.NET Core Identity см. в разделе Дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="4fb8e-114">Создайте проект веб-приложения ASP.NET Core с отдельными учетными записями пользователей.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4fb8e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4fb8e-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="4fb8e-116">В Visual Studio, выберите **файл** -> **New** -> **проекта**.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="4fb8e-117">Выберите **веб-приложения ASP.NET Core** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-117">Select **ASP.NET Core Web Application** and click **OK**.</span></span> 

    ![Диалоговое окно создания нового проекта](identity/_static/01-new-project.png)

    <span data-ttu-id="4fb8e-119">Выберите ASP.NET Core **веб-приложения (Model-View-Controller)** для ASP.NET Core 2.x, а затем выберите **изменить аутентификацию**.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-119">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span> 

    ![Диалоговое окно создания нового проекта](identity/_static/02-new-project.png)

    <span data-ttu-id="4fb8e-121">Откроется диалоговое окно предложения вариантов проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-121">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="4fb8e-122">Выберите **отдельных учетных записей пользователей** и нажмите кнопку **ОК** для возврата к диалоговому окну предыдущего.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-122">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![Диалоговое окно создания нового проекта](identity/_static/03-new-project-auth.png)
    
    <span data-ttu-id="4fb8e-124">При выборе **отдельных учетных записей пользователей** направляет Visual Studio для создания моделей, ViewModels, представления, контроллеры и другие активы, необходимые для проверки подлинности как часть шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-124">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>
 
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4fb8e-125">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="4fb8e-125">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="4fb8e-126">Если используется .NET Core CLI, создайте новый проект с помощью ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-126">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="4fb8e-127">Эта команда создает новый проект с тем же кодом шаблона удостоверений, создаваемых в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-127">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="4fb8e-128">Созданный проект содержит `Microsoft.AspNetCore.Identity.EntityFrameworkCore` пакет, который сохраняет данные удостоверений и схемы SQL Server с помощью [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="4fb8e-128">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>
    
    ---
 
2.  <span data-ttu-id="4fb8e-129">Настройка службы удостоверений и добавление по промежуточного слоя в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-129">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="4fb8e-130">Службы удостоверений добавляются к приложению в `ConfigureServices` метод `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="4fb8e-130">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4fb8e-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4fb8e-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="4fb8e-132">Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4fb8e-132">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="4fb8e-133">Путем вызова для приложения включена удостоверения `UseAuthentication` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-133">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="4fb8e-134">`UseAuthentication`Добавление проверки подлинности [по промежуточного слоя](xref:fundamentals/middleware) к конвейеру запросов.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-134">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4fb8e-135">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4fb8e-135">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="4fb8e-136">Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4fb8e-136">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="4fb8e-137">Путем вызова для приложения включена удостоверения `UseIdentity` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-137">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="4fb8e-138">`UseIdentity`Добавляет файл cookie проверки подлинности с помощью [по промежуточного слоя](xref:fundamentals/middleware) к конвейеру запросов.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-138">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="4fb8e-139">Дополнительные сведения о время загрузки приложения см. в разделе [запуска приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="4fb8e-139">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="4fb8e-140">Создайте пользователя.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-140">Create a user.</span></span>

    <span data-ttu-id="4fb8e-141">Запустите приложение и щелкните на **зарегистрировать** ссылку.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-141">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="4fb8e-142">Если при первом выполнении этого действия может потребоваться для выполнения миграции.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-142">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="4fb8e-143">Приложение предложит **применить миграции**.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-143">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="4fb8e-144">При необходимости обновите страницу.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-144">Refresh the page if needed.</span></span>
    
    ![Применить миграции веб-страницы](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="4fb8e-146">Кроме того можно проверить с помощью ASP.NET Core Identity вместе с приложением без постоянных баз данных с помощью базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-146">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="4fb8e-147">Чтобы использовать базу данных в памяти, добавьте ``Microsoft.EntityFrameworkCore.InMemory`` пакета приложения и изменить вызов приложения ``AddDbContext`` в ``ConfigureServices`` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4fb8e-147">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="4fb8e-148">Когда пользователь щелкает **зарегистрировать** ссылку, ``Register`` при вызове действия на ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-148">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="4fb8e-149">``Register`` Действие создает пользователя путем вызова `CreateAsync` на `_userManager` объекта (для ``AccountController`` путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="4fb8e-149">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="4fb8e-150">Если пользователь успешно создан, пользователь вошел вызове ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-150">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="4fb8e-151">**Примечание:** разделе [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) для меры по предотвращению немедленно входа при регистрации.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-151">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="4fb8e-152">Войти.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-152">Log in.</span></span>
 
    <span data-ttu-id="4fb8e-153">Пользователи могут войти, щелкнув **входа** ссылок в верхней части сайта, или может быть переход на страницу входа, если они пытаются получить доступ к части сайта, требующей авторизации.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-153">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="4fb8e-154">Когда пользователь отправляет форму на странице входа ``AccountController`` ``Login`` вызова действия.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-154">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="4fb8e-155">``Login`` Вызывает действие ``PasswordSignInAsync`` на ``_signInManager`` объекта (для ``AccountController`` путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="4fb8e-155">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="4fb8e-156">Базовый ``Controller`` предоставляемых классами ``User`` свойство, которое можно открыть из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-156">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="4fb8e-157">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-157">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="4fb8e-158">Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="4fb8e-158">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="4fb8e-159">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-159">Log out.</span></span>
 
    <span data-ttu-id="4fb8e-160">Щелкнув **Выход** связывать вызовы `LogOut` действие.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-160">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="4fb8e-161">Предыдущий код выше вызовы `_signInManager.SignOutAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-161">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="4fb8e-162">`SignOutAsync` Метод очищает утверждения пользователей, хранящихся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-162">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="4fb8e-163">Конфигурация.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-163">Configuration.</span></span>

    <span data-ttu-id="4fb8e-164">Удостоверение имеет некоторые виды поведения по умолчанию, которые можно переопределить в классе при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-164">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="4fb8e-165">Необходимо настроить ``IdentityOptions`` при использовании поведения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-165">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4fb8e-166">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4fb8e-166">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4fb8e-167">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4fb8e-167">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="4fb8e-168">Дополнительные сведения о настройке удостоверений см. в разделе [Настройка удостоверений](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="4fb8e-168">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="4fb8e-169">Можно также настроить тип данных первичный ключ. в разделе [тип данных первичные ключи Настройка удостоверения](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="4fb8e-169">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="4fb8e-170">Просмотр базы данных.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-170">View the database.</span></span>

    <span data-ttu-id="4fb8e-171">Если приложение использует базу данных SQL Server (по умолчанию в Windows и для пользователей Visual Studio), можно просматривать базы данных с приложением, создаваемым.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-171">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="4fb8e-172">Можно использовать **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-172">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="4fb8e-173">Кроме того, в Visual Studio, выберите **представление** -> **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-173">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="4fb8e-174">Подключиться к **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-174">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="4fb8e-175">База данных с именем, соответствующим  **aspnet - <*имя проекта*>-<*Дата строка*> ** отображается.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-175">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Контекстные меню AspNetUsers таблицу базы данных](identity/_static/04-db.png)
    
    <span data-ttu-id="4fb8e-177">Разверните базу данных и его **таблиц**, щелкните правой кнопкой мыши **dbo. AspNetUsers** таблицы и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-177">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="4fb8e-178">Проверки удостоверения работы</span><span class="sxs-lookup"><span data-stu-id="4fb8e-178">Verify Identity works</span></span>

    <span data-ttu-id="4fb8e-179">Значение по умолчанию *веб-приложения ASP.NET Core* шаблон проекта предоставляет пользователям доступ к любое действие в приложении без повторного входа.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-179">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="4fb8e-180">Чтобы проверить работоспособность ASP.NET Identity, добавьте`[Authorize]` атрибут `About` действие `Home` контроллера.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-180">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisualstudio"></a>[<span data-ttu-id="4fb8e-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4fb8e-181">Visual Studio</span></span>](#tab/visualstudio)     

    <span data-ttu-id="4fb8e-182">Запустите проект с помощью **Ctrl** + **F5** и перейдите к **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-182">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="4fb8e-183">Только прошедшие проверку подлинности пользователи могут получить доступ к **о** страницы, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-183">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4fb8e-184">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="4fb8e-184">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="4fb8e-185">Откройте окно командной строки и перейдите в корневой каталог проекта в каталог, содержащий `.csproj` файла.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-185">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="4fb8e-186">Запустите `dotnet run` команду, чтобы запустить приложение:</span><span class="sxs-lookup"><span data-stu-id="4fb8e-186">Run the `dotnet run` command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="4fb8e-187">Обзор URL-адрес, указанный в выходные данные `dotnet run` команды.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-187">Browse the URL specified in the output from the `dotnet run` command.</span></span> <span data-ttu-id="4fb8e-188">URL-адрес должен указывать на `localhost` с созданный номер порта.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-188">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="4fb8e-189">Перейдите к **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-189">Navigate to the **About** page.</span></span> <span data-ttu-id="4fb8e-190">Только прошедшие проверку подлинности пользователи могут получить доступ к **о** страницы, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-190">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="4fb8e-191">Компоненты идентификаторов</span><span class="sxs-lookup"><span data-stu-id="4fb8e-191">Identity Components</span></span>

<span data-ttu-id="4fb8e-192">Основная ссылка сборки для системы удостоверений `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-192">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="4fb8e-193">Данный пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включается по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-193">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="4fb8e-194">Эти зависимости необходимы для использования в приложениях ASP.NET Core системы удостоверений.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-194">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="4fb8e-195">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Типы, необходимые для использования с Entity Framework Core удостоверений.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-195">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="4fb8e-196">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core — технология доступа к данных, рекомендуемые корпорации Майкрософт для реляционных баз данных, как SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-196">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="4fb8e-197">Для тестирования, можно использовать `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-197">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="4fb8e-198">`Microsoft.AspNetCore.Authentication.Cookies`-Промежуточное по, которая позволяет приложению использовать проверку подлинности на основе файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="4fb8e-198">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="4fb8e-199">Переход к удостоверению ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4fb8e-199">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="4fb8e-200">Дополнительные сведения и инструкции по миграции существующего личности магазине см. в разделе [миграции проверку подлинности и удостоверение](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="4fb8e-200">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4fb8e-201">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="4fb8e-201">Next Steps</span></span>

* [<span data-ttu-id="4fb8e-202">Миграция на другой метод проверки подлинности и другие удостоверения</span><span class="sxs-lookup"><span data-stu-id="4fb8e-202">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="4fb8e-203">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="4fb8e-203">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="4fb8e-204">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="4fb8e-204">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="4fb8e-205">Включение проверки подлинности с помощью Facebook, Google и других внешних поставщиков</span><span class="sxs-lookup"><span data-stu-id="4fb8e-205">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
