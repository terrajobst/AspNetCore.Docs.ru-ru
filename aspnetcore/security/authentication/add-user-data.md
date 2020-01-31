---
title: Добавить, загрузки и удаления данных пользователя для удостоверения в проекте ASP.NET Core
author: rick-anderson
description: Узнайте, как добавить пользовательские данные для удостоверения в проекте ASP.NET Core. Удаление данных в соответствии с GDPR.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: e08c02e2e5d4a429aae10c59e7ae3ea48c975067
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885555"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="03e1b-104">Добавление, скачивание и удаление пользовательских данных для удостоверений в проекте ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03e1b-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="03e1b-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="03e1b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="03e1b-106">В этой статье показано, как:</span><span class="sxs-lookup"><span data-stu-id="03e1b-106">This article shows how to:</span></span>

* <span data-ttu-id="03e1b-107">Добавьте пользовательские данные веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="03e1b-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="03e1b-108">Пометьте пользовательскую модель данных атрибутом <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute>, чтобы она была автоматически доступна для скачивания и удаления.</span><span class="sxs-lookup"><span data-stu-id="03e1b-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="03e1b-109">Возможность загрузки и удаления данных помогает обеспечить соответствие [GDPR](xref:security/gdpr) требования.</span><span class="sxs-lookup"><span data-stu-id="03e1b-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="03e1b-110">В примере проекта создается на основе веб-приложения Razor Pages, но инструкции одинаковы для веб-приложения ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="03e1b-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="03e1b-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="03e1b-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="03e1b-112">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="03e1b-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="03e1b-113">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="03e1b-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03e1b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03e1b-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="03e1b-115">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="03e1b-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="03e1b-116">Назовите проект **WebApp1** Если вы хотите его совпадать с пространством имен [загрузить образец](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) кода.</span><span class="sxs-lookup"><span data-stu-id="03e1b-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="03e1b-117">Выберите **ASP.NET Core веб-приложение** > **ОК** .</span><span class="sxs-lookup"><span data-stu-id="03e1b-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="03e1b-118">Выберите **ASP.NET Core 3,0** в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="03e1b-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="03e1b-119">Выберите **веб-приложение** > **ОК** .</span><span class="sxs-lookup"><span data-stu-id="03e1b-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="03e1b-120">Постройте и запустите проект.</span><span class="sxs-lookup"><span data-stu-id="03e1b-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="03e1b-121">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="03e1b-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="03e1b-122">Назовите проект **WebApp1** Если вы хотите его совпадать с пространством имен [загрузить образец](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) кода.</span><span class="sxs-lookup"><span data-stu-id="03e1b-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="03e1b-123">Выберите **ASP.NET Core веб-приложение** > **ОК** .</span><span class="sxs-lookup"><span data-stu-id="03e1b-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="03e1b-124">Выберите **ASP.NET Core 2,2** в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="03e1b-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="03e1b-125">Выберите **веб-приложение** > **ОК** .</span><span class="sxs-lookup"><span data-stu-id="03e1b-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="03e1b-126">Постройте и запустите проект.</span><span class="sxs-lookup"><span data-stu-id="03e1b-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="03e1b-127">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="03e1b-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="03e1b-128">Запустите шаблон удостоверений</span><span class="sxs-lookup"><span data-stu-id="03e1b-128">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03e1b-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03e1b-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="03e1b-130">Из **обозревателе решений**, правой кнопкой мыши проект > **добавить** > **создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="03e1b-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="03e1b-131">В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="03e1b-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="03e1b-132">В диалоговом окне **Добавление удостоверения** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="03e1b-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="03e1b-133">Выберите существующий файл макета *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="03e1b-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="03e1b-134">Выберите следующие файлы для переопределения:</span><span class="sxs-lookup"><span data-stu-id="03e1b-134">Select the following files to override:</span></span>
    * <span data-ttu-id="03e1b-135">**Учетная запись: регистрация**</span><span class="sxs-lookup"><span data-stu-id="03e1b-135">**Account/Register**</span></span>
    * <span data-ttu-id="03e1b-136">**Учетная запись и управление/индексов**</span><span class="sxs-lookup"><span data-stu-id="03e1b-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="03e1b-137">Выберите **+** кнопку, чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="03e1b-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="03e1b-138">Выберите тип (**WebApp1.Models.WebApp1Context** Если проект называется **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="03e1b-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="03e1b-139">Выберите **+** кнопку, чтобы создать новый **класс пользователя**.</span><span class="sxs-lookup"><span data-stu-id="03e1b-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="03e1b-140">Выберите тип (**WebApp1User** Если проект называется **WebApp1**) > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="03e1b-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="03e1b-141">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="03e1b-141">Select **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="03e1b-142">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="03e1b-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="03e1b-143">Если вы еще не установлен шаблон ASP.NET Core, установите его:</span><span class="sxs-lookup"><span data-stu-id="03e1b-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="03e1b-144">Добавьте ссылку на пакет [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (csproj).</span><span class="sxs-lookup"><span data-stu-id="03e1b-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="03e1b-145">Выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="03e1b-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="03e1b-146">Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="03e1b-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="03e1b-147">В папке проекта запустите шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="03e1b-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="03e1b-148">Следуя инструкциям из [миграция, UseAuthentication и макет](xref:security/authentication/scaffold-identity#efm) для выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="03e1b-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="03e1b-149">Создание миграции и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="03e1b-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="03e1b-150">Добавьте `UseAuthentication` в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="03e1b-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="03e1b-151">Добавление `<partial name="_LoginPartial" />` с файлами макетов.</span><span class="sxs-lookup"><span data-stu-id="03e1b-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="03e1b-152">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="03e1b-152">Test the app:</span></span>
  * <span data-ttu-id="03e1b-153">Регистрация пользователя</span><span class="sxs-lookup"><span data-stu-id="03e1b-153">Register a user</span></span>
  * <span data-ttu-id="03e1b-154">Выберите новое имя пользователя (рядом с полем **выхода** ссылку).</span><span class="sxs-lookup"><span data-stu-id="03e1b-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="03e1b-155">Может потребоваться развернуть окно или выберите значок панели навигации, чтобы отобразить имя пользователя и другие ссылки.</span><span class="sxs-lookup"><span data-stu-id="03e1b-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="03e1b-156">Выберите **персональных данных** вкладки.</span><span class="sxs-lookup"><span data-stu-id="03e1b-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="03e1b-157">Выберите **загрузить** кнопку и изучить *PersonalData.json* файла.</span><span class="sxs-lookup"><span data-stu-id="03e1b-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="03e1b-158">Тест **удалить** кнопку, которая удаляет вошедшего пользователя.</span><span class="sxs-lookup"><span data-stu-id="03e1b-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="03e1b-159">Добавить пользовательские данные в базу данных удостоверений</span><span class="sxs-lookup"><span data-stu-id="03e1b-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="03e1b-160">Обновление `IdentityUser` производного класса с пользовательскими свойствами.</span><span class="sxs-lookup"><span data-stu-id="03e1b-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="03e1b-161">Если вы с именем проекта WebApp1, этот файл имеет имя *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="03e1b-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="03e1b-162">Обновление файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="03e1b-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="03e1b-163">Свойства с атрибутом [персоналдата](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) :</span><span class="sxs-lookup"><span data-stu-id="03e1b-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="03e1b-164">Удалено, когда *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* страница Razor вызывает `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="03e1b-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="03e1b-165">Включенные в загруженные данные по *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="03e1b-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="03e1b-166">Обновление страницы Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="03e1b-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="03e1b-167">Обновление `InputModel` в *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* на следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="03e1b-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="03e1b-168">Обновление *Areas/Identity/Pages/Account/Manage/Index.cshtml* с выделенную ниже разметку:</span><span class="sxs-lookup"><span data-stu-id="03e1b-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="03e1b-169">Обновление *Areas/Identity/Pages/Account/Manage/Index.cshtml* с выделенную ниже разметку:</span><span class="sxs-lookup"><span data-stu-id="03e1b-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="03e1b-170">Обновление страницы Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="03e1b-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="03e1b-171">Обновление `InputModel` в *Areas/Identity/Pages/Account/Register.cshtml.cs* на следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="03e1b-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="03e1b-172">Обновление *Areas/Identity/Pages/Account/Register.cshtml* с выделенную ниже разметку:</span><span class="sxs-lookup"><span data-stu-id="03e1b-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="03e1b-173">Обновление *Areas/Identity/Pages/Account/Register.cshtml* с выделенную ниже разметку:</span><span class="sxs-lookup"><span data-stu-id="03e1b-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="03e1b-174">Постройте проект.</span><span class="sxs-lookup"><span data-stu-id="03e1b-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="03e1b-175">Добавьте миграцию для пользовательских данных</span><span class="sxs-lookup"><span data-stu-id="03e1b-175">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03e1b-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03e1b-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="03e1b-177">В Visual Studio **консоль диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="03e1b-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="03e1b-178">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="03e1b-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="03e1b-179">Тест создание, просмотр, загрузка, удалить пользовательские данные</span><span class="sxs-lookup"><span data-stu-id="03e1b-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="03e1b-180">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="03e1b-180">Test the app:</span></span>

* <span data-ttu-id="03e1b-181">Регистрация нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="03e1b-181">Register a new user.</span></span>
* <span data-ttu-id="03e1b-182">Просмотр пользовательских данных на `/Identity/Account/Manage` страницы.</span><span class="sxs-lookup"><span data-stu-id="03e1b-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="03e1b-183">Скачать и просмотреть личные данные пользователей из `/Identity/Account/Manage/PersonalData` страницы.</span><span class="sxs-lookup"><span data-stu-id="03e1b-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
