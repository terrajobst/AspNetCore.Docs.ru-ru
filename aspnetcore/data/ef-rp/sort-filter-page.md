---
title: Razor Pages с EF Core в ASP.NET Core — сортировка, фильтрация, разбиение на страницы — 3 из 8
author: rick-anderson
description: Из этого руководства вы узнаете, как при помощи ASP.NET Core и Entity Framework Core добавить на страницу функции сортировки, фильтрации и разбиения на страницы.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 228722ecdc0fb19e024b7154393390fe6d34c5ff
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887309"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---sort-filter-paging---3-of-8"></a>Razor Pages с EF Core в ASP.NET Core — сортировка, фильтрация, разбиение на страницы — 3 из 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Это руководство описывает добавление функций сортировки, фильтрации, группировки и разбиения на страницы.

На следующем рисунке показана готовая страница. Заголовки столбцов являются ссылками, щелкнув которые, можно отсортировать столбец. Многократно щелкая заголовок столбца, можно переключаться между сортировкой по возрастанию и убыванию.

![Страница указателя учащихся](sort-filter-page/_static/paging.png)

При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

## <a name="add-sorting-to-the-index-page"></a>Добавление сортировки на страницу индекса

Добавьте строки в *Students/Index.cshtml.cs* `PageModel` для хранения параметров сортировки:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]

Измените *Students/Index.cshtml.cs* `OnGetAsync`, используя следующий код:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

