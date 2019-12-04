---
title: Введение в удостоверение на ASP.NET Core
author: rick-anderson
description: Используйте удостоверение с приложением ASP.NET Core. Узнайте, как устанавливать требования к паролю (Рекуиредигит, Рекуиредленгс, Рекуиредуникуечарс и др.).
ms.author: riande
ms.date: 12/7/2019
uid: security/authentication/identity
ms.openlocfilehash: 331ebe36eb4bb7fa694de8daa969bcabcab1c974
ms.sourcegitcommit: b3e1e31e5d8bdd94096cf27444594d4a7b065525
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74803400"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="20e2f-104">Введение в удостоверение на ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20e2f-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="20e2f-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="20e2f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="20e2f-106">Удостоверение ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="20e2f-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="20e2f-107">— Это API, поддерживающий функцию входа в пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="20e2f-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="20e2f-108">Управляет пользователями, паролями, данными профилирования, ролями, утверждениями, маркерами, подтверждением электронной почты и т. д.</span><span class="sxs-lookup"><span data-stu-id="20e2f-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="20e2f-109">Пользователи могут создать учетную запись с данными для входа, хранящимися в удостоверении, или использовать внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="20e2f-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="20e2f-110">Поддерживаемые внешние поставщики входа включают [Facebook, Google, учетную запись Майкрософт и Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="20e2f-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="20e2f-111">[Исходный код удостоверения](https://github.com/aspnet/AspNetCore/tree/master/src/Identity) доступен на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="20e2f-111">The [Identity source code](https://github.com/aspnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="20e2f-112">[Удостоверение формирования шаблонов](xref:security/authentication/scaffold-identity) и просмотр созданных файлов для проверки взаимодействия шаблона с удостоверением.</span><span class="sxs-lookup"><span data-stu-id="20e2f-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

<span data-ttu-id="20e2f-113">Удостоверение обычно настраивается с помощью SQL Server базы данных для хранения имен пользователей, паролей и данных профилей.</span><span class="sxs-lookup"><span data-stu-id="20e2f-113">Identity is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="20e2f-114">Кроме того, можно использовать еще одно постоянное хранилище, например хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="20e2f-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="20e2f-115">В этом разделе вы узнаете, как использовать удостоверение для регистрации, входа и выхода пользователя.</span><span class="sxs-lookup"><span data-stu-id="20e2f-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="20e2f-116">Более подробные инструкции по созданию приложений, использующих удостоверение, см. в разделе дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="20e2f-116">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="20e2f-117">[Платформа Microsoft Identity Platform](/azure/active-directory/develop/) :</span><span class="sxs-lookup"><span data-stu-id="20e2f-117">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="20e2f-118">Развитие платформы разработки Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="20e2f-118">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="20e2f-119">Не связано с удостоверением ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="20e2f-119">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="20e2f-120">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([Загрузка)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="20e2f-120">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="20e2f-121">Создание веб-приложения с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="20e2f-121">Create a Web app with authentication</span></span>

<span data-ttu-id="20e2f-122">Создание ASP.NET Core проекта веб-приложения с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="20e2f-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20e2f-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20e2f-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="20e2f-124">Выберите **File** > **New** > **Project** ( Файл > Создать > Проект).</span><span class="sxs-lookup"><span data-stu-id="20e2f-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="20e2f-125">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="20e2f-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="20e2f-126">Присвойте проекту имя **APP1** , которое будет иметь то же пространство имен, что и загружаемый проект.</span><span class="sxs-lookup"><span data-stu-id="20e2f-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="20e2f-127">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="20e2f-127">Click **OK**.</span></span>
* <span data-ttu-id="20e2f-128">Выберите ASP.NET Core **веб-приложение**, а затем щелкните **изменить проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="20e2f-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="20e2f-129">Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="20e2f-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20e2f-130">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="20e2f-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="20e2f-131">Предыдущая команда создает веб-приложение Razor с помощью SQLite.</span><span class="sxs-lookup"><span data-stu-id="20e2f-131">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="20e2f-132">Чтобы создать веб-приложение с помощью LocalDB, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="20e2f-132">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="20e2f-133">Созданный проект предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) в виде [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="20e2f-133">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="20e2f-134">Библиотека классов Razor Identity предоставляет конечные точки с областью `Identity`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-134">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="20e2f-135">Например:</span><span class="sxs-lookup"><span data-stu-id="20e2f-135">For example:</span></span>

* <span data-ttu-id="20e2f-136">/идентити/аккаунт/логин</span><span class="sxs-lookup"><span data-stu-id="20e2f-136">/Identity/Account/Login</span></span>
* <span data-ttu-id="20e2f-137">/идентити/аккаунт/логаут</span><span class="sxs-lookup"><span data-stu-id="20e2f-137">/Identity/Account/Logout</span></span>
* <span data-ttu-id="20e2f-138">/идентити/аккаунт/манаже</span><span class="sxs-lookup"><span data-stu-id="20e2f-138">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="20e2f-139">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="20e2f-139">Apply migrations</span></span>

<span data-ttu-id="20e2f-140">Примените миграции, чтобы инициализировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="20e2f-140">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20e2f-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20e2f-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20e2f-142">Выполните следующую команду в консоли диспетчера пакетов (PMC):</span><span class="sxs-lookup"><span data-stu-id="20e2f-142">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20e2f-143">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="20e2f-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20e2f-144">Миграция не требуется на этом этапе при использовании SQLite.</span><span class="sxs-lookup"><span data-stu-id="20e2f-144">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="20e2f-145">Для LocalDB выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="20e2f-145">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="20e2f-146">Проверка регистра и вход в систему</span><span class="sxs-lookup"><span data-stu-id="20e2f-146">Test Register and Login</span></span>

<span data-ttu-id="20e2f-147">Запустите приложение и зарегистрируйте пользователя.</span><span class="sxs-lookup"><span data-stu-id="20e2f-147">Run the app and register a user.</span></span> <span data-ttu-id="20e2f-148">В зависимости от размера экрана может потребоваться выбрать переключатель навигации, чтобы просмотреть ссылки **Регистрация** и **Вход** .</span><span class="sxs-lookup"><span data-stu-id="20e2f-148">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="20e2f-149">Настройка служб удостоверений</span><span class="sxs-lookup"><span data-stu-id="20e2f-149">Configure Identity services</span></span>

<span data-ttu-id="20e2f-150">Службы добавляются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-150">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="20e2f-151">По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-151">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="20e2f-152">Выделенный выше код настраивает удостоверение значениями параметров по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="20e2f-152">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="20e2f-153">Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="20e2f-153">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="20e2f-154">Удостоверение включается путем вызова <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="20e2f-154">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="20e2f-155">`UseAuthentication` добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="20e2f-155">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="20e2f-156">Созданное шаблоном приложение не использует [авторизацию](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="20e2f-156">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="20e2f-157">`app.UseAuthorization` включен, чтобы убедиться, что он добавлен в правильном порядке, если приложение добавляет авторизацию.</span><span class="sxs-lookup"><span data-stu-id="20e2f-157">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="20e2f-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`и `UseEndpoints` должны вызываться в порядке, показанном в предыдущем коде.</span><span class="sxs-lookup"><span data-stu-id="20e2f-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="20e2f-159">Дополнительные сведения о `IdentityOptions` и `Startup`см. в разделе <xref:Microsoft.AspNetCore.Identity.IdentityOptions> и [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="20e2f-159">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="20e2f-160">Регистрация, вход и выход из шаблона формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="20e2f-160">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20e2f-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20e2f-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20e2f-162">Добавьте файлы Register, login и LogOut.</span><span class="sxs-lookup"><span data-stu-id="20e2f-162">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="20e2f-163">Соблюдайте [идентификатор шаблона в проекте Razor с](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) инструкциями по авторизации для создания кода, приведенного в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="20e2f-163">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20e2f-164">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="20e2f-164">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20e2f-165">Если вы создали проект с именем " **APP1**", выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="20e2f-165">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="20e2f-166">В противном случае используйте правильное пространство имен для `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-166">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="20e2f-167">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="20e2f-167">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="20e2f-168">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки, как показано в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="20e2f-168">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="20e2f-169">Дополнительные сведения об удостоверении формирования шаблонов см. в разделе [удостоверение шаблона в проекте Razor с авторизацией](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="20e2f-169">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="20e2f-170">Проверка регистра</span><span class="sxs-lookup"><span data-stu-id="20e2f-170">Examine Register</span></span>

<span data-ttu-id="20e2f-171">Когда пользователь щелкает ссылку **Register** , вызывается действие `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-171">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="20e2f-172">Пользователь создается с помощью [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) на объекте `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-172">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="20e2f-173">`_userManager` предоставляется путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="20e2f-173">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="20e2f-174">Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="20e2f-175">Инструкции по предотвращению немедленного входа при регистрации см. в статье [Подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) .</span><span class="sxs-lookup"><span data-stu-id="20e2f-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="20e2f-176">Вход</span><span class="sxs-lookup"><span data-stu-id="20e2f-176">Log in</span></span>

<span data-ttu-id="20e2f-177">Форма входа отображается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="20e2f-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="20e2f-178">Выбрана ссылка для **входа** .</span><span class="sxs-lookup"><span data-stu-id="20e2f-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="20e2f-179">Пользователь пытается получить доступ к странице с ограниченным доступом, которая не авторизована для доступа, **или** если система не прошла проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="20e2f-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="20e2f-180">При отправке формы на странице входа вызывается действие `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="20e2f-181">`PasswordSignInAsync` вызывается для объекта `_signInManager` (предоставленного путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="20e2f-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="20e2f-182">Базовый класс `Controller` предоставляет свойство `User`, доступ к которому можно получить из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="20e2f-182">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="20e2f-183">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="20e2f-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="20e2f-184">Для получения дополнительной информации см. <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="20e2f-184">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="20e2f-185">Выход</span><span class="sxs-lookup"><span data-stu-id="20e2f-185">Log out</span></span>

<span data-ttu-id="20e2f-186">Ссылка **выхода** вызывает действие `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="20e2f-187">В приведенном выше коде код `return RedirectToPage();` должен быть перенаправлением, чтобы браузер выполнит новый запрос и удостоверение для пользователя будет обновлено.</span><span class="sxs-lookup"><span data-stu-id="20e2f-187">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="20e2f-188">[Сигнаутасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) очищает утверждения пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="20e2f-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="20e2f-189">Параметр POST указан в *страницах Pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20e2f-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="20e2f-190">Проверить удостоверение</span><span class="sxs-lookup"><span data-stu-id="20e2f-190">Test Identity</span></span>

<span data-ttu-id="20e2f-191">Шаблоны веб-проектов по умолчанию разрешают анонимный доступ к домашним страницам.</span><span class="sxs-lookup"><span data-stu-id="20e2f-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="20e2f-192">Чтобы проверить удостоверение, добавьте [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="20e2f-192">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="20e2f-193">Если вы вошли в систему, выйдите из нее. Запустите приложение и щелкните ссылку **Конфиденциальность** .</span><span class="sxs-lookup"><span data-stu-id="20e2f-193">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="20e2f-194">Вы перейдете на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="20e2f-194">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="20e2f-195">Просмотр удостоверений</span><span class="sxs-lookup"><span data-stu-id="20e2f-195">Explore Identity</span></span>

<span data-ttu-id="20e2f-196">Более подробное изучение удостоверений:</span><span class="sxs-lookup"><span data-stu-id="20e2f-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="20e2f-197">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="20e2f-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="20e2f-198">Изучите источник каждой страницы и пошаговое выполнение отладчика.</span><span class="sxs-lookup"><span data-stu-id="20e2f-198">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="20e2f-199">Компоненты идентификации</span><span class="sxs-lookup"><span data-stu-id="20e2f-199">Identity Components</span></span>

<span data-ttu-id="20e2f-200">Все пакеты NuGet, зависящие от удостоверения, включены в [ASP.NET Core общую платформу](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="20e2f-200">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="20e2f-201">Основным пакетом для удостоверения является [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="20e2f-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="20e2f-202">Этот пакет содержит основной набор интерфейсов для ASP.NET Core удостоверения и включается `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="20e2f-203">Миграция на ASP.NET Core удостоверение</span><span class="sxs-lookup"><span data-stu-id="20e2f-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="20e2f-204">Дополнительные сведения и рекомендации по переносу существующего хранилища удостоверений см. в статье [Миграция проверки подлинности и удостоверений](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="20e2f-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="20e2f-205">Установка силы пароля</span><span class="sxs-lookup"><span data-stu-id="20e2f-205">Setting password strength</span></span>

<span data-ttu-id="20e2f-206">См. раздел [Конфигурация](#pw) для примера, который устанавливает минимальные требования к паролю.</span><span class="sxs-lookup"><span data-stu-id="20e2f-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="20e2f-207">Адддефаултидентити и Аддидентити</span><span class="sxs-lookup"><span data-stu-id="20e2f-207">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="20e2f-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> было введено в ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="20e2f-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="20e2f-209">Вызов `AddDefaultIdentity` аналогичен вызову следующего:</span><span class="sxs-lookup"><span data-stu-id="20e2f-209">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="20e2f-210">Дополнительные сведения см. в разделе [адддефаултидентити Source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="20e2f-210">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="20e2f-211">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="20e2f-211">Next Steps</span></span>

* [<span data-ttu-id="20e2f-212">Настройка Identity</span><span class="sxs-lookup"><span data-stu-id="20e2f-212">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="20e2f-213">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="20e2f-213">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="20e2f-214">ASP.NET Core удостоверение — это система членства, которая добавляет функции входа в ASP.NET Core приложения.</span><span class="sxs-lookup"><span data-stu-id="20e2f-214">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="20e2f-215">Пользователи могут создать учетную запись с данными для входа, хранящимися в удостоверении, или использовать внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="20e2f-215">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="20e2f-216">Поддерживаемые внешние поставщики входа включают [Facebook, Google, учетную запись Майкрософт и Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="20e2f-216">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="20e2f-217">Идентификацию можно настроить с помощью SQL Server базы данных для хранения имен пользователей, паролей и данных профилей.</span><span class="sxs-lookup"><span data-stu-id="20e2f-217">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="20e2f-218">Кроме того, можно использовать еще одно постоянное хранилище, например хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="20e2f-218">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="20e2f-219">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([Загрузка)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="20e2f-219">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="20e2f-220">В этом разделе вы узнаете, как использовать удостоверение для регистрации, входа и выхода пользователя.</span><span class="sxs-lookup"><span data-stu-id="20e2f-220">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="20e2f-221">Более подробные инструкции по созданию приложений, использующих удостоверение, см. в разделе дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="20e2f-221">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="20e2f-222">Адддефаултидентити и Аддидентити</span><span class="sxs-lookup"><span data-stu-id="20e2f-222">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="20e2f-223"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> было введено в ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="20e2f-223"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="20e2f-224">Вызов `AddDefaultIdentity` аналогичен вызову следующего:</span><span class="sxs-lookup"><span data-stu-id="20e2f-224">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="20e2f-225">Дополнительные сведения см. в разделе [адддефаултидентити Source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="20e2f-225">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="20e2f-226">Создание веб-приложения с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="20e2f-226">Create a Web app with authentication</span></span>

<span data-ttu-id="20e2f-227">Создание ASP.NET Core проекта веб-приложения с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="20e2f-227">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20e2f-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20e2f-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="20e2f-229">Выберите **File** > **New** > **Project** ( Файл > Создать > Проект).</span><span class="sxs-lookup"><span data-stu-id="20e2f-229">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="20e2f-230">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="20e2f-230">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="20e2f-231">Присвойте проекту имя **APP1** , которое будет иметь то же пространство имен, что и загружаемый проект.</span><span class="sxs-lookup"><span data-stu-id="20e2f-231">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="20e2f-232">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="20e2f-232">Click **OK**.</span></span>
* <span data-ttu-id="20e2f-233">Выберите ASP.NET Core **веб-приложение**, а затем щелкните **изменить проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="20e2f-233">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="20e2f-234">Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="20e2f-234">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20e2f-235">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="20e2f-235">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="20e2f-236">Созданный проект предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) в виде [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="20e2f-236">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="20e2f-237">Библиотека классов Razor Identity предоставляет конечные точки с областью `Identity`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-237">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="20e2f-238">Например:</span><span class="sxs-lookup"><span data-stu-id="20e2f-238">For example:</span></span>

* <span data-ttu-id="20e2f-239">/идентити/аккаунт/логин</span><span class="sxs-lookup"><span data-stu-id="20e2f-239">/Identity/Account/Login</span></span>
* <span data-ttu-id="20e2f-240">/идентити/аккаунт/логаут</span><span class="sxs-lookup"><span data-stu-id="20e2f-240">/Identity/Account/Logout</span></span>
* <span data-ttu-id="20e2f-241">/идентити/аккаунт/манаже</span><span class="sxs-lookup"><span data-stu-id="20e2f-241">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="20e2f-242">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="20e2f-242">Apply migrations</span></span>

<span data-ttu-id="20e2f-243">Примените миграции, чтобы инициализировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="20e2f-243">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20e2f-244">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20e2f-244">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20e2f-245">Выполните следующую команду в консоли диспетчера пакетов (PMC):</span><span class="sxs-lookup"><span data-stu-id="20e2f-245">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20e2f-246">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="20e2f-246">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="20e2f-247">Проверка регистра и вход в систему</span><span class="sxs-lookup"><span data-stu-id="20e2f-247">Test Register and Login</span></span>

<span data-ttu-id="20e2f-248">Запустите приложение и зарегистрируйте пользователя.</span><span class="sxs-lookup"><span data-stu-id="20e2f-248">Run the app and register a user.</span></span> <span data-ttu-id="20e2f-249">В зависимости от размера экрана может потребоваться выбрать переключатель навигации, чтобы просмотреть ссылки **Регистрация** и **Вход** .</span><span class="sxs-lookup"><span data-stu-id="20e2f-249">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="20e2f-250">Настройка служб удостоверений</span><span class="sxs-lookup"><span data-stu-id="20e2f-250">Configure Identity services</span></span>

<span data-ttu-id="20e2f-251">Службы добавляются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-251">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="20e2f-252">По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-252">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="20e2f-253">Приведенный выше код настраивает удостоверение с использованием значений параметров по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="20e2f-253">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="20e2f-254">Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="20e2f-254">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="20e2f-255">Удостоверение включается путем вызова [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="20e2f-255">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="20e2f-256">`UseAuthentication` добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="20e2f-256">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="20e2f-257">Дополнительные сведения см. в разделе [класс идентитйоптионс](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) и [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="20e2f-257">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="20e2f-258">Регистрация, вход и выход из шаблона формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="20e2f-258">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="20e2f-259">Соблюдайте [идентификатор шаблона в проекте Razor с](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) инструкциями по авторизации для создания кода, приведенного в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="20e2f-259">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20e2f-260">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20e2f-260">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20e2f-261">Добавьте файлы Register, login и LogOut.</span><span class="sxs-lookup"><span data-stu-id="20e2f-261">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="20e2f-262">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="20e2f-262">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="20e2f-263">Если вы создали проект с именем " **APP1**", выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="20e2f-263">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="20e2f-264">В противном случае используйте правильное пространство имен для `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-264">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="20e2f-265">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="20e2f-265">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="20e2f-266">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки, как показано в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="20e2f-266">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="20e2f-267">Проверка регистра</span><span class="sxs-lookup"><span data-stu-id="20e2f-267">Examine Register</span></span>

<span data-ttu-id="20e2f-268">Когда пользователь щелкает ссылку **Register** , вызывается действие `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-268">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="20e2f-269">Пользователь создается с помощью [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) на объекте `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-269">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="20e2f-270">`_userManager` предоставляется путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="20e2f-270">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="20e2f-271">Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-271">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="20e2f-272">**Примечание.** Раздел [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) описывает, как предотвратить автоматический вход пользователя при регистрации.</span><span class="sxs-lookup"><span data-stu-id="20e2f-272">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="20e2f-273">Вход</span><span class="sxs-lookup"><span data-stu-id="20e2f-273">Log in</span></span>

<span data-ttu-id="20e2f-274">Форма входа отображается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="20e2f-274">The Login form is displayed when:</span></span>

* <span data-ttu-id="20e2f-275">Выбрана ссылка для **входа** .</span><span class="sxs-lookup"><span data-stu-id="20e2f-275">The **Log in** link is selected.</span></span>
* <span data-ttu-id="20e2f-276">Пользователь пытается получить доступ к странице с ограниченным доступом, которая не авторизована для доступа, **или** если система не прошла проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="20e2f-276">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="20e2f-277">При отправке формы на странице входа вызывается действие `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-277">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="20e2f-278">`PasswordSignInAsync` вызывается для объекта `_signInManager` (предоставленного путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="20e2f-278">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="20e2f-279">Базовый класс `Controller` предоставляет свойство `User`, доступ к которому можно получить из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="20e2f-279">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="20e2f-280">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="20e2f-280">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="20e2f-281">Для получения дополнительной информации см. <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="20e2f-281">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="20e2f-282">Выход</span><span class="sxs-lookup"><span data-stu-id="20e2f-282">Log out</span></span>

<span data-ttu-id="20e2f-283">Ссылка **выхода** вызывает действие `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-283">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="20e2f-284">[Сигнаутасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) очищает утверждения пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="20e2f-284">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="20e2f-285">Параметр POST указан в *страницах Pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20e2f-285">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="20e2f-286">Проверить удостоверение</span><span class="sxs-lookup"><span data-stu-id="20e2f-286">Test Identity</span></span>

<span data-ttu-id="20e2f-287">Шаблоны веб-проектов по умолчанию разрешают анонимный доступ к домашним страницам.</span><span class="sxs-lookup"><span data-stu-id="20e2f-287">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="20e2f-288">Чтобы проверить удостоверение, добавьте [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) на страницу конфиденциальности.</span><span class="sxs-lookup"><span data-stu-id="20e2f-288">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="20e2f-289">Если вы вошли в систему, выйдите из нее. Запустите приложение и щелкните ссылку **Конфиденциальность** .</span><span class="sxs-lookup"><span data-stu-id="20e2f-289">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="20e2f-290">Вы перейдете на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="20e2f-290">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="20e2f-291">Просмотр удостоверений</span><span class="sxs-lookup"><span data-stu-id="20e2f-291">Explore Identity</span></span>

<span data-ttu-id="20e2f-292">Более подробное изучение удостоверений:</span><span class="sxs-lookup"><span data-stu-id="20e2f-292">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="20e2f-293">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="20e2f-293">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="20e2f-294">Изучите источник каждой страницы и пошаговое выполнение отладчика.</span><span class="sxs-lookup"><span data-stu-id="20e2f-294">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="20e2f-295">Компоненты идентификации</span><span class="sxs-lookup"><span data-stu-id="20e2f-295">Identity Components</span></span>

<span data-ttu-id="20e2f-296">Все пакеты NuGet, зависящие от удостоверения, включены в пакет [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="20e2f-296">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="20e2f-297">Основным пакетом для удостоверения является [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="20e2f-297">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="20e2f-298">Этот пакет содержит основной набор интерфейсов для ASP.NET Core удостоверения и включается `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="20e2f-298">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="20e2f-299">Миграция на ASP.NET Core удостоверение</span><span class="sxs-lookup"><span data-stu-id="20e2f-299">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="20e2f-300">Дополнительные сведения и рекомендации по переносу существующего хранилища удостоверений см. в статье [Миграция проверки подлинности и удостоверений](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="20e2f-300">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="20e2f-301">Установка силы пароля</span><span class="sxs-lookup"><span data-stu-id="20e2f-301">Setting password strength</span></span>

<span data-ttu-id="20e2f-302">См. раздел [Конфигурация](#pw) для примера, который устанавливает минимальные требования к паролю.</span><span class="sxs-lookup"><span data-stu-id="20e2f-302">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="20e2f-303">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="20e2f-303">Next Steps</span></span>

* [<span data-ttu-id="20e2f-304">Настройка Identity</span><span class="sxs-lookup"><span data-stu-id="20e2f-304">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
