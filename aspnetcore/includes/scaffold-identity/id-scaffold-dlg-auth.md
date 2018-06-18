<span data-ttu-id="f86f4-101">Запустите scaffolder удостоверений:</span><span class="sxs-lookup"><span data-stu-id="f86f4-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f86f4-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f86f4-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f86f4-103">Из **обозревателе решений**, щелкните правой кнопкой мыши по проекту > **добавить** > **новый элемент формирования шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="f86f4-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="f86f4-104">В левой области **Добавление формирования шаблонов** диалогового окна выберите **удостоверение** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="f86f4-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="f86f4-105">В **добавить удостоверение** диалоговое окно, выберите нужные параметры.</span><span class="sxs-lookup"><span data-stu-id="f86f4-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="f86f4-106">Выберите существующий страница макета или файл макета будет заменен неверные разметки.</span><span class="sxs-lookup"><span data-stu-id="f86f4-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="f86f4-107">При выборе существующего файла _Layout.cshtml **не** перезаписан.</span><span class="sxs-lookup"><span data-stu-id="f86f4-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="f86f4-108">Например `~/Pages/Shared/_Layout.cshtml` страниц Razor `~/Views/Shared/_Layout.cshtml` для проектов MVC</span><span class="sxs-lookup"><span data-stu-id="f86f4-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="f86f4-109">Для использования существующего контекста данных, выберите хотя бы один файл для переопределения.</span><span class="sxs-lookup"><span data-stu-id="f86f4-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="f86f4-110">Необходимо выбрать по крайней мере один файл, чтобы добавить контекста данных.</span><span class="sxs-lookup"><span data-stu-id="f86f4-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="f86f4-111">Выберите класс контекста данных.</span><span class="sxs-lookup"><span data-stu-id="f86f4-111">Select your data context class.</span></span>
  * <span data-ttu-id="f86f4-112">Выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="f86f4-112">Select **ADD**.</span></span>
* <span data-ttu-id="f86f4-113">Для создания нового контекста пользователя и возможно создать пользовательский класс для удостоверения:</span><span class="sxs-lookup"><span data-stu-id="f86f4-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="f86f4-114">Выберите **+** кнопку, чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="f86f4-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="f86f4-115">Выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="f86f4-115">Select **ADD**.</span></span>

<span data-ttu-id="f86f4-116">Примечание: Если вы создаете новый контекст пользователя, не нужно выбрать файл для переопределения.</span><span class="sxs-lookup"><span data-stu-id="f86f4-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f86f4-117">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="f86f4-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f86f4-118">Если ASP.NET scaffolder ранее не установлен, установите его:</span><span class="sxs-lookup"><span data-stu-id="f86f4-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="f86f4-119">Добавьте ссылку на пакет [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в проект (\*.csproj) файла.</span><span class="sxs-lookup"><span data-stu-id="f86f4-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="f86f4-120">В каталоге проекта, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f86f4-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="f86f4-121">Выполните следующую команду, чтобы список параметров scaffolder удостоверений:</span><span class="sxs-lookup"><span data-stu-id="f86f4-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="f86f4-122">В папке проекта запустите scaffolder удостоверение с нужные параметры.</span><span class="sxs-lookup"><span data-stu-id="f86f4-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="f86f4-123">Например для настройки удостоверения по умолчанию пользовательского интерфейса и минимальное количество файлов, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="f86f4-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="f86f4-124">Используйте правильное полное имя в контексте базы данных:</span><span class="sxs-lookup"><span data-stu-id="f86f4-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
