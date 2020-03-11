::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a90a1-101">Запуск шаблона идентификации:</span><span class="sxs-lookup"><span data-stu-id="a90a1-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a90a1-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a90a1-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a90a1-103">В **Обозреватель решений**щелкните правой кнопкой мыши проект > **Добавить** > новый шаблонный **элемент**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a90a1-104">В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="a90a1-105">В диалоговом окне **Добавление удостоверения** выберите нужные параметры.</span><span class="sxs-lookup"><span data-stu-id="a90a1-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="a90a1-106">Выберите существующую страницу макета, или файл макета будет перезаписан с неверной разметкой.</span><span class="sxs-lookup"><span data-stu-id="a90a1-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="a90a1-107">При выборе существующего файла *\_Layout. cshtml* он **не** перезаписывается.</span><span class="sxs-lookup"><span data-stu-id="a90a1-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="a90a1-108">Например: `~/Pages/Shared/_Layout.cshtml` для Razor Pages `~/Views/Shared/_Layout.cshtml` для проектов MVC</span><span class="sxs-lookup"><span data-stu-id="a90a1-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="a90a1-109">Чтобы использовать существующий контекст данных, выберите по меньшей мере один файл для переопределения.</span><span class="sxs-lookup"><span data-stu-id="a90a1-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="a90a1-110">Необходимо выбрать по крайней мере один файл для добавления контекста данных.</span><span class="sxs-lookup"><span data-stu-id="a90a1-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="a90a1-111">Выберите класс контекста данных.</span><span class="sxs-lookup"><span data-stu-id="a90a1-111">Select your data context class.</span></span>
  * <span data-ttu-id="a90a1-112">Выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-112">Select **Add**.</span></span>
* <span data-ttu-id="a90a1-113">Чтобы создать новый контекст пользователя и, возможно, создать настраиваемый класс пользователя для удостоверения:</span><span class="sxs-lookup"><span data-stu-id="a90a1-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="a90a1-114">Нажмите кнопку **+** , чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="a90a1-115">Выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-115">Select **Add**.</span></span>

<span data-ttu-id="a90a1-116">Примечание. Если вы создаете новый контекст пользователя, вам не нужно выбирать файл для переопределения.</span><span class="sxs-lookup"><span data-stu-id="a90a1-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="a90a1-117">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a90a1-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a90a1-118">Если вы еще не установлен шаблон ASP.NET Core, установите его:</span><span class="sxs-lookup"><span data-stu-id="a90a1-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="a90a1-119">Добавьте необходимые ссылки на пакет NuGet в файл проекта (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="a90a1-119">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="a90a1-120">Выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="a90a1-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="a90a1-121">Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="a90a1-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="a90a1-122">В папке проекта запустите механизм формирования удостоверений с нужными параметрами.</span><span class="sxs-lookup"><span data-stu-id="a90a1-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="a90a1-123">Например, чтобы настроить удостоверение с пользовательским интерфейсом по умолчанию и минимальным числом файлов, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="a90a1-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="a90a1-124">Используйте правильное полное имя для контекста базы данных:</span><span class="sxs-lookup"><span data-stu-id="a90a1-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="a90a1-125">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="a90a1-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="a90a1-126">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="a90a1-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="a90a1-127">Пример:</span><span class="sxs-lookup"><span data-stu-id="a90a1-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="a90a1-128">Если вы запустите механизм формирования идентификаторов без указания флага `--files` или флага `--useDefaultUI`, в проекте будут созданы все доступные страницы пользовательского интерфейса идентификации.</span><span class="sxs-lookup"><span data-stu-id="a90a1-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a90a1-129">Запуск шаблона идентификации:</span><span class="sxs-lookup"><span data-stu-id="a90a1-129">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="a90a1-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a90a1-130">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a90a1-131">В **Обозреватель решений**щелкните правой кнопкой мыши проект > **Добавить** > новый шаблонный **элемент**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-131">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a90a1-132">В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-132">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="a90a1-133">В диалоговом окне **Добавление удостоверения** выберите нужные параметры.</span><span class="sxs-lookup"><span data-stu-id="a90a1-133">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="a90a1-134">Выберите существующую страницу макета, или файл макета будет перезаписан с неверной разметкой.</span><span class="sxs-lookup"><span data-stu-id="a90a1-134">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="a90a1-135">При выборе существующего файла *\_Layout. cshtml* он **не** перезаписывается.</span><span class="sxs-lookup"><span data-stu-id="a90a1-135">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="a90a1-136">Например: `~/Pages/Shared/_Layout.cshtml` для Razor Pages `~/Views/Shared/_Layout.cshtml` для проектов MVC</span><span class="sxs-lookup"><span data-stu-id="a90a1-136">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="a90a1-137">Чтобы использовать существующий контекст данных, выберите по меньшей мере один файл для переопределения.</span><span class="sxs-lookup"><span data-stu-id="a90a1-137">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="a90a1-138">Необходимо выбрать по крайней мере один файл для добавления контекста данных.</span><span class="sxs-lookup"><span data-stu-id="a90a1-138">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="a90a1-139">Выберите класс контекста данных.</span><span class="sxs-lookup"><span data-stu-id="a90a1-139">Select your data context class.</span></span>
  * <span data-ttu-id="a90a1-140">Выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-140">Select **Add**.</span></span>
* <span data-ttu-id="a90a1-141">Чтобы создать новый контекст пользователя и, возможно, создать настраиваемый класс пользователя для удостоверения:</span><span class="sxs-lookup"><span data-stu-id="a90a1-141">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="a90a1-142">Нажмите кнопку **+** , чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-142">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="a90a1-143">Выберите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="a90a1-143">Select **Add**.</span></span>

<span data-ttu-id="a90a1-144">Примечание. Если вы создаете новый контекст пользователя, вам не нужно выбирать файл для переопределения.</span><span class="sxs-lookup"><span data-stu-id="a90a1-144">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="a90a1-145">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a90a1-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a90a1-146">Если вы еще не установлен шаблон ASP.NET Core, установите его:</span><span class="sxs-lookup"><span data-stu-id="a90a1-146">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="a90a1-147">Добавьте ссылку на пакет в [Microsoft. VisualStudio. Web. стратегию. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="a90a1-147">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="a90a1-148">Выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="a90a1-148">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="a90a1-149">Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="a90a1-149">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="a90a1-150">В папке проекта запустите механизм формирования удостоверений с нужными параметрами.</span><span class="sxs-lookup"><span data-stu-id="a90a1-150">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="a90a1-151">Например, чтобы настроить удостоверение с пользовательским интерфейсом по умолчанию и минимальным числом файлов, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="a90a1-151">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="a90a1-152">Используйте правильное полное имя для контекста базы данных:</span><span class="sxs-lookup"><span data-stu-id="a90a1-152">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="a90a1-153">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="a90a1-153">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="a90a1-154">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="a90a1-154">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="a90a1-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="a90a1-155">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="a90a1-156">Если вы запустите механизм формирования идентификаторов без указания флага `--files` или флага `--useDefaultUI`, в проекте будут созданы все доступные страницы пользовательского интерфейса идентификации.</span><span class="sxs-lookup"><span data-stu-id="a90a1-156">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end