Предыдущий код принимает параметр `sortOrder` из строки запроса в URL-адресе. URL-адрес (включая строку запроса) формируется [вспомогательной функцией тегов привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

Параметр `sortOrder` имеет значение "Name" или "Date". После параметра `sortOrder` может стоять "_desc", чтобы указать порядок по убыванию. По умолчанию используется порядок сортировки по возрастанию.

При запросе страницы "Index" по ссылке **Students** строка запроса отсутствует. Учащиеся отображаются по фамилии в порядке возрастания. В операторе `switch` сортировка по фамилии в порядке возрастания используется по умолчанию. Когда пользователь щелкает ссылку заголовка столбца, в строке запроса указывается соответствующее значение `sortOrder`.

Для формирования гиперссылок в заголовках столбцов страница Razor использует `NameSort` и `DateSort` с соответствующими значениями строки запроса:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Следующий код содержит условный [оператор ?:](/dotnet/csharp/language-reference/operators/conditional-operator) C#:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

Первая строка указывает, что когда `sortOrder` равен null или пуст, `NameSort` имеет значение "name_desc". Если `sortOrder` **не является** равным null или пустым, для `NameSort` задается пустая строка.

`?: operator` также называется тернарным оператором.

Следующие два оператора устанавливают гиперссылки в заголовках столбцов на странице следующим образом:

| Текущий порядок сортировки | Гиперссылка "Last Name" (Фамилия) | Гиперссылка "Date" (Дата) |
|:--------------------:|:-------------------:|:--------------:|
| "Last Name" (Фамилия) по возрастанию | по убыванию        | по возрастанию      |
| "Last Name" (Фамилия) по убыванию | по возрастанию           | по возрастанию      |
| "Date" (Дата) по возрастанию       | по возрастанию           | по убыванию     |
| "Date" (Дата) по убыванию      | по возрастанию           | по возрастанию      |

Для указания столбца, по которому выполняется сортировка, этот метод использует LINQ to Entities. Код инициализирует `IQueryable<Student>` до оператора switch и изменяет его в этом операторе:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 При создании или изменении `IQueryable` запрос в базу данных не отправляется. Запрос не выполнится, пока объект `IQueryable` не будет преобразован в коллекцию. `IQueryable` преобразуются в коллекцию путем вызова метода, такого как `ToListAsync`. Таким образом, код `IQueryable` создает одиночный запрос, который не выполняется до следующего оператора:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` можно расширить на случай большого числа сортируемых столбцов.

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>Добавление гиперссылок для заголовков столбцов на странице индексов учащихся

Замените код в файле *Students/Index.cshtml* на следующий выделенный код:

[!code-html[](intro/samples/cu21/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Предыдущий код:

* Добавляет гиперссылки в заголовки столбцов `LastName` и `EnrollmentDate`.
* Использует эти сведения в `NameSort` и `DateSort` для настройки гиперссылок с использованием текущих значений порядка сортировки.

Чтобы проверить работу сортировки, сделайте следующее:

* Запустите приложение и откройте вкладку **Students** (Учащиеся).
* Щелкните **Last Name** (Фамилия).
* Щелкните **Enrollment Date** (Дата зачисления).

Чтобы лучше понять код:

* В *Student/Index.cshtml.cs* задайте точку останова в `switch (sortOrder)`.
* Добавьте контрольное значение для `NameSort` и `DateSort`.
* В *Student/Index.cshtml* задайте точку останова в `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Осуществите пошаговое выполнение в отладчике.

## <a name="add-a-search-box-to-the-students-index-page"></a>Добавление поля поиска на страницу указателя учащихся

Для добавления фильтрации на страницу индекса учащихся:

* На страницу Razor добавляется текстовое поле и кнопка отправки. Текстовое поле предоставляет строку поиска для имени или фамилии.
* Страничная модель обновляется для использования значения из текстового поля.

### <a name="add-filtering-functionality-to-the-index-method"></a>Добавление функций фильтрации в метод Index

Измените *Students/Index.cshtml.cs* `OnGetAsync`, используя следующий код:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Предыдущий код:

* Добавляет параметр `searchString` в метод `OnGetAsync`. Значение строки поиска получается из текстового поля, добавляемого в следующем разделе.
* Добавил в оператор LINQ предложение `Where`. Это предложение `Where` отбирает только учащихся, чье имя или фамилия содержат строку поиска. Оператор LINQ выполняется, только если задано значение для поиска.

Примечание. Предыдущий код вызывает метод `Where` объекта `IQueryable`, при этом фильтр обрабатывается на сервере. В некоторых случаях приложение может вызывать метод `Where` как метод расширения для коллекции в памяти. Предположим, например, что `_context.Students` меняется с EF Core `DbSet` на метод репозитория, возвращающий коллекцию `IEnumerable`. Обычно результат остается прежним, но в некоторых случаях он может отличаться.

Например, реализация `Contains` в .NET Framework по умолчанию выполняет сравнение с учетом регистра. В SQL Server `Contains` учет регистра определяется параметрами сортировки у экземпляра SQL Server. По умолчанию SQL Server не учитывает регистр. Можно вызвать `ToUpper`, чтобы явно велеть тесту не учитывать регистр.

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Предыдущий код обеспечивает, что результаты не учитывают регистр, если код изменяется для использования `IEnumerable`. Когда `Contains` вызывается для коллекции `IEnumerable`, используется реализация .NET Core. Когда `Contains` вызывается для объекта `IQueryable`, используется реализация базы данных. При возвращение `IEnumerable` из репозитория производительность может значительно снижаться:

1. Все строки возвращаются с сервера базы данных.
1. Фильтр применяется ко всем возвращенным строкам в приложении.

Вызов `ToUpper` снижает производительность. Код `ToUpper` добавляет функцию в предложение WHERE TSQL-оператора SELECT. Добавленная функция не позволяет оптимизатору использовать индекс. Учитывая, что SQL устанавливается без учета регистра, рекомендуется не использовать вызов `ToUpper`, когда он не требуется.

### <a name="add-a-search-box-to-the-student-index-page"></a>Добавление поля поиска на страницу индексов учащихся

Добавьте в *Pages/Student/Index.cshtml* приведенный ниже выделенный код, чтобы создать кнопку **Search** и различные элементы хрома.

[!code-html[](intro/samples/cu21/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

Для добавления кнопки и поля поиска предыдущий код использует [вспомогательную функцию тегов](xref:mvc/views/tag-helpers/intro) `<form>`. По умолчанию вспомогательная функция тегов `<form>` отправляет данные формы с помощью POST. При этом параметры передаются в тексте сообщения HTTP, а не в URL-адресе. При использовании HTTP GET данные формы передаются в виде строк запроса в URL-адресе. Передача данных со строками запроса позволяет пользователям добавлять URL-адрес в закладки. [Руководства консорциума W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) рекомендуют использовать GET, когда действие не приводит к обновлению.

Проверьте работу приложения:

* Выберите вкладку **Students** (Учащиеся) и введите строку поиска.
* Выберите **Search** (Поиск).

Обратите внимание, что URL-адрес содержит строку поиска.

```html
http://localhost:5000/Students?SearchString=an
```

Если страница добавлена в закладки, закладка содержит URL-адрес страницы и строку запроса `SearchString`. Формирование строки запроса обеспечивает `method="get"` в теге `form`.

Сейчас при выборе ссылки сортировки заголовка столбца теряется значение фильтра в поле **Search** (Поиск). Потерянное значение фильтра исправляется в следующем разделе.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Добавление на страницу указателя учащихся разбиения на страницы

В этом разделе создается класс `PaginatedList` для поддержки разбиения на страницы. Класс `PaginatedList` использует операторы `Skip` и `Take` для фильтрации данных на сервере вместо того, чтобы извлекать все строки таблицы. На следующем рисунке показаны кнопки перелистывания.

![Страница указателя учащихся со ссылками для перелистывания](sort-filter-page/_static/paging.png)

В папке проекта создайте файл `PaginatedList.cs` со следующим кодом:

[!code-csharp[](intro/samples/cu21/PaginatedList.cs)]

В предыдущем коде метод `CreateAsync` принимает размер и номер страницы и вызывает соответствующие методы `Skip` и `Take` объекта `IQueryable`. Метод `ToListAsync` объекта `IQueryable` при вызове возвращает список, содержащий только запрошенную страницу. Для включения и отключения кнопок перелистывания страниц **Previous** (Назад) и **Next** (Далее) используются свойства `HasPreviousPage` и `HasNextPage`.

Метод `CreateAsync` используется для создания `PaginatedList<T>`. Конструктор не позволяет создать объект`PaginatedList<T>`, так как конструкторы не могут выполнять асинхронный код.

## <a name="add-paging-functionality-to-the-index-method"></a>Добавление разбиения на страницы в метод Index

В *Students/Index.cshtml.cs* измените тип `Student` с `IList<Student>` на `PaginatedList<Student>`:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Измените *Students/Index.cshtml.cs* `OnGetAsync`, используя следующий код:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

Предыдущий код добавляет страницу индекса, текущий `sortOrder` и `currentFilter` в сигнатуру метода.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Все параметры равны null, когда:

* Страница вызывается по ссылке **Students** (Учащиеся).
* Пользователь не открывал ссылку перелистывания или сортировки.

При выборе ссылки перелистывания переменная индекса страницы содержит номер страницы для отображения.

`CurrentSort` предоставляет странице Razor текущий порядок сортировки. Текущий порядок сортировки нужно включить в ссылки перелистывания, чтобы сохранить его при смене страницы.

`CurrentFilter` предоставляет странице Razor текущую строку фильтра. Значение `CurrentFilter`:

* Нужно включить в ссылки для перелистывания, чтобы сохранить параметры фильтра при смене страницы.
* Нужно восстановить в текстовом поле после обновления страницы.

Если строка поиска изменяется во время перелистывания, номер страницы сбрасывается в значение 1. Номер страницы должен быть сброшен на 1, так как с новым фильтром может измениться состав отображаемых данных. Если введено значение для поиска и нажата кнопка **Submit** (Отправить):

* Строка поиска изменяется.
* Значение параметра `searchString` отличается от null.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

Метод `PaginatedList.CreateAsync` преобразует результат запроса учащихся в отдельную страницу коллекции, поддерживающую разбиение на страницы. Эта страница с учащимися передается на страницу Razor.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

Два вопросительных знака в `PaginatedList.CreateAsync` являются [оператором объединения с null](/dotnet/csharp/language-reference/operators/null-conditional-operator). Оператор объединения с null определяет значение по умолчанию для типа, допускающего значение null. Выражение `(pageIndex ?? 1)` означает возвращение значения `pageIndex`, если он имеет значение. Если у `pageIndex` нет значения, возвращается 1.

## <a name="add-paging-links-to-the-student-razor-page"></a>Добавление ссылок для перелистывания страниц на страницу Razor

Измените разметку в *Students/Index.cshtml*. Изменения выделены:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

Ссылки в заголовках столбцов передают в метод `OnGetAsync` с помощью строки запроса текущее значение строки поиска, чтобы пользователь мог сортировать отфильтрованные результаты:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=28-31)]

Кнопки перелистывания отображаются вспомогательными функциями тегов:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=72-)]

