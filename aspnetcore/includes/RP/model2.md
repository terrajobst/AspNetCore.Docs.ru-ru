<span data-ttu-id="5b7e4-101">Добавьте в класс `Movie` следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="5b7e4-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="5b7e4-102">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="5b7e4-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="5b7e4-103">Добавление класса контекста для базы данных</span><span class="sxs-lookup"><span data-stu-id="5b7e4-103">Add a database context class</span></span>

<span data-ttu-id="5b7e4-104">Добавьте производный класс `DbContext` с именем *MovieContext.cs* в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="5b7e4-104">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>
[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]

<span data-ttu-id="5b7e4-105">Представленный выше код создает свойство `DbSet` для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="5b7e4-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="5b7e4-106">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="5b7e4-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>
