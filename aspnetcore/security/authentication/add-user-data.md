---
title: Добавить, загрузить и удалить пользовательские данные для удостоверения в проекте ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о добавлении пользовательские данные для удостоверения в проекте ASP.NET Core. Удаление данных в каждом GDPR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: ecd0e6d1c71b24309fab70fbb06af7731463bb0e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271961"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="790ce-104">Добавить, загрузить и удалить пользовательские данные для удостоверения в проекте ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="790ce-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="790ce-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="790ce-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="790ce-106">В этой статье показано, как:</span><span class="sxs-lookup"><span data-stu-id="790ce-106">This article shows how to:</span></span>

* <span data-ttu-id="790ce-107">Добавьте пользовательские данные для веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="790ce-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="790ce-108">Украшение пользовательские модели данных с [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) атрибута, поэтому оно автоматически доступно для загрузки и удаления.</span><span class="sxs-lookup"><span data-stu-id="790ce-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="790ce-109">Что делает данные могут быть загружены и удалить помогает удовлетворять [GDPR](xref:security/gdpr) требования.</span><span class="sxs-lookup"><span data-stu-id="790ce-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="790ce-110">В образце проекта создается на основе страниц Razor веб-приложения, но инструкции похожи для веб-приложения ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="790ce-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="790ce-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="790ce-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="790ce-112">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="790ce-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="790ce-113">Создание веб-приложения Razor</span><span class="sxs-lookup"><span data-stu-id="790ce-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="790ce-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="790ce-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="790ce-115">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="790ce-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="790ce-116">Назовите проект **WebApp1** Если вы хотите его совпадать с пространством имен [загрузить пример](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) кода.</span><span class="sxs-lookup"><span data-stu-id="790ce-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="790ce-117">Выберите **веб-приложения ASP.NET Core** > **ОК**</span><span class="sxs-lookup"><span data-stu-id="790ce-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="790ce-118">Выберите **ASP.NET Core 2.1** в раскрывающемся списке</span><span class="sxs-lookup"><span data-stu-id="790ce-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="790ce-119">Выберите **веб-приложение**  > **ОК**</span><span class="sxs-lookup"><span data-stu-id="790ce-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="790ce-120">Постройте и запустите проект.</span><span class="sxs-lookup"><span data-stu-id="790ce-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="790ce-121">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="790ce-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="790ce-122">Запустите scaffolder удостоверений</span><span class="sxs-lookup"><span data-stu-id="790ce-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="790ce-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="790ce-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="790ce-124">Из **обозревателе решений**, щелкните правой кнопкой мыши по проекту > **добавить** > **новый элемент формирования шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="790ce-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="790ce-125">В левой области **Добавление формирования шаблонов** диалогового окна выберите **удостоверение** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="790ce-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="790ce-126">В **добавить удостоверение** диалогового окна, следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="790ce-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="790ce-127">Выберите существующий файл макета *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="790ce-127">Select your existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="790ce-128">Выберите следующие файлы для переопределения:</span><span class="sxs-lookup"><span data-stu-id="790ce-128">Select the following files to override:</span></span>
    * <span data-ttu-id="790ce-129">**Регистрация учетной записи**</span><span class="sxs-lookup"><span data-stu-id="790ce-129">**Account/Register**</span></span>
    * <span data-ttu-id="790ce-130">**Учетная запись или управление и индекса**</span><span class="sxs-lookup"><span data-stu-id="790ce-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="790ce-131">Выберите **+** кнопку, чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="790ce-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="790ce-132">Принимает тип (**WebApp1.Models.WebApp1Context** Если имя проекта **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="790ce-132">Accept the type (**WebApp1.Models.WebApp1Context** if you named the project **WebApp1**).</span></span>
  * <span data-ttu-id="790ce-133">Выберите **+** кнопку, чтобы создать новый **класс пользователя**.</span><span class="sxs-lookup"><span data-stu-id="790ce-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="790ce-134">Принимает тип (**WebApp1User** Если имя проекта **WebApp1**) > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="790ce-134">Accept the type (**WebApp1User** if you named the project **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="790ce-135">Выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="790ce-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="790ce-136">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="790ce-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="790ce-137">Если ASP.NET scaffolder ранее не установлен, установите его:</span><span class="sxs-lookup"><span data-stu-id="790ce-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="790ce-138">Добавьте ссылку на пакет [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (CSPROJ-файл).</span><span class="sxs-lookup"><span data-stu-id="790ce-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="790ce-139">В каталоге проекта, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="790ce-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="790ce-140">Выполните следующую команду, чтобы список параметров scaffolder удостоверений:</span><span class="sxs-lookup"><span data-stu-id="790ce-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="790ce-141">В папке проекта запустите scaffolder удостоверений:</span><span class="sxs-lookup"><span data-stu-id="790ce-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="790ce-142">Следуйте инструкциям в [миграции UseAuthentication, а также макет](xref:security/authentication/scaffold-identity#efm) для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="790ce-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="790ce-143">Создание миграции и обновить базу данных.</span><span class="sxs-lookup"><span data-stu-id="790ce-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="790ce-144">Добавьте `UseAuthentication` в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="790ce-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="790ce-145">Добавить `<partial name="_LoginPartial" />` файл макета.</span><span class="sxs-lookup"><span data-stu-id="790ce-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="790ce-146">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="790ce-146">Test the app:</span></span>
  * <span data-ttu-id="790ce-147">Регистрация пользователя</span><span class="sxs-lookup"><span data-stu-id="790ce-147">Register a user</span></span>
  * <span data-ttu-id="790ce-148">Выберите новое имя пользователя (рядом с **выхода** связи).</span><span class="sxs-lookup"><span data-stu-id="790ce-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="790ce-149">Может потребоваться развернуть окно или выберите значок панели навигации, чтобы отобразить имя пользователя и другие ссылки.</span><span class="sxs-lookup"><span data-stu-id="790ce-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="790ce-150">Выберите **персональные данные** вкладки.</span><span class="sxs-lookup"><span data-stu-id="790ce-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="790ce-151">Выберите **загрузки** кнопку и изучить *PersonalData.json* файла.</span><span class="sxs-lookup"><span data-stu-id="790ce-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="790ce-152">Тест **удалить** кнопку, которая удаляет вошедшего пользователя.</span><span class="sxs-lookup"><span data-stu-id="790ce-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="790ce-153">Добавить пользовательские данные в базу данных удостоверений</span><span class="sxs-lookup"><span data-stu-id="790ce-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="790ce-154">Обновление `IdentityUser` производного класса с пользовательскими свойствами.</span><span class="sxs-lookup"><span data-stu-id="790ce-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="790ce-155">Если название проекта WebApp1 этот файл имеет имя *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="790ce-155">If you named your project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="790ce-156">Обновление файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="790ce-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="790ce-157">Свойства декорированных [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) являются:</span><span class="sxs-lookup"><span data-stu-id="790ce-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="790ce-158">Удалены при *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor страница вызывает `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="790ce-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="790ce-159">Включенные в загруженные данные по *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="790ce-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="790ce-160">Страница «обновление» Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="790ce-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="790ce-161">Обновление `InputModel` в *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* на следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="790ce-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="790ce-162">Обновление *Areas/Identity/Pages/Account/Manage/Index.cshtml* с следующую выделенную разметку:</span><span class="sxs-lookup"><span data-stu-id="790ce-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="790ce-163">Страница «обновление» Account/Register.cshtml</span><span class="sxs-lookup"><span data-stu-id="790ce-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="790ce-164">Обновление `InputModel` в *Areas/Identity/Pages/Account/Register.cshtml.cs* на следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="790ce-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="790ce-165">Обновление *Areas/Identity/Pages/Account/Register.cshtml* с следующую выделенную разметку:</span><span class="sxs-lookup"><span data-stu-id="790ce-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="790ce-166">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="790ce-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="790ce-167">Добавить миграции для пользовательские данные</span><span class="sxs-lookup"><span data-stu-id="790ce-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="790ce-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="790ce-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="790ce-169">В Visual Studio **консоль диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="790ce-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="790ce-170">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="790ce-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="790ce-171">Тест создание, просмотр, загрузка, удалить пользовательские данные</span><span class="sxs-lookup"><span data-stu-id="790ce-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="790ce-172">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="790ce-172">Test the app:</span></span>

* <span data-ttu-id="790ce-173">Регистрация нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="790ce-173">Register a new user.</span></span>
* <span data-ttu-id="790ce-174">Просмотр пользовательских данных на `/Identity/Account/Manage` страницы.</span><span class="sxs-lookup"><span data-stu-id="790ce-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="790ce-175">Загрузить и просмотреть личные данные пользователей из `/Identity/Account/Manage/PersonalData` страницы.</span><span class="sxs-lookup"><span data-stu-id="790ce-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
