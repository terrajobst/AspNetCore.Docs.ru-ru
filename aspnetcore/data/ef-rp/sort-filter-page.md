---
title: "Страниц Razor с основными EF - сортировки, фильтрации, разбиение на страницы: 3 8."
author: rick-anderson
description: "В этом учебнике предстоит добавить сортировку, фильтрацию и разбиение по страницам функциональные возможности для разбиения на страницы с помощью ASP.NET Core и Entity Framework Core."
ms.author: riande
ms.date: 10/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 9c1ee6f8c00f3cd501ea86fbf73f51ae540a010a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-razor-pages-3-of-8"></a>Сортировка, фильтрация, разбиение по страницам и группирование - Core EF со страницами Razor (3 8)

По [Tom Dykstra](https://github.com/tdykstra), [Рик Андерсон](https://twitter.com/RickAndMSFT), и [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

В этот учебник, сортировки, фильтрации, группировки и разбиение на страницы добавляется функциональные возможности.

Заполненная страница на следующем рисунке. Заголовки столбцов являются ссылками, чтобы отсортировать столбец. Щелчком заголовка многократно переключается между сортировкой по возрастанию и убыванию.

![Страница индекса студентов](sort-filter-page/_static/paging.png)

Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

## <a name="add-sorting-to-the-index-page"></a>Добавить, сортировку, страница индекса

Добавление строк в *Students/Index.cshtml.cs* `PageModel` для хранения параметров сортировки:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]


Обновление *Students/Index.cshtml.cs* `OnGetAsync` следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

В приведенном выше коде `sortOrder` параметра из строки запроса в URL-АДРЕСЕ. URL-адрес (включая строку запроса), формируется [вспомогательный тег привязки](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

`sortOrder` Параметр имеет значение «Name» или «Date». `sortOrder` Параметр при необходимости следуют «_desc», чтобы указать порядок по убыванию. По умолчанию используется порядок сортировки по возрастанию.

При запросе страницы индекса из **учащихся** связь, без строка запроса. Студенты, отображаются в порядке возрастания по фамилии. Возрастающем порядке по фамилии значение по умолчанию (проходом регистр) в `switch` инструкции. Когда пользователь щелкает ссылку заголовка столбца, соответствующего `sortOrder` значение указано в значении строки запроса.

`NameSort`и `DateSort` используются на странице Razor для настройки гиперссылки заголовок столбца со строковыми значениями соответствующего запроса:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Содержит следующий код C# [?: оператор](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

Первая строка указывает, что при `sortOrder` равно null или пусто, `NameSort` имеет значение «name_desc». Если `sortOrder` — **не** равно null или пусто, `NameSort` устанавливается равным пустой строке.

`?: operator` Называется также троичный оператор.

Следующие два оператора включите представление для задания гиперссылки заголовка столбца следующим образом:

| Текущий порядок сортировки | Последнее имя гиперссылки | Дата гиперссылки |
|:--------------------:|:-------------------:|:--------------:|
| Последняя имени: по возрастанию | по убыванию        | по возрастанию      |
| Последняя имени: по убыванию | по возрастанию           | по возрастанию      |
| По возрастанию даты       | по возрастанию           | по убыванию     |
| Дата по убыванию      | по возрастанию           | по возрастанию      |

Метод использует LINQ to Entities позволяет выбрать столбец для сортировки. Этот код инициализирует `IQueryable<Student> ` перед оператором коммутатора и обновляет его в операторе switch:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-)]

 Когда`IQueryable` при создании или изменении, запрос отправляется в базу данных. Запрос не выполняется до `IQueryable` преобразовать объект в коллекцию. `IQueryable`преобразуются в коллекции, такие как вызов метода `ToListAsync`. Таким образом `IQueryable` код результаты в один запрос, который не выполняется до следующей инструкции:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync`Невозможно получить подробный с большим количеством столбцов.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Добавление гиперссылок заголовок столбца в представлении студента индекса

Замените код в *Students/Index.cshtml*, на следующий выделенный код:

[!code-html[](intro/samples/cu/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Предыдущий код:

* Добавление гиперссылки на `LastName` и `EnrollmentDate` заголовки столбцов.
* Использует эти сведения в `NameSort` и `DateSort` для настройки гиперссылки с текущими значениями порядка сортировки.

Проверка работы с сортировки.

* Запустите приложение и выберите **учащихся** вкладки.
* Нажмите кнопку **Фамилия**.
* Нажмите кнопку **Дата регистрации**.

Чтобы получить лучшее представление кода:

* В *Student/Index.cshtml.cs*, установите точку останова на `switch (sortOrder)`.
* Добавление контрольного значения для `NameSort` и `DateSort`.
* В *Student/Index.cshtml*, установите точку останова на `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Шаг с помощью отладчика.

