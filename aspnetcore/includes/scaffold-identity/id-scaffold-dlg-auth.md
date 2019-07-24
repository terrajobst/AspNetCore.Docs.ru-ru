<span data-ttu-id="d4d1b-101">Запуск шаблона идентификации:</span><span class="sxs-lookup"><span data-stu-id="d4d1b-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d4d1b-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4d1b-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d4d1b-103">Из **обозревателе решений**, правой кнопкой мыши проект > **добавить** > **создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d4d1b-104">В левой области диалогового окна **Добавление шаблона** выберите удостоверение  > **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="d4d1b-105">В диалоговом окне **Добавление удостоверения** выберите нужные параметры.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="d4d1b-106">Выберите существующую страницу макета, или файл макета будет перезаписан с неверной разметкой.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="d4d1b-107">При выборе существующего  *\_файла Layout. cshtml* он **не** перезаписывается.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="d4d1b-108">Пример: `~/Pages/Shared/_Layout.cshtml` для Razor Pages `~/Views/Shared/_Layout.cshtml` проектов MVC</span><span class="sxs-lookup"><span data-stu-id="d4d1b-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="d4d1b-109">Чтобы использовать существующий контекст данных, выберите по меньшей мере один файл для переопределения.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="d4d1b-110">Необходимо выбрать по крайней мере один файл для добавления контекста данных.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="d4d1b-111">Выберите класс контекста данных.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-111">Select your data context class.</span></span>
  * <span data-ttu-id="d4d1b-112">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-112">Select **Add**.</span></span>
* <span data-ttu-id="d4d1b-113">Чтобы создать новый контекст пользователя и, возможно, создать настраиваемый класс пользователя для удостоверения:</span><span class="sxs-lookup"><span data-stu-id="d4d1b-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="d4d1b-114">Выберите **+** кнопку, чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="d4d1b-115">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-115">Select **Add**.</span></span>

<span data-ttu-id="d4d1b-116">Примечание. Если вы создаете новый контекст пользователя, вам не нужно выбирать файл для переопределения.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d4d1b-117">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d4d1b-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d4d1b-118">Если вы еще не установлен шаблон ASP.NET Core, установите его:</span><span class="sxs-lookup"><span data-stu-id="d4d1b-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="d4d1b-119">Добавьте ссылку на пакет в [Microsoft. VisualStudio. Web. стратегию. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (\*с расширением CSPROJ).</span><span class="sxs-lookup"><span data-stu-id="d4d1b-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="d4d1b-120">Выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="d4d1b-120">Run the following command in the project directory:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="d4d1b-121">Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="d4d1b-121">Run the following command to list the Identity scaffolder options:</span></span>

```console
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="d4d1b-122">В папке проекта запустите механизм формирования удостоверений с нужными параметрами.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="d4d1b-123">Например, чтобы настроить удостоверение с пользовательским интерфейсом по умолчанию и минимальным числом файлов, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="d4d1b-124">Используйте правильное полное имя для контекста базы данных:</span><span class="sxs-lookup"><span data-stu-id="d4d1b-124">Use the correct fully qualified name for your DB context:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

<span data-ttu-id="d4d1b-125">В PowerShell используется точка с запятой в качестве разделителя команд.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="d4d1b-126">При использовании PowerShell заключите точку с запятой в список файлов или вставьте список файлов в двойные кавычки.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="d4d1b-127">Например:</span><span class="sxs-lookup"><span data-stu-id="d4d1b-127">For example:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="d4d1b-128">Если вы запустите механизм формирования удостоверений без указания `--files` флага `--useDefaultUI` или флага, в проекте будут созданы все доступные страницы пользовательского интерфейса удостоверений.</span><span class="sxs-lookup"><span data-stu-id="d4d1b-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---
