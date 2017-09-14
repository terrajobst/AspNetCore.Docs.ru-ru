<span data-ttu-id="1b043-101">Добавьте в класс `Movie` следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="1b043-101">Add the following properties to the `Movie` class:</span></span>

<span data-ttu-id="1b043-102">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]</span><span class="sxs-lookup"><span data-stu-id="1b043-102">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]</span></span>

<span data-ttu-id="1b043-103">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="1b043-103">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="1b043-104">Добавление класса контекста для базы данных</span><span class="sxs-lookup"><span data-stu-id="1b043-104">Add a database context class</span></span>

<span data-ttu-id="1b043-105">Добавьте производный класс `DbContext` с именем *MovieContext.cs* в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="1b043-105">Add a `DbContext` derived class named *MovieContext.cs* to the *Models* folder.</span></span>

<span data-ttu-id="1b043-106">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="1b043-106">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="1b043-107">Представленный выше код создает свойство `DbSet` для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="1b043-107">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="1b043-108">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="1b043-108">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>