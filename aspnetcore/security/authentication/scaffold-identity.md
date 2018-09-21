---
title: Каркас удостоверений в проектах ASP.NET Core
author: rick-anderson
description: Узнайте, как сформировать шаблон удостоверений в проекте ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 37ad9897fbc5eb1822ed2413334b4fce9050296b
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523042"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="57c78-103">Каркас удостоверений в проектах ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57c78-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="57c78-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="57c78-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="57c78-105">ASP.NET Core 2.1 и более поздних предоставляет [удостоверения ASP.NET Core](xref:security/authentication/identity) как [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="57c78-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="57c78-106">Приложения, включающие удостоверение можно применить шаблон выборочно Добавление исходного кода, содержащегося в библиотеке класса Identity Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="57c78-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="57c78-107">Вы можете создать исходный код, чтобы изменить код и тем самым изменить поведение.</span><span class="sxs-lookup"><span data-stu-id="57c78-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="57c78-108">Например, вы можете указать шаблону создать код, используемый при регистрации.</span><span class="sxs-lookup"><span data-stu-id="57c78-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="57c78-109">Созданный код имеет приоритет над тем же кодом в RCL для идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="57c78-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="57c78-110">Чтобы получить полный контроль над пользовательского интерфейса и не используйте значение по умолчанию RCL, см. в разделе [создать полное удостоверение пользовательского интерфейса источник](#full).</span><span class="sxs-lookup"><span data-stu-id="57c78-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="57c78-111">Приложения, которые **не** включения проверки подлинности можно применить шаблон для добавления пакета RCL удостоверений.</span><span class="sxs-lookup"><span data-stu-id="57c78-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="57c78-112">Вы можете выбрать, какой код идентификатора будет создан.</span><span class="sxs-lookup"><span data-stu-id="57c78-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="57c78-113">Несмотря на то, что шаблон создает большую часть необходимый код, необходимо обновить проект так, чтобы завершить процесс.</span><span class="sxs-lookup"><span data-stu-id="57c78-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="57c78-114">Этом документе описываются шаги, необходимые для завершения обновления удостоверений формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="57c78-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="57c78-115">При запуске шаблон удостоверений *ScaffoldingReadme.txt* файл создается в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="57c78-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="57c78-116">*ScaffoldingReadme.txt* файл содержит общие инструкции, на что нужно для завершения обновления удостоверений формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="57c78-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="57c78-117">Этот документ содержит более подробные инструкции, чем *ScaffoldingReadme.txt* файла.</span><span class="sxs-lookup"><span data-stu-id="57c78-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="57c78-118">Мы рекомендуем использовать систему управления версиями, показаны различия в файл и дает возможность отката изменений.</span><span class="sxs-lookup"><span data-stu-id="57c78-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="57c78-119">Какие изменения после выполнения шаблон удостоверений.</span><span class="sxs-lookup"><span data-stu-id="57c78-119">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="57c78-120">При использовании служб требуются [двухфакторная проверка подлинности](xref:security/authentication/identity-enable-qrcodes), [учетной записи пароль и Подтверждение восстановления](xref:security/authentication/accconfirm)и другие функции безопасности с удостоверением.</span><span class="sxs-lookup"><span data-stu-id="57c78-120">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="57c78-121">Службы или службы заглушки не появляется, если формирование шаблонов удостоверений.</span><span class="sxs-lookup"><span data-stu-id="57c78-121">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="57c78-122">Службы, чтобы включить эти функции должны быть добавлены вручную.</span><span class="sxs-lookup"><span data-stu-id="57c78-122">Services to enable these features must be added manually.</span></span> <span data-ttu-id="57c78-123">Например, см. в разделе [требуют подтверждение по электронной почте](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="57c78-123">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="57c78-124">Каркас identity в пустой проект</span><span class="sxs-lookup"><span data-stu-id="57c78-124">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="57c78-125">Добавьте следующие выделенные вызовы `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="57c78-125">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="57c78-126">Удостоверение шаблона в проект Razor без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="57c78-126">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
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

<span data-ttu-id="57c78-127">Удостоверение настроено в *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="57c78-127">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="57c78-128">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="57c78-128">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="57c78-129">Миграция, UseAuthentication и макет</span><span class="sxs-lookup"><span data-stu-id="57c78-129">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="57c78-130">Включить проверку подлинности</span><span class="sxs-lookup"><span data-stu-id="57c78-130">Enable authentication</span></span>

<span data-ttu-id="57c78-131">В `Configure` метод `Startup` вызовите [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="57c78-131">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="57c78-132">Изменение макета</span><span class="sxs-lookup"><span data-stu-id="57c78-132">Layout changes</span></span>

<span data-ttu-id="57c78-133">Необязательно: Добавьте частичное имя входа (`_LoginPartial`) с файлами макетов:</span><span class="sxs-lookup"><span data-stu-id="57c78-133">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="57c78-134">Удостоверение шаблона в проект Razor без авторизации</span><span class="sxs-lookup"><span data-stu-id="57c78-134">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth

dotnet new webapp -au Individual -o RPauth

dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="57c78-135">Некоторые параметры идентификаторов настраиваются в *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="57c78-135">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="57c78-136">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="57c78-136">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="57c78-137">Каркас удостоверений в проекте MVC без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="57c78-137">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="57c78-138">Необязательно: Добавьте частичное имя входа (`_LoginPartial`) для *Views/Shared/_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="57c78-138">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="57c78-139">Переместить *Pages/Shared/_LoginPartial.cshtml* файл *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="57c78-139">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="57c78-140">Удостоверение настроено в *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="57c78-140">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="57c78-141">Дополнительные сведения см. в разделе IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="57c78-141">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="57c78-142">Вызовите [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="57c78-142">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="57c78-143">Каркас удостоверений в проекте MVC с авторизацией</span><span class="sxs-lookup"><span data-stu-id="57c78-143">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="57c78-144">Удалить *страниц/Shared* папки и файлы в этой папке.</span><span class="sxs-lookup"><span data-stu-id="57c78-144">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="57c78-145">Создать источник полное удостоверение пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="57c78-145">Create full identity UI source</span></span>

<span data-ttu-id="57c78-146">Чтобы сохранить полный контроль удостоверение пользовательского интерфейса, запустите шаблон удостоверений и выберите **переопределить все файлы**.</span><span class="sxs-lookup"><span data-stu-id="57c78-146">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="57c78-147">Следующий выделенный код показывает изменения для замены стандартного пользовательского интерфейса удостоверения с удостоверением в веб-приложение ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="57c78-147">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="57c78-148">Может потребоваться сделать это для полного управления удостоверение пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="57c78-148">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="57c78-149">По умолчанию удостоверение будет заменен в следующий код:</span><span class="sxs-lookup"><span data-stu-id="57c78-149">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="57c78-150">Следующий код задает [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), и [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="57c78-150">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="57c78-151">Зарегистрировать `IEmailSender` реализации, например:</span><span class="sxs-lookup"><span data-stu-id="57c78-151">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="57c78-152">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="57c78-152">Additional resources</span></span>

* [<span data-ttu-id="57c78-153">Изменения в код проверки подлинности для ASP.NET Core 2.1 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="57c78-153">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)