---
title: Добавить, загрузки и удаления данных пользователя для удостоверения в проекте ASP.NET Core
author: rick-anderson
description: Узнайте, как добавить пользовательские данные для удостоверения в проекте ASP.NET Core. Удаление данных в соответствии с GDPR.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 7a67f55da0e685ed3fd5badb30e8be683411a5ae
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653284"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="cf2e9-104">Добавление, скачивание и удаление пользовательских данных для удостоверений в проекте ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf2e9-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="cf2e9-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cf2e9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cf2e9-106">В этой статье показано, как сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-106">This article shows how to:</span></span>

* <span data-ttu-id="cf2e9-107">Добавьте пользовательские данные веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="cf2e9-108">Пометьте пользовательскую модель данных атрибутом <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute>, чтобы она была автоматически доступна для скачивания и удаления.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="cf2e9-109">Обеспечение возможности загрузки и удаления данных помогает удовлетворить требования [GDPR](xref:security/gdpr) .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="cf2e9-110">В примере проекта создается на основе веб-приложения Razor Pages, но инструкции одинаковы для веб-приложения ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="cf2e9-111">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cf2e9-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf2e9-112">предварительные требования</span><span class="sxs-lookup"><span data-stu-id="cf2e9-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="cf2e9-113">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="cf2e9-113">Create a Razor web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="cf2e9-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf2e9-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="cf2e9-115">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="cf2e9-116">Присвойте проекту имя **APP1** , если вы хотите, чтобы оно соответствовало пространству имен примера кода для [скачивания](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="cf2e9-117">Выберите **ASP.NET Core веб-приложение** > **ОК** .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="cf2e9-118">Выберите **ASP.NET Core 3,0** в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="cf2e9-119">Выберите **веб-приложение** > **ОК** .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="cf2e9-120">Постройте и запустите проект.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="cf2e9-121">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="cf2e9-122">Присвойте проекту имя **APP1** , если вы хотите, чтобы оно соответствовало пространству имен примера кода для [скачивания](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="cf2e9-123">Выберите **ASP.NET Core веб-приложение** > **ОК** .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="cf2e9-124">Выберите **ASP.NET Core 2,2** в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="cf2e9-125">Выберите **веб-приложение** > **ОК** .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="cf2e9-126">Постройте и запустите проект.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="cf2e9-127">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cf2e9-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="cf2e9-128">Запустите шаблон удостоверений</span><span class="sxs-lookup"><span data-stu-id="cf2e9-128">Run the Identity scaffolder</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="cf2e9-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf2e9-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cf2e9-130">В **Обозреватель решений**щелкните правой кнопкой мыши проект > **Добавить** > новый шаблонный **элемент**.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="cf2e9-131">В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="cf2e9-132">В диалоговом окне **Добавление удостоверения** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="cf2e9-133">Выберите существующий файл макета *~/пажес/шаред/_layout. cshtml*</span><span class="sxs-lookup"><span data-stu-id="cf2e9-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="cf2e9-134">Выберите следующие файлы для переопределения:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-134">Select the following files to override:</span></span>
    * <span data-ttu-id="cf2e9-135">**Учетная запись или регистр**</span><span class="sxs-lookup"><span data-stu-id="cf2e9-135">**Account/Register**</span></span>
    * <span data-ttu-id="cf2e9-136">**Учетная запись/управление/индекс**</span><span class="sxs-lookup"><span data-stu-id="cf2e9-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="cf2e9-137">Нажмите кнопку **+** , чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="cf2e9-138">Примите тип ("имя_проекта **. Models. WebApp1Context** ", если проект называется " **APP1**").</span><span class="sxs-lookup"><span data-stu-id="cf2e9-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="cf2e9-139">Нажмите кнопку **+** , чтобы создать новый **класс пользователя**.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="cf2e9-140">Примите тип (**WebApp1User** , если проект называется "имя_проекта **") >** **добавить**.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="cf2e9-141">Выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-141">Select **Add**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="cf2e9-142">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cf2e9-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="cf2e9-143">Если вы еще не установлен шаблон ASP.NET Core, установите его:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="cf2e9-144">Добавьте ссылку на пакет в [Microsoft. VisualStudio. Web. стратегию. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (с расширением CSPROJ).</span><span class="sxs-lookup"><span data-stu-id="cf2e9-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="cf2e9-145">Выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="cf2e9-146">Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="cf2e9-147">В папке проекта запустите шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="cf2e9-148">Следуйте инструкциям в разделе [миграция, усеаусентикатион и макет](xref:security/authentication/scaffold-identity#efm) , чтобы выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="cf2e9-149">Создание миграции и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="cf2e9-150">Добавлен `UseAuthentication` в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="cf2e9-151">Добавьте `<partial name="_LoginPartial" />` в файл макета.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="cf2e9-152">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-152">Test the app:</span></span>
  * <span data-ttu-id="cf2e9-153">Регистрация пользователя</span><span class="sxs-lookup"><span data-stu-id="cf2e9-153">Register a user</span></span>
  * <span data-ttu-id="cf2e9-154">Выберите новое имя пользователя (рядом с ссылкой для **выхода** ).</span><span class="sxs-lookup"><span data-stu-id="cf2e9-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="cf2e9-155">Может потребоваться развернуть окно или выберите значок панели навигации, чтобы отобразить имя пользователя и другие ссылки.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="cf2e9-156">Перейдите на вкладку **личные данные** .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="cf2e9-157">Нажмите кнопку **скачать** и рассмотрели файл *персоналдата. JSON* .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="cf2e9-158">Протестируйте кнопку **Удалить** , которая удаляет пользователя, выполнившего вход в систему.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="cf2e9-159">Добавить пользовательские данные в базу данных удостоверений</span><span class="sxs-lookup"><span data-stu-id="cf2e9-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="cf2e9-160">Обновление производного класса `IdentityUser` с помощью пользовательских свойств.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="cf2e9-161">Если вы назвали имя проекта Project, файл будет называться *Areas/Identity/Data/WebApp1User. CS*.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="cf2e9-162">Обновление файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="cf2e9-163">Свойства с атрибутом [персоналдата](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) :</span><span class="sxs-lookup"><span data-stu-id="cf2e9-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="cf2e9-164">Удаляется, когда страница Razor *Areas/Identity/Pages/Account/Manage/делетеперсоналдата. cshtml* вызывает `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="cf2e9-165">Включается в Скачанные данные на странице Razor *Areas/Identity/Pages/Account/Manage/довнлоадперсоналдата. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="cf2e9-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="cf2e9-166">Обновление страницы Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="cf2e9-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="cf2e9-167">Обновите `InputModel` в *области/удостоверение/страницы/учетная запись/управление/index. cshtml. CS* со следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="cf2e9-168">Обновите *области, идентификаторы, страницы, учетные записи, а также управление/index. cshtml* с помощью следующей выделенной разметки:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="cf2e9-169">Обновите *области, идентификаторы, страницы, учетные записи, а также управление/index. cshtml* с помощью следующей выделенной разметки:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="cf2e9-170">Обновление страницы Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="cf2e9-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="cf2e9-171">Обновите `InputModel` в *области, Identity, Pages/Account/Register. cshtml. CS* со следующим выделенным кодом:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="cf2e9-172">Обновите *области, идентификаторы, страницы, учетную запись или Register. cshtml* со следующей выделенной разметкой:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="cf2e9-173">Обновите *области, идентификаторы, страницы, учетную запись или Register. cshtml* со следующей выделенной разметкой:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="cf2e9-174">Создайте проект.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="cf2e9-175">Добавьте миграцию для пользовательских данных</span><span class="sxs-lookup"><span data-stu-id="cf2e9-175">Add a migration for the custom user data</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="cf2e9-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf2e9-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cf2e9-177">В **консоли диспетчера пакетов**Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="cf2e9-178">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cf2e9-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="cf2e9-179">Тест создание, просмотр, загрузка, удалить пользовательские данные</span><span class="sxs-lookup"><span data-stu-id="cf2e9-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="cf2e9-180">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="cf2e9-180">Test the app:</span></span>

* <span data-ttu-id="cf2e9-181">Регистрация нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-181">Register a new user.</span></span>
* <span data-ttu-id="cf2e9-182">Просмотр настраиваемых данных пользователя на странице `/Identity/Account/Manage`.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="cf2e9-183">Скачайте и просмотрите персональные данные пользователей на странице `/Identity/Account/Manage/PersonalData`.</span><span class="sxs-lookup"><span data-stu-id="cf2e9-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
