---
title: Общие сведения об учетных данных ASP.NET Core
author: rick-anderson
description: Используйте удостоверение с приложением ASP.NET Core. Включает параметр паролей (RequireDigit, RequiredLength, RequiredUniqueChars и многое другое).
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: b3bfae665403162db1fb012fac227275b1dfd6c9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="844b0-104">Общие сведения об учетных данных ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="844b0-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="844b0-105">По [Pranav Rastogi](https://github.com/rustd), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Джон Гэллоуэй [Reitan Эрик](https://github.com/Erikre), и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="844b0-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="844b0-106">ASP.NET Core Identity позволяет реализовать проверку подлинности в приложении.</span><span class="sxs-lookup"><span data-stu-id="844b0-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="844b0-107">Пользователи могут создавать учетную запись с логином и паролем либо использовать внешнего поставщика, например Facebook, Google, учетную запись Майкрософт, Twitter и другие.</span><span class="sxs-lookup"><span data-stu-id="844b0-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="844b0-108">Вы можете настроить в ASP.NET Identity Core использование базы данных SQL Server для хранения пользовательских имен, паролей и данных профиля.</span><span class="sxs-lookup"><span data-stu-id="844b0-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="844b0-109">Кроме того можно использовать собственные постоянное хранилище, например, табличное хранилище Azure.</span><span class="sxs-lookup"><span data-stu-id="844b0-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="844b0-110">Этот документ содержит инструкции по использованию Visual Studio и CLI.</span><span class="sxs-lookup"><span data-stu-id="844b0-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="844b0-111">Просмотреть или загрузить образец кода.</span><span class="sxs-lookup"><span data-stu-id="844b0-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="844b0-112">(Сведения о загрузке)</span><span class="sxs-lookup"><span data-stu-id="844b0-112">(How to download)</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="844b0-113">Общие сведения об идентификации</span><span class="sxs-lookup"><span data-stu-id="844b0-113">Overview of Identity</span></span>

