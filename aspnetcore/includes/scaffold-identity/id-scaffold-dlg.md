<span data-ttu-id="032ba-101">Запуск шаблона идентификации:</span><span class="sxs-lookup"><span data-stu-id="032ba-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="032ba-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="032ba-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="032ba-103">Из **обозревателе решений**, правой кнопкой мыши проект > **добавить** > **создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="032ba-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="032ba-104">В области слева от **Добавление шаблона** диалоговое окно, выберите **удостоверений** > **добавить**.</span><span class="sxs-lookup"><span data-stu-id="032ba-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="032ba-105">В диалоговом окне **Добавление удостоверения** выберите нужные параметры.</span><span class="sxs-lookup"><span data-stu-id="032ba-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="032ba-106">Выберите существующую страницу макета, или файл макета будет перезаписан с неверной разметкой.</span><span class="sxs-lookup"><span data-stu-id="032ba-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="032ba-107">Пример `~/Pages/Shared/_Layout.cshtml` RazorPages`~/Views/Shared/_Layout.cshtml` для проектов MVC</span><span class="sxs-lookup"><span data-stu-id="032ba-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="032ba-108">Выберите **+** кнопку, чтобы создать новый **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="032ba-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="032ba-109">Выберите **добавить**.</span><span class="sxs-lookup"><span data-stu-id="032ba-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="032ba-110">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="032ba-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="032ba-111">Если вы еще не установлен шаблон ASP.NET Core, установите его:</span><span class="sxs-lookup"><span data-stu-id="032ba-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="032ba-112">Добавьте ссылку на пакет в [Microsoft. VisualStudio. Web. стратегию. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) в файл проекта (\*с расширением CSPROJ).</span><span class="sxs-lookup"><span data-stu-id="032ba-112">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="032ba-113">Выполните следующую команду в каталоге проекта:</span><span class="sxs-lookup"><span data-stu-id="032ba-113">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="032ba-114">Выполните следующую команду, чтобы получить список вариантов шаблон удостоверений:</span><span class="sxs-lookup"><span data-stu-id="032ba-114">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="032ba-115">В папке проекта запустите механизм формирования удостоверений с нужными параметрами.</span><span class="sxs-lookup"><span data-stu-id="032ba-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="032ba-116">Например, чтобы настроить удостоверение с пользовательским интерфейсом по умолчанию и минимальным числом файлов, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="032ba-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
