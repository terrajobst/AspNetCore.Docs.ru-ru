# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a>Работа с SQLite в приложении Razor Pages ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Объект `MovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных. Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

На веб-сайте [SQLite](https://www.sqlite.org/) указывается следующее:

> SQLite — это автономная внедряемая полнофункциональная общедоступная система управления базами данных SQL с высокой степенью надежности. На данный момент SQLite является самой популярной СУБД в мире.

Для просмотра баз данных SQLite, а также управления ими можно использовать множество самых разных сторонних инструментов. На следующем рисунке показан [DB Browser для SQLite](http://sqlitebrowser.org/). Если вы предпочитаете использовать другое средство для работы с SQLite, напишите нам в комментариях, чем именно оно нравится вам.

![DB Browser для SQLite с базой данных movie](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Заполнение базы данных

Создайте класс `SeedData` в папке *Models*. Замените сгенерированный код следующим кодом:

[!code-csharp[](code/Models/SeedData.cs)]

Если в базе данных есть фильмы, возвращается инициализатор заполнения.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Добавление инициализатора заполнения

Добавьте инициализатор заполнения в метод `Main` в файле *Program.cs*:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a>Тестирование приложения

Удалите все записи в базе данных для запуска метода заполнения. Остановите и запустите приложение, чтобы начать заполнение базы данных.

В приложении будут отображены данные.