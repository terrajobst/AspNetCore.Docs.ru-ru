---
title: Общие сведения об Identity в ASP.NET Core
author: rick-anderson
description: Использование удостоверения с приложения ASP.NET Core. Узнайте, как задать требования к паролю (RequireDigit, RequiredLength, RequiredUniqueChars и многое другое).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: d427932bb175c09105534379be4d71760f4e04e5
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860957"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="630ce-104">Общие сведения об Identity в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="630ce-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="630ce-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="630ce-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="630ce-106">Удостоверение ASP.NET Core — это система членства, который расширяет функциональные возможности входа для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="630ce-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="630ce-107">Пользователи могут создавать учетную запись входа сведений, хранящихся в удостоверении или они могут использовать внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="630ce-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="630ce-108">Поставщики поддерживаемых внешней учетной записи включают [Facebook, Google, учетной записи Майкрософт и Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="630ce-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="630ce-109">Удостоверений можно настроить с помощью базы данных SQL Server для хранения имен пользователей, пароли и данные профиля.</span><span class="sxs-lookup"><span data-stu-id="630ce-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="630ce-110">Кроме того других постоянных хранилищах можно использовать, например, хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="630ce-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="630ce-111">Просмотреть или скачать образец кода.</span><span class="sxs-lookup"><span data-stu-id="630ce-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="630ce-112">(Как для загрузки)</span><span class="sxs-lookup"><span data-stu-id="630ce-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="630ce-113">В этом разделе сведения об использовании идентификаторов регистрация, вход и выход пользователя.</span><span class="sxs-lookup"><span data-stu-id="630ce-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="630ce-114">Более подробные инструкции по созданию приложений, использующих удостоверения см. в разделе "Дальнейшие действия" в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="630ce-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="630ce-115">AddDefaultIdentity и AddIdentity</span><span class="sxs-lookup"><span data-stu-id="630ce-115">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="630ce-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) была введена в ASP. Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="630ce-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.Core 2.1.</span></span> <span data-ttu-id="630ce-117">Вызов `AddDefaultIdentity` аналогичен вызову следующее:</span><span class="sxs-lookup"><span data-stu-id="630ce-117">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="630ce-118">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="630ce-118">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="630ce-119">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="630ce-119">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="630ce-120">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="630ce-120">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="630ce-121">См. в разделе [AddDefaultIdentity источника](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="630ce-121">See [AddDefaultIdentity source](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="630ce-122">Создание веб-приложения с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="630ce-122">Create a Web app with authentication</span></span>

<span data-ttu-id="630ce-123">Создайте проект веб-приложение ASP.NET Core с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="630ce-123">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="630ce-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="630ce-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="630ce-125">Выберите **Файл** > **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="630ce-125">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="630ce-126">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="630ce-126">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="630ce-127">Назовите проект **WebApp1** иметь то же пространство имен, как загрузка проекта.</span><span class="sxs-lookup"><span data-stu-id="630ce-127">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="630ce-128">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="630ce-128">Click **OK**.</span></span>
* <span data-ttu-id="630ce-129">Выберите ASP.NET Core **веб-приложение** ASP.NET Core 2.1, затем выберите **изменить способ проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="630ce-129">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="630ce-130">Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="630ce-130">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="630ce-131">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="630ce-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="630ce-132">Созданный проект предоставляет [удостоверения ASP.NET Core](xref:security/authentication/identity) как [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="630ce-132">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="630ce-133">Тест регистра и имени входа</span><span class="sxs-lookup"><span data-stu-id="630ce-133">Test Register and Login</span></span>

