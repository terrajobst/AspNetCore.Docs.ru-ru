# <a name="update-the-generated-pages"></a>Изменение созданных страниц

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Все готово для приложения по работе с фильмами, но презентация далеко не идеальна. Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов: **Release Date**.

![Приложение Movie с данными по фильмам, открытое в Chrome](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Обновление созданного кода

Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:

[!code-csharp[Main](code/Models/Movie.cs?highlight=2,11-12)]
