<a name="codegenerator"></a> В следующей таблице представлены параметры генератора кода ASP.NET Core:

| Параметр               | Описание|
| ----------------- | ------------ |
| -m  | Имя модели |
| -dc  | Класс `DbContext` для использования. |
| -udl | Использование макета по умолчанию |
| -outDir | Относительный путь к папке выходных данных для создания представлений |
| --referenceScriptLibraries | Добавляет `_ValidationScriptsPartial` для страниц редактирования и создания |

Чтобы получить справку по команде `aspnet-codegenerator razorpage`, используйте коммутатор `h`.

```console
dotnet aspnet-codegenerator razorpage -h
```
