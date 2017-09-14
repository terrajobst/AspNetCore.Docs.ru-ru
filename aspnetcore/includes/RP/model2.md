Добавьте в класс `Movie` следующие свойства:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

Поле `ID` является обязательным для первичного ключа базы данных.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Добавление класса контекста для базы данных

Добавьте производный класс `DbContext` с именем *MovieContext.cs* в папку *Models*.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs)]

Представленный выше код создает свойство `DbSet` для набора сущностей. В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.