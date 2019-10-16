---
title: Введение в удостоверение на ASP.NET Core
author: rick-anderson
description: Используйте удостоверение с приложением ASP.NET Core. Узнайте, как устанавливать требования к паролю (Рекуиредигит, Рекуиредленгс, Рекуиредуникуечарс и др.).
ms.author: riande
ms.date: 10/15/2019
uid: security/authentication/identity
ms.openlocfilehash: 8da13ca5f74a9c829eb8137d33af0684ff88266d
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333548"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="ea602-104">Введение в удостоверение на ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea602-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ea602-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ea602-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ea602-106">ASP.NET Core удостоверение — это система членства, которая поддерживает функции входа в пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="ea602-106">ASP.NET Core Identity is a membership system that supports user interface (UI) login functionality.</span></span> <span data-ttu-id="ea602-107">Пользователи могут создать учетную запись с данными для входа, хранящимися в удостоверении, или использовать внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="ea602-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="ea602-108">Поддерживаемые внешние поставщики входа включают [Facebook, Google, учетную запись Майкрософт и Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ea602-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="ea602-109">Идентификацию можно настроить с помощью SQL Server базы данных для хранения имен пользователей, паролей и данных профилей.</span><span class="sxs-lookup"><span data-stu-id="ea602-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="ea602-110">Кроме того, можно использовать еще одно постоянное хранилище, например хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="ea602-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="ea602-111">В этом разделе вы узнаете, как использовать удостоверение для регистрации, входа и выхода пользователя.</span><span class="sxs-lookup"><span data-stu-id="ea602-111">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="ea602-112">Более подробные инструкции по созданию приложений, использующих удостоверение, см. в разделе дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="ea602-112">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="ea602-113">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([Загрузка)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="ea602-113">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="ea602-114">Создание веб-приложения с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="ea602-114">Create a Web app with authentication</span></span>

