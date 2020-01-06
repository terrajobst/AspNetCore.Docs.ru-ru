---
title: Введение в удостоверение на ASP.NET Core
author: rick-anderson
description: Используйте удостоверение с приложением ASP.NET Core. Узнайте, как устанавливать требования к паролю (Рекуиредигит, Рекуиредленгс, Рекуиредуникуечарс и др.).
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/identity
ms.openlocfilehash: 787d39dd7824f912128e6af849fa268c3e8eb908
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75359203"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="2a0f4-104">Введение в удостоверение на ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a0f4-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2a0f4-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="2a0f4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2a0f4-106">Удостоверение ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="2a0f4-107">— Это API, поддерживающий функцию входа в пользовательский интерфейс.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="2a0f4-108">Управляет пользователями, паролями, данными профилирования, ролями, утверждениями, маркерами, подтверждением электронной почты и т. д.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="2a0f4-109">Пользователи могут создать учетную запись с данными для входа, хранящимися в удостоверении, или использовать внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="2a0f4-110">Поддерживаемые внешние поставщики входа включают [Facebook, Google, учетную запись Майкрософт и Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="2a0f4-111">[Исходный код удостоверения](https://github.com/aspnet/AspNetCore/tree/master/src/Identity) доступен на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-111">The [Identity source code](https://github.com/aspnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="2a0f4-112">[Удостоверение формирования шаблонов](xref:security/authentication/scaffold-identity) и просмотр созданных файлов для проверки взаимодействия шаблона с удостоверением.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

<span data-ttu-id="2a0f4-113">Удостоверение обычно настраивается с помощью SQL Server базы данных для хранения имен пользователей, паролей и данных профилей.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-113">Identity is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="2a0f4-114">Кроме того, можно использовать еще одно постоянное хранилище, например хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="2a0f4-115">В этом разделе вы узнаете, как использовать удостоверение для регистрации, входа и выхода пользователя.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="2a0f4-116">Более подробные инструкции по созданию приложений, использующих удостоверение, см. в разделе дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-116">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="2a0f4-117">[Платформа Microsoft Identity Platform](/azure/active-directory/develop/) :</span><span class="sxs-lookup"><span data-stu-id="2a0f4-117">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="2a0f4-118">Развитие платформы разработки Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-118">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="2a0f4-119">Не связано с удостоверением ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-119">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="2a0f4-120">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([Загрузка)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-120">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="2a0f4-121">Создание веб-приложения с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="2a0f4-121">Create a Web app with authentication</span></span>

<span data-ttu-id="2a0f4-122">Создание ASP.NET Core проекта веб-приложения с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2a0f4-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a0f4-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2a0f4-124">Выберите **файл** > **Новый** > **проект**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="2a0f4-125">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2a0f4-126">Присвойте проекту имя **APP1** , которое будет иметь то же пространство имен, что и загружаемый проект.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="2a0f4-127">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-127">Click **OK**.</span></span>
* <span data-ttu-id="2a0f4-128">Выберите ASP.NET Core **веб-приложение**, а затем щелкните **изменить проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="2a0f4-129">Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2a0f4-130">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a0f4-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="2a0f4-131">Предыдущая команда создает веб-приложение Razor с помощью SQLite.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-131">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="2a0f4-132">Чтобы создать веб-приложение с помощью LocalDB, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-132">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="2a0f4-133">Созданный проект предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) в виде [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-133">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="2a0f4-134">Библиотека классов Razor Identity предоставляет конечные точки с областью `Identity`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-134">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="2a0f4-135">Например:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-135">For example:</span></span>

* <span data-ttu-id="2a0f4-136">/идентити/аккаунт/логин</span><span class="sxs-lookup"><span data-stu-id="2a0f4-136">/Identity/Account/Login</span></span>
* <span data-ttu-id="2a0f4-137">/идентити/аккаунт/логаут</span><span class="sxs-lookup"><span data-stu-id="2a0f4-137">/Identity/Account/Logout</span></span>
* <span data-ttu-id="2a0f4-138">/идентити/аккаунт/манаже</span><span class="sxs-lookup"><span data-stu-id="2a0f4-138">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="2a0f4-139">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="2a0f4-139">Apply migrations</span></span>

<span data-ttu-id="2a0f4-140">Примените миграции, чтобы инициализировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-140">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2a0f4-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a0f4-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2a0f4-142">Выполните следующую команду в консоли диспетчера пакетов (PMC):</span><span class="sxs-lookup"><span data-stu-id="2a0f4-142">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2a0f4-143">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a0f4-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2a0f4-144">Миграция не требуется на этом этапе при использовании SQLite.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-144">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="2a0f4-145">Для LocalDB выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-145">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="2a0f4-146">Проверка регистра и вход в систему</span><span class="sxs-lookup"><span data-stu-id="2a0f4-146">Test Register and Login</span></span>

<span data-ttu-id="2a0f4-147">Запустите приложение и зарегистрируйте пользователя.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-147">Run the app and register a user.</span></span> <span data-ttu-id="2a0f4-148">В зависимости от размера экрана может потребоваться выбрать переключатель навигации, чтобы просмотреть ссылки **Регистрация** и **Вход** .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-148">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="2a0f4-149">Настройка служб удостоверений</span><span class="sxs-lookup"><span data-stu-id="2a0f4-149">Configure Identity services</span></span>

<span data-ttu-id="2a0f4-150">Службы добавляются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-150">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="2a0f4-151">По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-151">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="2a0f4-152">Выделенный выше код настраивает удостоверение значениями параметров по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-152">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="2a0f4-153">Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-153">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="2a0f4-154">Удостоверение включается путем вызова <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-154">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="2a0f4-155">`UseAuthentication` добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-155">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="2a0f4-156">Созданное шаблоном приложение не использует [авторизацию](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-156">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="2a0f4-157">`app.UseAuthorization` включен, чтобы убедиться, что он добавлен в правильном порядке, если приложение добавляет авторизацию.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-157">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="2a0f4-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`и `UseEndpoints` должны вызываться в порядке, показанном в предыдущем коде.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="2a0f4-159">Дополнительные сведения о `IdentityOptions` и `Startup`см. в разделе <xref:Microsoft.AspNetCore.Identity.IdentityOptions> и [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-159">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="2a0f4-160">Регистрация, вход и выход из шаблона формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="2a0f4-160">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2a0f4-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a0f4-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2a0f4-162">Добавьте файлы Register, login и LogOut.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-162">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="2a0f4-163">Соблюдайте [идентификатор шаблона в проекте Razor с](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) инструкциями по авторизации для создания кода, приведенного в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-163">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2a0f4-164">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a0f4-164">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2a0f4-165">Если вы создали проект с именем " **APP1**", выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-165">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="2a0f4-166">В противном случае используйте правильное пространство имен для `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-166">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="2a0f4-167">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-167">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="2a0f4-168">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки, как показано в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-168">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="2a0f4-169">Дополнительные сведения об удостоверении формирования шаблонов см. в разделе [удостоверение шаблона в проекте Razor с авторизацией](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-169">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="2a0f4-170">Проверка регистра</span><span class="sxs-lookup"><span data-stu-id="2a0f4-170">Examine Register</span></span>

<span data-ttu-id="2a0f4-171">Когда пользователь щелкает ссылку **Register** , вызывается действие `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-171">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="2a0f4-172">Пользователь создается с помощью [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) на объекте `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-172">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="2a0f4-173">`_userManager` предоставляется путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="2a0f4-173">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="2a0f4-174">Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="2a0f4-175">Инструкции по предотвращению немедленного входа при регистрации см. в статье [Подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="2a0f4-176">Вход</span><span class="sxs-lookup"><span data-stu-id="2a0f4-176">Log in</span></span>

<span data-ttu-id="2a0f4-177">Форма входа отображается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="2a0f4-178">Выбрана ссылка для **входа** .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="2a0f4-179">Пользователь пытается получить доступ к странице с ограниченным доступом, которая не авторизована для доступа, **или** если система не прошла проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="2a0f4-180">При отправке формы на странице входа вызывается действие `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="2a0f4-181">`PasswordSignInAsync` вызывается для объекта `_signInManager` (предоставленного путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="2a0f4-182">Базовый класс `Controller` предоставляет свойство `User`, доступ к которому можно получить из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-182">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="2a0f4-183">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="2a0f4-184">Для получения дополнительной информации см. <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-184">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="2a0f4-185">Выход</span><span class="sxs-lookup"><span data-stu-id="2a0f4-185">Log out</span></span>

<span data-ttu-id="2a0f4-186">Ссылка **выхода** вызывает действие `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="2a0f4-187">В приведенном выше коде код `return RedirectToPage();` должен быть перенаправлением, чтобы браузер выполнит новый запрос и удостоверение для пользователя будет обновлено.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-187">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="2a0f4-188">[Сигнаутасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) очищает утверждения пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="2a0f4-189">Параметр POST указан в *страницах Pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="2a0f4-190">Проверить удостоверение</span><span class="sxs-lookup"><span data-stu-id="2a0f4-190">Test Identity</span></span>

<span data-ttu-id="2a0f4-191">Шаблоны веб-проектов по умолчанию разрешают анонимный доступ к домашним страницам.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="2a0f4-192">Чтобы проверить удостоверение, добавьте [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="2a0f4-192">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="2a0f4-193">Если вы вошли в систему, выйдите из нее. Запустите приложение и щелкните ссылку **Конфиденциальность** .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-193">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="2a0f4-194">Вы перейдете на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-194">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="2a0f4-195">Просмотр удостоверений</span><span class="sxs-lookup"><span data-stu-id="2a0f4-195">Explore Identity</span></span>

<span data-ttu-id="2a0f4-196">Более подробное изучение удостоверений:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="2a0f4-197">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="2a0f4-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="2a0f4-198">Изучите источник каждой страницы и пошаговое выполнение отладчика.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-198">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="2a0f4-199">Компоненты идентификации</span><span class="sxs-lookup"><span data-stu-id="2a0f4-199">Identity Components</span></span>

<span data-ttu-id="2a0f4-200">Все пакеты NuGet, зависящие от удостоверения, включены в [ASP.NET Core общую платформу](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-200">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="2a0f4-201">Основным пакетом для удостоверения является [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="2a0f4-202">Этот пакет содержит основной набор интерфейсов для ASP.NET Core удостоверения и включается `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="2a0f4-203">Миграция на ASP.NET Core удостоверение</span><span class="sxs-lookup"><span data-stu-id="2a0f4-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="2a0f4-204">Дополнительные сведения и рекомендации по переносу существующего хранилища удостоверений см. в статье [Миграция проверки подлинности и удостоверений](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="2a0f4-205">Установка силы пароля</span><span class="sxs-lookup"><span data-stu-id="2a0f4-205">Setting password strength</span></span>

<span data-ttu-id="2a0f4-206">См. раздел [Конфигурация](#pw) для примера, который устанавливает минимальные требования к паролю.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="2a0f4-207">Адддефаултидентити и Аддидентити</span><span class="sxs-lookup"><span data-stu-id="2a0f4-207">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="2a0f4-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> было введено в ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="2a0f4-209">Вызов `AddDefaultIdentity` аналогичен вызову следующего:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-209">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="2a0f4-210">Дополнительные сведения см. в разделе [адддефаултидентити Source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-210">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a0f4-211">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="2a0f4-211">Next Steps</span></span>

* <span data-ttu-id="2a0f4-212">Сведения о настройке удостоверений с помощью SQLite см. в [этой статье о проблемах GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/5131) .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-212">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* [<span data-ttu-id="2a0f4-213">Настройка Identity</span><span class="sxs-lookup"><span data-stu-id="2a0f4-213">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2a0f4-214">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="2a0f4-214">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2a0f4-215">ASP.NET Core удостоверение — это система членства, которая добавляет функции входа в ASP.NET Core приложения.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-215">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="2a0f4-216">Пользователи могут создать учетную запись с данными для входа, хранящимися в удостоверении, или использовать внешнего поставщика входа.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-216">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="2a0f4-217">Поддерживаемые внешние поставщики входа включают [Facebook, Google, учетную запись Майкрософт и Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-217">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="2a0f4-218">Идентификацию можно настроить с помощью SQL Server базы данных для хранения имен пользователей, паролей и данных профилей.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-218">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="2a0f4-219">Кроме того, можно использовать еще одно постоянное хранилище, например хранилище таблиц Azure.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-219">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="2a0f4-220">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([Загрузка)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-220">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="2a0f4-221">В этом разделе вы узнаете, как использовать удостоверение для регистрации, входа и выхода пользователя.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-221">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="2a0f4-222">Более подробные инструкции по созданию приложений, использующих удостоверение, см. в разделе дальнейшие действия в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-222">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="2a0f4-223">Адддефаултидентити и Аддидентити</span><span class="sxs-lookup"><span data-stu-id="2a0f4-223">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="2a0f4-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> было введено в ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="2a0f4-225">Вызов `AddDefaultIdentity` аналогичен вызову следующего:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-225">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="2a0f4-226">Дополнительные сведения см. в разделе [адддефаултидентити Source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-226">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="2a0f4-227">Создание веб-приложения с проверкой подлинности</span><span class="sxs-lookup"><span data-stu-id="2a0f4-227">Create a Web app with authentication</span></span>

<span data-ttu-id="2a0f4-228">Создание ASP.NET Core проекта веб-приложения с учетными записями отдельных пользователей.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-228">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2a0f4-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a0f4-229">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2a0f4-230">Выберите **файл** > **Новый** > **проект**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-230">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="2a0f4-231">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-231">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2a0f4-232">Присвойте проекту имя **APP1** , которое будет иметь то же пространство имен, что и загружаемый проект.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-232">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="2a0f4-233">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-233">Click **OK**.</span></span>
* <span data-ttu-id="2a0f4-234">Выберите ASP.NET Core **веб-приложение**, а затем щелкните **изменить проверку подлинности**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-234">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="2a0f4-235">Выберите **учетные записи отдельных пользователей** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-235">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2a0f4-236">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a0f4-236">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="2a0f4-237">Созданный проект предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) в виде [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-237">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="2a0f4-238">Библиотека классов Razor Identity предоставляет конечные точки с областью `Identity`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-238">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="2a0f4-239">Например:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-239">For example:</span></span>

* <span data-ttu-id="2a0f4-240">/идентити/аккаунт/логин</span><span class="sxs-lookup"><span data-stu-id="2a0f4-240">/Identity/Account/Login</span></span>
* <span data-ttu-id="2a0f4-241">/идентити/аккаунт/логаут</span><span class="sxs-lookup"><span data-stu-id="2a0f4-241">/Identity/Account/Logout</span></span>
* <span data-ttu-id="2a0f4-242">/идентити/аккаунт/манаже</span><span class="sxs-lookup"><span data-stu-id="2a0f4-242">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="2a0f4-243">Применение миграции</span><span class="sxs-lookup"><span data-stu-id="2a0f4-243">Apply migrations</span></span>

<span data-ttu-id="2a0f4-244">Примените миграции, чтобы инициализировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-244">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2a0f4-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a0f4-245">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2a0f4-246">Выполните следующую команду в консоли диспетчера пакетов (PMC):</span><span class="sxs-lookup"><span data-stu-id="2a0f4-246">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2a0f4-247">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a0f4-247">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="2a0f4-248">Проверка регистра и вход в систему</span><span class="sxs-lookup"><span data-stu-id="2a0f4-248">Test Register and Login</span></span>

<span data-ttu-id="2a0f4-249">Запустите приложение и зарегистрируйте пользователя.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-249">Run the app and register a user.</span></span> <span data-ttu-id="2a0f4-250">В зависимости от размера экрана может потребоваться выбрать переключатель навигации, чтобы просмотреть ссылки **Регистрация** и **Вход** .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-250">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="2a0f4-251">Настройка служб удостоверений</span><span class="sxs-lookup"><span data-stu-id="2a0f4-251">Configure Identity services</span></span>

<span data-ttu-id="2a0f4-252">Службы добавляются в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-252">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="2a0f4-253">По стандартному шаблону сначала вызываются все методы `Add{Service}`, а затем все методы `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-253">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="2a0f4-254">Приведенный выше код настраивает удостоверение с использованием значений параметров по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-254">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="2a0f4-255">Доступ к службам предоставляется приложению посредством [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-255">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="2a0f4-256">Удостоверение включается путем вызова [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-256">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="2a0f4-257">`UseAuthentication` добавляет по [промежуточного слоя](xref:fundamentals/middleware/index) проверки подлинности в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-257">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="2a0f4-258">Дополнительные сведения см. в разделе [класс идентитйоптионс](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) и [Запуск приложения](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-258">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="2a0f4-259">Регистрация, вход и выход из шаблона формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="2a0f4-259">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="2a0f4-260">Соблюдайте [идентификатор шаблона в проекте Razor с](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) инструкциями по авторизации для создания кода, приведенного в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-260">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2a0f4-261">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2a0f4-261">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2a0f4-262">Добавьте файлы Register, login и LogOut.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-262">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2a0f4-263">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="2a0f4-263">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2a0f4-264">Если вы создали проект с именем " **APP1**", выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-264">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="2a0f4-265">В противном случае используйте правильное пространство имен для `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-265">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="2a0f4-266">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-266">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="2a0f4-267">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки, как показано в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-267">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="2a0f4-268">Проверка регистра</span><span class="sxs-lookup"><span data-stu-id="2a0f4-268">Examine Register</span></span>

<span data-ttu-id="2a0f4-269">Когда пользователь щелкает ссылку **Register** , вызывается действие `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-269">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="2a0f4-270">Пользователь создается с помощью [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) на объекте `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-270">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="2a0f4-271">`_userManager` предоставляется путем внедрения зависимостей):</span><span class="sxs-lookup"><span data-stu-id="2a0f4-271">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="2a0f4-272">Если пользователь создан, происходит вход пользователя путем вызова метода `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-272">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="2a0f4-273">**Примечание.** Раздел [подтверждение учетной записи](xref:security/authentication/accconfirm#prevent-login-at-registration) описывает, как предотвратить автоматический вход пользователя при регистрации.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-273">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="2a0f4-274">Вход</span><span class="sxs-lookup"><span data-stu-id="2a0f4-274">Log in</span></span>

<span data-ttu-id="2a0f4-275">Форма входа отображается в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-275">The Login form is displayed when:</span></span>

* <span data-ttu-id="2a0f4-276">Выбрана ссылка для **входа** .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-276">The **Log in** link is selected.</span></span>
* <span data-ttu-id="2a0f4-277">Пользователь пытается получить доступ к странице с ограниченным доступом, которая не авторизована для доступа, **или** если система не прошла проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-277">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="2a0f4-278">При отправке формы на странице входа вызывается действие `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-278">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="2a0f4-279">`PasswordSignInAsync` вызывается для объекта `_signInManager` (предоставленного путем внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-279">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="2a0f4-280">Базовый класс `Controller` предоставляет свойство `User`, доступ к которому можно получить из методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-280">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="2a0f4-281">Например, можно перечислить `User.Claims` и принимать решения об авторизации.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-281">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="2a0f4-282">Для получения дополнительной информации см. <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-282">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="2a0f4-283">Выход</span><span class="sxs-lookup"><span data-stu-id="2a0f4-283">Log out</span></span>

<span data-ttu-id="2a0f4-284">Ссылка **выхода** вызывает действие `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-284">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="2a0f4-285">[Сигнаутасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) очищает утверждения пользователя, хранящиеся в файле cookie.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-285">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="2a0f4-286">Параметр POST указан в *страницах Pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-286">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="2a0f4-287">Проверить удостоверение</span><span class="sxs-lookup"><span data-stu-id="2a0f4-287">Test Identity</span></span>

<span data-ttu-id="2a0f4-288">Шаблоны веб-проектов по умолчанию разрешают анонимный доступ к домашним страницам.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-288">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="2a0f4-289">Чтобы проверить удостоверение, добавьте [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) на страницу конфиденциальности.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-289">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="2a0f4-290">Если вы вошли в систему, выйдите из нее. Запустите приложение и щелкните ссылку **Конфиденциальность** .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-290">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="2a0f4-291">Вы перейдете на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-291">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="2a0f4-292">Просмотр удостоверений</span><span class="sxs-lookup"><span data-stu-id="2a0f4-292">Explore Identity</span></span>

<span data-ttu-id="2a0f4-293">Более подробное изучение удостоверений:</span><span class="sxs-lookup"><span data-stu-id="2a0f4-293">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="2a0f4-294">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="2a0f4-294">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="2a0f4-295">Изучите источник каждой страницы и пошаговое выполнение отладчика.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-295">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="2a0f4-296">Компоненты идентификации</span><span class="sxs-lookup"><span data-stu-id="2a0f4-296">Identity Components</span></span>

<span data-ttu-id="2a0f4-297">Все пакеты NuGet, зависящие от удостоверения, включены в пакет [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-297">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="2a0f4-298">Основным пакетом для удостоверения является [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-298">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="2a0f4-299">Этот пакет содержит основной набор интерфейсов для ASP.NET Core удостоверения и включается `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-299">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="2a0f4-300">Миграция на ASP.NET Core удостоверение</span><span class="sxs-lookup"><span data-stu-id="2a0f4-300">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="2a0f4-301">Дополнительные сведения и рекомендации по переносу существующего хранилища удостоверений см. в статье [Миграция проверки подлинности и удостоверений](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="2a0f4-301">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="2a0f4-302">Установка силы пароля</span><span class="sxs-lookup"><span data-stu-id="2a0f4-302">Setting password strength</span></span>

<span data-ttu-id="2a0f4-303">См. раздел [Конфигурация](#pw) для примера, который устанавливает минимальные требования к паролю.</span><span class="sxs-lookup"><span data-stu-id="2a0f4-303">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a0f4-304">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="2a0f4-304">Next Steps</span></span>

* <span data-ttu-id="2a0f4-305">Сведения о настройке удостоверений с помощью SQLite см. в [этой статье о проблемах GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/5131) .</span><span class="sxs-lookup"><span data-stu-id="2a0f4-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* [<span data-ttu-id="2a0f4-306">Настройка Identity</span><span class="sxs-lookup"><span data-stu-id="2a0f4-306">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
