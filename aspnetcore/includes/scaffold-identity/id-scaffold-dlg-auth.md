<span data-ttu-id="bd7ed-101">Запустите шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="bd7ed-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bd7ed-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bd7ed-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bd7ed-103">Из **обозревателе решений**, правой кнопкой мыши проект > **добавить** > **создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="bd7ed-104">В области слева от **Добавление шаблона** диалоговое окно, выберите **удостоверений** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="bd7ed-105">В **удостоверение ADD** диалоговое окно, выберите нужные параметры.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="bd7ed-106">Выберите существующий макет страницы или файл макета будет заменен неверные разметки.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="bd7ed-107">Если в имеющемся  *\_Layout.cshtml* выбран файл, это **не** перезаписаны.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="bd7ed-108">Например `~/Pages/Shared/_Layout.cshtml` для Razor Pages `~/Views/Shared/_Layout.cshtml` для проектов MVC</span><span class="sxs-lookup"><span data-stu-id="bd7ed-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="bd7ed-109">Чтобы использовать существующий контекста данных, выберите хотя бы один файл для переопределения.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="bd7ed-110">Необходимо выбрать по крайней мере один файл, чтобы добавить контекста данных.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="bd7ed-111">Выберите класс контекста данных.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-111">Select your data context class.</span></span>
  * <span data-ttu-id="bd7ed-112">Выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-112">Select **ADD**.</span></span>
* <span data-ttu-id="bd7ed-113">Для создания нового контекста пользователя и возможно создать пользовательский класс для удостоверения:</span><span class="sxs-lookup"><span data-stu-id="bd7ed-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="bd7ed-114">Выберите **+** кнопку, чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="bd7ed-115">Выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-115">Select **ADD**.</span></span>

<span data-ttu-id="bd7ed-116">Примечание. Если вы создаете новый контекст пользователя, у вас нет для выбора файла для переопределения.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bd7ed-117">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="bd7ed-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bd7ed-118">Если вы еще не установлен шаблон ASP.NET Core, установите его:</span><span class="sxs-lookup"><span data-stu-id="bd7ed-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="bd7ed-119">Добавьте ссылку на пакет [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в проект (\*.csproj) файла.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="bd7ed-120">Выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="bd7ed-120">Run the following command in the project directory:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="bd7ed-121">Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="bd7ed-121">Run the following command to list the Identity scaffolder options:</span></span>

```console
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="bd7ed-122">В папке проекта запустите шаблон удостоверение с нужные параметры.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="bd7ed-123">Например чтобы настроить удостоверение с помощью пользовательского интерфейса по умолчанию и минимальное число файлов, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="bd7ed-124">Используется правильное полное доменное имя для вашего контекста базы данных:</span><span class="sxs-lookup"><span data-stu-id="bd7ed-124">Use the correct fully qualified name for your DB context:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

<span data-ttu-id="bd7ed-125">PowerShell используется точка с запятой в качестве разделителя команды.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="bd7ed-126">Если вы используете PowerShell, escape-точка с запятой в списке файлов или поместить в список файлов в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="bd7ed-127">Например:</span><span class="sxs-lookup"><span data-stu-id="bd7ed-127">For example:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="bd7ed-128">Если запустить шаблон удостоверений без указания `--files` флаг или `--useDefaultUI` флаг, все доступные страницы удостоверение пользовательского интерфейса будет создан в проекте.</span><span class="sxs-lookup"><span data-stu-id="bd7ed-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---