## <a name="add-a-search-box-to-the-students-index-page"></a>Добавить поле поиска на страницу индекса студентов

Добавление фильтрации на страницу индекса студентов:

* Текстовое поле и кнопку submit добавляется страница Razor. Текстовое поле предоставляет строку поиска для имени первого или последнего.
* Модель страницы обновляется для использования в качестве значения текстового поля.

### <a name="add-filtering-functionality-to-the-index-method"></a>Добавьте функцию фильтра в метод индекса

Обновление *Students/Index.cshtml.cs* `OnGetAsync` следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Предыдущий код:

* Добавляет `searchString` параметр `OnGetAsync` метода. Строковое значение поиска получается из текстового поля, которое добавляется в следующем разделе.
* Добавлены инструкции LINQ `Where` предложения. `Where` Предложение выбирает только учащихся, имя или фамилия содержит строку поиска. Инструкции LINQ выполняется только в том случае, если значение для поиска.

Примечание: Предыдущий код вызывает метод `Where` метод `IQueryable` объект и фильтр обрабатывается на сервере. В некоторых сценариях может вызов приложения tha `Where` метод как метод расширения для коллекции в памяти. Предположим, например, `_context.Students` изменяется с ядром EF `DbSet` для репозитория метод, возвращающий `IEnumerable` коллекции. Результат обычно останется прежним, но в некоторых случаях могут различаться.

Например, реализация .NET Framework `Contains` выполняет сравнение с учетом регистра по умолчанию. В SQL Server `Contains` чувствительность к регистру определяется параметров сортировки экземпляра SQL Server. SQL Serve по умолчанию с учетом регистра. `ToUpper`может вызываться производится проверка явно без учета регистра.

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Предыдущий код гарантирует, результаты, без учета регистра при изменении кода для использования `IEnumerable`. Когда `Contains` будет вызван на `IEnumerable` коллекции .NET Core, используется реализация. Когда `Contains` будет вызван на `IQueryable` объекта, используется реализация базы данных. Возвращение `IEnumerable` из репозитория может иметь penality значительного увеличения производительности:

1. На сервере базы данных будут возвращены все строки.
1. Фильтр применяется для всех возвращенных строк в приложении.

Производительность снижается для вызова `ToUpper`. `ToUpper` Код добавляет функцию в предложении WHERE инструкции TSQL SELECT. Добавлена функция запрещает оптимизатор использовать индекс. Учитывая, что сервер SQL Server установлен как без учета регистра, рекомендуется избежать `ToUpper` вызвать, если оно не требуется.

### <a name="add-a-search-box-to-the-student-index-view"></a>Добавить поле поиска в представление индекс студента

В *Views/Student/Index.cshtml*, добавьте следующий выделенный код для создания **поиска** кнопку и различные chrome.

