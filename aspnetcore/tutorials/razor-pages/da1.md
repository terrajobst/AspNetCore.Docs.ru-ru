---
title: "Изменение созданных страниц"
author: rick-anderson
description: "Обновление созданных страниц для оптимизации интерфейса."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 201e8d9c77d8e022bc56ffcf46456fada6fcfe25
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="updating-the-generated-pages"></a>Изменение созданных страниц

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Все готово для приложения Movie, но презентация далеко не идеальна. Мы не хотим видеть время (12:00:00 AM на рисунке ниже), а заголовок **ReleaseDate** (ДатаВыпуска) должен состоять из двух слов: **Release Date**.

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Обновление созданного кода

Откройте файл *Models/Movie.cs* и добавьте указанные ниже выделенные строки кода:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Щелкните правой кнопкой мыши строку, подчеркнутую волнистой красной линией, и выберите пункт **Быстрые действия и рефакторинг**.

  ![Контекстное меню с пунктом **Быстрые действия и рефакторинг**.](da1/qa.png)

Выберите `using System.ComponentModel.DataAnnotations;`.

  ![using System.ComponentModel.DataAnnotations в верхней части списка.](da1/da.png)

  Visual Studio добавит `using System.ComponentModel.DataAnnotations;`.

Пространство имен [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) будет рассмотрено в следующем учебнике. Атрибут [Display](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) определяет отображаемое имя поля (в этом случае "Release Date" вместо "ReleaseDate"). Атрибут [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) определяет тип данных (Date), поэтому сведения о времени, хранящиеся в поле, не отображаются.

Перейдите к Pages/Movies и наведите указатель мыши на ссылку **Edit**, чтобы увидеть конечный URL-адрес.

![Окно браузера с указателем, наведенным на ссылку "Edit" (Изменить), и URL-адресом ссылки http://localhost:1234/Movies/Edit/5](da1/edit7.png)

Ссылки **Edit**, **Details** и **Delete** создаются [вспомогательной функцией тегов привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) в файле *Pages/Movies/Index.cshtml*.

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Вспомогательные функции тегов](xref:mvc/views/tag-helpers/intro) позволяют серверному коду участвовать в создании и отображении HTML-элементов в файлах Razor. В приведенном выше коде `AnchorTagHelper` динамически создает значение атрибута HTML `href` на основе страницы Razor (маршрут является относительным), атрибут `asp-page` и идентификатор маршрута (`asp-route-id`). Дополнительные сведения см. в разделе [Формирование URL-адресов для страниц](xref:mvc/razor-pages/index#url-generation-for-pages).

Для изучения созданной разметки используйте функцию **просмотра исходного кода** в вашем любимом браузере. Ниже показана часть созданного кода HTML:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

В динамически созданных ссылках идентификаторы фильмов передаются с помощью строки запроса (например, `http://localhost:5000/Movies/Details?id=2`). 

Обновите страницы Razor Edit, Details и Delete так, чтобы использовался шаблон маршрута "{id:int}". Измените директиву страницы для каждой из этих страниц c `@page` на `@page "{id:int}"`. Запустите приложение и просмотрите исходный код. Созданный код HTML добавляет идентификатор в путь URL-адреса:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Запрос к странице с шаблоном маршрута "{id:int}", который **не** включает в себя целое число, приводит к ошибке HTTP 404 (не найдено). Например, `http://localhost:5000/Movies/Details` приведет к ошибке 404. Чтобы сделать идентификатор необязательным, добавьте `?` к ограничению маршрута:

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a>Обновление обработки исключений нежесткой блокировки

Измените метод `OnPostAsync` в файле *Pages/Movies/Edit.cshtml.cs*. Эти изменения выделены в следующем примере кода:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

Приведенный выше код обнаруживает исключения нежесткой блокировки, только когда один работающий параллельно клиент удаляет фильм, а другой публикует изменения для этого фильма.

Чтобы протестировать блок `catch`, выполните указанные ниже действия.

* Задайте точку останова в `catch (DbUpdateConcurrencyException)`
* Измените фильм.
* В другом окне браузера щелкните ссылку **Delete** для этого же фильма, а затем удалите его.
* В первом окне браузера опубликуйте изменения для фильма.

В рабочем коде, как правило, обнаруживаются конфликты нежесткой блокировки, когда два или несколько клиентов одновременно изменяют запись. Дополнительные сведения см. в статье [Обработка конфликтов параллелизма](xref:data/ef-rp/concurrency).

### <a name="posting-and-binding-review"></a>Проверка публикации и привязки

Изучите файл *Pages/Movies/Edit.cshtml.cs*: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]

При выполнении HTTP-запроса GET к странице Movies/Edit (например, `http://localhost:5000/Movies/Edit/2`) происходит следующее:

* Метод `OnGetAsync` извлекает запись фильма из базы данных и возвращает метод `Page`. 
* Метод `Page` отображает страницу Razor *Pages/Movies/Edit.cshtml*. Файл *Pages/Movies/Edit.cshtml* содержит директиву модели (`@model RazorPagesMovie.Pages.Movies.EditModel`), которая делает модель фильма доступной на странице.
* Отображается форма Edit со значениями из записи фильма.

При публикации страницы Movies/Edit происходит следующее:

* Значения формы на странице привязываются к свойству `Movie`. Атрибут `[BindProperty]` обеспечивает [привязку модели](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* При наличии ошибок в состоянии модели (например, `ReleaseDate` невозможно преобразовать в дату) форма публикуется снова с предоставленными значениями.
* Если ошибки модели отсутствуют, данные фильма сохраняются.

Методы HTTP GET на страницах Razor Index, Create и Delete работают аналогично. Метод HTTP POST `OnPostAsync` на странице Razor Create работает аналогично методу `OnPostAsync` на странице Razor Edit.

В следующем учебнике будет добавлена функция поиска.

>[!div class="step-by-step"]
[Назад: работа с SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Добавление поиска](xref:tutorials/razor-pages/search)
