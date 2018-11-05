<a name="cli"></a>

## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="e6a72-101">Добавление средств формирования шаблонов и выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="e6a72-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="e6a72-102">Добавьте следующие строки в файл *RazorPagesMovie.csproj* непосредственно перед закрывающим тегом `</Project>`:</span><span class="sxs-lookup"><span data-stu-id="e6a72-102">Add the following lines to the *RazorPagesMovie.csproj* file, just before the closing `</Project>` tag:</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```
  
<span data-ttu-id="e6a72-103">Из командной строки выполните следующие команды интерфейса командной строки .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6a72-103">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="e6a72-104">Элемент `DotNetCliToolReference` с командой `add package` устанавливают средства, необходимые для запуска ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="e6a72-104">The `DotNetCliToolReference` element and the `add package` command install the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="e6a72-105">Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="e6a72-105">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e6a72-106">Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="e6a72-106">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="e6a72-107">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="e6a72-107">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="e6a72-108">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="e6a72-108">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="e6a72-109">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="e6a72-109">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="e6a72-110">Команда `ef database update` выполняет `Up` метод в файле *Migrations/\<отметка-времени>_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="e6a72-110">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
