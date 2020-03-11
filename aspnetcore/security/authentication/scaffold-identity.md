---
title: Удостоверение шаблона в ASP.NET Core проектах
author: rick-anderson
description: Узнайте, как сформировать удостоверение в проекте ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: security/authentication/scaffold-identity
ms.openlocfilehash: b3e077aeac11e62d9e992884100476f7be35b59a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653716"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="6ca74-103">Удостоверение шаблона в ASP.NET Core проектах</span><span class="sxs-lookup"><span data-stu-id="6ca74-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="6ca74-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6ca74-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6ca74-105">ASP.NET Core предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) как [библиотеку классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="6ca74-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="6ca74-106">Приложения, включающие удостоверение, могут применять шаблон для выборочного добавления исходного кода, содержащегося в библиотеке классов Razor класса Identity (РКЛ).</span><span class="sxs-lookup"><span data-stu-id="6ca74-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="6ca74-107">Вы можете создать исходный код, чтобы изменить код и тем самым изменить поведение.</span><span class="sxs-lookup"><span data-stu-id="6ca74-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="6ca74-108">Например, вы можете указать шаблону создать код, используемый при регистрации.</span><span class="sxs-lookup"><span data-stu-id="6ca74-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="6ca74-109">Созданный код имеет приоритет над тем же кодом в RCL для идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="6ca74-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="6ca74-110">Чтобы получить полный контроль над пользовательским интерфейсом и не использовать РКЛ по умолчанию, см. раздел [Создание полного идентификатора пользовательского интерфейса идентификации](#full).</span><span class="sxs-lookup"><span data-stu-id="6ca74-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="6ca74-111">Приложения, которые **не** включают проверку подлинности, могут применять механизм формирования шаблонов для добавления пакета РКЛ Identity.</span><span class="sxs-lookup"><span data-stu-id="6ca74-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="6ca74-112">Вы можете выбрать, какой код идентификатора будет создан.</span><span class="sxs-lookup"><span data-stu-id="6ca74-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="6ca74-113">Хотя этот механизм создает большую часть необходимого кода, необходимо обновить проект, чтобы завершить процесс.</span><span class="sxs-lookup"><span data-stu-id="6ca74-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="6ca74-114">В этом документе объясняются шаги, необходимые для завершения обновления формирования шаблонов удостоверений.</span><span class="sxs-lookup"><span data-stu-id="6ca74-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="6ca74-115">Рекомендуется использовать систему управления версиями, которая показывает различия в файлах и позволяет вносить изменения.</span><span class="sxs-lookup"><span data-stu-id="6ca74-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="6ca74-116">Проверьте изменения после запуска шаблона удостоверений.</span><span class="sxs-lookup"><span data-stu-id="6ca74-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="6ca74-117">Службы необходимы при использовании [двухфакторной проверки подлинности](xref:security/authentication/identity-enable-qrcodes), [подтверждения учетной записи и восстановления пароля](xref:security/authentication/accconfirm), а также других функций безопасности с удостоверениями.</span><span class="sxs-lookup"><span data-stu-id="6ca74-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="6ca74-118">Службы или заглушки служб не создаются при формировании удостоверений формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="6ca74-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="6ca74-119">Службы для включения этих функций необходимо добавить вручную.</span><span class="sxs-lookup"><span data-stu-id="6ca74-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="6ca74-120">Например, см. статью [требование подтверждения по электронной почте](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="6ca74-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="6ca74-121">Этот документ содержит более полные инструкции, чем файл *скаффолдингреадме. txt* , который создается при запуске механизма формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="6ca74-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="6ca74-122">Удостоверение шаблона в пустой проект</span><span class="sxs-lookup"><span data-stu-id="6ca74-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="6ca74-123">Обновите класс `Startup` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="6ca74-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="6ca74-124">Удостоверение шаблона в проекте Razor без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="6ca74-124">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef database drop
dotnet ef migrations add CreateIdentitySchema0
dotnet ef database update
-->

<!-- ERROR
There is already an object named 'AspNetRoles' in the database.

Fixed via dotnet ef database drop
before dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="6ca74-125">Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="6ca74-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="6ca74-126">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="6ca74-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="6ca74-127">Миграция, Усеаусентикатион и макет</span><span class="sxs-lookup"><span data-stu-id="6ca74-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="6ca74-128">Включить проверку подлинности</span><span class="sxs-lookup"><span data-stu-id="6ca74-128">Enable authentication</span></span>

<span data-ttu-id="6ca74-129">Обновите класс `Startup` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="6ca74-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="6ca74-130">Изменения макета</span><span class="sxs-lookup"><span data-stu-id="6ca74-130">Layout changes</span></span>

<span data-ttu-id="6ca74-131">Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл макета:</span><span class="sxs-lookup"><span data-stu-id="6ca74-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="6ca74-132">Удостоверение шаблона в проекте Razor с авторизацией</span><span class="sxs-lookup"><span data-stu-id="6ca74-132">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="6ca74-133">Некоторые параметры удостоверения настраиваются в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="6ca74-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="6ca74-134">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="6ca74-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="6ca74-135">Удостоверение шаблона в проекте MVC без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="6ca74-135">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="6ca74-136">Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл *views/Shared/_layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6ca74-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="6ca74-137">Переместить файл *pages/Shared/_LoginPartial. cshtml* в *views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="6ca74-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="6ca74-138">Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="6ca74-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="6ca74-139">Дополнительные сведения см. в разделе IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="6ca74-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="6ca74-140">Обновите класс `Startup` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="6ca74-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="6ca74-141">Удостоверение шаблона в проекте MVC с авторизацией</span><span class="sxs-lookup"><span data-stu-id="6ca74-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="6ca74-142">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="6ca74-142">Create full identity UI source</span></span>

<span data-ttu-id="6ca74-143">Чтобы обеспечить полный контроль над пользовательским интерфейсом удостоверений, запустите механизм формирования удостоверений и выберите **переопределить все файлы**.</span><span class="sxs-lookup"><span data-stu-id="6ca74-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="6ca74-144">В следующем выделенном коде показаны изменения, которые заменяют пользовательский интерфейс удостоверения по умолчанию удостоверением в веб-приложении ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="6ca74-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="6ca74-145">Это может потребоваться для полного управления пользовательским интерфейсом идентификации.</span><span class="sxs-lookup"><span data-stu-id="6ca74-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="6ca74-146">Удостоверение по умолчанию заменяется в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="6ca74-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="6ca74-147">Следующий код задает [логинпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [логаутпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)и [акцессдениедпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="6ca74-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="6ca74-148">Зарегистрируйте реализацию `IEmailSender`, например:</span><span class="sxs-lookup"><span data-stu-id="6ca74-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="6ca74-149">Отключить страницу регистрации</span><span class="sxs-lookup"><span data-stu-id="6ca74-149">Disable register page</span></span>

<span data-ttu-id="6ca74-150">Чтобы отключить регистрацию пользователей, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="6ca74-150">To disable user registration:</span></span>

* <span data-ttu-id="6ca74-151">Удостоверение шаблона.</span><span class="sxs-lookup"><span data-stu-id="6ca74-151">Scaffold Identity.</span></span> <span data-ttu-id="6ca74-152">Включить учетную запись. регистрация, учетная запись. Login и Account. Регистерконфирматион.</span><span class="sxs-lookup"><span data-stu-id="6ca74-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="6ca74-153">Пример:</span><span class="sxs-lookup"><span data-stu-id="6ca74-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="6ca74-154">Обновите *области/идентификаторы/страницы/учетная запись/Register. cshtml. CS* , чтобы пользователи не могли зарегистрироваться из этой конечной точки:</span><span class="sxs-lookup"><span data-stu-id="6ca74-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="6ca74-155">Обновите *области, идентификаторы, страницы, учетную запись или Register. cshtml* , чтобы они были совместимы с предыдущими изменениями:</span><span class="sxs-lookup"><span data-stu-id="6ca74-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="6ca74-156">Закомментируйте или удалите ссылку регистрации из *областей, удостоверений, страниц, учетной записи или имени входа. cshtml*</span><span class="sxs-lookup"><span data-stu-id="6ca74-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="6ca74-157">Обновите страницу *области, удостоверение, страницы/учетная запись или регистерконфирматион* .</span><span class="sxs-lookup"><span data-stu-id="6ca74-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="6ca74-158">Удалите код и ссылки из файла CSHTML.</span><span class="sxs-lookup"><span data-stu-id="6ca74-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="6ca74-159">Удалите код подтверждения из `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="6ca74-159">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="6ca74-160">Использование другого приложения для добавления пользователей</span><span class="sxs-lookup"><span data-stu-id="6ca74-160">Use another app to add users</span></span>

<span data-ttu-id="6ca74-161">Предоставьте механизм для добавления пользователей за пределами веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6ca74-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="6ca74-162">Ниже перечислены параметры для добавления пользователей.</span><span class="sxs-lookup"><span data-stu-id="6ca74-162">Options to add users include:</span></span>

* <span data-ttu-id="6ca74-163">Выделенное административное веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="6ca74-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="6ca74-164">Консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="6ca74-164">A console app.</span></span>

<span data-ttu-id="6ca74-165">В следующем примере кода один из подходов к добавлению пользователей:</span><span class="sxs-lookup"><span data-stu-id="6ca74-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="6ca74-166">Список пользователей считывается в память.</span><span class="sxs-lookup"><span data-stu-id="6ca74-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="6ca74-167">Для каждого пользователя создается надежный уникальный пароль.</span><span class="sxs-lookup"><span data-stu-id="6ca74-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="6ca74-168">Пользователь будет добавлен в базу данных удостоверений.</span><span class="sxs-lookup"><span data-stu-id="6ca74-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="6ca74-169">Пользователь получает уведомления и сообщил об изменении пароля.</span><span class="sxs-lookup"><span data-stu-id="6ca74-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="6ca74-170">В следующем коде показано, как добавить пользователя:</span><span class="sxs-lookup"><span data-stu-id="6ca74-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="6ca74-171">Подобный подход можно выполнить для рабочих сценариев.</span><span class="sxs-lookup"><span data-stu-id="6ca74-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="6ca74-172">Запретить публикацию ресурсов статических удостоверений</span><span class="sxs-lookup"><span data-stu-id="6ca74-172">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="6ca74-173">Сведения о запрете публикации ресурсов статических удостоверений в корневом веб-каталоге см. в разделе <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="6ca74-173">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ca74-174">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6ca74-174">Additional resources</span></span>

* [<span data-ttu-id="6ca74-175">Изменения кода проверки подлинности на ASP.NET Core 2,1 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="6ca74-175">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6ca74-176">ASP.NET Core 2,1 и более поздних версий предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) как [библиотеку классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="6ca74-176">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="6ca74-177">Приложения, включающие удостоверение, могут применять шаблон для выборочного добавления исходного кода, содержащегося в библиотеке классов Razor класса Identity (РКЛ).</span><span class="sxs-lookup"><span data-stu-id="6ca74-177">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="6ca74-178">Вы можете создать исходный код, чтобы изменить код и тем самым изменить поведение.</span><span class="sxs-lookup"><span data-stu-id="6ca74-178">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="6ca74-179">Например, вы можете указать шаблону создать код, используемый при регистрации.</span><span class="sxs-lookup"><span data-stu-id="6ca74-179">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="6ca74-180">Созданный код имеет приоритет над тем же кодом в RCL для идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="6ca74-180">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="6ca74-181">Чтобы получить полный контроль над пользовательским интерфейсом и не использовать РКЛ по умолчанию, см. раздел [Создание полного идентификатора пользовательского интерфейса идентификации](#full).</span><span class="sxs-lookup"><span data-stu-id="6ca74-181">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="6ca74-182">Приложения, которые **не** включают проверку подлинности, могут применять механизм формирования шаблонов для добавления пакета РКЛ Identity.</span><span class="sxs-lookup"><span data-stu-id="6ca74-182">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="6ca74-183">Вы можете выбрать, какой код идентификатора будет создан.</span><span class="sxs-lookup"><span data-stu-id="6ca74-183">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="6ca74-184">Хотя этот механизм создает большую часть необходимого кода, необходимо обновить проект, чтобы завершить процесс.</span><span class="sxs-lookup"><span data-stu-id="6ca74-184">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="6ca74-185">В этом документе объясняются шаги, необходимые для завершения обновления формирования шаблонов удостоверений.</span><span class="sxs-lookup"><span data-stu-id="6ca74-185">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="6ca74-186">При запуске механизма формирования идентификаторов в каталоге проекта создается файл *скаффолдингреадме. txt* .</span><span class="sxs-lookup"><span data-stu-id="6ca74-186">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="6ca74-187">Файл *скаффолдингреадме. txt* содержит общие инструкции о том, что необходимо для завершения обновления формирования шаблонов удостоверений.</span><span class="sxs-lookup"><span data-stu-id="6ca74-187">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="6ca74-188">Этот документ содержит более полные инструкции, чем файл *скаффолдингреадме. txt* .</span><span class="sxs-lookup"><span data-stu-id="6ca74-188">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="6ca74-189">Рекомендуется использовать систему управления версиями, которая показывает различия в файлах и позволяет вносить изменения.</span><span class="sxs-lookup"><span data-stu-id="6ca74-189">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="6ca74-190">Проверьте изменения после запуска шаблона удостоверений.</span><span class="sxs-lookup"><span data-stu-id="6ca74-190">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="6ca74-191">Службы необходимы при использовании [двухфакторной проверки подлинности](xref:security/authentication/identity-enable-qrcodes), [подтверждения учетной записи и восстановления пароля](xref:security/authentication/accconfirm), а также других функций безопасности с удостоверениями.</span><span class="sxs-lookup"><span data-stu-id="6ca74-191">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="6ca74-192">Службы или заглушки служб не создаются при формировании удостоверений формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="6ca74-192">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="6ca74-193">Службы для включения этих функций необходимо добавить вручную.</span><span class="sxs-lookup"><span data-stu-id="6ca74-193">Services to enable these features must be added manually.</span></span> <span data-ttu-id="6ca74-194">Например, см. статью [требование подтверждения по электронной почте](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="6ca74-194">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="6ca74-195">Удостоверение шаблона в пустой проект</span><span class="sxs-lookup"><span data-stu-id="6ca74-195">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="6ca74-196">Добавьте следующие выделенные вызовы в класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="6ca74-196">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="6ca74-197">Удостоверение шаблона в проекте Razor без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="6ca74-197">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="6ca74-198">Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="6ca74-198">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="6ca74-199">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="6ca74-199">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="6ca74-200">Миграция, Усеаусентикатион и макет</span><span class="sxs-lookup"><span data-stu-id="6ca74-200">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="6ca74-201">Включить проверку подлинности</span><span class="sxs-lookup"><span data-stu-id="6ca74-201">Enable authentication</span></span>

<span data-ttu-id="6ca74-202">В методе `Configure` класса `Startup` вызовите [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="6ca74-202">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="6ca74-203">Изменения макета</span><span class="sxs-lookup"><span data-stu-id="6ca74-203">Layout changes</span></span>

<span data-ttu-id="6ca74-204">Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл макета:</span><span class="sxs-lookup"><span data-stu-id="6ca74-204">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="6ca74-205">Удостоверение шаблона в проекте Razor с авторизацией</span><span class="sxs-lookup"><span data-stu-id="6ca74-205">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="6ca74-206">Некоторые параметры удостоверения настраиваются в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="6ca74-206">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="6ca74-207">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="6ca74-207">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="6ca74-208">Удостоверение шаблона в проекте MVC без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="6ca74-208">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="6ca74-209">Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл *views/Shared/_layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="6ca74-209">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="6ca74-210">Переместить файл *pages/Shared/_LoginPartial. cshtml* в *views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="6ca74-210">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="6ca74-211">Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="6ca74-211">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="6ca74-212">Дополнительные сведения см. в разделе IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="6ca74-212">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="6ca74-213">Вызовите [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="6ca74-213">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="6ca74-214">Удостоверение шаблона в проекте MVC с авторизацией</span><span class="sxs-lookup"><span data-stu-id="6ca74-214">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="6ca74-215">Удалите папки *pages/Shared* и файлы в этой папке.</span><span class="sxs-lookup"><span data-stu-id="6ca74-215">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="6ca74-216">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="6ca74-216">Create full identity UI source</span></span>

<span data-ttu-id="6ca74-217">Чтобы обеспечить полный контроль над пользовательским интерфейсом удостоверений, запустите механизм формирования удостоверений и выберите **переопределить все файлы**.</span><span class="sxs-lookup"><span data-stu-id="6ca74-217">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="6ca74-218">В следующем выделенном коде показаны изменения, которые заменяют пользовательский интерфейс удостоверения по умолчанию удостоверением в веб-приложении ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="6ca74-218">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="6ca74-219">Это может потребоваться для полного управления пользовательским интерфейсом идентификации.</span><span class="sxs-lookup"><span data-stu-id="6ca74-219">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="6ca74-220">Удостоверение по умолчанию заменяется в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="6ca74-220">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="6ca74-221">Следующий код задает [логинпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [логаутпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)и [акцессдениедпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="6ca74-221">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="6ca74-222">Зарегистрируйте реализацию `IEmailSender`, например:</span><span class="sxs-lookup"><span data-stu-id="6ca74-222">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="6ca74-223">Отключить страницу регистрации</span><span class="sxs-lookup"><span data-stu-id="6ca74-223">Disable register page</span></span>

<span data-ttu-id="6ca74-224">Чтобы отключить регистрацию пользователей, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="6ca74-224">To disable user registration:</span></span>

* <span data-ttu-id="6ca74-225">Удостоверение шаблона.</span><span class="sxs-lookup"><span data-stu-id="6ca74-225">Scaffold Identity.</span></span> <span data-ttu-id="6ca74-226">Включить учетную запись. регистрация, учетная запись. Login и Account. Регистерконфирматион.</span><span class="sxs-lookup"><span data-stu-id="6ca74-226">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="6ca74-227">Пример:</span><span class="sxs-lookup"><span data-stu-id="6ca74-227">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="6ca74-228">Обновите *области/идентификаторы/страницы/учетная запись/Register. cshtml. CS* , чтобы пользователи не могли зарегистрироваться из этой конечной точки:</span><span class="sxs-lookup"><span data-stu-id="6ca74-228">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="6ca74-229">Обновите *области, идентификаторы, страницы, учетную запись или Register. cshtml* , чтобы они были совместимы с предыдущими изменениями:</span><span class="sxs-lookup"><span data-stu-id="6ca74-229">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="6ca74-230">Закомментируйте или удалите ссылку регистрации из *областей, удостоверений, страниц, учетной записи или имени входа. cshtml*</span><span class="sxs-lookup"><span data-stu-id="6ca74-230">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="6ca74-231">Обновите страницу *области, удостоверение, страницы/учетная запись или регистерконфирматион* .</span><span class="sxs-lookup"><span data-stu-id="6ca74-231">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="6ca74-232">Удалите код и ссылки из файла CSHTML.</span><span class="sxs-lookup"><span data-stu-id="6ca74-232">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="6ca74-233">Удалите код подтверждения из `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="6ca74-233">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="6ca74-234">Использование другого приложения для добавления пользователей</span><span class="sxs-lookup"><span data-stu-id="6ca74-234">Use another app to add users</span></span>

<span data-ttu-id="6ca74-235">Предоставьте механизм для добавления пользователей за пределами веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6ca74-235">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="6ca74-236">Ниже перечислены параметры для добавления пользователей.</span><span class="sxs-lookup"><span data-stu-id="6ca74-236">Options to add users include:</span></span>

* <span data-ttu-id="6ca74-237">Выделенное административное веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="6ca74-237">A dedicated admin web app.</span></span>
* <span data-ttu-id="6ca74-238">Консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="6ca74-238">A console app.</span></span>

<span data-ttu-id="6ca74-239">В следующем примере кода один из подходов к добавлению пользователей:</span><span class="sxs-lookup"><span data-stu-id="6ca74-239">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="6ca74-240">Список пользователей считывается в память.</span><span class="sxs-lookup"><span data-stu-id="6ca74-240">A list of users is read into memory.</span></span>
* <span data-ttu-id="6ca74-241">Для каждого пользователя создается надежный уникальный пароль.</span><span class="sxs-lookup"><span data-stu-id="6ca74-241">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="6ca74-242">Пользователь будет добавлен в базу данных удостоверений.</span><span class="sxs-lookup"><span data-stu-id="6ca74-242">The user is added to the Identity database.</span></span>
* <span data-ttu-id="6ca74-243">Пользователь получает уведомления и сообщил об изменении пароля.</span><span class="sxs-lookup"><span data-stu-id="6ca74-243">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="6ca74-244">В следующем коде показано, как добавить пользователя:</span><span class="sxs-lookup"><span data-stu-id="6ca74-244">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="6ca74-245">Подобный подход можно выполнить для рабочих сценариев.</span><span class="sxs-lookup"><span data-stu-id="6ca74-245">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6ca74-246">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6ca74-246">Additional resources</span></span>

* [<span data-ttu-id="6ca74-247">Изменения кода проверки подлинности на ASP.NET Core 2,1 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="6ca74-247">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end