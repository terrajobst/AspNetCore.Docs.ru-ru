---
title: "Страниц Razor с основными EF - обновления связанных данных - 7, 8."
author: rick-anderson
description: "В этом учебнике будет обновить связанные данные, обновив внешних ключевых полей и свойств навигации."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 817bfd48dce94e7dbad96cb6f822494e3adfae1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>Обновление связанных данных - страниц Razor EF Core (7, 8)

По [Tom Dykstra](https://github.com/tdykstra), и [Рик Андерсон](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

В этом учебнике показано обновление связанных данных. Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).

На следующих рисунках показаны некоторые завершенной страницы.

![Страница изменения курса](update-related-data/_static/course-edit.png)
![инструктора изменения страницы](update-related-data/_static/instructor-edit-courses.png)

Проверьте и тестирование Создание и редактирование страниц курса. Создание нового курса. Подразделение выбирается по первичному ключу (целое число), не его имя. Изменение нового курса. После завершения тестирования удалите нового курса.

## <a name="create-a-base-class-to-share-common-code"></a>Создание базового класса для совместного использования кода

Курсы/Create курсы или изменение страницы и каждого требуется список названий отделов. Создание *Pages/Courses/DepartmentNamePageModel.cshtml.cs* базовый класс для создания и редактирования страниц:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Приведенный выше код создает [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) будет содержать список имен отдела. Если `selectedDepartment` указан, этот отдел выбран в `SelectList`.

Создание и изменение классы модели страницы, производной от `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Настройка страниц курсов

При создании новой сущности курса, он должен иметь отношение к отдела. Чтобы добавить подразделение при создании курса, базовый класс для создания и редактирования содержит раскрывающегося списка для выбора подразделения. В раскрывающемся списке устанавливается `Course.DepartmentID` свойство внешний ключ (FK). EF Core использует `Course.DepartmentID` внешнего ключа для загрузки `Department` свойство навигации.

![Создание курса](update-related-data/_static/ddl.png)

Обновление создать модель страницы с помощью следующего кода:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

Предыдущий код:

* Происходит от `DepartmentNamePageModel`.
* Использует `TryUpdateModelAsync` для предотвращения [оверпостинга](xref:data/ef-rp/crud#overposting).
* Заменяет `ViewData["DepartmentID"]` с `DepartmentNameSL` (от базового класса).

`ViewData["DepartmentID"]`заменяется со строгой типизацией `DepartmentNameSL`. Строго типизированные моделей предпочтительные через слабо типизированным. Дополнительные сведения см. в разделе [слабо типизированных данных (ViewData и ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Страница «обновление» создайте курсов

Обновление *Pages/Courses/Create.cshtml* следующей разметкой:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Предыдущей разметки вносит следующие изменения:

* Изменяет заголовок из **DepartmentID** для **отдел**.
* Заменяет `"ViewBag.DepartmentID"` с `DepartmentNameSL` (от базового класса).
* Добавлен параметр «Выберите отдела». Это изменение отображает «Select отдела» вместо первого отдела.
* Добавляет сообщение проверки, если отдел не выбран.

На странице Razor используется [выберите тег вспомогательный](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Проверьте страницы создания. На странице создания имеется название отдела, а не идентификатор отдела.

### <a name="update-the-courses-edit-page"></a>Страница «обновление» изменить курсы.

Обновление модели редактирования страниц с помощью следующего кода:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

Изменения, аналогичны внесенные в модель создания страницы. В приведенном выше коде `PopulateDepartmentsDropDownList` пройден в Идентификаторе отдела, что подразделение, указанное в раскрывающемся списке выберите.

Обновление *Pages/Courses/Edit.cshtml* следующей разметкой:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Предыдущей разметки вносит следующие изменения:

* Отображает идентификатор курса Обычно первичный ключ (PK) сущности не отображаются. Первичные ключи не обычно имеют смысла для пользователей. В этом случае первичного ключа является номером курса.
* Изменяет заголовок из **DepartmentID** для **отдел**.
* Заменяет `"ViewBag.DepartmentID"` с `DepartmentNameSL` (от базового класса).
* Добавлен параметр «Выберите отдела». Это изменение отображает «Select отдела» вместо первого отдела.
* Добавляет сообщение проверки, если отдел не выбран.

На этой странице содержатся скрытое поле (`<input type="hidden">`) для номера курса. Добавление `<label>` тег вспомогательное приложение с помощью `asp-for="Course.CourseID"` не устраняют необходимость в скрытое поле. `<input type="hidden">`требуется для должны быть включены в данные, когда пользователь щелкает номер **Сохранить**.

Проверьте обновленный код. Создание, изменение и удаление курса.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Добавьте сведения об AsNoTracking и удаление моделей страницы

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) может повысить производительность, если трассировка не требуется. Добавить `AsNoTracking` Delete и сведения о модели страницы. В следующем коде показано обновленной модели страницы Delete:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Обновление `OnGetAsync` метод в *Pages/Courses/Details.cshtml.cs* файла:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Изменять страницы, Delete и подробные сведения

Обновление страницы Razor удалить следующую разметку:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Внесения изменений на страницу сведений.

### <a name="test-the-course-pages"></a>Тестирование страниц курса

Тест создать, изменить сведения и удалить.

## <a name="update-the-instructor-pages"></a>Обновление страниц инструктора

Следующие разделы обновить инструктора страницы.

### <a name="add-office-location"></a>Добавить расположение office

При изменении записи инструктора, может потребоваться обновить назначение инструктора office. `Instructor` Сущность имеет отношение "один к нулю или одному" с `OfficeAssignment` сущности. Код инструктора должен обрабатывать:

* Если пользователь удаляет назначение office, удалите `OfficeAssignment` сущности.
* Если пользователь вводит назначения office, и он был пуст, создайте новый `OfficeAssignment` сущности.
* Если пользователь изменяет office назначения, обновить `OfficeAssignment` сущности.

Обновление модели страницы инструкторов изменить следующий код:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

Предыдущий код:

- Возвращает текущий `Instructor` сущностей из базы данных, используя упреждающую для `OfficeAssignment` свойства навигации.
- Обновляет извлеченного `Instructor` сущности значениями из связывателя модели. `TryUpdateModel`предотвращает [оверпостинга](xref:data/ef-rp/crud#overposting).
- Если отсутствует в расположении офиса, задает `Instructor.OfficeAssignment` значение null. Когда `Instructor.OfficeAssignment` равен null, связанной строки в `OfficeAssignment` удалить таблицу.

### <a name="update-the-instructor-edit-page"></a>Обновление страницы правки инструктора

Обновление *Pages/Instructors/Edit.cshtml* с расположением office:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Убедитесь, что можно изменить расположение инструкторов office.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Добавление назначения курса инструктора страницы правки

Преподаватели могут обучить любое количество курсов. В этом разделе добавьте возможность изменения курса назначения. Ниже приведен инструктора обновленные страницы правки:

![Страница изменения инструктора курсы](update-related-data/_static/instructor-edit-courses.png)

`Course`и `Instructor` имеет отношение многие ко многим. Чтобы добавить и удалить связи, можно добавлять и удалять сущности из `CourseAssignments` join набора сущностей.

Флажки производить изменения курсов, присвоенного инструктор. Флажок отображается для каждого курса в базе данных. Курсы, которые назначены инструктора проверяются. Пользователь может установите или снимите флажки, чтобы изменить назначения курса. Если количество курсов были гораздо выше:

* Возможно, используется другой пользовательский интерфейс для отображения курсов.
* Метод управления сущности объединения можно создавать и удалять связи не будут изменяться.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Добавлять классы для поддержки создания и редактирования страниц инструктора

Создание *SchoolViewModels/AssignedCourseData.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

`AssignedCourseData` Класс содержит данные, создавать флажки для назначенного курсов по инструктор.

Создание *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* базового класса:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` Является базовым классом, будет использоваться для изменения и создавать модели страницы. `PopulateAssignedCourseData`Считывает все `Course` сущностей для заполнения `AssignedCourseDataList`. Для каждого курса код задает `CourseID`, заголовок и ли инструктора назначается курса. Объект [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) используется для создания эффективного поиска.

