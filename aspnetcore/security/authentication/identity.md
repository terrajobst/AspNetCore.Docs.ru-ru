---
title: Общие сведения об Identity в ASP.NET Core
author: rick-anderson
description: Использование удостоверения с приложения ASP.NET Core. Включает параметр паролей (RequireDigit, RequiredLength, RequiredUniqueChars и многое другое).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: c231a7619a4433ce004342ce68564e4c3892e702
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829306"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="a9299-104">Общие сведения об Identity в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9299-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="a9299-105">По [Пранавом Растоги](https://github.com/rustd), [Рик Андерсон](https://twitter.com/RickAndMSFT), [том Дайкстра](https://github.com/tdykstra), Джон Гэллоуэй [Erik Reitan](https://github.com/Erikre), и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a9299-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a9299-106">Удостоверение ASP.NET Core — это система членства, который позволяет добавить функциональные возможности входа в приложение.</span><span class="sxs-lookup"><span data-stu-id="a9299-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="a9299-107">Пользователи могут создавать учетную запись и имя входа с именем пользователя и пароль или их можно использовать поставщик внешней учетной записи, например Facebook, Google, учетной записи Майкрософт, Twitter или другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="a9299-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="a9299-108">Вы можете настроить удостоверение ASP.NET Core, чтобы использовать базу данных SQL Server для хранения имен пользователей, пароли и данные профиля.</span><span class="sxs-lookup"><span data-stu-id="a9299-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="a9299-109">Кроме того можно использовать собственные постоянное хранилище, например, хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="a9299-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="a9299-110">Этот документ содержит инструкции по Visual Studio и с помощью интерфейса командной строки.</span><span class="sxs-lookup"><span data-stu-id="a9299-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="a9299-111">Просмотреть или скачать образец кода.</span><span class="sxs-lookup"><span data-stu-id="a9299-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="a9299-112">(Как для загрузки)</span><span class="sxs-lookup"><span data-stu-id="a9299-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="a9299-113">Общие сведения об удостоверениях</span><span class="sxs-lookup"><span data-stu-id="a9299-113">Overview of Identity</span></span>