<span data-ttu-id="630ce-134">Запустите приложение и регистрации пользователя.</span><span class="sxs-lookup"><span data-stu-id="630ce-134">Run the app and register a user.</span></span> <span data-ttu-id="630ce-135">Зависимости от размера экрана, может потребоваться выбрать кнопки-переключателя навигации для просмотра **зарегистрировать** и **входа** ссылки.</span><span class="sxs-lookup"><span data-stu-id="630ce-135">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![выключатель переходов.](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="630ce-137">Настройка служб удостоверений</span><span class="sxs-lookup"><span data-stu-id="630ce-137">Configure Identity services</span></span>

<span data-ttu-id="630ce-138">Службы добавляются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="630ce-138">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="630ce-139">По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="630ce-139">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="630ce-140">Следующий код не включает создание шаблона `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="630ce-140">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="630ce-141">Приведенный выше код настраивает удостоверений со значениями по умолчанию параметр.</span><span class="sxs-lookup"><span data-stu-id="630ce-141">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="630ce-142">Доступ к службе предоставляется в приложение с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="630ce-142">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="630ce-143">Удостоверение включено, вызвав [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="630ce-143">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="630ce-144">`UseAuthentication` Добавляет проверку подлинности [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="630ce-144">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="630ce-145">Доступ к службе предоставляется приложению через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="630ce-145">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="630ce-146">Удостоверение включено для приложения, вызвав `UseAuthentication` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="630ce-146">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="630ce-147">`UseAuthentication` Добавляет проверку подлинности [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="630ce-147">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="630ce-148">Эти службы доступны приложению через [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="630ce-148">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="630ce-149">Удостоверение включено для приложения, вызвав `UseIdentity` в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="630ce-149">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="630ce-150">`UseIdentity` Добавляет проверку подлинности на основе файлов cookie [по промежуточного слоя](xref:fundamentals/middleware/index) в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="630ce-150">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="630ce-151">Дополнительные сведения см. в разделе [класс IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) и [запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="630ce-151">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="630ce-152">Сформировать шаблон регистрации, входа и выхода</span><span class="sxs-lookup"><span data-stu-id="630ce-152">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="630ce-153">Выполните [позволяет формировать удостоверений в проект Razor без авторизации](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) инструкции.</span><span class="sxs-lookup"><span data-stu-id="630ce-153">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="630ce-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="630ce-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="630ce-155">Добавьте файлы регистрации, входа и выхода.</span><span class="sxs-lookup"><span data-stu-id="630ce-155">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="630ce-156">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="630ce-156">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="630ce-157">Если вы создали проект с именем **WebApp1**, выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="630ce-157">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="630ce-158">В противном случае использовать правильное пространство имен для `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="630ce-158">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="630ce-159">PowerShell используется точка с запятой в качестве разделителя команды.</span><span class="sxs-lookup"><span data-stu-id="630ce-159">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="630ce-160">При использовании PowerShell экранировать точку с запятой в списке файлов или поместить в список файлов в двойные кавычки, как показано в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="630ce-160">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="630ce-161">Проверьте регистр</span><span class="sxs-lookup"><span data-stu-id="630ce-161">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="630ce-162">Когда пользователь щелкает **зарегистрировать** ссылку, `RegisterModel.OnPostAsync` вызова действия.</span><span class="sxs-lookup"><span data-stu-id="630ce-162">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="630ce-163">Пользователь создается путем [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) на `_userManager` объекта.</span><span class="sxs-lookup"><span data-stu-id="630ce-163">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="630ce-164">`_userManager` предоставляется с помощью внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="630ce-164">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="630ce-165">Когда пользователь щелкает **зарегистрировать** ссылку, `Register` действие вызывается в `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="630ce-165">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="630ce-166">Действие `Register` создает пользователя путем вызова метода `CreateAsync` объекта`_userManager` (обеспечивается в `AccountController` путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="630ce-166">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="630ce-167">Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="630ce-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="630ce-168">**Примечание.** Раздел [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) описывает, как предотвратить автоматический вход пользователя при регистрации.</span><span class="sxs-lookup"><span data-stu-id="630ce-168">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="630ce-169">Войти</span><span class="sxs-lookup"><span data-stu-id="630ce-169">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="630ce-170">Форма входа отображается при:</span><span class="sxs-lookup"><span data-stu-id="630ce-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="630ce-171">**Вход** выбора ссылки.</span><span class="sxs-lookup"><span data-stu-id="630ce-171">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="630ce-172">Когда пользователь обращается к странице, где они не проходят проверку подлинности **или** авторизован, они перенаправляются на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="630ce-172">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span>

<span data-ttu-id="630ce-173">При отправке формы на странице входа, `OnPostAsync` вызова действия.</span><span class="sxs-lookup"><span data-stu-id="630ce-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="630ce-174">`PasswordSignInAsync` вызывается для `_signInManager` объекта (предоставляется с помощью внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="630ce-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="630ce-175">Базовый `Controller` предоставляет `User` свойство, которое можно получить из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="630ce-175">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="630ce-176">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="630ce-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="630ce-177">Дополнительные сведения см. в разделе [авторизации](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="630ce-177">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="630ce-178">Форма входа отображается в том случае, когда пользователь выбирает **вход** связывание или перенаправляются, когда доступ к странице, который требует проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="630ce-178">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="630ce-179">Когда пользователь отправляет форму на странице входа `AccountController` `Login` вызова действия.</span><span class="sxs-lookup"><span data-stu-id="630ce-179">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="630ce-180">`Login` Вызовов действия `PasswordSignInAsync` на `_signInManager` объекта (для `AccountController` с помощью внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="630ce-180">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="630ce-181">Базовый (`Controller` или `PageModel`) предоставляет `User` свойство.</span><span class="sxs-lookup"><span data-stu-id="630ce-181">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="630ce-182">Например `User.Claims` может быть перечислимым для принятия решений об авторизации.</span><span class="sxs-lookup"><span data-stu-id="630ce-182">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="630ce-183">Выход</span><span class="sxs-lookup"><span data-stu-id="630ce-183">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="630ce-184">**Выход** открывается `LogoutModel.OnPost` действие.</span><span class="sxs-lookup"><span data-stu-id="630ce-184">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="630ce-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) удаляет файл cookie утверждения пользователя.</span><span class="sxs-lookup"><span data-stu-id="630ce-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="630ce-186">Не перенаправления после вызова метода `SignOutAsync` или пользователь будет **не** выполнен выход.</span><span class="sxs-lookup"><span data-stu-id="630ce-186">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="630ce-187">POST указывается в *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="630ce-187">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="630ce-188">Щелкнув **Выход** связать вызовы `LogOut` действие.</span><span class="sxs-lookup"><span data-stu-id="630ce-188">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="630ce-189">Предыдущий код вызывает `_signInManager.SignOutAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="630ce-189">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="630ce-190">`SignOutAsync` Метод очищает утверждений пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="630ce-190">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="630ce-191">Проверить идентификатор</span><span class="sxs-lookup"><span data-stu-id="630ce-191">Test Identity</span></span>

<span data-ttu-id="630ce-192">Шаблоны веб-проектов по умолчанию разрешает анонимный доступ к домашней страницы.</span><span class="sxs-lookup"><span data-stu-id="630ce-192">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="630ce-193">Чтобы проверить тождество, добавьте [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) на страницу About.</span><span class="sxs-lookup"><span data-stu-id="630ce-193">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="630ce-194">Если вы вошли, выйдите из системы. Запустите приложение и выберите **о** ссылку.</span><span class="sxs-lookup"><span data-stu-id="630ce-194">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="630ce-195">Вы будете перенаправлены на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="630ce-195">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="630ce-196">Изучите удостоверений</span><span class="sxs-lookup"><span data-stu-id="630ce-196">Explore Identity</span></span>

<span data-ttu-id="630ce-197">Для просмотра идентификации, более подробно:</span><span class="sxs-lookup"><span data-stu-id="630ce-197">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="630ce-198">Создать источник полное удостоверение пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="630ce-198">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="630ce-199">Проверьте источник каждой страницы и пошагового выполнения отладчика.</span><span class="sxs-lookup"><span data-stu-id="630ce-199">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="630ce-200">Компонентами системы идентификации</span><span class="sxs-lookup"><span data-stu-id="630ce-200">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="630ce-201">Все удостоверения зависимые пакеты NuGet, включаются в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="630ce-201">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="630ce-202">Основной пакет для удостоверения [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="630ce-202">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="630ce-203">Этот пакет содержит базовый набор интерфейсов для ASP.NET Core Identity и включена в состав по `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="630ce-203">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="630ce-204">Переход на ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="630ce-204">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="630ce-205">Дополнительные сведения и инструкции по миграции существующего хранилища удостоверений, см. в разделе [перенести проверки подлинности и идентификации](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="630ce-205">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="630ce-206">Стойкость пароля параметр</span><span class="sxs-lookup"><span data-stu-id="630ce-206">Setting password strength</span></span>

<span data-ttu-id="630ce-207">См. в разделе [конфигурации](#pw) пример, задает минимальное паролей.</span><span class="sxs-lookup"><span data-stu-id="630ce-207">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="630ce-208">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="630ce-208">Next Steps</span></span>

* [<span data-ttu-id="630ce-209">Настройка Identity</span><span class="sxs-lookup"><span data-stu-id="630ce-209">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
