---
title: Удостоверение формирования шаблонов в проектах ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о формировать удостоверения в проекте ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: cf6544d8b671f026c8466fa8dff506027b64cf1f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276322"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="d4e9d-103">Удостоверение формирования шаблонов в проектах ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4e9d-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="d4e9d-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d4e9d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d4e9d-105">Предоставляет ASP.NET Core 2.1 и более поздние версии [ASP.NET Core Identity](xref:security/authentication/identity) как [библиотеки классов Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="d4e9d-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="d4e9d-106">Приложения, включающие удостоверение можно применить scaffolder выборочно Добавление исходного кода, содержащегося в библиотеке класса Razor идентификаторов (RCL).</span><span class="sxs-lookup"><span data-stu-id="d4e9d-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="d4e9d-107">Можно создать исходный код, чтобы можно было изменить код и изменить поведение.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="d4e9d-108">Например можно указать scaffolder для создания кода, используемое при регистрации.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="d4e9d-109">Созданный код имеет приоритет над один и тот же код в RCL удостоверений.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="d4e9d-110">Чтобы получить полный контроль над пользовательского интерфейса и не использовать значение по умолчанию RCL, см. в разделе [создать полное удостоверение пользовательского интерфейса источника](#full).</span><span class="sxs-lookup"><span data-stu-id="d4e9d-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="d4e9d-111">Приложения, которые **не** включения проверки подлинности можно применить scaffolder Добавление RCL удостоверение пакета.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="d4e9d-112">У вас есть возможность выбрать удостоверение код должен быть создан.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="d4e9d-113">Несмотря на то, что scaffolder создает большую часть необходимый код, необходимо обновить проект, чтобы завершить процесс.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="d4e9d-114">В этом документе объясняется шаги, необходимые для завершения обновления удостоверения формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="d4e9d-115">При запуске удостоверения scaffolder *ScaffoldingReadme.txt* файл создается в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="d4e9d-116">*ScaffoldingReadme.txt* файл содержит общие инструкции, на что требуется для завершения обновления удостоверения формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="d4e9d-117">Этот документ содержит более подробные инструкции, чем *ScaffoldingReadme.txt* файла.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="d4e9d-118">Мы рекомендуем использовать систему управления версиями, показаны различия в файл и дает возможность отката изменений.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="d4e9d-119">Проверьте изменения после выполнения scaffolder удостоверений.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="d4e9d-120">Удостоверение формирования шаблонов в пустой проект</span><span class="sxs-lookup"><span data-stu-id="d4e9d-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d4e9d-121">Добавьте следующий выделенный вызовы `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="d4e9d-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="d4e9d-122">Удостоверение формирования шаблонов в проект Razor без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="d4e9d-122">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="d4e9d-123">Удостоверение настраивается в *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d4e9d-124">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d4e9d-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="d4e9d-125">Миграция, UseAuthentication и макет</span><span class="sxs-lookup"><span data-stu-id="d4e9d-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="d4e9d-126">В `Configure` метод `Startup` вызовите [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d4e9d-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="d4e9d-127">Изменение макета</span><span class="sxs-lookup"><span data-stu-id="d4e9d-127">Layout changes</span></span>

<span data-ttu-id="d4e9d-128">Необязательно: Добавьте имя входа частичного (`_LoginPartial`) файл макета:</span><span class="sxs-lookup"><span data-stu-id="d4e9d-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="d4e9d-129">Удостоверение формирования шаблонов в проект Razor с авторизации</span><span class="sxs-lookup"><span data-stu-id="d4e9d-129">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="d4e9d-130">Некоторые параметры идентификаторов настраиваются в *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-130">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d4e9d-131">Дополнительные сведения см. в разделе [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d4e9d-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="d4e9d-132">Удостоверение формирования шаблонов в проект MVC без существующей авторизации</span><span class="sxs-lookup"><span data-stu-id="d4e9d-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="d4e9d-133">Необязательно: Добавьте имя входа частичного (`_LoginPartial`) для *Views/Shared/_Layout.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="d4e9d-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="d4e9d-134">Переместить *Pages/Shared/_LoginPartial.cshtml* файл *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d4e9d-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="d4e9d-135">Удостоверение настраивается в *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d4e9d-136">Дополнительные сведения см. в разделе IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="d4e9d-137">Вызовите [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) после `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d4e9d-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="d4e9d-138">Удостоверение формирования шаблонов в проект MVC с авторизации</span><span class="sxs-lookup"><span data-stu-id="d4e9d-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="d4e9d-139">Удалить *страниц/Общие* папки и файлы в этой папке.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="d4e9d-140">Создать источник полное удостоверение пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="d4e9d-140">Create full identity UI source</span></span>

<span data-ttu-id="d4e9d-141">Чтобы сохранить полный контроль удостоверения пользовательского интерфейса, запустите scaffolder удостоверений и выберите **переопределить все файлы**.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="d4e9d-142">Следующий выделенный код показывает изменения, замените имя по умолчанию удостоверения пользовательского удостоверения в веб-приложение ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="d4e9d-143">Может потребоваться этого имеют полный контроль над удостоверения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="d4e9d-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="d4e9d-144">Значение по умолчанию удостоверение будет заменен в следующий код:</span><span class="sxs-lookup"><span data-stu-id="d4e9d-144">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="d4e9d-145">Следующий код задает [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), и [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="d4e9d-145">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="d4e9d-146">Зарегистрировать `IEmailSender` реализации, например:</span><span class="sxs-lookup"><span data-stu-id="d4e9d-146">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
