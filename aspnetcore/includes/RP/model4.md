В следующей таблице представлены параметры генераторов кода ASP.NET Core.

| Параметр               | Описание|
| ----------------- | ------------ |
| -m  | Имя модели |
| -dc  | Контекст данных |
| -udl | Использование макета по умолчанию |
| -outDir | Относительный путь к папке выходных данных для создания представлений |
| --referenceScriptLibraries | Добавляет `_ValidationScriptsPartial` для редактирования и создания страниц. |

Чтобы получить справку по команде `aspnet-codegenerator razorpage`, используйте коммутатор `h`.

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Тестирование приложения

* Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).
* Протестируйте ссылку **Создать**.

 ![Страница "Создать"](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.

Если появляется следующая ошибка, выполните миграции и обновите базу данных.

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```