<span data-ttu-id="a9299-114">В этом разделе будет использование удостоверения ASP.NET Core для добавления функций, регистрация, вход и выход пользователя.</span><span class="sxs-lookup"><span data-stu-id="a9299-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="a9299-115">Более подробные инструкции по созданию приложений с помощью удостоверения ASP.NET Core см. в разделе "Дальнейшие действия" в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="a9299-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="a9299-116">Создайте проект веб-приложение ASP.NET Core с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="a9299-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9299-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9299-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="a9299-118">В Visual Studio выберите **файл** > **New** > **проекта**.</span><span class="sxs-lookup"><span data-stu-id="a9299-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="a9299-119">Выберите **веб-приложение ASP.NET Core** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="a9299-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![Диалоговое окно создания нового проекта](identity/_static/01-new-project.png)

   <span data-ttu-id="a9299-121">Выберите ASP.NET Core **веб-приложение (Model-View-Controller)** для ASP.NET Core 2.x, а затем выберите **изменить способ проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="a9299-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![Диалоговое окно создания нового проекта](identity/_static/02-new-project.png)

   <span data-ttu-id="a9299-123">Откроется диалоговое окно, предложить варианты проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9299-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="a9299-124">Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК** чтобы вернуться в предыдущем диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="a9299-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![Диалоговое окно создания нового проекта](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="a9299-126">Выбрав **учетные записи отдельных пользователей** направляет Visual Studio для создания модели, модели ViewModel, представления, контроллеры и другие активы, необходимые для проверки подлинности как часть шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="a9299-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a9299-127">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a9299-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="a9299-128">Если с помощью интерфейса командной строки .NET Core, создайте новый проект с помощью `dotnet new mvc --auth Individual`.</span><span class="sxs-lookup"><span data-stu-id="a9299-128">If using the .NET Core CLI, create the new project using `dotnet new mvc --auth Individual`.</span></span> <span data-ttu-id="a9299-129">Эта команда создает новый проект с тем же кодом шаблона удостоверений, которые создает Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9299-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="a9299-130">Созданный проект содержит `Microsoft.AspNetCore.Identity.EntityFrameworkCore` пакет, который сохраняет данные удостоверений и схемы для SQL Server с помощью [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="a9299-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="a9299-131">Настройка службы удостоверений и добавьте по промежуточного слоя в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="a9299-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="a9299-132">Службы удостоверений добавляются к приложению в `ConfigureServices` метод в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="a9299-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a9299-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a9299-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="a9299-134">Эти службы доступны приложению через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a9299-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a9299-135">Удостоверение включено для приложения, вызвав `UseAuthentication` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="a9299-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="a9299-136">`UseAuthentication` Добавляет проверку подлинности [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="a9299-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a9299-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a9299-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="a9299-138">Эти службы доступны приложению через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a9299-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a9299-139">Удостоверение включено для приложения, вызвав `UseIdentity` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="a9299-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="a9299-140">`UseIdentity` Добавляет проверку подлинности на основе файлов cookie [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="a9299-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   <span data-ttu-id="a9299-141">Дополнительные сведения о время загрузки приложения, см. в разделе [запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a9299-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="a9299-142">Создание пользователя.</span><span class="sxs-lookup"><span data-stu-id="a9299-142">Create a user.</span></span>

   <span data-ttu-id="a9299-143">Запустите приложение и щелкните ссылку **Register** (Регистрация).</span><span class="sxs-lookup"><span data-stu-id="a9299-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="a9299-144">При первом запуске вам может потребоваться выполнить миграцию.</span><span class="sxs-lookup"><span data-stu-id="a9299-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="a9299-145">Приложение предложит **Apply Migrations** (Выполнить миграцию):</span><span class="sxs-lookup"><span data-stu-id="a9299-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="a9299-146">Обновите страницу, при необходимости.</span><span class="sxs-lookup"><span data-stu-id="a9299-146">Refresh the page if needed.</span></span>

   ![Веб-страница применения миграции](identity/_static/apply-migrations.png)

   <span data-ttu-id="a9299-148">Кроме того, можно проверить работу удостоверения ASP.NET Core с приложением без постоянной базы данных, а с использованием базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="a9299-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="a9299-149">Для этого добавьте в приложение пакет `Microsoft.EntityFrameworkCore.InMemory`и измените вызов метода `AddDbContext` в `ConfigureServices` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a9299-149">To use an in-memory database, add the `Microsoft.EntityFrameworkCore.InMemory` package to your app and modify your app's call to `AddDbContext` in `ConfigureServices` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="a9299-150">Когда пользователь нажимает на ссылку **Register** (Регистрация), вызывается метод действия `Register` контроллера `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="a9299-150">When the user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="a9299-151">Действие `Register` создает пользователя путем вызова метода `CreateAsync` объекта`_userManager` (обеспечивается в `AccountController` путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="a9299-151">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="a9299-152">Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a9299-152">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="a9299-153">**Примечание.** Раздел [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) описывает, как предотвратить автоматический вход пользователя при регистрации.</span><span class="sxs-lookup"><span data-stu-id="a9299-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="a9299-154">Вход.</span><span class="sxs-lookup"><span data-stu-id="a9299-154">Log in.</span></span>

   <span data-ttu-id="a9299-155">Пользователи могут входить, щелкнув **вход** ссылку в верхней части сайта, или может быть переход на страницу входа, если они пытаются получить доступ к части сайта, требующему проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9299-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="a9299-156">Когда пользователь отправляет форму на странице входа `AccountController` `Login` вызова действия.</span><span class="sxs-lookup"><span data-stu-id="a9299-156">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

   <span data-ttu-id="a9299-157">`Login` Вызовов действия `PasswordSignInAsync` на `_signInManager` объекта (для `AccountController` с помощью внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="a9299-157">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="a9299-158">Базовый `Controller` предоставляет `User` свойство, которое можно получить из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="a9299-158">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="a9299-159">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="a9299-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="a9299-160">Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="a9299-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="a9299-161">Выход.</span><span class="sxs-lookup"><span data-stu-id="a9299-161">Log out.</span></span>

   <span data-ttu-id="a9299-162">Щелкнув **Выход** связать вызовы `LogOut` действие.</span><span class="sxs-lookup"><span data-stu-id="a9299-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="a9299-163">Предыдущий код выше вызовы `_signInManager.SignOutAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="a9299-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="a9299-164">`SignOutAsync` Метод очищает утверждений пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="a9299-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="a9299-165">Конфигурация.</span><span class="sxs-lookup"><span data-stu-id="a9299-165">Configuration.</span></span>

   <span data-ttu-id="a9299-166">Удостоверение имеет некоторые поведения по умолчанию, которые могут быть переопределены в классе запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="a9299-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="a9299-167">`IdentityOptions` не нужно настроить при использовании поведения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a9299-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="a9299-168">В следующем коде задается несколько вариантов стойкость пароля:</span><span class="sxs-lookup"><span data-stu-id="a9299-168">The following code sets several password strength options:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a9299-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a9299-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a9299-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a9299-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   <span data-ttu-id="a9299-171">Дополнительные сведения о том, как настроить удостоверение, см. в разделе [настроить удостоверение](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="a9299-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="a9299-172">Вы также можете настроить тип данных первичного ключа, см. в разделе [тип данных Настройка идентификаторов первичных ключей](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="a9299-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="a9299-173">Просмотр базы данных.</span><span class="sxs-lookup"><span data-stu-id="a9299-173">View the database.</span></span>

   <span data-ttu-id="a9299-174">Если приложение использует базу данных SQL Server (по умолчанию на Windows и для пользователей Visual Studio), можно просмотреть приложение, созданное для базы данных.</span><span class="sxs-lookup"><span data-stu-id="a9299-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="a9299-175">Можно использовать **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="a9299-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="a9299-176">Кроме того, в Visual Studio последовательно выберите **представление** > **обозреватель объектов SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="a9299-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="a9299-177">Подключение к **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="a9299-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="a9299-178">Базы данных с именем, соответствующим `aspnet-<name of your project>-<guid>` отображается.</span><span class="sxs-lookup"><span data-stu-id="a9299-178">The database with a name matching `aspnet-<name of your project>-<guid>` is displayed.</span></span>

   ![Контекстные меню в таблице AspNetUsers базы данных](identity/_static/04-db.png)

   <span data-ttu-id="a9299-180">Разверните базу данных и его **таблиц**, щелкните правой кнопкой мыши **dbo. AspNetUsers** таблицы и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="a9299-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="a9299-181">Проверка работы удостоверений</span><span class="sxs-lookup"><span data-stu-id="a9299-181">Verify Identity works</span></span>

    <span data-ttu-id="a9299-182">Значение по умолчанию *веб-приложение ASP.NET Core* шаблон проекта предоставляет пользователям доступ к любое действие в приложении без необходимости с именем входа.</span><span class="sxs-lookup"><span data-stu-id="a9299-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="a9299-183">Чтобы убедиться, что ASP.NET Identity работает, добавьте`[Authorize]` атрибут `About` действие `Home` контроллера.</span><span class="sxs-lookup"><span data-stu-id="a9299-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9299-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9299-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="a9299-185">Запустите проект, используя **Ctrl** + **F5** и перейдите к **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="a9299-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="a9299-186">Только прошедшие проверку подлинности пользователи могут получать доступ к **о** странице, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.</span><span class="sxs-lookup"><span data-stu-id="a9299-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a9299-187">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a9299-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="a9299-188">Откройте окно командной строки и перейдите в корневой каталог проекта каталог, содержащий `.csproj` файл.</span><span class="sxs-lookup"><span data-stu-id="a9299-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="a9299-189">Запустите [запуска dotnet](/dotnet/core/tools/dotnet-run) команду, чтобы запустить приложение:</span><span class="sxs-lookup"><span data-stu-id="a9299-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```csharp
    dotnet run 
    ```

    <span data-ttu-id="a9299-190">Обзор URL-адрес, указанный в выходных данных из [запуска dotnet](/dotnet/core/tools/dotnet-run) команды.</span><span class="sxs-lookup"><span data-stu-id="a9299-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="a9299-191">URL-адрес должен указывать `localhost` с созданный номер порта.</span><span class="sxs-lookup"><span data-stu-id="a9299-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="a9299-192">Перейдите к **о** страницы.</span><span class="sxs-lookup"><span data-stu-id="a9299-192">Navigate to the **About** page.</span></span> <span data-ttu-id="a9299-193">Только прошедшие проверку подлинности пользователи могут получать доступ к **о** странице, поэтому ASP.NET вы будете перенаправлены на страницу входа для входа или регистрации.</span><span class="sxs-lookup"><span data-stu-id="a9299-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="a9299-194">Компонентами системы идентификации</span><span class="sxs-lookup"><span data-stu-id="a9299-194">Identity Components</span></span>

<span data-ttu-id="a9299-195">Первичное эталонное сборки для системы идентификации `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="a9299-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="a9299-196">Этот пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включена в состав по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="a9299-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="a9299-197">Эти зависимости необходимы для использования системы идентификации в приложениях ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a9299-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="a9299-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` — Содержит необходимые типы использовать удостоверение с Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="a9299-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="a9299-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core — технология доступа к данных, рекомендуемые корпорации Майкрософт для реляционных баз данных, таких как SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a9299-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="a9299-200">Для тестирования, можно использовать `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="a9299-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="a9299-201">`Microsoft.AspNetCore.Authentication.Cookies` — Промежуточный слой, который позволяет приложению использовать проверку подлинности на основе файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="a9299-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="a9299-202">Переход на ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="a9299-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="a9299-203">Дополнительные сведения и инструкции по миграции существующее удостоверение в магазине см. в разделе [перенести проверки подлинности и идентификации](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="a9299-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="a9299-204">Стойкость пароля параметр</span><span class="sxs-lookup"><span data-stu-id="a9299-204">Setting password strength</span></span>

<span data-ttu-id="a9299-205">См. в разделе [конфигурации](#pw) пример, задает минимальное паролей.</span><span class="sxs-lookup"><span data-stu-id="a9299-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9299-206">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a9299-206">Next Steps</span></span>

* [<span data-ttu-id="a9299-207">Миграция проверки подлинности и удостоверения</span><span class="sxs-lookup"><span data-stu-id="a9299-207">Migrate Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="a9299-208">Подтверждение учетной записи и восстановление пароля</span><span class="sxs-lookup"><span data-stu-id="a9299-208">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="a9299-209">Двухфакторная проверка подлинности с помощью SMS</span><span class="sxs-lookup"><span data-stu-id="a9299-209">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="a9299-210">Facebook, Google и внешних поставщиков проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="a9299-210">Facebook, Google, and external provider authentication</span></span>](xref:security/authentication/social/index)
