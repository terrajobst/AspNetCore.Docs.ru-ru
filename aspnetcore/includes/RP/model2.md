<span data-ttu-id="285d6-101">Добавьте в класс `Movie` следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="285d6-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="285d6-102">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="285d6-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="285d6-103">Добавление класса контекста для базы данных</span><span class="sxs-lookup"><span data-stu-id="285d6-103">Add a database context class</span></span>

<span data-ttu-id="285d6-104">Добавьте производный класс `DbContext` с именем *MovieContext.cs* в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="285d6-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?range=1-12,14-17,19-21)]

<span data-ttu-id="285d6-105">Представленный выше код создает свойство `DbSet` для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="285d6-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="285d6-106">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="285d6-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span> <span data-ttu-id="285d6-107">Свойство `DbSet` имеет имя `Movies`.</span><span class="sxs-lookup"><span data-stu-id="285d6-107">The `DbSet` property name is `Movies`.</span></span> <span data-ttu-id="285d6-108">Так как база данных использует имена в единственном числе, в примере `OnModelCreating` переопределяется, чтобы использовать имя таблицы в единственном числе (`Movie`).</span><span class="sxs-lookup"><span data-stu-id="285d6-108">Since the database uses singular names, the sample overrides `OnModelCreating` to use the singular form (`Movie`) for the table name.</span></span>