Запустите приложение и перейдите на страницу учащихся.

* Чтобы убедиться, что разбиение на страницы работает, нажимайте кнопки перелистывания при различном порядке сортировки.
* Чтобы убедиться, что разбиение на страницы работает корректно вместе с сортировкой и фильтрацией, введите строку поиска и попробуйте перелистнуть страницу.

![Страница указателя учащихся со ссылками для перелистывания](sort-filter-page/_static/paging.png)

Чтобы лучше понять код:

* В *Student/Index.cshtml.cs* задайте точку останова в `switch (sortOrder)`.
* Добавьте контрольное значение для `NameSort`, `DateSort`, `CurrentSort` и `Model.Student.PageIndex`.
* В *Student/Index.cshtml* задайте точку останова в `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Осуществите пошаговое выполнение в отладчике.

## <a name="update-the-about-page-to-show-student-statistics"></a>Изменение страницы "About" (О программе) для отображения статистики учащихся

В этом шаге изменяется страница *Pages/About.cshtml*, чтобы отобразить количество зачисленных учащихся по дням. Это изменение использует группирование и включает следующие шаги:

* Создание модели представления для данных, используемых страницей **About** (О программе).
* Обновление страницы "About" (О программе) для использования модели представления.

### <a name="create-the-view-model"></a>Создание модели представления

Создайте папку *SchoolViewModels* в папке *Models*.

Добавьте в папку *SchoolViewModels* файл *EnrollmentDateGroup.cs* со следующим кодом:

[!code-csharp[](intro/samples/cu21/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Обновление модели страницы "About" (О программе)

Веб-шаблоны в ASP.NET Core 2.2 не включают страницу About. Если вы используете ASP.NET Core 2.2, создайте страницу About Razor Page.

Измените файл *Pages/About.cshtml.cs*, используя следующий код:

[!code-csharp[](intro/samples/cu21/Pages/About.cshtml.cs)]

Запрос LINQ группирует записи из таблицы студентов по дате зачисления, вычисляет число записей в каждой группе и сохраняет результаты в коллекцию объектов моделей представления `EnrollmentDateGroup`.

### <a name="modify-the-about-razor-page"></a>Изменение страницы Razor "About" (О программе)

Замените код в файле *Pages/About.cshtml* следующим кодом:

[!code-html[](intro/samples/cu21/Pages/About.cshtml)]

Запустите приложение и перейдите на страницу "About" (О программе). Количество зачисленных студентов по дням отображается в таблице.

При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение для этого этапа](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Страница About](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Отладка источника ASP.NET Core 2.x](https://github.com/aspnet/AspNetCore.Docs/issues/4155)
* [Версия руководства на YouTube](https://www.youtube.com/watch?v=MDs7PFpoMqI)

В следующем руководстве приложение использует миграции для обновления модели данных.

::: moniker-end

> [!div class="step-by-step"]
> [Назад](xref:data/ef-rp/crud)
> [Вперед](xref:data/ef-rp/migrations)
