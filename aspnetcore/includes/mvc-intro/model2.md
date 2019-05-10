<a name="dc"></a>

Добавьте следующий класс `MvcMovieContext` в папку *Models*:  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

Представленный выше код создает свойство `DbSet` для набора сущностей. В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>Добавление строки подключения базы данных

Добавьте строку подключения в файл *appsettings.json*:

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a>Добавьте необходимые пакеты NuGet

Выполните следующую команду .NET Core CLI, чтобы добавить в проект SQLite и CodeGeneration.Design:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

Пакет `Microsoft.VisualStudio.Web.CodeGeneration.Design` необходим для формирования шаблонов.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Регистрация контекста базы данных

Добавьте следующие инструкции `using` в начало файла *Startup.cs*.

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

Соберите проект как проверку на ошибки.