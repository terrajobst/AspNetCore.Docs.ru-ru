<a name="cli"></a>
## <a name="perform-initial-migration"></a>Выполнение первоначальной миграции

Из командной строки выполните следующие команды интерфейса командной строки .NET Core.

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

Команда `dotnet ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных. Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MvcMovieContext.cs*). Аргумент `Initial` используется для присвоения имен миграциям. Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию. Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).

Команда `dotnet ef database update` выполняет `Up` метод в файле *Migrations/\<отметка-времени>_InitialCreate.cs*, который создает базу данных.