[!code-html[](intro/samples/cu/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

В предыдущем коде используется `<form>` [тег вспомогательный](xref:mvc/views/tag-helpers/intro) Добавление поиска текстовое поле и кнопку. По умолчанию `<form>` тег вспомогательный отправки данных с помощью POST. С помощью запроса POST передачи параметров в тексте сообщения HTTP, а не в URL-адрес. При использовании HTTP GET данные формы передается как строки запроса в URL-АДРЕСЕ. Передача данных со строками запроса позволяет пользователям bookmark URL-адрес. [Рекомендации W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) Майкрософт рекомендует использовать GET при действия не приведут к обновления.

Проверьте работу приложения:

* Выберите **учащихся** вкладку и введите строку поиска.
* Выберите **поиска**.

Обратите внимание, что URL-адрес содержит строку поиска.

```html
http://localhost:5000/Students?SearchString=an
```

Если страницы закладку, закладка содержит URL-адрес страницы и `SearchString` строку запроса. `method="get"` В `form` тег является причину строке запроса должен быть создан.

В настоящее время при выборе ссылки сортировки заголовок столбца фильтр значения **поиска** поле теряется. Значение фильтра потеряны фиксируется в следующем разделе.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Добавить на страницу индекса учащихся разбиения по страницам

В этом разделе `PaginatedList` класса создается для поддержки разбиения на страницы. `PaginatedList` Класс использует `Skip` и `Take` инструкций для фильтрации данных на сервере, а не получение всех строк таблицы. На следующем рисунке кнопки разбиения по страницам.

![Студенты индексная страница со ссылками на разбиение на страницы](sort-filter-page/_static/paging.png)

Создайте в папке проекта `PaginatedList.cs` следующим кодом:

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

`CreateAsync` Метод в приведенном выше коде принимает размер страницы и номер страницы и применяет соответствующие `Skip` и `Take` инструкции, чтобы `IQueryable`. Когда `ToListAsync` будет вызван на `IQueryable`, он возвращает список, содержащий только запрошенную страницу. Свойства `HasPreviousPage` и `HasNextPage` используются для включения или отключения **Назад** и **Далее** разбиение по страницам кнопок.

`CreateAsync` Метод используется для создания `PaginatedList<T>`. Не удается создать конструктор `PaginatedList<T>` объекта, конструкторы не могут выполнять асинхронный код.

## <a name="add-paging-functionality-to-the-index-method"></a>Добавьте в метод индекс разбиения по страницам

В *Students/Index.cshtml.cs*, обновить тип `Student` из `IList<Student>` для `PaginatedList<Student>`:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Обновление *Students/Index.cshtml.cs* `OnGetAsync` следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-)]

Предыдущий код добавляет индекс страницы текущего `sortOrder`и `currentFilter` к сигнатуре метода.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Все параметры имеют значение null, если:

* Страницы вызывается из **учащихся** ссылку.
* Разбиение по страницам или сортировку ссылок еще не щелкнул пользователь.

При щелчке ссылки разбиения на страницы, переменная индекса страницы содержит номер страницы для отображения.

`CurrentSort`Предоставляет страницу Razor определения порядка сортировки. Текущий порядок сортировки должны быть включены в ссылки разбиения на страницы для сохранения порядка сортировки во время разбиения на страницы.

`CurrentFilter`предоставляет страницы Razor с текущей строки фильтра. `CurrentFilter` Значение:

* Должны быть включены в ссылки разбиения на страницы для поддержки настройки фильтра во время разбиения на страницы.
* Должна быть восстановлена в текстовое поле, когда отобразится страница.

Если строка поиска, которая изменяется во время разбиения на страницы, страницы сбрасывается до 1. На странице имеются сбрасывается значение 1, так как новый фильтр может показать различные данные. Если введено значение для поиска и **отправить** установлен:

* Строка поиска, которая изменяется.
* `searchString` Параметра не равно null.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

`PaginatedList.CreateAsync` Метод преобразует запрос студента на одной странице учеников в тип коллекции, который поддерживает разбиение по страницам. Страницы учащихся передается на страницу Razor.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