<span data-ttu-id="844b0-114">В этом разделе вы узнаете, как использовать ASP.NET Core Identity для реализации регистрации, входа и выхода пользователя.</span><span class="sxs-lookup"><span data-stu-id="844b0-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="844b0-115">Более подробные инструкции по созданию приложений с помощью ASP.NET Core Identity см. в разделе "Дальнейшие действия" в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="844b0-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="844b0-116">Создание проекта "Веб-приложение ASP.NET Core" с использованием проверки подлинности "Индивидуальные учетные записи пользователей".</span><span class="sxs-lookup"><span data-stu-id="844b0-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="844b0-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="844b0-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="844b0-118">В Visual Studio, выберите **файл** > **New** > **проекта**.</span><span class="sxs-lookup"><span data-stu-id="844b0-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="844b0-119">Выберите **веб-приложения ASP.NET Core** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="844b0-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![Диалоговое окно создания нового проекта](identity/_static/01-new-project.png)

   <span data-ttu-id="844b0-121">Выберите ASP.NET Core **веб-приложения (Model-View-Controller)** для ASP.NET Core 2.x, а затем выберите **изменить аутентификацию**.</span><span class="sxs-lookup"><span data-stu-id="844b0-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![Диалоговое окно создания нового проекта](identity/_static/02-new-project.png)

   <span data-ttu-id="844b0-123">Откроется диалоговое окно предложения вариантов проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="844b0-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="844b0-124">Выберите **отдельных учетных записей пользователей** и нажмите кнопку **ОК** для возврата к диалоговому окну предыдущего.</span><span class="sxs-lookup"><span data-stu-id="844b0-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![Диалоговое окно создания нового проекта](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="844b0-126">При выборе **отдельных учетных записей пользователей** направляет Visual Studio для создания моделей, ViewModels, представления, контроллеры и другие активы, необходимые для проверки подлинности как часть шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="844b0-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="844b0-127">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="844b0-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="844b0-128">Если используется .NET Core CLI, выполните команду ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="844b0-128">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="844b0-129">Эта команда создает новый проект с тем же кодом шаблона удостоверений, создаваемых в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="844b0-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="844b0-130">Созданный проект содержит `Microsoft.AspNetCore.Identity.EntityFrameworkCore` пакет, который сохраняет данные удостоверений и схемы SQL Server с помощью [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="844b0-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="844b0-131">Настройка службы удостоверений и добавление ПО промежуточного слоя в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="844b0-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="844b0-132">Службы удостоверений регистрируются в приложении в методе `ConfigureServices` класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="844b0-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="844b0-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="844b0-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="844b0-134">Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="844b0-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="844b0-135">Удостоверения для приложения включаются с помощью вызова `UseAuthentication` в методе `Configure` .</span><span class="sxs-lookup"><span data-stu-id="844b0-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="844b0-136">`UseAuthentication` Добавление проверки подлинности [по промежуточного слоя](xref:fundamentals/middleware/index) к конвейеру запросов.</span><span class="sxs-lookup"><span data-stu-id="844b0-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="844b0-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="844b0-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="844b0-138">Эти службы доступны для приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="844b0-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="844b0-139">Путем вызова для приложения включена удостоверения `UseIdentity` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="844b0-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="844b0-140">`UseIdentity` Добавляет файл cookie проверки подлинности с помощью [по промежуточного слоя](xref:fundamentals/middleware/index) к конвейеру запросов.</span><span class="sxs-lookup"><span data-stu-id="844b0-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   * * *
   <span data-ttu-id="844b0-141">Дополнительные сведения о запуске приложения см. в разделе [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="844b0-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="844b0-142">Создание пользователя.</span><span class="sxs-lookup"><span data-stu-id="844b0-142">Create a user.</span></span>

   <span data-ttu-id="844b0-143">Запустите приложение и щелкните ссылку **Register** (Регистрация).</span><span class="sxs-lookup"><span data-stu-id="844b0-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="844b0-144">При первом запуске вам может потребоваться выполнить миграцию.</span><span class="sxs-lookup"><span data-stu-id="844b0-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="844b0-145">Приложение предложит **Apply Migrations** (Выполнить миграцию):</span><span class="sxs-lookup"><span data-stu-id="844b0-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="844b0-146">При необходимости обновите страницу.</span><span class="sxs-lookup"><span data-stu-id="844b0-146">Refresh the page if needed.</span></span>

   ![Веб-страница применения миграции](identity/_static/apply-migrations.png)

   <span data-ttu-id="844b0-148">Кроме того, можно проверить работу удостоверения ASP.NET Core с приложением без постоянной базы данных, а с использованием базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="844b0-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="844b0-149">Для этого добавьте в приложение пакет ``Microsoft.EntityFrameworkCore.InMemory``и измените вызов метода ``AddDbContext`` в ``ConfigureServices`` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="844b0-149">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="844b0-150">Когда пользователь нажимает на ссылку **Register** (Регистрация), вызывается метод действия ``Register`` контроллера ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="844b0-150">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="844b0-151">Действие ``Register`` создает пользователя путем вызова метода `CreateAsync` объекта`_userManager` (обеспечивается в ``AccountController`` путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="844b0-151">The ``Register`` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="844b0-152">Если пользователь создан, происходит вход пользователя путем вызова метода ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="844b0-152">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

   <span data-ttu-id="844b0-153">**Примечание.** Раздел [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) описывает, как предотвратить автоматический вход пользователя при регистрации.</span><span class="sxs-lookup"><span data-stu-id="844b0-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="844b0-154">Вход.</span><span class="sxs-lookup"><span data-stu-id="844b0-154">Log in.</span></span>

   <span data-ttu-id="844b0-155">Пользователи могут войти, щелкнув **входа** ссылок в верхней части сайта, или может быть переход на страницу входа, если они пытаются получить доступ к части сайта, требующей авторизации.</span><span class="sxs-lookup"><span data-stu-id="844b0-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="844b0-156">Когда пользователь отправляет форму на странице входа ``AccountController`` ``Login`` вызова действия.</span><span class="sxs-lookup"><span data-stu-id="844b0-156">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

   <span data-ttu-id="844b0-157">``Login`` Вызывает действие ``PasswordSignInAsync`` на ``_signInManager`` объекта (для ``AccountController`` путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="844b0-157">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="844b0-158">Базовый ``Controller`` предоставляемых классами ``User`` свойство, которое можно открыть из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="844b0-158">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="844b0-159">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="844b0-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="844b0-160">Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="844b0-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="844b0-161">Выход.</span><span class="sxs-lookup"><span data-stu-id="844b0-161">Log out.</span></span>

   <span data-ttu-id="844b0-162">Щелкнув **Выход** связывать вызовы `LogOut` действие.</span><span class="sxs-lookup"><span data-stu-id="844b0-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="844b0-163">Предыдущий код выше вызовы `_signInManager.SignOutAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="844b0-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="844b0-164">`SignOutAsync` Метод очищает утверждения пользователей, хранящихся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="844b0-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="844b0-165">Конфигурация.</span><span class="sxs-lookup"><span data-stu-id="844b0-165">Configuration.</span></span>

   <span data-ttu-id="844b0-166">Удостоверение имеет некоторые виды поведения по умолчанию, которые могут быть переопределены в классе при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="844b0-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="844b0-167">`IdentityOptions` Нет необходимости быть настроены при использовании поведения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="844b0-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="844b0-168">В следующем коде задается несколько параметров стойкость пароля:</span><span class="sxs-lookup"><span data-stu-id="844b0-168">The following code sets several password strength options:</span></span>

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="844b0-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="844b0-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="844b0-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="844b0-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   * * *
   <span data-ttu-id="844b0-171">Дополнительные сведения о настройке удостоверений см. в разделе [Настройка удостоверений](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="844b0-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="844b0-172">Можно также настроить тип данных первичный ключ. в разделе [тип данных первичные ключи Настройка удостоверения](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="844b0-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="844b0-173">Просмотр базы данных.</span><span class="sxs-lookup"><span data-stu-id="844b0-173">View the database.</span></span>

   <span data-ttu-id="844b0-174">Если приложение использует базу данных SQL Server (по умолчанию в Windows и для пользователей Visual Studio), можно просматривать базы данных с приложением, создаваемым.</span><span class="sxs-lookup"><span data-stu-id="844b0-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="844b0-175">Можно использовать **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="844b0-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="844b0-176">Кроме того, в Visual Studio, выберите **представление** > **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="844b0-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="844b0-177">Подключиться к **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="844b0-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="844b0-178">База данных с именем, соответствующим **aspnet - <*имя проекта*>-<*Дата строка* >**  отображается.</span><span class="sxs-lookup"><span data-stu-id="844b0-178">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

   ![Контекстные меню AspNetUsers таблицу базы данных](identity/_static/04-db.png)

   <span data-ttu-id="844b0-180">Разверните базу данных и его **таблиц**, щелкните правой кнопкой мыши **dbo. AspNetUsers** таблицы и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="844b0-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="844b0-181">Проверки удостоверения работы</span><span class="sxs-lookup"><span data-stu-id="844b0-181">Verify Identity works</span></span>

    <span data-ttu-id="844b0-182">Значение по умолчанию *веб-приложения ASP.NET Core* шаблон проекта предоставляет пользователям доступ к любое действие в приложении без повторного входа.</span><span class="sxs-lookup"><span data-stu-id="844b0-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="844b0-183">Чтобы проверить работоспособность ASP.NET Identity, добавьте`[Authorize]` атрибут `About` действие `Home` контроллера.</span><span class="sxs-lookup"><span data-stu-id="844b0-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="844b0-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="844b0-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="844b0-185">Запустите проект с помощью **Ctrl** + **F5** и перейдите к **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="844b0-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="844b0-186">Только прошедшие проверку подлинности пользователи могут получить доступ к **о** страницы, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.</span><span class="sxs-lookup"><span data-stu-id="844b0-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="844b0-187">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="844b0-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="844b0-188">Откройте окно командной строки и перейдите в корневой каталог проекта в каталог, содержащий `.csproj` файла.</span><span class="sxs-lookup"><span data-stu-id="844b0-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="844b0-189">Запустите [dotnet запуска](/dotnet/core/tools/dotnet-run) команду, чтобы запустить приложение:</span><span class="sxs-lookup"><span data-stu-id="844b0-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="844b0-190">Обзор URL-адрес, указанный в выходные данные [dotnet запуска](/dotnet/core/tools/dotnet-run) команды.</span><span class="sxs-lookup"><span data-stu-id="844b0-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="844b0-191">URL-адрес должен указывать на `localhost` с созданный номер порта.</span><span class="sxs-lookup"><span data-stu-id="844b0-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="844b0-192">Перейдите к **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="844b0-192">Navigate to the **About** page.</span></span> <span data-ttu-id="844b0-193">Только прошедшие проверку подлинности пользователи могут получить доступ к **о** страницы, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.</span><span class="sxs-lookup"><span data-stu-id="844b0-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="844b0-194">Компоненты идентификаторов</span><span class="sxs-lookup"><span data-stu-id="844b0-194">Identity Components</span></span>

<span data-ttu-id="844b0-195">Основная ссылка сборки для системы удостоверений `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="844b0-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="844b0-196">Данный пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включается по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="844b0-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="844b0-197">Эти зависимости необходимы для использования в приложениях ASP.NET Core системы удостоверений.</span><span class="sxs-lookup"><span data-stu-id="844b0-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="844b0-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Типы, необходимые для использования с Entity Framework Core удостоверений.</span><span class="sxs-lookup"><span data-stu-id="844b0-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="844b0-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core — технология доступа к данных, рекомендуемые корпорации Майкрософт для реляционных баз данных, как SQL Server.</span><span class="sxs-lookup"><span data-stu-id="844b0-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="844b0-200">Для тестирования, можно использовать `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="844b0-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="844b0-201">`Microsoft.AspNetCore.Authentication.Cookies` -Промежуточное по, которая позволяет приложению использовать проверку подлинности на основе файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="844b0-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="844b0-202">Переход к удостоверению ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="844b0-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="844b0-203">Дополнительные сведения и инструкции по миграции существующего личности магазине см. в разделе [перенести проверку подлинности и удостоверение](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="844b0-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="844b0-204">Параметр стойкость пароля</span><span class="sxs-lookup"><span data-stu-id="844b0-204">Setting password strength</span></span>

<span data-ttu-id="844b0-205">В разделе [конфигурации](#pw) для примера, устанавливает требования минимальные требования к паролю.</span><span class="sxs-lookup"><span data-stu-id="844b0-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="844b0-206">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="844b0-206">Next Steps</span></span>

* [<span data-ttu-id="844b0-207">Перенос проверку подлинности и удостоверение</span><span class="sxs-lookup"><span data-stu-id="844b0-207">Migrate Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="844b0-208">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="844b0-208">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="844b0-209">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="844b0-209">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="844b0-210">Facebook, Google и внешнего поставщика проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="844b0-210">Facebook, Google, and external provider authentication</span></span>](xref:security/authentication/social/index)
