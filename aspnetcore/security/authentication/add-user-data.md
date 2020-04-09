---
title: Добавление, загрузка и удаление пользовательских данных в Identity в проекте ASP.NET Core
author: rick-anderson
description: Узнайте, как добавить пользовательские пользовательские данные в Identity в проекте ASP.NET Core. Удаление данных на GDPR.
ms.author: riande
ms.date: 03/26/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 76b83df22381429feab80056c36dbdac1e5f20c7
ms.sourcegitcommit: 1d8f1396ccc66a0c3fcb5e5f36ea29b50db6d92a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2020
ms.locfileid: "80501229"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="a28bb-104">Добавление, загрузка и удаление пользовательских пользовательских данных пользователей в Identity в проекте ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a28bb-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="a28bb-105">Автор: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a28bb-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a28bb-106">В этой статье показано, как сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="a28bb-106">This article shows how to:</span></span>

* <span data-ttu-id="a28bb-107">Добавьте пользовательские пользовательские данные в веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a28bb-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="a28bb-108">Отметьте пользовательскую модель <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> пользовательских данных с помощью атрибута, чтобы она была автоматически доступна для скачивания и удаления.</span><span class="sxs-lookup"><span data-stu-id="a28bb-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="a28bb-109">Возможность загрузки и удаления данных помогает удовлетворить требования [GDPR.](xref:security/gdpr)</span><span class="sxs-lookup"><span data-stu-id="a28bb-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="a28bb-110">Образец проекта создается из веб-приложения Razor Pages, но инструкции аналогичны для веб-приложения core MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a28bb-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="a28bb-111">[Просмотр или загрузка образца кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) [(как скачать)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a28bb-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a28bb-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a28bb-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="a28bb-113">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="a28bb-113">Create a Razor web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a28bb-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28bb-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="a28bb-115">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="a28bb-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="a28bb-116">Назовите проект **WebApp1,** если хотите, чтобы он соответствовал названию [кода скачать образец.](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)</span><span class="sxs-lookup"><span data-stu-id="a28bb-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="a28bb-117">Выберите **ASP.NET основных веб-приложений** > **OK**</span><span class="sxs-lookup"><span data-stu-id="a28bb-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="a28bb-118">Выберите **ASP.NET Core 3.0** в выпадении</span><span class="sxs-lookup"><span data-stu-id="a28bb-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="a28bb-119">Выберите **WEB-приложение** > **OK**</span><span class="sxs-lookup"><span data-stu-id="a28bb-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="a28bb-120">Постройте и запустите проект.</span><span class="sxs-lookup"><span data-stu-id="a28bb-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="a28bb-121">В Visual Studio в меню **Файл** щелкните **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="a28bb-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="a28bb-122">Назовите проект **WebApp1,** если хотите, чтобы он соответствовал названию [кода скачать образец.](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)</span><span class="sxs-lookup"><span data-stu-id="a28bb-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="a28bb-123">Выберите **ASP.NET основных веб-приложений** > **OK**</span><span class="sxs-lookup"><span data-stu-id="a28bb-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="a28bb-124">Выберите **ASP.NET Core 2.2** в выпадении</span><span class="sxs-lookup"><span data-stu-id="a28bb-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="a28bb-125">Выберите **WEB-приложение** > **OK**</span><span class="sxs-lookup"><span data-stu-id="a28bb-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="a28bb-126">Постройте и запустите проект.</span><span class="sxs-lookup"><span data-stu-id="a28bb-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="a28bb-127">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a28bb-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="a28bb-128">Выполнить идентичность леса</span><span class="sxs-lookup"><span data-stu-id="a28bb-128">Run the Identity scaffolder</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a28bb-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28bb-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a28bb-130">От **решения Explorer**, право нажмите на проект > **Добавить** > **новые Scaffolded пункт**.</span><span class="sxs-lookup"><span data-stu-id="a28bb-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a28bb-131">Из левой панели диалога **Добавить Scaffold** выберите **Identity** > **Add.**</span><span class="sxs-lookup"><span data-stu-id="a28bb-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="a28bb-132">В диалоге **Add Identity** следующие варианты:</span><span class="sxs-lookup"><span data-stu-id="a28bb-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="a28bb-133">Выберите существующий файл *макета : Страницы/Общие/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a28bb-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="a28bb-134">Выберите следующие файлы для переопределения:</span><span class="sxs-lookup"><span data-stu-id="a28bb-134">Select the following files to override:</span></span>
    * <span data-ttu-id="a28bb-135">**Учетная запись/регистрация**</span><span class="sxs-lookup"><span data-stu-id="a28bb-135">**Account/Register**</span></span>
    * <span data-ttu-id="a28bb-136">**Учетная запись/Управление/Индекс**</span><span class="sxs-lookup"><span data-stu-id="a28bb-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="a28bb-137">Выберите **+** кнопку для создания нового **класса контекста данных.**</span><span class="sxs-lookup"><span data-stu-id="a28bb-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="a28bb-138">Примите тип (**WebApp1.Models.WebApp1Контекст,** если проект называется **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="a28bb-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="a28bb-139">Выберите **+** кнопку для создания нового **класса пользователя.**</span><span class="sxs-lookup"><span data-stu-id="a28bb-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="a28bb-140">Примите тип **(WebApp1User,** если проект называется **WebApp1)**> **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a28bb-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="a28bb-141">Выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a28bb-141">Select **Add**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="a28bb-142">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a28bb-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a28bb-143">Если ранее вы не установили эшафот ASP.NET Core, установите его сейчас:</span><span class="sxs-lookup"><span data-stu-id="a28bb-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="a28bb-144">Добавьте ссылку на пакет к файлу [проекта](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) (.csproj).</span><span class="sxs-lookup"><span data-stu-id="a28bb-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="a28bb-145">Выполнить следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="a28bb-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="a28bb-146">Выполнить следующую команду, чтобы перечислить параметры леса Identity:</span><span class="sxs-lookup"><span data-stu-id="a28bb-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="a28bb-147">В папке проекта запустите эшафот Identity:</span><span class="sxs-lookup"><span data-stu-id="a28bb-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="a28bb-148">Следуйте инструкциям в [Миграция, UseAuthentication, и макет](xref:security/authentication/scaffold-identity#efm) для выполнения следующих шагов:</span><span class="sxs-lookup"><span data-stu-id="a28bb-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="a28bb-149">Создайте миграцию и обновите базу данных.</span><span class="sxs-lookup"><span data-stu-id="a28bb-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="a28bb-150">Добавлен `UseAuthentication` в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a28bb-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="a28bb-151">Добавьте `<partial name="_LoginPartial" />` в файл макета.</span><span class="sxs-lookup"><span data-stu-id="a28bb-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="a28bb-152">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="a28bb-152">Test the app:</span></span>
  * <span data-ttu-id="a28bb-153">Регистрация пользователя</span><span class="sxs-lookup"><span data-stu-id="a28bb-153">Register a user</span></span>
  * <span data-ttu-id="a28bb-154">Выберите новое имя пользователя (рядом со ссылкой **На Logout).**</span><span class="sxs-lookup"><span data-stu-id="a28bb-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="a28bb-155">Возможно, потребуется расширить окно или выбрать значок панели навигации, чтобы показать имя пользователя и другие ссылки.</span><span class="sxs-lookup"><span data-stu-id="a28bb-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="a28bb-156">Выберите вкладку **«Личные данные».**</span><span class="sxs-lookup"><span data-stu-id="a28bb-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="a28bb-157">Выберите кнопку **Загрузка** и изучили файл *PersonalData.json.*</span><span class="sxs-lookup"><span data-stu-id="a28bb-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="a28bb-158">Проверьте кнопку **«Удалить»,** которая удаляет зарегистрированное на пользователя.</span><span class="sxs-lookup"><span data-stu-id="a28bb-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="a28bb-159">Добавление пользовательских пользовательских данных в DB Identity</span><span class="sxs-lookup"><span data-stu-id="a28bb-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="a28bb-160">Обновление `IdentityUser` производного класса с пользовательскими свойствами.</span><span class="sxs-lookup"><span data-stu-id="a28bb-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="a28bb-161">Если вы назвали проект WebApp1, файл называется *области / Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="a28bb-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="a28bb-162">Обновление файла со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a28bb-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="a28bb-163">Свойства с атрибутом [PersonalData:](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute)</span><span class="sxs-lookup"><span data-stu-id="a28bb-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="a28bb-164">Удален, когда *области / идентичность / Страницы / учетная запись / Управление / DeletePersonalData.cshtml* Razor Page звонки `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="a28bb-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="a28bb-165">Включено в загруженные данные *по странице Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span><span class="sxs-lookup"><span data-stu-id="a28bb-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="a28bb-166">Обновление страницы счета/Управления/Индекса.cshtml</span><span class="sxs-lookup"><span data-stu-id="a28bb-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="a28bb-167">Обновление `InputModel` в *областях / Идентичность / Страницы / Счет / Управление / Index.cshtml.cs* со следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="a28bb-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="a28bb-168">Обновление *зон/идентификации/Страницы/Счет/Управление/Индекс.cshtml* со следующей выделенной разметкой:</span><span class="sxs-lookup"><span data-stu-id="a28bb-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="a28bb-169">Обновление *зон/идентификации/Страницы/Счет/Управление/Индекс.cshtml* со следующей выделенной разметкой:</span><span class="sxs-lookup"><span data-stu-id="a28bb-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="a28bb-170">Обновление страницы Счета/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="a28bb-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="a28bb-171">Обновление `InputModel` в *области / идентичность / Страницы / Счета / Register.cshtml.cs* со следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="a28bb-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="a28bb-172">Обновление *зон/идентификации/Страницы/Счет/Register.cshtml* со следующей выделенной разметкой:</span><span class="sxs-lookup"><span data-stu-id="a28bb-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="a28bb-173">Обновление *зон/идентификации/Страницы/Счет/Register.cshtml* со следующей выделенной разметкой:</span><span class="sxs-lookup"><span data-stu-id="a28bb-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="a28bb-174">Создайте проект.</span><span class="sxs-lookup"><span data-stu-id="a28bb-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="a28bb-175">Добавление миграции для пользовательских пользовательских данных пользователя</span><span class="sxs-lookup"><span data-stu-id="a28bb-175">Add a migration for the custom user data</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a28bb-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a28bb-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a28bb-177">В визуальной студии **пакет менеджер консоли**:</span><span class="sxs-lookup"><span data-stu-id="a28bb-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="a28bb-178">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a28bb-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="a28bb-179">Тест создать, просмотреть, скачать, удалить пользовательские данные пользователя</span><span class="sxs-lookup"><span data-stu-id="a28bb-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="a28bb-180">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="a28bb-180">Test the app:</span></span>

* <span data-ttu-id="a28bb-181">Зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="a28bb-181">Register a new user.</span></span>
* <span data-ttu-id="a28bb-182">Просмотр пользовательских пользовательских `/Identity/Account/Manage` данных пользователей на странице.</span><span class="sxs-lookup"><span data-stu-id="a28bb-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="a28bb-183">Загрузка и просмотр персональных `/Identity/Account/Manage/PersonalData` данных пользователей со страницы.</span><span class="sxs-lookup"><span data-stu-id="a28bb-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>

## <a name="add-claims-to-identity-using-iuserclaimsprincipalfactoryapplicationuser"></a><span data-ttu-id="a28bb-184">Добавление претензий к идентификации с помощью IUserClaimsPrincipalFactory<ApplicationUser></span><span class="sxs-lookup"><span data-stu-id="a28bb-184">Add claims to Identity using IUserClaimsPrincipalFactory<ApplicationUser></span></span>

<span data-ttu-id="a28bb-185">Дополнительные требования могут быть добавлены `IUserClaimsPrincipalFactory<T>` к ASP.NET Core Identity с помощью интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a28bb-185">Additional claims can be added to ASP.NET Core Identity by using the `IUserClaimsPrincipalFactory<T>` interface.</span></span> <span data-ttu-id="a28bb-186">Этот класс можно добавить в `Startup.ConfigureServices` приложение в методе.</span><span class="sxs-lookup"><span data-stu-id="a28bb-186">This class can be added to the app in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a28bb-187">Добавьте пользовательскую реализацию класса следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a28bb-187">Add the custom implementation of the class as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddScoped<IUserClaimsPrincipalFactory<ApplicationUser>, 
        AdditionalUserClaimsPrincipalFactory>();
```

<span data-ttu-id="a28bb-188">Демо-код `ApplicationUser` использует класс.</span><span class="sxs-lookup"><span data-stu-id="a28bb-188">The demo code uses the `ApplicationUser` class.</span></span> <span data-ttu-id="a28bb-189">Этот класс `IsAdmin` добавляет свойство, которое используется для добавления дополнительной претензии.</span><span class="sxs-lookup"><span data-stu-id="a28bb-189">This class adds an `IsAdmin` property which is used to add the additional claim.</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public bool IsAdmin { get; set; }
}
```

<span data-ttu-id="a28bb-190">Класс `AdditionalUserClaimsPrincipalFactory` реализует интерфейс `UserClaimsPrincipalFactory`.</span><span class="sxs-lookup"><span data-stu-id="a28bb-190">The `AdditionalUserClaimsPrincipalFactory` implements the `UserClaimsPrincipalFactory` interface.</span></span> <span data-ttu-id="a28bb-191">Новая претензия роли добавляется к `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="a28bb-191">A new role claim is added to the `ClaimsPrincipal`.</span></span>

```csharp
public class AdditionalUserClaimsPrincipalFactory 
        : UserClaimsPrincipalFactory<ApplicationUser, IdentityRole>
{
    public AdditionalUserClaimsPrincipalFactory( 
        UserManager<ApplicationUser> userManager,
        RoleManager<IdentityRole> roleManager, 
        IOptions<IdentityOptions> optionsAccessor) 
        : base(userManager, roleManager, optionsAccessor)
    {}

    public async override Task<ClaimsPrincipal> CreateAsync(ApplicationUser user)
    {
        var principal = await base.CreateAsync(user);
        var identity = (ClaimsIdentity)principal.Identity;

        var claims = new List<Claim>();
        if (user.IsAdmin)
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "admin"));
        }
        else
        {
            claims.Add(new Claim(JwtClaimTypes.Role, "user"));
        }

        identity.AddClaims(claims);
        return principal;
    }
}
```

<span data-ttu-id="a28bb-192">Дополнительная претензия может быть использована в приложении.</span><span class="sxs-lookup"><span data-stu-id="a28bb-192">The additional claim can then be used in the app.</span></span> <span data-ttu-id="a28bb-193">На странице Razor `IAuthorizationService` экземпляр может использоваться для доступа к значению претензии.</span><span class="sxs-lookup"><span data-stu-id="a28bb-193">In a Razor Page, the `IAuthorizationService` instance can be used to access the claim value.</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

@if ((await AuthorizationService.AuthorizeAsync(User, "IsAdmin")).Succeeded)
{
    <ul class="mr-auto navbar-nav">
        <li class="nav-item">
            <a class="nav-link" asp-controller="Admin" asp-action="Index">ADMIN</a>
        </li>
    </ul>
}
```
