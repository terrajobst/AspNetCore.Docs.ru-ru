<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Добавление средств формирования шаблонов и выполнение первоначальной миграции

Добавьте следующие строки в файл *RazorPagesMovie.csproj* непосредственно перед закрывающим тегом `</Project>`:

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.1.0-preview1-final"/>
</ItemGroup>
```  
Из командной строки выполните следующие команды интерфейса командной строки .NET Core.

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

Элемент `DotNetCliToolReference` с командой `add package` устанавливают средства, необходимые для запуска ядра формирования шаблонов.

Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных. Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MovieContext.cs*). Аргумент `InitialCreate` используется для присвоения имен миграциям. Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию. Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).

Команда `ef database update` выполняет `Up` метод в файле *Migrations/\<отметка-времени>_InitialCreate.cs*, который создает базу данных.
