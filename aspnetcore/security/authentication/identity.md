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
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="8831f-104">Общие сведения об учетных данных ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8831f-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="8831f-105">По [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Джон Гэллоуэй [Reitan Эрик](https://github.com/Erikre), и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8831f-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8831f-106">Удостоверение ASP.NET Core является система членства, в котором можно добавить функциональные возможности входа в приложение.</span><span class="sxs-lookup"><span data-stu-id="8831f-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="8831f-107">Пользователи могут создавать учетную запись и имя входа с именем пользователя и пароль или их можно использовать поставщик внешней учетной записи, например Facebook, Google, учетной записи Майкрософт, Twitter или другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="8831f-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="8831f-108">Вы можете настроить ASP.NET Identity Core использование базы данных SQL Server для хранения имен пользователей, пароли и данные профиля.</span><span class="sxs-lookup"><span data-stu-id="8831f-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="8831f-109">Кроме того можно использовать собственные постоянное хранилище, например, табличное хранилище Azure.</span><span class="sxs-lookup"><span data-stu-id="8831f-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="8831f-110">Этот документ содержит инструкции по Visual Studio и с помощью CLI.</span><span class="sxs-lookup"><span data-stu-id="8831f-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="8831f-111">Просмотреть или загрузить образец кода.</span><span class="sxs-lookup"><span data-stu-id="8831f-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="8831f-112">(Сведения о загрузке)</span><span class="sxs-lookup"><span data-stu-id="8831f-112">(How to download)</span></span>](https://docs.microsoft.com/en-us/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="8831f-113">Общие сведения об идентификации</span><span class="sxs-lookup"><span data-stu-id="8831f-113">Overview of Identity</span></span>

<span data-ttu-id="8831f-114">В этом разделе будет использование ASP.NET Core Identity Добавление функциональности для регистрации, вход и выход пользователя.</span><span class="sxs-lookup"><span data-stu-id="8831f-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="8831f-115">Более подробные инструкции по созданию приложений с помощью ASP.NET Core Identity см. в разделе Дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="8831f-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="8831f-116">Создайте проект веб-приложения ASP.NET Core с отдельными учетными записями пользователей.</span><span class="sxs-lookup"><span data-stu-id="8831f-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8831f-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8831f-117">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="8831f-118">В Visual Studio, выберите **файл** > **New** > **проекта**.</span><span class="sxs-lookup"><span data-stu-id="8831f-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="8831f-119">Выберите **веб-приложения ASP.NET Core** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8831f-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

    ![Диалоговое окно создания нового проекта](identity/_static/01-new-project.png)

    <span data-ttu-id="8831f-121">Выберите ASP.NET Core **веб-приложения (Model-View-Controller)** для ASP.NET Core 2.x, а затем выберите **изменить аутентификацию**.</span><span class="sxs-lookup"><span data-stu-id="8831f-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

    ![Диалоговое окно создания нового проекта](identity/_static/02-new-project.png)

    <span data-ttu-id="8831f-123">Откроется диалоговое окно предложения вариантов проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="8831f-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="8831f-124">Выберите **отдельных учетных записей пользователей** и нажмите кнопку **ОК** для возврата к диалоговому окну предыдущего.</span><span class="sxs-lookup"><span data-stu-id="8831f-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![Диалоговое окно создания нового проекта](identity/_static/03-new-project-auth.png)

    <span data-ttu-id="8831f-126">При выборе **отдельных учетных записей пользователей** направляет Visual Studio для создания моделей, ViewModels, представления, контроллеры и другие активы, необходимые для проверки подлинности как часть шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="8831f-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8831f-127">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="8831f-127">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="8831f-128">Если используется .NET Core CLI, создайте новый проект с помощью ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="8831f-128">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="8831f-129">Эта команда создает новый проект с тем же кодом шаблона удостоверений, создаваемых в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8831f-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

    <span data-ttu-id="8831f-130">Созданный проект содержит `Microsoft.AspNetCore.Identity.EntityFrameworkCore` пакет, который сохраняет данные удостоверений и схемы SQL Server с помощью [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="8831f-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

    ---

2.  <span data-ttu-id="8831f-131">Настройка службы удостоверений и добавление по промежуточного слоя в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8831f-131">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="8831f-132">Службы удостоверений добавляются к приложению в `ConfigureServices` метод `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="8831f-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8831f-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8831f-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="8831f-134">Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8831f-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="8831f-135">Путем вызова для приложения включена удостоверения `UseAuthentication` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="8831f-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="8831f-136">`UseAuthentication`Добавление проверки подлинности [по промежуточного слоя](xref:fundamentals/middleware) к конвейеру запросов.</span><span class="sxs-lookup"><span data-stu-id="8831f-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8831f-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8831f-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="8831f-138">Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8831f-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="8831f-139">Путем вызова для приложения включена удостоверения `UseIdentity` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="8831f-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="8831f-140">`UseIdentity`Добавляет файл cookie проверки подлинности с помощью [по промежуточного слоя](xref:fundamentals/middleware) к конвейеру запросов.</span><span class="sxs-lookup"><span data-stu-id="8831f-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="8831f-141">Дополнительные сведения о время загрузки приложения см. в разделе [запуска приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="8831f-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="8831f-142">Создайте пользователя.</span><span class="sxs-lookup"><span data-stu-id="8831f-142">Create a user.</span></span>

    <span data-ttu-id="8831f-143">Запустите приложение и щелкните на **зарегистрировать** ссылку.</span><span class="sxs-lookup"><span data-stu-id="8831f-143">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="8831f-144">Если при первом выполнении этого действия может потребоваться для выполнения миграции.</span><span class="sxs-lookup"><span data-stu-id="8831f-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="8831f-145">Приложение предложит **применить миграции**.</span><span class="sxs-lookup"><span data-stu-id="8831f-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="8831f-146">При необходимости обновите страницу.</span><span class="sxs-lookup"><span data-stu-id="8831f-146">Refresh the page if needed.</span></span>
    
    ![Применить миграции веб-страницы](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="8831f-148">Кроме того можно проверить с помощью ASP.NET Core Identity вместе с приложением без постоянных баз данных с помощью базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="8831f-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="8831f-149">Чтобы использовать базу данных в памяти, добавьте ``Microsoft.EntityFrameworkCore.InMemory`` пакета приложения и изменить вызов приложения ``AddDbContext`` в ``ConfigureServices`` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8831f-149">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="8831f-150">Когда пользователь щелкает **зарегистрировать** ссылку, ``Register`` при вызове действия на ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="8831f-150">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="8831f-151">``Register`` Действие создает пользователя путем вызова `CreateAsync` на `_userManager` объекта (для ``AccountController`` путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="8831f-151">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="8831f-152">Если пользователь успешно создан, пользователь вошел вызове ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="8831f-152">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="8831f-153">**Примечание:** разделе [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) для меры по предотвращению немедленно входа при регистрации.</span><span class="sxs-lookup"><span data-stu-id="8831f-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="8831f-154">Войти.</span><span class="sxs-lookup"><span data-stu-id="8831f-154">Log in.</span></span>
 
    <span data-ttu-id="8831f-155">Пользователи могут войти, щелкнув **входа** ссылок в верхней части сайта, или может быть переход на страницу входа, если они пытаются получить доступ к части сайта, требующей авторизации.</span><span class="sxs-lookup"><span data-stu-id="8831f-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="8831f-156">Когда пользователь отправляет форму на странице входа ``AccountController`` ``Login`` вызова действия.</span><span class="sxs-lookup"><span data-stu-id="8831f-156">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="8831f-157">``Login`` Вызывает действие ``PasswordSignInAsync`` на ``_signInManager`` объекта (для ``AccountController`` путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="8831f-157">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="8831f-158">Базовый ``Controller`` предоставляемых классами ``User`` свойство, которое можно открыть из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="8831f-158">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="8831f-159">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="8831f-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="8831f-160">Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="8831f-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="8831f-161">Выйдите из системы.</span><span class="sxs-lookup"><span data-stu-id="8831f-161">Log out.</span></span>
 
    <span data-ttu-id="8831f-162">Щелкнув **Выход** связывать вызовы `LogOut` действие.</span><span class="sxs-lookup"><span data-stu-id="8831f-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="8831f-163">Предыдущий код выше вызовы `_signInManager.SignOutAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="8831f-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="8831f-164">`SignOutAsync` Метод очищает утверждения пользователей, хранящихся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="8831f-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="8831f-165">Конфигурация.</span><span class="sxs-lookup"><span data-stu-id="8831f-165">Configuration.</span></span>

    <span data-ttu-id="8831f-166">Удостоверение имеет некоторые виды поведения по умолчанию, которые можно переопределить в классе при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="8831f-166">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="8831f-167">Необходимо настроить ``IdentityOptions`` при использовании поведения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8831f-167">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8831f-168">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8831f-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8831f-169">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8831f-169">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="8831f-170">Дополнительные сведения о настройке удостоверений см. в разделе [Настройка удостоверений](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="8831f-170">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="8831f-171">Можно также настроить тип данных первичный ключ. в разделе [тип данных первичные ключи Настройка удостоверения](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="8831f-171">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="8831f-172">Просмотр базы данных.</span><span class="sxs-lookup"><span data-stu-id="8831f-172">View the database.</span></span>

    <span data-ttu-id="8831f-173">Если приложение использует базу данных SQL Server (по умолчанию в Windows и для пользователей Visual Studio), можно просматривать базы данных с приложением, создаваемым.</span><span class="sxs-lookup"><span data-stu-id="8831f-173">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="8831f-174">Можно использовать **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="8831f-174">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="8831f-175">Кроме того, в Visual Studio, выберите **представление** > **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="8831f-175">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="8831f-176">Подключиться к **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="8831f-176">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="8831f-177">База данных с именем, соответствующим  **aspnet - <*имя проекта*>-<*Дата строка*> ** отображается.</span><span class="sxs-lookup"><span data-stu-id="8831f-177">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Контекстные меню AspNetUsers таблицу базы данных](identity/_static/04-db.png)
    
    <span data-ttu-id="8831f-179">Разверните базу данных и его **таблиц**, щелкните правой кнопкой мыши **dbo. AspNetUsers** таблицы и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="8831f-179">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="8831f-180">Проверки удостоверения работы</span><span class="sxs-lookup"><span data-stu-id="8831f-180">Verify Identity works</span></span>

    <span data-ttu-id="8831f-181">Значение по умолчанию *веб-приложения ASP.NET Core* шаблон проекта предоставляет пользователям доступ к любое действие в приложении без повторного входа.</span><span class="sxs-lookup"><span data-stu-id="8831f-181">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="8831f-182">Чтобы проверить работоспособность ASP.NET Identity, добавьте`[Authorize]` атрибут `About` действие `Home` контроллера.</span><span class="sxs-lookup"><span data-stu-id="8831f-182">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8831f-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8831f-183">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="8831f-184">Запустите проект с помощью **Ctrl** + **F5** и перейдите к **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="8831f-184">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="8831f-185">Только прошедшие проверку подлинности пользователи могут получить доступ к **о** страницы, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.</span><span class="sxs-lookup"><span data-stu-id="8831f-185">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8831f-186">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="8831f-186">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="8831f-187">Откройте окно командной строки и перейдите в корневой каталог проекта в каталог, содержащий `.csproj` файла.</span><span class="sxs-lookup"><span data-stu-id="8831f-187">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="8831f-188">Запустите `dotnet run` команду, чтобы запустить приложение:</span><span class="sxs-lookup"><span data-stu-id="8831f-188">Run the `dotnet run` command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="8831f-189">Обзор URL-адрес, указанный в выходные данные `dotnet run` команды.</span><span class="sxs-lookup"><span data-stu-id="8831f-189">Browse the URL specified in the output from the `dotnet run` command.</span></span> <span data-ttu-id="8831f-190">URL-адрес должен указывать на `localhost` с созданный номер порта.</span><span class="sxs-lookup"><span data-stu-id="8831f-190">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="8831f-191">Перейдите к **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="8831f-191">Navigate to the **About** page.</span></span> <span data-ttu-id="8831f-192">Только прошедшие проверку подлинности пользователи могут получить доступ к **о** страницы, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.</span><span class="sxs-lookup"><span data-stu-id="8831f-192">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="8831f-193">Компоненты идентификаторов</span><span class="sxs-lookup"><span data-stu-id="8831f-193">Identity Components</span></span>

<span data-ttu-id="8831f-194">Основная ссылка сборки для системы удостоверений `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="8831f-194">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="8831f-195">Данный пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включается по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="8831f-195">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="8831f-196">Эти зависимости необходимы для использования в приложениях ASP.NET Core системы удостоверений.</span><span class="sxs-lookup"><span data-stu-id="8831f-196">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="8831f-197">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Типы, необходимые для использования с Entity Framework Core удостоверений.</span><span class="sxs-lookup"><span data-stu-id="8831f-197">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="8831f-198">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core — технология доступа к данных, рекомендуемые корпорации Майкрософт для реляционных баз данных, как SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8831f-198">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="8831f-199">Для тестирования, можно использовать `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="8831f-199">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="8831f-200">`Microsoft.AspNetCore.Authentication.Cookies`-Промежуточное по, которая позволяет приложению использовать проверку подлинности на основе файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="8831f-200">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="8831f-201">Переход к удостоверению ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8831f-201">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="8831f-202">Дополнительные сведения и инструкции по миграции существующего личности магазине см. в разделе [миграции проверку подлинности и удостоверение](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="8831f-202">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8831f-203">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="8831f-203">Next Steps</span></span>

* [<span data-ttu-id="8831f-204">Миграция на другой метод проверки подлинности и другие удостоверения</span><span class="sxs-lookup"><span data-stu-id="8831f-204">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="8831f-205">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="8831f-205">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="8831f-206">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="8831f-206">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="8831f-207">Включение проверки подлинности с помощью Facebook, Google и других внешних поставщиков</span><span class="sxs-lookup"><span data-stu-id="8831f-207">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