### <a name="instructors-edit-page-model"></a>Модель страницы редактирование инструкторов

Обновление модели страницы инструктора изменить следующий код:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

Предыдущий код обрабатывает изменения в назначения office.

Обновление представления Razor инструктора:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> При вставке кода в Visual Studio способом, нарушающим код изменяются разрывы строки. Нажмите один раз сочетание клавиш Ctrl + Z для отмены автоматического форматирования. CTRL + Z устраняет разрывы строки, чтобы они выглядят как показано ниже. Отступы не должен быть идеальным, но `@</tr><tr>`, `@:<td>`, `@:</td>`, и `@:</tr>` строки должны быть в одной строке, как показано. С блоком выбран новый код нажмите клавишу Tab трижды Чтобы выровнять новый код с существующим кодом. Проголосовать или просмотр состояния этой ошибки [с этой ссылкой](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Приведенный выше код создает таблицу HTML, который содержит три столбца. Каждый столбец имеет флажок и заголовок, содержащий номер и название. Все флажки имеют одно и то же имя («selectedCourses»). С тем же именем информирует связывателя модели, обрабатывать их как группу. Присвоено недопустимое значение атрибута каждого флажка `CourseID`. При отправке страницы связывателя модели передает массив, состоящий из `CourseID` только флажки, выбранные значения.

При первоначальном отображении флажки курсов, назначенные инструктора проверки атрибутов.

Запустите приложение и проверить обновленные инструкторов страницы правки. Измените некоторые назначения курса. Изменения отражаются на странице индекса.

Примечание: Подходом, который используется здесь для изменения данных курса инструктора удобно использовать, когда имеется ограниченное число курсов. Для коллекций, которые являются гораздо большего размера более готовый к применению и эффективный будет другой пользовательский Интерфейс и другой метод обновления.

### <a name="update-the-instructors-create-page"></a>Обновление страницы создания инструкторов

Обновление модели инструктора создать страницу следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Предыдущий код аналогичен *Pages/Instructors/Edit.cshtml.cs* кода.

Обновление страницы инструктора Razor, создайте следующую разметку:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Тестовая страница создания инструктора.

## <a name="update-the-delete-page"></a>Обновление страницы удаления

Обновление модели страницы удаления со следующим кодом:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

Приведенный выше код выполняет следующие изменения:

* Использует упреждающую для `CourseAssignments` свойства навигации. `CourseAssignments`должен быть включен или они не удаляются при удалении инструктора. Чтобы избежать необходимости их прочитать, настройте каскадное удаление в базе данных.

* Если администратор любые отделы назначается инструктора для удаления, удаляет инструктора назначения из тех отделов.

>[!div class="step-by-step"]
[Назад](xref:data/ef-rp/read-related-data)
[Вперед](xref:data/ef-rp/concurrency)
