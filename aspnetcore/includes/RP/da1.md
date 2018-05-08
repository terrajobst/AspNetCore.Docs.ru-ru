# <a name="update-the-generated-pages"></a><span data-ttu-id="b9160-101">Изменение созданных страниц</span><span class="sxs-lookup"><span data-stu-id="b9160-101">Update the generated pages</span></span>

<span data-ttu-id="b9160-102">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="b9160-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b9160-103">Все готово для приложения по работе с фильмами, но презентация далеко не идеальна.</span><span class="sxs-lookup"><span data-stu-id="b9160-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="b9160-104">Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов: **Release Date**.</span><span class="sxs-lookup"><span data-stu-id="b9160-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="b9160-106">Обновление созданного кода</span><span class="sxs-lookup"><span data-stu-id="b9160-106">Update the generated code</span></span>

<span data-ttu-id="b9160-107">Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:</span><span class="sxs-lookup"><span data-stu-id="b9160-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
