---
ms.openlocfilehash: 86cf1874677dc8b79e3223fb0819eb1881c69a11
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265601"
---
<a name="dc"></a>

### <a name="add-a-database-context-class"></a>Добавление класса контекста для базы данных

Добавьте следующий класс `RazorPagesMovieContext` в папку *Models*:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

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
dotnet add package Microsoft.EntityFrameworkCore.Design

```

Пакет `Microsoft.VisualStudio.Web.CodeGeneration.Design` необходим для формирования шаблонов.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Регистрация контекста базы данных

Добавьте следующие инструкции `using` в начало файла *Startup.cs*.

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

Соберите проект как проверку на ошибки.
