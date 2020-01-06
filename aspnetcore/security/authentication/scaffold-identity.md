---
title: Удостоверение шаблона в ASP.NET Core проектах
author: rick-anderson
description: Узнайте, как сформировать удостоверение в проекте ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 2432d346d9678157848a38fa01d9057cdd7503ff
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75356259"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="90fe0-103">Удостоверение шаблона в ASP.NET Core проектах</span><span class="sxs-lookup"><span data-stu-id="90fe0-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="90fe0-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="90fe0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="90fe0-105">ASP.NET Core предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) как [библиотеку классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="90fe0-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="90fe0-106">Приложения, включающие удостоверение, могут применять шаблон для выборочного добавления исходного кода, содержащегося в библиотеке классов Razor класса Identity (РКЛ).</span><span class="sxs-lookup"><span data-stu-id="90fe0-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="90fe0-107">Вы можете создать исходный код, чтобы изменить код и тем самым изменить поведение.</span><span class="sxs-lookup"><span data-stu-id="90fe0-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="90fe0-108">Например, вы можете указать шаблону создать код, используемый при регистрации.</span><span class="sxs-lookup"><span data-stu-id="90fe0-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="90fe0-109">Созданный код имеет приоритет над тем же кодом в RCL для идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="90fe0-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="90fe0-110">Чтобы получить полный контроль над пользовательским интерфейсом и не использовать РКЛ по умолчанию, см. раздел [Создание полного идентификатора пользовательского интерфейса идентификации](#full).</span><span class="sxs-lookup"><span data-stu-id="90fe0-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="90fe0-111">Приложения, которые **не** включают проверку подлинности, могут применять механизм формирования шаблонов для добавления пакета РКЛ Identity.</span><span class="sxs-lookup"><span data-stu-id="90fe0-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="90fe0-112">Вы можете выбрать, какой код идентификатора будет создан.</span><span class="sxs-lookup"><span data-stu-id="90fe0-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="90fe0-113">Хотя этот механизм создает большую часть необходимого кода, необходимо обновить проект, чтобы завершить процесс.</span><span class="sxs-lookup"><span data-stu-id="90fe0-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="90fe0-114">В этом документе объясняются шаги, необходимые для завершения обновления формирования шаблонов удостоверений.</span><span class="sxs-lookup"><span data-stu-id="90fe0-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="90fe0-115">Рекомендуется использовать систему управления версиями, которая показывает различия в файлах и позволяет вносить изменения.</span><span class="sxs-lookup"><span data-stu-id="90fe0-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="90fe0-116">Проверьте изменения после запуска шаблона удостоверений.</span><span class="sxs-lookup"><span data-stu-id="90fe0-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="90fe0-117">Службы необходимы при использовании [двухфакторной проверки подлинности](xref:security/authentication/identity-enable-qrcodes), [подтверждения учетной записи и восстановления пароля](xref:security/authentication/accconfirm), а также других функций безопасности с удостоверениями.</span><span class="sxs-lookup"><span data-stu-id="90fe0-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="90fe0-118">Службы или заглушки служб не создаются при формировании удостоверений формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="90fe0-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="90fe0-119">Службы для включения этих функций необходимо добавить вручную.</span><span class="sxs-lookup"><span data-stu-id="90fe0-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="90fe0-120">Например, см. статью [требование подтверждения по электронной почте](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="90fe0-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="90fe0-121">Этот документ содержит более полные инструкции, чем файл *скаффолдингреадме. txt* , который создается при запуске шаблона.</span><span class="sxs-lookup"><span data-stu-id="90fe0-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="90fe0-122">Удостоверение шаблона в пустой проект</span><span class="sxs-lookup"><span data-stu-id="90fe0-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="90fe0-123">Обновите класс `Startup` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="90fe0-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="90fe0-124">Удостоверение шаблона в проекте Razor без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="90fe0-124">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="90fe0-125">Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="90fe0-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="90fe0-126">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="90fe0-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="90fe0-127">Миграция, Усеаусентикатион и макет</span><span class="sxs-lookup"><span data-stu-id="90fe0-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="90fe0-128">Включение проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="90fe0-128">Enable authentication</span></span>

<span data-ttu-id="90fe0-129">Обновите класс `Startup` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="90fe0-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="90fe0-130">Изменения макета</span><span class="sxs-lookup"><span data-stu-id="90fe0-130">Layout changes</span></span>

<span data-ttu-id="90fe0-131">Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл макета:</span><span class="sxs-lookup"><span data-stu-id="90fe0-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="90fe0-132">Удостоверение шаблона в проекте Razor с авторизацией</span><span class="sxs-lookup"><span data-stu-id="90fe0-132">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="90fe0-133">Некоторые параметры удостоверения настраиваются в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="90fe0-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="90fe0-134">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="90fe0-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="90fe0-135">Удостоверение шаблона в проекте MVC без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="90fe0-135">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="90fe0-136">Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл *views/Shared/_layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="90fe0-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="90fe0-137">Переместить файл *pages/Shared/_LoginPartial. cshtml* в *views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="90fe0-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="90fe0-138">Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="90fe0-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="90fe0-139">Дополнительные сведения см. в разделе IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="90fe0-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="90fe0-140">Обновите класс `Startup` с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="90fe0-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="90fe0-141">Удостоверение шаблона в проекте MVC с авторизацией</span><span class="sxs-lookup"><span data-stu-id="90fe0-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="90fe0-142">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="90fe0-142">Create full identity UI source</span></span>

<span data-ttu-id="90fe0-143">Чтобы обеспечить полный контроль над пользовательским интерфейсом удостоверений, запустите механизм формирования удостоверений и выберите **переопределить все файлы**.</span><span class="sxs-lookup"><span data-stu-id="90fe0-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="90fe0-144">В следующем выделенном коде показаны изменения, которые заменяют пользовательский интерфейс удостоверения по умолчанию удостоверением в веб-приложении ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="90fe0-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="90fe0-145">Это может потребоваться для полного управления пользовательским интерфейсом идентификации.</span><span class="sxs-lookup"><span data-stu-id="90fe0-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="90fe0-146">Удостоверение по умолчанию заменяется в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="90fe0-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="90fe0-147">Следующий код задает [логинпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [логаутпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)и [акцессдениедпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="90fe0-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="90fe0-148">Зарегистрируйте реализацию `IEmailSender`, например:</span><span class="sxs-lookup"><span data-stu-id="90fe0-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="90fe0-149">Отключить страницу регистрации</span><span class="sxs-lookup"><span data-stu-id="90fe0-149">Disable register page</span></span>

<span data-ttu-id="90fe0-150">Чтобы отключить регистрацию пользователей, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="90fe0-150">To disable user registration:</span></span>

* <span data-ttu-id="90fe0-151">Удостоверение шаблона.</span><span class="sxs-lookup"><span data-stu-id="90fe0-151">Scaffold Identity.</span></span> <span data-ttu-id="90fe0-152">Включить учетную запись. регистрация, учетная запись. Login и Account. Регистерконфирматион.</span><span class="sxs-lookup"><span data-stu-id="90fe0-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="90fe0-153">Например:</span><span class="sxs-lookup"><span data-stu-id="90fe0-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="90fe0-154">Обновите *области/идентификаторы/страницы/учетная запись/Register. cshtml. CS* , чтобы пользователи не могли зарегистрироваться из этой конечной точки:</span><span class="sxs-lookup"><span data-stu-id="90fe0-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="90fe0-155">Обновите *области, идентификаторы, страницы, учетную запись или Register. cshtml* , чтобы они были совместимы с предыдущими изменениями:</span><span class="sxs-lookup"><span data-stu-id="90fe0-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="90fe0-156">Закомментируйте или удалите ссылку регистрации из *областей, удостоверений, страниц, учетной записи или имени входа. cshtml*</span><span class="sxs-lookup"><span data-stu-id="90fe0-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="90fe0-157">Обновите страницу *области, удостоверение, страницы/учетная запись или регистерконфирматион* .</span><span class="sxs-lookup"><span data-stu-id="90fe0-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="90fe0-158">Удалите код и ссылки из файла CSHTML.</span><span class="sxs-lookup"><span data-stu-id="90fe0-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="90fe0-159">Удалите код подтверждения из `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="90fe0-159">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="90fe0-160">Использование другого приложения для добавления пользователей</span><span class="sxs-lookup"><span data-stu-id="90fe0-160">Use another app to add users</span></span>

<span data-ttu-id="90fe0-161">Предоставьте механизм для добавления пользователей за пределами веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="90fe0-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="90fe0-162">Ниже перечислены параметры для добавления пользователей.</span><span class="sxs-lookup"><span data-stu-id="90fe0-162">Options to add users include:</span></span>

* <span data-ttu-id="90fe0-163">Выделенное административное веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="90fe0-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="90fe0-164">Консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="90fe0-164">A console app.</span></span>

<span data-ttu-id="90fe0-165">В следующем примере кода один из подходов к добавлению пользователей:</span><span class="sxs-lookup"><span data-stu-id="90fe0-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="90fe0-166">Список пользователей считывается в память.</span><span class="sxs-lookup"><span data-stu-id="90fe0-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="90fe0-167">Для каждого пользователя создается надежный уникальный пароль.</span><span class="sxs-lookup"><span data-stu-id="90fe0-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="90fe0-168">Пользователь будет добавлен в базу данных удостоверений.</span><span class="sxs-lookup"><span data-stu-id="90fe0-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="90fe0-169">Пользователь получает уведомления и сообщил об изменении пароля.</span><span class="sxs-lookup"><span data-stu-id="90fe0-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="90fe0-170">В следующем коде показано, как добавить пользователя:</span><span class="sxs-lookup"><span data-stu-id="90fe0-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="90fe0-171">Подобный подход можно выполнить для рабочих сценариев.</span><span class="sxs-lookup"><span data-stu-id="90fe0-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90fe0-172">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="90fe0-172">Additional resources</span></span>

* [<span data-ttu-id="90fe0-173">Изменения кода проверки подлинности на ASP.NET Core 2,1 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="90fe0-173">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="90fe0-174">ASP.NET Core 2,1 и более поздних версий предоставляет [ASP.NET Core удостоверение](xref:security/authentication/identity) как [библиотеку классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="90fe0-174">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="90fe0-175">Приложения, включающие удостоверение, могут применять шаблон для выборочного добавления исходного кода, содержащегося в библиотеке классов Razor класса Identity (РКЛ).</span><span class="sxs-lookup"><span data-stu-id="90fe0-175">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="90fe0-176">Вы можете создать исходный код, чтобы изменить код и тем самым изменить поведение.</span><span class="sxs-lookup"><span data-stu-id="90fe0-176">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="90fe0-177">Например, вы можете указать шаблону создать код, используемый при регистрации.</span><span class="sxs-lookup"><span data-stu-id="90fe0-177">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="90fe0-178">Созданный код имеет приоритет над тем же кодом в RCL для идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="90fe0-178">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="90fe0-179">Чтобы получить полный контроль над пользовательским интерфейсом и не использовать РКЛ по умолчанию, см. раздел [Создание полного идентификатора пользовательского интерфейса идентификации](#full).</span><span class="sxs-lookup"><span data-stu-id="90fe0-179">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="90fe0-180">Приложения, которые **не** включают проверку подлинности, могут применять механизм формирования шаблонов для добавления пакета РКЛ Identity.</span><span class="sxs-lookup"><span data-stu-id="90fe0-180">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="90fe0-181">Вы можете выбрать, какой код идентификатора будет создан.</span><span class="sxs-lookup"><span data-stu-id="90fe0-181">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="90fe0-182">Хотя этот механизм создает большую часть необходимого кода, необходимо обновить проект, чтобы завершить процесс.</span><span class="sxs-lookup"><span data-stu-id="90fe0-182">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="90fe0-183">В этом документе объясняются шаги, необходимые для завершения обновления формирования шаблонов удостоверений.</span><span class="sxs-lookup"><span data-stu-id="90fe0-183">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="90fe0-184">При запуске механизма формирования идентификаторов в каталоге проекта создается файл *скаффолдингреадме. txt* .</span><span class="sxs-lookup"><span data-stu-id="90fe0-184">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="90fe0-185">Файл *скаффолдингреадме. txt* содержит общие инструкции о том, что необходимо для завершения обновления формирования шаблонов удостоверений.</span><span class="sxs-lookup"><span data-stu-id="90fe0-185">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="90fe0-186">Этот документ содержит более полные инструкции, чем файл *скаффолдингреадме. txt* .</span><span class="sxs-lookup"><span data-stu-id="90fe0-186">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="90fe0-187">Рекомендуется использовать систему управления версиями, которая показывает различия в файлах и позволяет вносить изменения.</span><span class="sxs-lookup"><span data-stu-id="90fe0-187">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="90fe0-188">Проверьте изменения после запуска шаблона удостоверений.</span><span class="sxs-lookup"><span data-stu-id="90fe0-188">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="90fe0-189">Службы необходимы при использовании [двухфакторной проверки подлинности](xref:security/authentication/identity-enable-qrcodes), [подтверждения учетной записи и восстановления пароля](xref:security/authentication/accconfirm), а также других функций безопасности с удостоверениями.</span><span class="sxs-lookup"><span data-stu-id="90fe0-189">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="90fe0-190">Службы или заглушки служб не создаются при формировании удостоверений формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="90fe0-190">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="90fe0-191">Службы для включения этих функций необходимо добавить вручную.</span><span class="sxs-lookup"><span data-stu-id="90fe0-191">Services to enable these features must be added manually.</span></span> <span data-ttu-id="90fe0-192">Например, см. статью [требование подтверждения по электронной почте](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="90fe0-192">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="90fe0-193">Удостоверение шаблона в пустой проект</span><span class="sxs-lookup"><span data-stu-id="90fe0-193">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="90fe0-194">Добавьте следующие выделенные вызовы в класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="90fe0-194">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="90fe0-195">Удостоверение шаблона в проекте Razor без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="90fe0-195">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="90fe0-196">Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="90fe0-196">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="90fe0-197">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="90fe0-197">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="90fe0-198">Миграция, Усеаусентикатион и макет</span><span class="sxs-lookup"><span data-stu-id="90fe0-198">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="90fe0-199">Включение проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="90fe0-199">Enable authentication</span></span>

<span data-ttu-id="90fe0-200">В методе `Configure` класса `Startup` вызовите [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="90fe0-200">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="90fe0-201">Изменения макета</span><span class="sxs-lookup"><span data-stu-id="90fe0-201">Layout changes</span></span>

<span data-ttu-id="90fe0-202">Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл макета:</span><span class="sxs-lookup"><span data-stu-id="90fe0-202">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="90fe0-203">Удостоверение шаблона в проекте Razor с авторизацией</span><span class="sxs-lookup"><span data-stu-id="90fe0-203">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="90fe0-204">Некоторые параметры удостоверения настраиваются в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="90fe0-204">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="90fe0-205">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="90fe0-205">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="90fe0-206">Удостоверение шаблона в проекте MVC без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="90fe0-206">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="90fe0-207">Необязательно: Добавьте часть имени для входа partial (`_LoginPartial`) в файл *views/Shared/_layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="90fe0-207">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="90fe0-208">Переместить файл *pages/Shared/_LoginPartial. cshtml* в *views/shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="90fe0-208">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="90fe0-209">Удостоверение настраивается в *области/Identity/идентитихостингстартуп. CS*.</span><span class="sxs-lookup"><span data-stu-id="90fe0-209">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="90fe0-210">Дополнительные сведения см. в разделе IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="90fe0-210">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="90fe0-211">Вызовите [усеаусентикатион](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="90fe0-211">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="90fe0-212">Удостоверение шаблона в проекте MVC с авторизацией</span><span class="sxs-lookup"><span data-stu-id="90fe0-212">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="90fe0-213">Удалите папки *pages/Shared* и файлы в этой папке.</span><span class="sxs-lookup"><span data-stu-id="90fe0-213">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="90fe0-214">Создание полного источника идентификатора пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="90fe0-214">Create full identity UI source</span></span>

<span data-ttu-id="90fe0-215">Чтобы обеспечить полный контроль над пользовательским интерфейсом удостоверений, запустите механизм формирования удостоверений и выберите **переопределить все файлы**.</span><span class="sxs-lookup"><span data-stu-id="90fe0-215">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="90fe0-216">В следующем выделенном коде показаны изменения, которые заменяют пользовательский интерфейс удостоверения по умолчанию удостоверением в веб-приложении ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="90fe0-216">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="90fe0-217">Это может потребоваться для полного управления пользовательским интерфейсом идентификации.</span><span class="sxs-lookup"><span data-stu-id="90fe0-217">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="90fe0-218">Удостоверение по умолчанию заменяется в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="90fe0-218">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="90fe0-219">Следующий код задает [логинпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [логаутпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)и [акцессдениедпас](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="90fe0-219">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="90fe0-220">Зарегистрируйте реализацию `IEmailSender`, например:</span><span class="sxs-lookup"><span data-stu-id="90fe0-220">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="90fe0-221">Отключить страницу регистрации</span><span class="sxs-lookup"><span data-stu-id="90fe0-221">Disable register page</span></span>

<span data-ttu-id="90fe0-222">Чтобы отключить регистрацию пользователей, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="90fe0-222">To disable user registration:</span></span>

* <span data-ttu-id="90fe0-223">Удостоверение шаблона.</span><span class="sxs-lookup"><span data-stu-id="90fe0-223">Scaffold Identity.</span></span> <span data-ttu-id="90fe0-224">Включить учетную запись. регистрация, учетная запись. Login и Account. Регистерконфирматион.</span><span class="sxs-lookup"><span data-stu-id="90fe0-224">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="90fe0-225">Например:</span><span class="sxs-lookup"><span data-stu-id="90fe0-225">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="90fe0-226">Обновите *области/идентификаторы/страницы/учетная запись/Register. cshtml. CS* , чтобы пользователи не могли зарегистрироваться из этой конечной точки:</span><span class="sxs-lookup"><span data-stu-id="90fe0-226">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="90fe0-227">Обновите *области, идентификаторы, страницы, учетную запись или Register. cshtml* , чтобы они были совместимы с предыдущими изменениями:</span><span class="sxs-lookup"><span data-stu-id="90fe0-227">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="90fe0-228">Закомментируйте или удалите ссылку регистрации из *областей, удостоверений, страниц, учетной записи или имени входа. cshtml*</span><span class="sxs-lookup"><span data-stu-id="90fe0-228">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="90fe0-229">Обновите страницу *области, удостоверение, страницы/учетная запись или регистерконфирматион* .</span><span class="sxs-lookup"><span data-stu-id="90fe0-229">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="90fe0-230">Удалите код и ссылки из файла CSHTML.</span><span class="sxs-lookup"><span data-stu-id="90fe0-230">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="90fe0-231">Удалите код подтверждения из `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="90fe0-231">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="90fe0-232">Использование другого приложения для добавления пользователей</span><span class="sxs-lookup"><span data-stu-id="90fe0-232">Use another app to add users</span></span>

<span data-ttu-id="90fe0-233">Предоставьте механизм для добавления пользователей за пределами веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="90fe0-233">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="90fe0-234">Ниже перечислены параметры для добавления пользователей.</span><span class="sxs-lookup"><span data-stu-id="90fe0-234">Options to add users include:</span></span>

* <span data-ttu-id="90fe0-235">Выделенное административное веб-приложение.</span><span class="sxs-lookup"><span data-stu-id="90fe0-235">A dedicated admin web app.</span></span>
* <span data-ttu-id="90fe0-236">Консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="90fe0-236">A console app.</span></span>

<span data-ttu-id="90fe0-237">В следующем примере кода один из подходов к добавлению пользователей:</span><span class="sxs-lookup"><span data-stu-id="90fe0-237">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="90fe0-238">Список пользователей считывается в память.</span><span class="sxs-lookup"><span data-stu-id="90fe0-238">A list of users is read into memory.</span></span>
* <span data-ttu-id="90fe0-239">Для каждого пользователя создается надежный уникальный пароль.</span><span class="sxs-lookup"><span data-stu-id="90fe0-239">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="90fe0-240">Пользователь будет добавлен в базу данных удостоверений.</span><span class="sxs-lookup"><span data-stu-id="90fe0-240">The user is added to the Identity database.</span></span>
* <span data-ttu-id="90fe0-241">Пользователь получает уведомления и сообщил об изменении пароля.</span><span class="sxs-lookup"><span data-stu-id="90fe0-241">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="90fe0-242">В следующем коде показано, как добавить пользователя:</span><span class="sxs-lookup"><span data-stu-id="90fe0-242">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="90fe0-243">Подобный подход можно выполнить для рабочих сценариев.</span><span class="sxs-lookup"><span data-stu-id="90fe0-243">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90fe0-244">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="90fe0-244">Additional resources</span></span>

* [<span data-ttu-id="90fe0-245">Изменения кода проверки подлинности на ASP.NET Core 2,1 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="90fe0-245">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end