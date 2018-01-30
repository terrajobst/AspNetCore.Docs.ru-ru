---
title: "Страниц Razor с основными EF - чтение связанных данных - 6, 8."
author: rick-anderson
description: "В этом учебнике чтения и отображения связанных данных — то есть данных, загружающий Entity Framework в свойствах навигации."
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ccb1e95ae2b43fd0a4c4b1ac9ed58a4d474ab3b6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>Чтение связанных данных - Core EF со страницами Razor (6, 8)

По [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), и [Рик Андерсон](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

В этом учебнике связанных данных считывается и отображается. Связанные данные — это данные, основные EF загружает в свойствах навигации.

Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).

На следующих рисунках показаны завершенного страниц для этого учебника:

![Страница индекса курсов](read-related-data/_static/courses-index.png)

![Страница индекса инструкторов](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Упреждающая, явные и отложенной загрузки связанных данных

Существует несколько способов, что EF Core можно загрузить связанные данные в свойства навигации сущности:

* [Безотложная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading). Безотложная загрузка при запросе для одного типа сущности также загружает связанные сущности. При чтении сущности извлекается соответствующими данными. Обычно в результате запроса одного соединения, который получает все данные, которые необходимы. EF Core выдаст несколько запросов для некоторых типов упреждающую. Выдача нескольких запросов может оказаться более эффективным, чем в случае для некоторых запросов в EF6 которых произошла один запрос. Безотложная загрузка, указанном с помощью `Include` и `ThenInclude` методы.

 ![Пример Безотложная загрузка](read-related-data/_static/eager-loading.png)
 
 Безотложная загрузка отправляет несколько запросов, когда включено nvavigation коллекции:

 * Один запрос в основном запросе 
 * Один запрос для каждой коллекции «границы» в дереве нагрузочных.

* Отдельные запросы с `Load`: данные могут быть получены в отдельных запросах и EF Core «устраняет» свойства навигации. «исправления вверх» означает, что EF Core автоматически заполняет свойства навигации. Отдельные запросы с `Load` близко explict, чем упреждающую загрузки.

 ![Пример отдельных запросов](read-related-data/_static/separate-queries.png)

 Примечание: Основной EF автоматически исправляет свойства навигации к другим сущностям, которые были ранее загружены в экземпляр контекста. Даже если данные для свойства навигации *не* явно включен, свойство по-прежнему могут быть заполнены, если некоторые или все связанные сущности были ранее загружены.

* [Явная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading). При считывании сущности связанные данные не получены. Код должен быть написан для получения взаимосвязанных данных в том случае, когда он необходим. Явная загрузка с отдельных запросов приводит несколько запросов, отправляемых на базу данных. При явной загрузке, код задает свойства навигации для загрузки. Используйте `Load` метод для выполнения явная загрузка. Пример:

 ![Пример явная загрузка](read-related-data/_static/explicit-loading.png)

* [Отложенная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading). [Основные EF в настоящее время не поддерживает отложенную загрузку](https://github.com/aspnet/EntityFrameworkCore/issues/3797). При считывании сущности связанные данные не получены. Время первого обращения к свойству навигации, автоматически извлекается данные, необходимые для этого свойства навигации. Запрос отправляется в базу данных, каждый раз к свойству навигации в первый раз.

* `Select` Оператор загружает связанные данные, необходимые.

## <a name="create-a-courses-page-that-displays-department-name"></a>Создание страницы курсов, отображающий название отдела

Сущность курс включает свойство навигации, которое содержит `Department` сущности. `Department` Сущность содержит отдела, которые назначены курса.

Чтобы отобразить имя подразделения, назначенный в списке курсов:

* Получить `Name` свойство из `Department` сущности.
* `Department` Сущность поступают из `Course.Department` свойство навигации.

![ourse. Подразделение](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Формировать модели курса

* Выйдите из Visual Studio.
* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* Выполните следующую команду:

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

Предыдущий закрепление команда `Course` модели. Откройте проект в Visual Studio.

Выполните построение проекта. Сборки приводит к возникновению ошибки следующим образом:

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Глобальная замена `_context.Course` для `_context.Courses` (то есть, добавить «s» `Course`). 7 вхождений обнаружены и обновлены.

Откройте *Pages/Courses/Index.cshtml.cs* и изучить `OnGetAsync` метод. Формирование шаблонов engine указано упреждающую для `Department` свойства навигации. `Include` Метод задает упреждающую.

Запустите приложение и выберите **курсы** ссылку. Отображает столбец отдел `DepartmentID`, который не является полезным.

Обновите метод `OnGetAsync`, используя следующий код:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Предыдущий код добавляет `AsNoTracking`. `AsNoTracking`повышает производительность, поскольку сущностей, возвращаемых не отслеживаются. Сущности, не отслеживаются, так как они не обновлялись в текущем контексте.

Обновление *Views/Courses/Index.cshtml* с следующую выделенную разметку:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Для формирования шаблонов код были внесены следующие изменения:

* Изменить заголовок из индекса к курсам.
* Добавлен **номер** столбца, отображающего `CourseID` значение свойства. По умолчанию первичные ключи не являются формирования шаблонов, так как обычно они имеют смысла для конечных пользователей. Однако в этом случае первичный ключ имеет значение.
* Изменить **отдел** столбец, отображающий название отдела. Код отображает `Name` свойство `Department` сущность, которая загружается в `Department` свойство навигации:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Запустите приложение и выберите **курсы** вкладку, чтобы просмотреть список с именами отдела.

![Страница индекса курсов](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Выполняется загрузка связанных данных с помощью Select

`OnGetAsync` Метод загружает данные, связанные с `Include` метод:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`Select` Оператор загружает связанные данные, необходимые. Для отдельных элементов таких как `Department.Name` используется SQL INNER JOIN. Для коллекций, он использует доступ к другой базе данных, но также возрастает.`Include` оператор с коллекциями.

Следующий код загружает данные, связанные с `Select` метод:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

В разделе [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) и [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) полный пример.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Создание страницы инструкторов курсов и регистрации

В этом разделе создается инструкторов страницы.

<a name="IP"></a>
![Страница индекса инструкторов](read-related-data/_static/instructors-index.png)

Эта страница считывает и взаимосвязанные данные отображаются следующим образом:

* В списке инструкторов отображаются связанные данные из `OfficeAssignment` сущности (Office на предыдущем рисунке). `Instructor` И `OfficeAssignment` сущности находятся в отношении один к нулю или одному. Безотложная загрузка используется для `OfficeAssignment` сущностей. Безотложная загрузка обычно более эффективны, когда требуется отобразить связанные данные. В этом случае отображаются назначения office для инструкторов.
* Когда пользователь выбирает инструктора (Harui на предыдущем рисунке), связанные с `Course` сущности отображаются. `Instructor` И `Course` сущности находятся в отношении многие ко многим. Загрузка для Eager `Course` сущности и связанных с ними `Department` используется сущностей. В этом случае отдельные запросы могут оказаться эффективными, поскольку требуются только курсов для выбранного инструктора. В этом примере показано, как использовать упреждающую для свойств навигации в сущности, находящиеся в свойства навигации.
* Когда пользователь выбирает курса (химии на предыдущем рисунке), связанные с данными из `Enrollments` сущность отображается. На предыдущем рисунке отображаются студента имя и уровень. `Course` И `Enrollment` сущности находятся в отношении один ко многим.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Создать модель представлений для представления инструктора индекса

На странице инструкторов отображаются данные из трех разных таблицах. Модель представления создается, включает три сущности, представляющие три таблицы.

В *SchoolViewModels* папке создайте *InstructorIndexData.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Формировать модели инструктора

* Выйдите из Visual Studio.
* Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).
* Выполните следующую команду:

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

Предыдущий закрепление команда `Instructor` модели. Откройте проект в Visual Studio.

Выполните построение проекта. Сборки приводит к возникновению ошибки.

Глобальная замена `_context.Instructor` для `_context.Instructors` (то есть, добавить «s» `Instructor`). 7 вхождений обнаружены и обновлены.

Запустите приложение и перейдите на страницу инструкторов.

Замените *Pages/Instructors/Index.cshtml.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

`OnGetAsync` Метод принимает необязательный маршрутизации данных для идентификатора выбранного инструктора.

Проверьте запрос на *Pages/Instructors/Index.cshtml* страницы:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

Запрос имеет два включает в себя:

* `OfficeAssignment`: Отображение [инструкторов представление](#IP).
* `CourseAssignments`: Вызывает в курсов обучения.


### <a name="update-the-instructors-index-page"></a>Обновление страницы индекса инструкторов

Обновление *Pages/Instructors/Index.cshtml* следующей разметкой:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Предыдущей разметки вносит следующие изменения:

* Обновления `page` директив из `@page` для `@page "{id:int?}"`. `"{id:int?}"`Это шаблон маршрута. Шаблон маршрута изменяет целое число строк запроса в URL-адрес для маршрутизации данных. Например, если щелкнуть **выберите** ссылку для приема инструктора при директивы страницы выводятся URL-адрес, как показано ниже:

    `http://localhost:1234/Instructors?id=2`

    После директивы страницы `@page "{id:int?}"`, предыдущие URL-адрес:

    `http://localhost:1234/Instructors/2`

* Заголовок страницы является **инструкторов**.
* Добавлен **Office** столбец, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не равно null. Так как один к нулю или одному связь, могут не существовать OfficeAssignment связанной сущности.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Добавлен **курсы** столбец, отображающий курсы при изучении каждого инструктора. В разделе [явные строки перехода с `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) для получения дополнительных сведений о данного синтаксиса razor.

* Добавленный код, который динамически добавляет `class="success"` для `tr` инструктора выбранного элемента. Это задает цвет фона для строк, выделенных с помощью класса начальной загрузки.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Добавлена новая гиперссылка с меткой **выберите**. Идентификатор выбранной инструктора отправляет эту ссылку `Index` метод и задает цвет фона.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Запустите приложение и выберите **инструкторов** вкладки. На странице отображается `Location` (office) из связанного `OfficeAssignment` сущности. Если OfficeAssignment "имеет значение null, отображается по пустой ячейке таблице.

![Страница индекса инструкторов ничего не выбраны](read-related-data/_static/instructors-index-no-selection.png)

Щелкните **выберите** ссылки. Изменения стиля строки.

### <a name="add-courses-taught-by-selected-instructor"></a>Добавить выбранные инструктора при изучении курсы

Обновление `OnGetAsync` метод в *Pages/Instructors/Index.cshtml.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

Проверьте обновленный запрос:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

Предыдущий запрос добавляет `Department` сущностей.

В следующем коде выполняется при выборе инструктор (`id != null`). Выбранный инструктора извлекается из списка инструкторов в модели представления. Модель представления `Courses` заполняемые свойства `Course` сущностей из этого инструктора `CourseAssignments` свойство навигации.

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

`Where` Метод возвращает коллекцию. В предыдущем `Where` метод только один `Instructor` возвращается сущность. `Single` Метод преобразует коллекцию в один `Instructor` сущности. `Instructor` Сущность предоставляет доступ к `CourseAssignments` свойство. `CourseAssignments`предоставляет доступ к связанным `Course` сущностей.

![M:M инструктора курсов](complex-data-model/_static/courseassignment.png)

`Single` Метод используется с коллекцией, если в коллекции присутствует только один элемент. `Single` Метод создает исключение, если коллекция пуста, или если существует более одного элемента. Альтернативным вариантом является `SingleOrDefault`, который возвращает значение по умолчанию (null в данном случае), если коллекция пуста. С помощью `SingleOrDefault` для пустой коллекции:

* Приводит к возникновению исключения (из найти `Courses` свойство на указатель null).
* Сообщение об исключении менее четко укажет причину проблемы.

Следующий код заполняет модель представления `Enrollments` свойств при выборе курса:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Добавьте следующую разметку в конец *Pages/Courses/Index.cshtml* Razor страницы:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

Предыдущей разметки список связанных с инструктор при выборе инструктор курсов.

Тестирование приложения. Щелкните **выберите** ссылку на странице инструкторов.

![Инструкторов индекс страницы инструктора выбран](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Показать учащихся данные

В этом разделе приложение обновляется для отображения данных студента выбранного курса.

Обновить запрос в `OnGetAsync` метод в *Pages/Instructors/Index.cshtml.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Обновление *Pages/Instructors/Index.cshtml*. Добавьте следующую разметку в конец файла:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Предыдущей разметки список учащихся, которые участвуют в выбранного курса.

Обновите страницу и выберите инструктор. Выберите курс, чтобы просмотреть список зарегистрированных студентов и их оценки.

![Инструктора страницы индекса инструкторов и выбранного курса](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>С помощью одного

`Single` Метод можно передавать в `Where` условие вместо вызова метода `Where` метод отдельно:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

Приведенный выше `Single` подход не дает преимущества по сравнению с использованием `Where`. Некоторые разработчики предпочитают `Single` подхода стиля.

## <a name="explicit-loading"></a>Явная загрузка

Текущий код указывает упреждающую для `Enrollments` и `Students`:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Предположим, что пользователи редко хотят видеть регистрации курса. В этом случае оптимизация будет загружать данных регистрации, только если она запрошена. В этом разделе `OnGetAsync` обновляется для использования явное загрузку `Enrollments` и `Students`.

Обновление `OnGetAsync` следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Предыдущий код удаляет *ThenInclude* вызывает метод для регистрации и студентов данных. Если выбран курса, получает выделенный код:

* `Enrollment` Сущностей для выбранного курса.
* `Student` Сущностей для каждого `Enrollment`.

Обратите внимание на то, приведенный выше код переводит в комментарий `.AsNoTracking()`. Свойства навигации могут быть явно загружены только для отслеживаемых объектов.

Тестирование приложения. С точки зрения пользователей приложение работает аналогично предыдущей версии.

Далее учебнике показано, как обновить связанные данные.

>[!div class="step-by-step"]
>[Назад](xref:data/ef-rp/complex-data-model)
>[Вперед](xref:data/ef-rp/update-related-data)