<span data-ttu-id="ea602-115">Создание ASP.NET Core проекта веб-приложения с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="ea602-115">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ea602-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea602-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ea602-117">Выберите **файл** > **Новый** **проект**>.</span><span class="sxs-lookup"><span data-stu-id="ea602-117">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="ea602-118">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ea602-118">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ea602-119">Присвойте проекту имя **APP1** , которое будет иметь то же пространство имен, что и загружаемый проект.</span><span class="sxs-lookup"><span data-stu-id="ea602-119">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="ea602-120">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ea602-120">Click **OK**.</span></span>
* <span data-ttu-id="ea602-121">Выберите ASP.NET Core **веб-приложение**, а затем щелкните **изменить проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="ea602-121">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="ea602-122">Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ea602-122">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ea602-123">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ea602-123">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="ea602-124">Предыдущая команда создает веб-приложение Razor с помощью SQLite.</span><span class="sxs-lookup"><span data-stu-id="ea602-124">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="ea602-125">Чтобы создать веб-приложение с помощью LocalDB, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ea602-125">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="ea602-126">Созданный проект предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) в виде [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="ea602-126">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="ea602-127">Библиотека классов Razor Identity предоставляет конечные точки с областью `Identity`.</span><span class="sxs-lookup"><span data-stu-id="ea602-127">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="ea602-128">Пример:</span><span class="sxs-lookup"><span data-stu-id="ea602-128">For example:</span></span>

* <span data-ttu-id="ea602-129">/идентити/аккаунт/логин</span><span class="sxs-lookup"><span data-stu-id="ea602-129">/Identity/Account/Login</span></span>
* <span data-ttu-id="ea602-130">/идентити/аккаунт/логаут</span><span class="sxs-lookup"><span data-stu-id="ea602-130">/Identity/Account/Logout</span></span>
* <span data-ttu-id="ea602-131">/идентити/аккаунт/манаже</span><span class="sxs-lookup"><span data-stu-id="ea602-131">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="ea602-132">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="ea602-132">Apply migrations</span></span>

<span data-ttu-id="ea602-133">Примените миграции, чтобы инициализировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="ea602-133">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ea602-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea602-134">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ea602-135">Выполните следующую команду в консоли диспетчера пакетов (PMC):</span><span class="sxs-lookup"><span data-stu-id="ea602-135">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ea602-136">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ea602-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ea602-137">Миграция не требуется на этом этапе при использовании SQLite.</span><span class="sxs-lookup"><span data-stu-id="ea602-137">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="ea602-138">Для LocalDB выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ea602-138">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="ea602-139">Проверка регистра и вход в систему</span><span class="sxs-lookup"><span data-stu-id="ea602-139">Test Register and Login</span></span>

<span data-ttu-id="ea602-140">Запустите приложение и зарегистрируйте пользователя.</span><span class="sxs-lookup"><span data-stu-id="ea602-140">Run the app and register a user.</span></span> <span data-ttu-id="ea602-141">В зависимости от размера экрана может потребоваться выбрать переключатель навигации, чтобы просмотреть ссылки **Регистрация** и **Вход** .</span><span class="sxs-lookup"><span data-stu-id="ea602-141">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="ea602-142">Настройка служб удостоверений</span><span class="sxs-lookup"><span data-stu-id="ea602-142">Configure Identity services</span></span>

<span data-ttu-id="ea602-143">Службы добавляются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ea602-143">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="ea602-144">По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="ea602-144">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="ea602-145">Выделенный выше код настраивает удостоверение значениями параметров по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ea602-145">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="ea602-146">Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ea602-146">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="ea602-147">Удостоверение включается путем вызова <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="ea602-147">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="ea602-148">`UseAuthentication` добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="ea602-148">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="ea602-149">Созданное шаблоном приложение не использует [авторизацию](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="ea602-149">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="ea602-150">`app.UseAuthorization` включается, чтобы гарантировать его добавление в правильном порядке, если приложение добавляет авторизацию.</span><span class="sxs-lookup"><span data-stu-id="ea602-150">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="ea602-151">`UseRouting`, `UseAuthentication`, `UseAuthorization` и `UseEndpoints` должны вызываться в порядке, показанном в приведенном выше коде.</span><span class="sxs-lookup"><span data-stu-id="ea602-151">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="ea602-152">Дополнительные сведения о `IdentityOptions` и `Startup` см. в разделе <xref:Microsoft.AspNetCore.Identity.IdentityOptions> и [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="ea602-152">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="ea602-153">Регистрация, вход и выход из шаблона формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="ea602-153">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ea602-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea602-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ea602-155">Добавьте файлы Register, login и LogOut.</span><span class="sxs-lookup"><span data-stu-id="ea602-155">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="ea602-156">Соблюдайте [идентификатор шаблона в проекте Razor с](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) инструкциями по авторизации для создания кода, приведенного в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="ea602-156">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ea602-157">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ea602-157">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ea602-158">Если вы создали проект с именем " **APP1**", выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="ea602-158">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="ea602-159">В противном случае используйте правильное пространство имен для `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="ea602-159">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="ea602-160">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="ea602-160">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="ea602-161">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки, как показано в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="ea602-161">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="ea602-162">Дополнительные сведения об удостоверении формирования шаблонов см. в разделе [удостоверение шаблона в проекте Razor с авторизацией](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="ea602-162">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="ea602-163">Проверка регистра</span><span class="sxs-lookup"><span data-stu-id="ea602-163">Examine Register</span></span>

<span data-ttu-id="ea602-164">Когда пользователь щелкает ссылку **Register** , вызывается действие `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="ea602-164">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="ea602-165">Пользователь создается с помощью [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) на объекте `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="ea602-165">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="ea602-166">`_userManager` предоставляется путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="ea602-166">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="ea602-167">Если пользователь успешно создан, пользователь входит в систему с помощью вызова `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="ea602-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="ea602-168">Инструкции по предотвращению немедленного входа при регистрации см. в статье [Подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) .</span><span class="sxs-lookup"><span data-stu-id="ea602-168">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="ea602-169">Войти</span><span class="sxs-lookup"><span data-stu-id="ea602-169">Log in</span></span>

<span data-ttu-id="ea602-170">Форма входа отображается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="ea602-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="ea602-171">Выбрана ссылка для **входа** .</span><span class="sxs-lookup"><span data-stu-id="ea602-171">The **Log in** link is selected.</span></span>
* <span data-ttu-id="ea602-172">Пользователь пытается получить доступ к странице с ограниченным доступом, которая не авторизована для доступа, **или** если система не прошла проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="ea602-172">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="ea602-173">При отправке формы на странице входа вызывается действие `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="ea602-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="ea602-174">`PasswordSignInAsync` вызывается для объекта `_signInManager` (предоставляется путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="ea602-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="ea602-175">Базовый класс `Controller` предоставляет свойство `User`, доступ к которому можно получить из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="ea602-175">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="ea602-176">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="ea602-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="ea602-177">Для получения дополнительной информации см. <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="ea602-177">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="ea602-178">Выход</span><span class="sxs-lookup"><span data-stu-id="ea602-178">Log out</span></span>

<span data-ttu-id="ea602-179">Ссылка **выхода** вызывает действие `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="ea602-179">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="ea602-180">В приведенном выше коде код `return RedirectToPage();` должен быть перенаправлением, чтобы браузер выполнит новый запрос и удостоверение для пользователя будет обновлено.</span><span class="sxs-lookup"><span data-stu-id="ea602-180">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="ea602-181">[Сигнаутасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) очищает утверждения пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="ea602-181">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="ea602-182">POST указывается в параметре *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea602-182">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="ea602-183">Проверить удостоверение</span><span class="sxs-lookup"><span data-stu-id="ea602-183">Test Identity</span></span>

<span data-ttu-id="ea602-184">Шаблоны веб-проектов по умолчанию разрешают анонимный доступ к домашним страницам.</span><span class="sxs-lookup"><span data-stu-id="ea602-184">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="ea602-185">Чтобы проверить удостоверение, добавьте [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="ea602-185">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="ea602-186">Если вы вошли в систему, выйдите из нее. Запустите приложение и щелкните ссылку **Конфиденциальность** .</span><span class="sxs-lookup"><span data-stu-id="ea602-186">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="ea602-187">Вы будете перенаправлены на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="ea602-187">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="ea602-188">Просмотр удостоверений</span><span class="sxs-lookup"><span data-stu-id="ea602-188">Explore Identity</span></span>

<span data-ttu-id="ea602-189">Более подробное изучение удостоверений:</span><span class="sxs-lookup"><span data-stu-id="ea602-189">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="ea602-190">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="ea602-190">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="ea602-191">Изучите источник каждой страницы и пошаговое выполнение отладчика.</span><span class="sxs-lookup"><span data-stu-id="ea602-191">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="ea602-192">Компоненты идентификации</span><span class="sxs-lookup"><span data-stu-id="ea602-192">Identity Components</span></span>

<span data-ttu-id="ea602-193">Все пакеты NuGet, зависящие от удостоверения, включены в [ASP.NET Core общую платформу](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="ea602-193">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="ea602-194">Основным пакетом для удостоверения является [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="ea602-194">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="ea602-195">Этот пакет содержит основной набор интерфейсов для ASP.NET Core удостоверения и включается `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="ea602-195">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="ea602-196">Миграция на ASP.NET Core удостоверение</span><span class="sxs-lookup"><span data-stu-id="ea602-196">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="ea602-197">Дополнительные сведения и рекомендации по переносу существующего хранилища удостоверений см. в статье [Миграция проверки подлинности и удостоверений](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="ea602-197">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="ea602-198">Установка силы пароля</span><span class="sxs-lookup"><span data-stu-id="ea602-198">Setting password strength</span></span>

<span data-ttu-id="ea602-199">См. раздел [Конфигурация](#pw) для примера, который устанавливает минимальные требования к паролю.</span><span class="sxs-lookup"><span data-stu-id="ea602-199">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="ea602-200">Адддефаултидентити и Аддидентити</span><span class="sxs-lookup"><span data-stu-id="ea602-200">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="ea602-201"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> был введен в ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="ea602-201"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="ea602-202">Вызов `AddDefaultIdentity` аналогичен вызову следующего:</span><span class="sxs-lookup"><span data-stu-id="ea602-202">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="ea602-203">Дополнительные сведения см. в разделе [адддефаултидентити Source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="ea602-203">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea602-204">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ea602-204">Next Steps</span></span>

* [<span data-ttu-id="ea602-205">Настройка Identity</span><span class="sxs-lookup"><span data-stu-id="ea602-205">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ea602-206">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ea602-206">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ea602-207">ASP.NET Core удостоверение — это система членства, которая добавляет функции входа в ASP.NET Core приложения.</span><span class="sxs-lookup"><span data-stu-id="ea602-207">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="ea602-208">Пользователи могут создать учетную запись с данными для входа, хранящимися в удостоверении, или использовать внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="ea602-208">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="ea602-209">Поддерживаемые внешние поставщики входа включают [Facebook, Google, учетную запись Майкрософт и Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ea602-209">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="ea602-210">Идентификацию можно настроить с помощью SQL Server базы данных для хранения имен пользователей, паролей и данных профилей.</span><span class="sxs-lookup"><span data-stu-id="ea602-210">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="ea602-211">Кроме того, можно использовать еще одно постоянное хранилище, например хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="ea602-211">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="ea602-212">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([Загрузка)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="ea602-212">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="ea602-213">В этом разделе вы узнаете, как использовать удостоверение для регистрации, входа и выхода пользователя.</span><span class="sxs-lookup"><span data-stu-id="ea602-213">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="ea602-214">Более подробные инструкции по созданию приложений, использующих удостоверение, см. в разделе дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="ea602-214">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="ea602-215">Адддефаултидентити и Аддидентити</span><span class="sxs-lookup"><span data-stu-id="ea602-215">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="ea602-216"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> был введен в ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="ea602-216"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="ea602-217">Вызов `AddDefaultIdentity` аналогичен вызову следующего:</span><span class="sxs-lookup"><span data-stu-id="ea602-217">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="ea602-218">Дополнительные сведения см. в разделе [адддефаултидентити Source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="ea602-218">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="ea602-219">Создание веб-приложения с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="ea602-219">Create a Web app with authentication</span></span>

<span data-ttu-id="ea602-220">Создание ASP.NET Core проекта веб-приложения с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="ea602-220">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ea602-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea602-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ea602-222">Выберите **файл** > **Новый** **проект**>.</span><span class="sxs-lookup"><span data-stu-id="ea602-222">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="ea602-223">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ea602-223">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ea602-224">Присвойте проекту имя **APP1** , которое будет иметь то же пространство имен, что и загружаемый проект.</span><span class="sxs-lookup"><span data-stu-id="ea602-224">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="ea602-225">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ea602-225">Click **OK**.</span></span>
* <span data-ttu-id="ea602-226">Выберите ASP.NET Core **веб-приложение**, а затем щелкните **изменить проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="ea602-226">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="ea602-227">Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ea602-227">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ea602-228">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ea602-228">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="ea602-229">Созданный проект предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) в виде [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="ea602-229">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="ea602-230">Библиотека классов Razor Identity предоставляет конечные точки с областью `Identity`.</span><span class="sxs-lookup"><span data-stu-id="ea602-230">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="ea602-231">Пример:</span><span class="sxs-lookup"><span data-stu-id="ea602-231">For example:</span></span>

* <span data-ttu-id="ea602-232">/идентити/аккаунт/логин</span><span class="sxs-lookup"><span data-stu-id="ea602-232">/Identity/Account/Login</span></span>
* <span data-ttu-id="ea602-233">/идентити/аккаунт/логаут</span><span class="sxs-lookup"><span data-stu-id="ea602-233">/Identity/Account/Logout</span></span>
* <span data-ttu-id="ea602-234">/идентити/аккаунт/манаже</span><span class="sxs-lookup"><span data-stu-id="ea602-234">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="ea602-235">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="ea602-235">Apply migrations</span></span>

<span data-ttu-id="ea602-236">Примените миграции, чтобы инициализировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="ea602-236">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ea602-237">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea602-237">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ea602-238">Выполните следующую команду в консоли диспетчера пакетов (PMC):</span><span class="sxs-lookup"><span data-stu-id="ea602-238">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ea602-239">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ea602-239">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="ea602-240">Проверка регистра и вход в систему</span><span class="sxs-lookup"><span data-stu-id="ea602-240">Test Register and Login</span></span>

<span data-ttu-id="ea602-241">Запустите приложение и зарегистрируйте пользователя.</span><span class="sxs-lookup"><span data-stu-id="ea602-241">Run the app and register a user.</span></span> <span data-ttu-id="ea602-242">В зависимости от размера экрана может потребоваться выбрать переключатель навигации, чтобы просмотреть ссылки **Регистрация** и **Вход** .</span><span class="sxs-lookup"><span data-stu-id="ea602-242">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="ea602-243">Настройка служб удостоверений</span><span class="sxs-lookup"><span data-stu-id="ea602-243">Configure Identity services</span></span>

<span data-ttu-id="ea602-244">Службы добавляются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ea602-244">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="ea602-245">По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="ea602-245">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="ea602-246">Приведенный выше код настраивает удостоверение с использованием значений параметров по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ea602-246">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="ea602-247">Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ea602-247">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="ea602-248">Удостоверение включается путем вызова [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="ea602-248">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="ea602-249">`UseAuthentication` добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="ea602-249">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="ea602-250">Дополнительные сведения см. в разделе [класс идентитйоптионс](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) и [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="ea602-250">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="ea602-251">Регистрация, вход и выход из шаблона формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="ea602-251">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="ea602-252">Соблюдайте [идентификатор шаблона в проекте Razor с](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) инструкциями по авторизации для создания кода, приведенного в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="ea602-252">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ea602-253">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea602-253">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ea602-254">Добавьте файлы Register, login и LogOut.</span><span class="sxs-lookup"><span data-stu-id="ea602-254">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ea602-255">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ea602-255">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ea602-256">Если вы создали проект с именем " **APP1**", выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="ea602-256">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="ea602-257">В противном случае используйте правильное пространство имен для `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="ea602-257">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="ea602-258">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="ea602-258">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="ea602-259">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки, как показано в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="ea602-259">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="ea602-260">Проверка регистра</span><span class="sxs-lookup"><span data-stu-id="ea602-260">Examine Register</span></span>

<span data-ttu-id="ea602-261">Когда пользователь щелкает ссылку **Register** , вызывается действие `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="ea602-261">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="ea602-262">Пользователь создается с помощью [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) на объекте `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="ea602-262">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="ea602-263">`_userManager` предоставляется путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="ea602-263">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="ea602-264">Если пользователь успешно создан, пользователь входит в систему с помощью вызова `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="ea602-264">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="ea602-265">**Примечание.** Инструкции по предотвращению немедленного входа при регистрации см. в статье [Подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) .</span><span class="sxs-lookup"><span data-stu-id="ea602-265">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="ea602-266">Войти</span><span class="sxs-lookup"><span data-stu-id="ea602-266">Log in</span></span>

<span data-ttu-id="ea602-267">Форма входа отображается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="ea602-267">The Login form is displayed when:</span></span>

* <span data-ttu-id="ea602-268">Выбрана ссылка для **входа** .</span><span class="sxs-lookup"><span data-stu-id="ea602-268">The **Log in** link is selected.</span></span>
* <span data-ttu-id="ea602-269">Пользователь пытается получить доступ к странице с ограниченным доступом, которая не авторизована для доступа, **или** если система не прошла проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="ea602-269">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="ea602-270">При отправке формы на странице входа вызывается действие `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="ea602-270">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="ea602-271">`PasswordSignInAsync` вызывается для объекта `_signInManager` (предоставляется путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="ea602-271">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="ea602-272">Базовый класс `Controller` предоставляет свойство `User`, доступ к которому можно получить из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="ea602-272">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="ea602-273">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="ea602-273">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="ea602-274">Для получения дополнительной информации см. <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="ea602-274">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="ea602-275">Выход</span><span class="sxs-lookup"><span data-stu-id="ea602-275">Log out</span></span>

<span data-ttu-id="ea602-276">Ссылка **выхода** вызывает действие `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="ea602-276">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="ea602-277">[Сигнаутасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) очищает утверждения пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="ea602-277">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="ea602-278">POST указывается в параметре *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ea602-278">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="ea602-279">Проверить удостоверение</span><span class="sxs-lookup"><span data-stu-id="ea602-279">Test Identity</span></span>

<span data-ttu-id="ea602-280">Шаблоны веб-проектов по умолчанию разрешают анонимный доступ к домашним страницам.</span><span class="sxs-lookup"><span data-stu-id="ea602-280">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="ea602-281">Чтобы проверить удостоверение, добавьте [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) на страницу конфиденциальности.</span><span class="sxs-lookup"><span data-stu-id="ea602-281">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="ea602-282">Если вы вошли в систему, выйдите из нее. Запустите приложение и щелкните ссылку **Конфиденциальность** .</span><span class="sxs-lookup"><span data-stu-id="ea602-282">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="ea602-283">Вы будете перенаправлены на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="ea602-283">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="ea602-284">Просмотр удостоверений</span><span class="sxs-lookup"><span data-stu-id="ea602-284">Explore Identity</span></span>

<span data-ttu-id="ea602-285">Более подробное изучение удостоверений:</span><span class="sxs-lookup"><span data-stu-id="ea602-285">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="ea602-286">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="ea602-286">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="ea602-287">Изучите источник каждой страницы и пошаговое выполнение отладчика.</span><span class="sxs-lookup"><span data-stu-id="ea602-287">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="ea602-288">Компоненты идентификации</span><span class="sxs-lookup"><span data-stu-id="ea602-288">Identity Components</span></span>

<span data-ttu-id="ea602-289">Все пакеты NuGet, зависящие от удостоверения, включены в пакет [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="ea602-289">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="ea602-290">Основным пакетом для удостоверения является [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="ea602-290">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="ea602-291">Этот пакет содержит основной набор интерфейсов для ASP.NET Core удостоверения и включается `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="ea602-291">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="ea602-292">Миграция на ASP.NET Core удостоверение</span><span class="sxs-lookup"><span data-stu-id="ea602-292">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="ea602-293">Дополнительные сведения и рекомендации по переносу существующего хранилища удостоверений см. в статье [Миграция проверки подлинности и удостоверений](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="ea602-293">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="ea602-294">Установка силы пароля</span><span class="sxs-lookup"><span data-stu-id="ea602-294">Setting password strength</span></span>

<span data-ttu-id="ea602-295">См. раздел [Конфигурация](#pw) для примера, который устанавливает минимальные требования к паролю.</span><span class="sxs-lookup"><span data-stu-id="ea602-295">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea602-296">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ea602-296">Next Steps</span></span>

* [<span data-ttu-id="ea602-297">Настройка Identity</span><span class="sxs-lookup"><span data-stu-id="ea602-297">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
