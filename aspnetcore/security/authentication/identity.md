---
title: Введение в удостоверение на ASP.NET Core
author: rick-anderson
description: Используйте удостоверение с приложением ASP.NET Core. Узнайте, как устанавливать требования к паролю (Рекуиредигит, Рекуиредленгс, Рекуиредуникуечарс и др.).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 325a61e6038e79b9a0db72c8360a5cbff2c8ddae
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011209"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="6c5ed-104">Введение в удостоверение на ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c5ed-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="6c5ed-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="6c5ed-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6c5ed-106">ASP.NET Core удостоверение — это система членства, которая добавляет функции входа в ASP.NET Core приложения.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="6c5ed-107">Пользователи могут создать учетную запись с данными для входа, хранящимися в удостоверении, или использовать внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="6c5ed-108">Поддерживаемые внешние поставщики входа включают [Facebook, Google, учетную запись Майкрософт и Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="6c5ed-109">Идентификацию можно настроить с помощью SQL Server базы данных для хранения имен пользователей, паролей и данных профилей.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="6c5ed-110">Кроме того, можно использовать еще одно постоянное хранилище, например хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="6c5ed-111">[Просмотр или скачивание примера кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([как загрузить)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-111">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="6c5ed-112">В этом разделе вы узнаете, как использовать удостоверение для регистрации, входа и выхода пользователя.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="6c5ed-113">Более подробные инструкции по созданию приложений, использующих удостоверение, см. в разделе дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="6c5ed-114">Адддефаултидентити и Аддидентити</span><span class="sxs-lookup"><span data-stu-id="6c5ed-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="6c5ed-115">[Адддефаултидентити](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) был введен в ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="6c5ed-116">Вызов `AddDefaultIdentity` аналогичен вызову следующего:</span><span class="sxs-lookup"><span data-stu-id="6c5ed-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="6c5ed-117">аддидентити</span><span class="sxs-lookup"><span data-stu-id="6c5ed-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="6c5ed-118">адддефаултуи</span><span class="sxs-lookup"><span data-stu-id="6c5ed-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="6c5ed-119">адддефаулттокенпровидерс</span><span class="sxs-lookup"><span data-stu-id="6c5ed-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="6c5ed-120">Дополнительные сведения см. в разделе [адддефаултидентити Source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="6c5ed-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="6c5ed-121">Создание веб-приложения с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="6c5ed-121">Create a Web app with authentication</span></span>

<span data-ttu-id="6c5ed-122">Создание ASP.NET Core проекта веб-приложения с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c5ed-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c5ed-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6c5ed-124">Выберите **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="6c5ed-125">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="6c5ed-126">Присвойте проекту имя **APP1** , которое будет иметь то же пространство имен, что и загружаемый проект.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="6c5ed-127">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-127">Click **OK**.</span></span>
* <span data-ttu-id="6c5ed-128">Выберите ASP.NET Core **веб-приложение**, а затем щелкните **изменить проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="6c5ed-129">Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6c5ed-130">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6c5ed-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="6c5ed-131">Созданный проект предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) в виде [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="6c5ed-132">Библиотека классов Razor Identity предоставляет конечные точки с `Identity` областью.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="6c5ed-133">Например:</span><span class="sxs-lookup"><span data-stu-id="6c5ed-133">For example:</span></span>

* <span data-ttu-id="6c5ed-134">/идентити/аккаунт/логин</span><span class="sxs-lookup"><span data-stu-id="6c5ed-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="6c5ed-135">/идентити/аккаунт/логаут</span><span class="sxs-lookup"><span data-stu-id="6c5ed-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="6c5ed-136">/идентити/аккаунт/манаже</span><span class="sxs-lookup"><span data-stu-id="6c5ed-136">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="6c5ed-137">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="6c5ed-137">Apply migrations</span></span>

<span data-ttu-id="6c5ed-138">Примените миграцию, чтобы инициализировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-138">Apply the migrations to initialise the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c5ed-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c5ed-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6c5ed-140">Выполните следующую команду в консоли диспетчера пакетов (PMC):</span><span class="sxs-lookup"><span data-stu-id="6c5ed-140">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6c5ed-141">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6c5ed-141">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="6c5ed-142">Проверка регистра и вход в систему</span><span class="sxs-lookup"><span data-stu-id="6c5ed-142">Test Register and Login</span></span>

<span data-ttu-id="6c5ed-143">Запустите приложение и зарегистрируйте пользователя.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-143">Run the app and register a user.</span></span> <span data-ttu-id="6c5ed-144">В зависимости от размера экрана может потребоваться выбрать переключатель навигации, чтобы просмотреть ссылки **Регистрация** и **Вход** .</span><span class="sxs-lookup"><span data-stu-id="6c5ed-144">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="6c5ed-145">Настройка служб удостоверений</span><span class="sxs-lookup"><span data-stu-id="6c5ed-145">Configure Identity services</span></span>

<span data-ttu-id="6c5ed-146">Службы добавляются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-146">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="6c5ed-147">По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-147">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="6c5ed-148">Приведенный выше код настраивает удостоверение с использованием значений параметров по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-148">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="6c5ed-149">Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-149">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="6c5ed-150">Удостоверение включается путем вызова [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-150">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="6c5ed-151">`UseAuthentication`добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-151">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="6c5ed-152">Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-152">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="6c5ed-153">Удостоверение включается для приложения путем вызова `UseAuthentication` `Configure` метода в методе.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-153">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="6c5ed-154">`UseAuthentication`добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-154">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="6c5ed-155">Эти службы становятся доступными для приложения посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-155">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="6c5ed-156">Удостоверение включается для приложения путем вызова `UseIdentity` `Configure` метода в методе.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-156">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="6c5ed-157">`UseIdentity`добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности на основе файлов cookie в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-157">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="6c5ed-158">Дополнительные сведения см. в разделе [класс идентитйоптионс](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) и [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-158">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="6c5ed-159">Регистрация, вход и выход из шаблона формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="6c5ed-159">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="6c5ed-160">Соблюдайте [идентификатор шаблона в проекте Razor с](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) инструкциями по авторизации для создания кода, приведенного в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-160">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6c5ed-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c5ed-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6c5ed-162">Добавьте файлы Register, login и LogOut.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-162">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6c5ed-163">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6c5ed-163">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6c5ed-164">Если вы создали проект с именем " **APP1**", выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-164">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="6c5ed-165">В противном случае используйте правильное пространство имен `ApplicationDbContext`для:</span><span class="sxs-lookup"><span data-stu-id="6c5ed-165">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="6c5ed-166">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-166">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="6c5ed-167">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки, как показано в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-167">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="6c5ed-168">Проверка регистра</span><span class="sxs-lookup"><span data-stu-id="6c5ed-168">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="6c5ed-169">Когда пользователь щелкает ссылку **Register** , `RegisterModel.OnPostAsync` вызывается действие.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-169">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="6c5ed-170">Пользователь создается с помощью [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) для `_userManager` объекта.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-170">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="6c5ed-171">`_userManager`предоставляется путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="6c5ed-171">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="6c5ed-172">Когда пользователь щелкает ссылку **Register** , `Register` действие вызывается в `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-172">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="6c5ed-173">Действие `Register` создает пользователя путем вызова метода `CreateAsync` объекта`_userManager` (обеспечивается в `AccountController` путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="6c5ed-173">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="6c5ed-174">Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="6c5ed-175">**Примечание.** Инструкции по предотвращению немедленного входа при регистрации см. в статье [Подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) .</span><span class="sxs-lookup"><span data-stu-id="6c5ed-175">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="6c5ed-176">Войти</span><span class="sxs-lookup"><span data-stu-id="6c5ed-176">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c5ed-177">Форма входа отображается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="6c5ed-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="6c5ed-178">Выбрана ссылка для **входа** .</span><span class="sxs-lookup"><span data-stu-id="6c5ed-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="6c5ed-179">Пользователь пытается получить доступ к странице с ограниченным доступом, которая не авторизована для доступа, **или** если система не прошла проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="6c5ed-180">При отправке `OnPostAsync` формы на странице входа вызывается действие.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="6c5ed-181">`PasswordSignInAsync`вызывается для `_signInManager` объекта (предоставляется путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="6c5ed-182">Базовый `Controller` класс`User` предоставляет свойство, доступ к которому можно получить из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-182">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="6c5ed-183">Например, можно перечислять `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="6c5ed-184">Дополнительные сведения см. в разделе <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-184">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6c5ed-185">Форма входа отображается, когда пользователь выбирает ссылку для **входа** или перенаправляется при обращении к странице, требующей проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-185">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="6c5ed-186">Когда пользователь отправляет форму на страницу входа, `AccountController` `Login` вызывается действие.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-186">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="6c5ed-187">Действие вызывает `PasswordSignInAsync` объект(`AccountController` предоставляется путем внедрения зависимостей). `_signInManager` `Login`</span><span class="sxs-lookup"><span data-stu-id="6c5ed-187">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="6c5ed-188">Базовый класс (`Controller` или `PageModel`) предоставляет `User` свойство.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-188">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="6c5ed-189">Например, можно `User.Claims` перечислить, чтобы принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-189">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="6c5ed-190">Выход</span><span class="sxs-lookup"><span data-stu-id="6c5ed-190">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c5ed-191">Ссылка **выхода** вызывает `LogoutModel.OnPost` действие.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-191">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="6c5ed-192">[Сигнаутасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) очищает утверждения пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="6c5ed-193">POST указывается в параметре *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6c5ed-193">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="6c5ed-194">Щелкнув ссылку **выход** , вы вызываете `LogOut` действие.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-194">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="6c5ed-195">Приведенный выше код вызывает `_signInManager.SignOutAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-195">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="6c5ed-196">`SignOutAsync` Метод очищает утверждения пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-196">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="6c5ed-197">Проверить удостоверение</span><span class="sxs-lookup"><span data-stu-id="6c5ed-197">Test Identity</span></span>

<span data-ttu-id="6c5ed-198">Шаблоны веб-проектов по умолчанию разрешают анонимный доступ к домашним страницам.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-198">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="6c5ed-199">Чтобы проверить удостоверение, [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) добавьте на страницу конфиденциальность.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-199">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="6c5ed-200">Если вы вошли в систему, выйдите из нее. Запустите приложение и щелкните ссылку **Конфиденциальность** .</span><span class="sxs-lookup"><span data-stu-id="6c5ed-200">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="6c5ed-201">Вы будете перенаправлены на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-201">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="6c5ed-202">Просмотр удостоверений</span><span class="sxs-lookup"><span data-stu-id="6c5ed-202">Explore Identity</span></span>

<span data-ttu-id="6c5ed-203">Более подробное изучение удостоверений:</span><span class="sxs-lookup"><span data-stu-id="6c5ed-203">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="6c5ed-204">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="6c5ed-204">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="6c5ed-205">Изучите источник каждой страницы и пошаговое выполнение отладчика.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-205">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="6c5ed-206">Компоненты идентификации</span><span class="sxs-lookup"><span data-stu-id="6c5ed-206">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6c5ed-207">Все пакеты NuGet, зависящие от удостоверения, включены в пакет [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-207">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="6c5ed-208">Основным пакетом для удостоверения является [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-208">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="6c5ed-209">Этот пакет содержит основной набор интерфейсов для ASP.NET Core удостоверения и включается `Microsoft.AspNetCore.Identity.EntityFrameworkCore`в.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-209">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="6c5ed-210">Миграция на ASP.NET Core удостоверение</span><span class="sxs-lookup"><span data-stu-id="6c5ed-210">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="6c5ed-211">Дополнительные сведения и рекомендации по переносу существующего хранилища удостоверений см. в статье [Миграция проверки подлинности и удостоверений](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="6c5ed-211">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="6c5ed-212">Установка силы пароля</span><span class="sxs-lookup"><span data-stu-id="6c5ed-212">Setting password strength</span></span>

<span data-ttu-id="6c5ed-213">См. раздел [Конфигурация](#pw) для примера, который устанавливает минимальные требования к паролю.</span><span class="sxs-lookup"><span data-stu-id="6c5ed-213">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c5ed-214">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="6c5ed-214">Next Steps</span></span>

* [<span data-ttu-id="6c5ed-215">Настройка Identity</span><span class="sxs-lookup"><span data-stu-id="6c5ed-215">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
