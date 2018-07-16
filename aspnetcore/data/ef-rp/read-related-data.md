---
title: Razor Pages с EF Core в ASP.NET Core — чтение связанных данных — 6 из 8
author: rick-anderson
description: Из этого руководства вы узнаете, как читать и отображать связанные данные — данные, которые Entity Framework загружает в свойства навигации.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: fa3147cc4ad121784911eef802e04ca91f16448f
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063316"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>Razor Pages с EF Core в ASP.NET Core — чтение связанных данных — 6 из 8

Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

В этом руководстве выполняется чтение и отображение связанных данных. Связанными называются данные, которые EF Core загружает в свойства навигации.

При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение для этого этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).

На следующих рисунках изображены готовые страницы для этого руководства:

![Страница индекса курсов](read-related-data/_static/courses-index.png)

![Страница индекса преподавателей](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Безотложная, явная и отложенная загрузка связанных данных

Существует несколько способов, которыми EF Core может загружать связанные данные в свойства навигации сущности:

* [Безотложная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading). Безотложной является загрузка, когда запрос для одного типа сущности также загружает связанные сущности. При чтении сущности связанные данные извлекаются. Обычно такая загрузка представляет собой одиночный запрос с соединением, который получает все необходимые данные. Для некоторых типов безотложной загрузки EF Core выдает несколько запросов. Выдача нескольких запросов может оказаться более эффективной по сравнению с отдельными ситуациями в EF6, когда выполнялся отдельный запрос. Безотложная загрузка указывается с помощью методов `Include` и `ThenInclude`.

  ![Пример безотложной загрузки](read-related-data/_static/eager-loading.png)
 
  Безотложная загрузка отправляет несколько запросов, когда включена навигация коллекции:

  * Один запрос для основного запроса 
  * Один запрос для каждого "края" коллекции в дереве загрузки.

* Отдельные запросы с `Load`: данные можно получить в отдельных запросах, а EF Core "исправляет" свойства навигации. Термин "исправляет" означает, что EF Core автоматически заполняет свойства навигации. Отдельные запросы с `Load` больше похожи на явную загрузку, чем безотложную.

  ![Пример отдельных запросов](read-related-data/_static/separate-queries.png)

  Примечание: EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста. Даже если данные для свойства навигации *не* включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.

* [Явная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading). При первом чтении сущности связанные данные не извлекаются. Нужно написать код, извлекающий связанные данные, когда они необходимы. Явная загрузка с отдельными запросами приводит к отправке нескольких запросов к базе данных. При явной загрузке код указывает, какие свойства навигации нужно загрузить. Для выполнения явной загрузки используется метод `Load`. Пример:

  ![Пример явной загрузки](read-related-data/_static/explicit-loading.png)

* [Отложенная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading). [Сейчас EF Core не поддерживает отложенную загрузку](https://github.com/aspnet/EntityFrameworkCore/issues/3797). При первом чтении сущности связанные данные не извлекаются. При первом обращении к свойству навигации необходимые для этого свойства данные извлекаются автоматически. Запрос к базе данных отправляется при каждом первом обращении к свойству навигации.

* Оператор `Select` загружает только необходимые связанные данные.

## <a name="create-a-courses-page-that-displays-department-name"></a>Создание страницы "Courses" (Курсы) с отображением названий кафедр

Сущность "Course" включает в себя свойство навигации, содержащее сущность `Department`. Сущность `Department` содержит кафедру, которой назначен курс.

Отображение имени назначенной кафедры в списке курсов:

* Получите свойство `Name` из сущности `Department`.
* Сущность `Department` получается из свойства навигации `Course.Department`.

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Формирование шаблона для модели Course

* Закройте Visual Studio.
* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* Выполните следующую команду:

  ```console
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

Предыдущая команда формирует шаблон для модели `Course`. Откройте проект в Visual Studio.

Откройте *Pages/Courses/Index.cshtml.cs* и изучите метод `OnGetAsync`. Подсистема формирования шаблонов указала безотложную загрузку для свойства навигации `Department`. Метод `Include` задает безотложную загрузку.

Запустите приложение и выберите ссылку **Courses** (Курсы). В столбце кафедры отображается бесполезный `DepartmentID`.

Обновите метод `OnGetAsync`, используя следующий код:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Предыдущий код добавляет `AsNoTracking`. `AsNoTracking` повышает производительность, так как возвращаемые сущности не отслеживаются. Отслеживание отсутствует, так как сущности не изменяются в текущем контексте.

Измените *Pages/Courses/Index.cshtml*, используя выделенную ниже разметку:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

В шаблонный код были внесены следующие изменения:

* Изменен заголовок с "Index" (Индекс) на "Courses" (Курсы).
* Добавлен столбец **Number** (Номер), отображающий значение свойства `CourseID`. По умолчанию в шаблоне отсутствуют первичные ключи, поскольку для конечных пользователей они не имеют смысла. Однако в нашем случае первичный ключ имеет смысл.
* Изменен столбец **Department** (Кафедра) для отображения названия кафедры. Код отображает свойство `Name` сущности `Department`, которая загружена в свойство навигации `Department`:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Для просмотра списка с названиями кафедр запустите приложение и выберите вкладку **Courses** (Курсы).

![Страница индекса курсов](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Загрузка связанных данных с помощью "Select"

Метод `OnGetAsync` загружает связанные данные с помощью метода `Include`:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

Оператор `Select` загружает только необходимые связанные данные. Для отдельных элементов, таких как `Department.Name`, используется SQL INNER JOIN. Для коллекций используется доступ к другой базе данных, но это же делает и оператор `Include` в коллекциях.

Следующий код загружает связанные данные с помощью метода `Select`:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Полный пример см. в описании [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) и [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Создание страницы "Instructors" (Преподаватели) с отображением курсов и дат зачисления

В этом разделе создается страница "Instructors" (Преподаватели).

<a name="IP"></a>
![Страница индекса преподавателей](read-related-data/_static/instructors-index.png)

Эта страница считывает и отображает связанные данные следующим образом:

* Список преподавателей отображает связанные данные из сущности `OfficeAssignment` ("Office" (Кабинет) на предыдущем изображении). Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному. Безотложная загрузка используется для сущностей `OfficeAssignment`. Безотложная загрузка обычно более эффективна, когда требуется отобразить связанные данные. В этом случае отображается принадлежность к кабинету для каждого преподавателя.
* Когда пользователь выбирает преподавателя (Harui на предыдущем изображении), отображаются связанные сущности `Course`. Между сущностями `Instructor` и `Course` действует связь многие ко многим. Используется безотложная загрузка для сущностей `Course` и связанных сущностей `Department`. В этом случае отдельные запросы могут оказаться эффективнее, так как требуются только курсы для выбранного преподавателя. Этот пример показывает, как использовать безотложную загрузку для свойств навигации в сущностях, находящихся в свойствах навигации.
* Когда пользователь выбирает курс ("Chemistry" (Химия) на предыдущем изображении), отображаются связанные данные из сущности `Enrollments`. На предыдущем изображении отображается имя и оценка учащегося. Между сущностями `Course` и `Enrollment` действует связь один ко многим.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Создание модели для представления индекса преподавателей

На странице "Instructors" (Преподаватели) отображаются данные из трех различных таблиц. Создается модель представления, которая включает три сущности, представляющие три таблицы.

Создайте в папке *SchoolViewModels* файл *InstructorIndexData.cs* со следующим кодом:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Формирование шаблона для модели "Instructor" (Преподаватель)

* Закройте Visual Studio.
* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* Выполните следующую команду:

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

Предыдущая команда формирует шаблон для модели `Instructor`. Откройте проект в Visual Studio.

Выполните построение проекта. Эта сборка выдает ошибки.

Выполните глобальную замену `_context.Instructor` на `_context.Instructors` (она заключается в добавлении "s" к `Instructor`). Обнаруживаются и изменяются 7 экземпляров.

Запустите приложение и перейдите на страницу преподавателей.

Замените содержимое *Pages/Instructors/Index.cshtml.cs* на следующий код:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

Метод `OnGetAsync` принимает необязательные данные маршрутизации для идентификатора выбранного преподавателя.

Изучите запрос в файле *Pages/Instructors/Index.cshtml.cs*:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

Запрос имеет две операции включения:

* `OfficeAssignment`: отображается в [представлении преподавателей](#IP).
* `CourseAssignments`: вызывает проводимые курсы.


### <a name="update-the-instructors-index-page"></a>Изменение страницы индекса преподавателей

Измените *Pages/Instructors/Index.cshtml*, используя следующую разметку:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Приведенная выше разметка вносит следующие изменения:

* Изменяет директиву `page` с `@page` на `@page "{id:int?}"`. `"{id:int?}"` является шаблоном маршрута. Шаблон маршрута изменяет целочисленные строки запроса в URL-адресе для маршрутизации данных. Например, при выборе ссылки **Select** для преподавателя только с директивой `@page` формируется URL-адрес следующего вида:

    `http://localhost:1234/Instructors?id=2`

    Когда используется директива страницы `@page "{id:int?}"`, предыдущий URL-адрес имеет следующее значение:

    `http://localhost:1234/Instructors/2`

* Заголовком страницы является **Instructors**.
* Добавили столбец **Office**, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не равно null. Так как это связь один к нулю или одному, то связанная сущность OfficeAssignment может отсутствовать.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Добавили столбец **Courses**, отображающий курсы, которые ведет конкретный преподаватель. Подробные сведения об использовании синтаксиса Razor см. в разделе [Явные строки перехода с `@:` ](xref:mvc/views/razor#explicit-line-transition-with-).

* Добавили код, который динамически добавляет `class="success"` к элементу `tr` выбранного преподавателя. Этот параметр задает цвет фона для выделенных строк c помощью класса Bootstrap.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Добавлена новая гиперссылка с меткой **Select** (Выбрать). Она отправляет идентификатор выбранного преподавателя в метод `Index` и задает цвет фона.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Запустите приложение и выберите вкладку **Instructors**. На странице отображается `Location` (кабинет) из связанной сущности `OfficeAssignment`. Если OfficeAssignment имеет значение null, отображается пустая ячейка таблицы.

![Страница индекса преподавателей, когда ничего не выбрано](read-related-data/_static/instructors-index-no-selection.png)

Щелкните ссылку **Select** (Выбрать). Изменяется стиль строки.

### <a name="add-courses-taught-by-selected-instructor"></a>Добавление курсов, проводимых выбранным преподавателем

Измените метод `OnGetAsync` в *Pages/Instructors/Index.cshtml.cs*, используя следующий код:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

Добавление `public int CourseID { get; set; }`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

Проверьте измененный запрос:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Предыдущий запрос добавляет сущности `Department`.

Приведенный ниже код выполняется при выборе преподавателя (`id != null`). Выбранный преподаватель извлекается из списка преподавателей в модели представления. Из свойства навигации `CourseAssignments` этого преподавателя загружается свойство модели представления `Courses` вместе с сущностями `Course`.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

Метод `Where` возвращает коллекцию. В предыдущем методе `Where` возвращается всего одна сущность `Instructor`. Метод `Single` преобразует коллекцию в отдельную сущность `Instructor`. Сущность `Instructor` предоставляет доступ к свойству `CourseAssignments`. `CourseAssignments` предоставляет доступ к связанным сущностям `Course`.

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

Метод `Single` используется для коллекции, когда она содержит всего один элемент. Метод `Single` вызывает исключение, если коллекция пуста или содержит больше одного элемента. Альтернативным вариантом является метод `SingleOrDefault`, который возвращает значение по умолчанию (в данном случае null), если коллекция пуста. Использование `SingleOrDefault` для пустой коллекции:

* Возникает исключение (из-за попытки найти свойство `Courses` по пустой ссылке).
* Сообщение об исключении не так четко указывает на причину проблемы.

Следующий код заполняет свойство `Enrollments` модели представления при выборе курса:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Добавьте следующую разметку в конец страницы Razor *Pages/Instructors/Index.cshtml*:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

Предыдущая разметка отображает список связанных с преподавателем курсов при выборе преподавателя.

Проверьте работу приложения. Щелкните ссылку **Select** (Выбрать) на странице преподавателей.

![Страница индекса преподавателей, выбран преподаватель](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Отображение данных об учащихся

В этом разделе приложение изменяется, чтобы отобразить данные об учащихся для выбранного курса.

Измените запрос в методе `OnGetAsync` в *Pages/Instructors/Index.cshtml.cs*, используя следующий код:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Измените *Pages/Instructors/Index.cshtml*. Добавьте следующую разметку в конец файла:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Предыдущая разметка отображает список учащихся, которые зачислены на курс.

Обновите страницу и выберите преподавателя. Выберите курс, чтобы увидеть список зачисленных учащихся и их оценки.

![Страница индекса преподавателей, выбраны преподаватель и курс](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Использование метода Single

Метод `Single` может передать условие `Where` вместо отдельного вызова метода `Where`:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

Приведенный выше подход на основе `Single` не дает преимуществ по сравнению с использованием `Where`. Некоторые разработчики предпочитают использовать подход `Single`.

## <a name="explicit-loading"></a>Явная загрузка

Текущий код указывает упреждающую загрузку для `Enrollments` и `Students`:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Предположим, что пользователям редко требуется просматривать зачисления на курс. В этом случае можно оптимизировать работу, загружая данные о зачислении только при их запросе. В этом разделе изменяется `OnGetAsync` для использования явной загрузки `Enrollments` и `Students`.

Измените `OnGetAsync`, используя следующий код:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Предыдущий код удаляет вызовы метода *ThenInclude* для данных о зачислении и учащихся. Если курс выбран, выделенный код извлекает следующее:

* Сущности `Enrollment` для выбранного курса.
* Сущности `Student` для каждого `Enrollment`.

Обратите внимание, что предыдущий код закомментирует `.AsNoTracking()`. Для отслеживаемых сущностей свойства навигации можно загрузить лишь явно.

Проверьте работу приложения. С точки зрения пользователей приложение работает аналогично предыдущей версии.

Следующее руководство посвящено обновлению связанных данных.

>[!div class="step-by-step"]
>[Назад](xref:data/ef-rp/complex-data-model)
>[Вперед](xref:data/ef-rp/update-related-data)