Два вопросительные знаки в `PaginatedList.CreateAsync` представляют [оператор объединения с null](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator). Оператор объединения с null определяет значение по умолчанию для типа значения NULL. Выражение `(pageIndex ?? 1)` означает возвращают значение `pageIndex` , если он имеет значение. Если `pageIndex` не имеет значение, возвращают значение 1.

## <a name="add-paging-links-to-the-student-razor-page"></a>Добавление ссылок разбиения на страницы на студента страниц Razor

Обновить разметки в *Students/Index.cshtml*. Изменения выделены:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-)]

Ссылки в заголовках столбцов использовать строку запроса для текущей искомой строки для передачи `OnGetAsync` метод, чтобы пользователь может сортировать в результаты фильтрации:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=28-31)]

По вспомогательных функций тегов отображаются кнопки разбиения на страницы:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=72-)]

Запустите приложение и перейдите на страницу учащихся.

* Чтобы убедиться, что работает разбиение на страницы, по ссылкам разбиения на страницы в различные порядки сортировки.
* Чтобы проверить, правильно работает разбиение по страницам с сортировкой и фильтрацией, введите строку поиска и повторите разбиения на страницы.

![Студенты индексная страница со ссылками на разбиение на страницы](sort-filter-page/_static/paging.png)

Чтобы получить лучшее представление кода:

* В *Student/Index.cshtml.cs*, установите точку останова на `switch (sortOrder)`.
* Добавление контрольного значения для `NameSort`, `DateSort`, `CurrentSort`, и `Model.Student.PageIndex`.
* В *Student/Index.cshtml*, установите точку останова на `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Шаг с помощью отладчика.

## <a name="update-the-about-page-to-show-student-statistics"></a>Страница с информацией о Статистика студента обновления

На этом шаге *Pages/About.cshtml* обновляется для отображения количества учащихся регистрации для каждой даты регистрации. Обновление использует группирование и включает следующие шаги:

* Создание класса модели представления для данных, используемых в **о** страницы.
* Измените модель о Razor и страницы.

### <a name="create-the-view-model"></a>Создание модели представления

Создание *SchoolViewModels* папки в *моделей* папки.

В *SchoolViewModels* папки, добавьте *EnrollmentDateGroup.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Обновление модели страницы о программе

Обновление *Pages/About.cshtml.cs* файла следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/About.cshtml.cs)]

Инструкции LINQ группирует учащихся сущностей, Дата регистрации, вычисляет число сущностей в каждой группе и сохраняет результаты в коллекцию `EnrollmentDateGroup` просматривать объекты модели.

Примечание: LINQ `group` команда не поддерживается в настоящее время ядром EF. В приведенном выше коде все записи студентов возвращаются из SQL Server. `group` Команда применяется в приложении страниц Razor не на сервере SQL Server. EF Core 2.1 будет поддерживать этот LINQ `group` оператор и группирование выполняется на сервере SQL Server. В разделе [реляционные: поддерживает преобразование GroupBy() to SQL](https://github.com/aspnet/EntityFrameworkCore/issues/2341). [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap) будет выпущен с .NET Core 2.1. Дополнительные сведения см. в разделе [.NET Core стратегия](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="modify-the-about-razor-page"></a>Изменение о странице Razor

Замените код в *Views/Home/About.cshtml* файла следующим кодом:

[!code-html[](intro/samples/cu/Pages/About.cshtml)]

Запустите приложение и перейдите на страницу о программе. Количества учащихся для каждой даты регистрации отображается в таблице.

Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![О странице](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Отладка источника ASP.NET Core 2.x](https://github.com/aspnet/Docs/issues/4155)

В следующем уроке приложение использует миграции для обновления данных модели.

>[!div class="step-by-step"]
[Назад](xref:data/ef-rp/crud)
[Вперед](xref:data/ef-rp/migrations)
