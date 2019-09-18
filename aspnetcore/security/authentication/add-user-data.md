---
title: Добавить, загрузки и удаления данных пользователя для удостоверения в проекте ASP.NET Core
author: rick-anderson
description: Узнайте, как добавить пользовательские данные для удостоверения в проекте ASP.NET Core. Удаление данных в соответствии с GDPR.
ms.author: riande
ms.date: 06/18/2019
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: f5a47ffd2e068414268ed9037d4376bfd21ba1bb
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080801"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="328e8-104">Добавление, скачивание и удаление пользовательских данных для удостоверений в проекте ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="328e8-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="328e8-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="328e8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="328e8-106">В этой статье показано, как:</span><span class="sxs-lookup"><span data-stu-id="328e8-106">This article shows how to:</span></span>

* <span data-ttu-id="328e8-107">Добавьте пользовательские данные веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="328e8-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="328e8-108">Снабдите пользовательскую модель <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> данных атрибутом, чтобы она автоматически была доступна для скачивания и удаления.</span><span class="sxs-lookup"><span data-stu-id="328e8-108">Decorate the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="328e8-109">Возможность загрузки и удаления данных помогает обеспечить соответствие [GDPR](xref:security/gdpr) требования.</span><span class="sxs-lookup"><span data-stu-id="328e8-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="328e8-110">В примере проекта создается на основе веб-приложения Razor Pages, но инструкции одинаковы для веб-приложения ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="328e8-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="328e8-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="328e8-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="328e8-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="328e8-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.2-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="328e8-113">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="328e8-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="328e8-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="328e8-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="328e8-115">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="328e8-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="328e8-116">Назовите проект **WebApp1** Если вы хотите его совпадать с пространством имен [загрузить образец](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) кода.</span><span class="sxs-lookup"><span data-stu-id="328e8-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="328e8-117">Выберите **веб-приложение ASP.NET Core** > **ОК**</span><span class="sxs-lookup"><span data-stu-id="328e8-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="328e8-118">Выберите **ASP.NET Core 2,2** в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="328e8-118">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="328e8-119">Выберите **веб-приложение**  > **ОК**</span><span class="sxs-lookup"><span data-stu-id="328e8-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="328e8-120">Постройте и запустите проект.</span><span class="sxs-lookup"><span data-stu-id="328e8-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="328e8-121">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="328e8-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="328e8-122">Запустите шаблон удостоверений</span><span class="sxs-lookup"><span data-stu-id="328e8-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="328e8-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="328e8-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="328e8-124">Из **обозревателе решений**, правой кнопкой мыши проект > **добавить** > **создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="328e8-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="328e8-125">В области слева от **Добавление шаблона** диалоговое окно, выберите **удостоверений** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="328e8-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="328e8-126">В **удостоверение ADD** диалоговом окне следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="328e8-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="328e8-127">Выберите существующий файл макета *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="328e8-127">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="328e8-128">Выберите следующие файлы для переопределения:</span><span class="sxs-lookup"><span data-stu-id="328e8-128">Select the following files to override:</span></span>
    * <span data-ttu-id="328e8-129">**Учетная запись: регистрация**</span><span class="sxs-lookup"><span data-stu-id="328e8-129">**Account/Register**</span></span>
    * <span data-ttu-id="328e8-130">**Учетная запись и управление/индексов**</span><span class="sxs-lookup"><span data-stu-id="328e8-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="328e8-131">Выберите **+** кнопку, чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="328e8-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="328e8-132">Выберите тип (**WebApp1.Models.WebApp1Context** Если проект называется **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="328e8-132">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="328e8-133">Выберите **+** кнопку, чтобы создать новый **класс пользователя**.</span><span class="sxs-lookup"><span data-stu-id="328e8-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="328e8-134">Выберите тип (**WebApp1User** Если проект называется **WebApp1**) > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="328e8-134">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="328e8-135">Выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="328e8-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="328e8-136">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="328e8-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="328e8-137">Если вы еще не установлен шаблон ASP.NET Core, установите его:</span><span class="sxs-lookup"><span data-stu-id="328e8-137">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="328e8-138">Добавьте ссылку на пакет [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (csproj).</span><span class="sxs-lookup"><span data-stu-id="328e8-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="328e8-139">Выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="328e8-139">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="328e8-140">Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="328e8-140">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="328e8-141">В папке проекта запустите шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="328e8-141">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="328e8-142">Следуя инструкциям из [миграция, UseAuthentication и макет](xref:security/authentication/scaffold-identity#efm) для выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="328e8-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="328e8-143">Создание миграции и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="328e8-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="328e8-144">Добавьте `UseAuthentication` в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="328e8-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="328e8-145">Добавление `<partial name="_LoginPartial" />` с файлами макетов.</span><span class="sxs-lookup"><span data-stu-id="328e8-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="328e8-146">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="328e8-146">Test the app:</span></span>
  * <span data-ttu-id="328e8-147">Регистрация пользователя</span><span class="sxs-lookup"><span data-stu-id="328e8-147">Register a user</span></span>
  * <span data-ttu-id="328e8-148">Выберите новое имя пользователя (рядом с полем **выхода** ссылку).</span><span class="sxs-lookup"><span data-stu-id="328e8-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="328e8-149">Может потребоваться развернуть окно или выберите значок панели навигации, чтобы отобразить имя пользователя и другие ссылки.</span><span class="sxs-lookup"><span data-stu-id="328e8-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="328e8-150">Выберите **персональных данных** вкладки.</span><span class="sxs-lookup"><span data-stu-id="328e8-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="328e8-151">Выберите **загрузить** кнопку и изучить *PersonalData.json* файла.</span><span class="sxs-lookup"><span data-stu-id="328e8-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="328e8-152">Тест **удалить** кнопку, которая удаляет вошедшего пользователя.</span><span class="sxs-lookup"><span data-stu-id="328e8-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="328e8-153">Добавить пользовательские данные в базу данных удостоверений</span><span class="sxs-lookup"><span data-stu-id="328e8-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="328e8-154">Обновление `IdentityUser` производного класса с пользовательскими свойствами.</span><span class="sxs-lookup"><span data-stu-id="328e8-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="328e8-155">Если вы с именем проекта WebApp1, этот файл имеет имя *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="328e8-155">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="328e8-156">Обновление файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="328e8-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="328e8-157">Свойства с атрибутом [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) , атрибут:</span><span class="sxs-lookup"><span data-stu-id="328e8-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="328e8-158">Удалено, когда *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* страница Razor вызывает `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="328e8-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="328e8-159">Включенные в загруженные данные по *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* страницу Razor.</span><span class="sxs-lookup"><span data-stu-id="328e8-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="328e8-160">Обновление страницы Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="328e8-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="328e8-161">Обновление `InputModel` в *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* на следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="328e8-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="328e8-162">Обновление *Areas/Identity/Pages/Account/Manage/Index.cshtml* с выделенную ниже разметку:</span><span class="sxs-lookup"><span data-stu-id="328e8-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="328e8-163">Обновление страницы Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="328e8-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="328e8-164">Обновление `InputModel` в *Areas/Identity/Pages/Account/Register.cshtml.cs* на следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="328e8-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="328e8-165">Обновление *Areas/Identity/Pages/Account/Register.cshtml* с выделенную ниже разметку:</span><span class="sxs-lookup"><span data-stu-id="328e8-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="328e8-166">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="328e8-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="328e8-167">Добавьте миграцию для пользовательских данных</span><span class="sxs-lookup"><span data-stu-id="328e8-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="328e8-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="328e8-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="328e8-169">В Visual Studio **консоль диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="328e8-169">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="328e8-170">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="328e8-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="328e8-171">Тест создание, просмотр, загрузка, удалить пользовательские данные</span><span class="sxs-lookup"><span data-stu-id="328e8-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="328e8-172">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="328e8-172">Test the app:</span></span>

* <span data-ttu-id="328e8-173">Регистрация нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="328e8-173">Register a new user.</span></span>
* <span data-ttu-id="328e8-174">Просмотр пользовательских данных на `/Identity/Account/Manage` страницы.</span><span class="sxs-lookup"><span data-stu-id="328e8-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="328e8-175">Скачать и просмотреть личные данные пользователей из `/Identity/Account/Manage/PersonalData` страницы.</span><span class="sxs-lookup"><span data-stu-id="328e8-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
