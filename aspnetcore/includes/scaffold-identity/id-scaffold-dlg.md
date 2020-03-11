<span data-ttu-id="c7a48-101">Запуск шаблона идентификации:</span><span class="sxs-lookup"><span data-stu-id="c7a48-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c7a48-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c7a48-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c7a48-103">В **Обозреватель решений**щелкните правой кнопкой мыши проект > **Добавить** > новый шаблонный **элемент**.</span><span class="sxs-lookup"><span data-stu-id="c7a48-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c7a48-104">В левой области диалогового окна **Добавление шаблона** выберите **удостоверение** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="c7a48-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="c7a48-105">В диалоговом окне **Добавление удостоверения** выберите нужные параметры.</span><span class="sxs-lookup"><span data-stu-id="c7a48-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="c7a48-106">Выберите существующую страницу макета, или файл макета будет перезаписан с неверной разметкой.</span><span class="sxs-lookup"><span data-stu-id="c7a48-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="c7a48-107">Например `~/Pages/Shared/_Layout.cshtml` для Razor Pages `~/Views/Shared/_Layout.cshtml` для проектов MVC</span><span class="sxs-lookup"><span data-stu-id="c7a48-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="c7a48-108">Нажмите кнопку **+** , чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="c7a48-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="c7a48-109">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="c7a48-109">Select **ADD**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="c7a48-110">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c7a48-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c7a48-111">Если вы еще не установлен шаблон ASP.NET Core, установите его:</span><span class="sxs-lookup"><span data-stu-id="c7a48-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="c7a48-112">Добавьте необходимые ссылки на пакет NuGet в файл проекта (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="c7a48-112">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="c7a48-113">Выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="c7a48-113">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="c7a48-114">Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="c7a48-114">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="c7a48-115">В папке проекта запустите механизм формирования удостоверений с нужными параметрами.</span><span class="sxs-lookup"><span data-stu-id="c7a48-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="c7a48-116">Например, чтобы настроить удостоверение с пользовательским интерфейсом по умолчанию и минимальным числом файлов, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c7a48-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
