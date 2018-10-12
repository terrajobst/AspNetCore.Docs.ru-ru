---
title: Razor Pages с EF Core в ASP.NET Core — обновление связанных данных — 7 из 8
author: rick-anderson
description: В этом руководстве описано обновление связанных данных путем обновления полей внешнего ключа и свойств навигации.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: c9d0e97dc612744f0023c48a27eb1c89fd153b95
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523172"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a>Razor Pages с EF Core в ASP.NET Core — обновление связанных данных — 7 из 8

Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

В этом учебнике демонстрируется обновление связанных данных. При возникновении проблем, которые вам не удается устранить, [скачайте или просмотрите готовое приложение.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Указания по скачиванию](xref:tutorials/index#how-to-download-a-sample).

На следующих рисунках изображены некоторые готовые страницы.

![Страница редактирования курса](update-related-data/_static/course-edit.png)
![Страница редактирования преподавателя](update-related-data/_static/instructor-edit-courses.png)

Просмотрите и протестируйте страницы создания и редактирования курса. Создайте новый курс. Кафедра выбирается по целочисленному первичному ключу, а не по названию. Измените новый курс. После завершения тестирования удалите новый курс.

## <a name="create-a-base-class-to-share-common-code"></a>Создание базового класса для совместного использования общего кода

На страницах Courses/Create и Courses/Edit используется список названий кафедр. Создайте базовый класс *Pages/Courses/DepartmentNamePageModel.cshtml.cs* для страниц Create и Edit:

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

Приведенный выше код создает [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0), содержащий список названий кафедр. Если указан параметр `selectedDepartment`, кафедра выбрана в списке `SelectList`.

Классы моделей страниц Create и Edit являются производными от `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Страницы настройки курсов

Создаваемая сущность курса должна иметь связь с существующей кафедрой. Чтобы добавить кафедру при создании курса, в базовом классе для страниц Create и Edit реализован раскрывающийся список, в котором можно выбрать кафедру. Этот раскрывающийся список устанавливает свойство внешнего ключа `Course.DepartmentID`. Платформа EF Core использует внешний ключ `Course.DepartmentID` для загрузки свойства навигации `Department`.

![Создание курса](update-related-data/_static/ddl.png)

Обновите модель страницы Create, используя следующий код:

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

Предыдущий код:

* Происходит от `DepartmentNamePageModel`.
* Использует `TryUpdateModelAsync`, чтобы предотвратить [чрезмерную передачу данных](xref:data/ef-rp/crud#overposting).
* Заменяет `ViewData["DepartmentID"]` на `DepartmentNameSL` (из базового класса).

`ViewData["DepartmentID"]` заменяется строго типизированным `DepartmentNameSL`. Вместо слабо типизированных моделей рекомендуется использовать строго типизированные. Дополнительные сведения см. в разделе [Слабо типизированные данные (ViewData и ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Обновление страницы создания курсов

Обновите файл *Pages/Courses/Create.cshtml*, используя следующую разметку:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

Приведенная выше разметка вносит следующие изменения:

* Изменяет заголовок с **DepartmentID** на **Department**.
* Заменяет `"ViewBag.DepartmentID"` на `DepartmentNameSL` (из базового класса).
* Добавляет параметр "Select Department" (Выбор кафедры). В результате этого изменения вместо первой кафедры отображается параметр "Select Department" (Выбор кафедры).
* Добавляет сообщение о проверке в том случае, если не выбрана кафедра.

На странице Razor Pages используется [вспомогательная функция тега Select](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Протестируйте страницу создания. На странице Create отображается название, а не идентификатор кафедры.

### <a name="update-the-courses-edit-page"></a>Обновите страницу редактирования курсов.

Обновите модель страницы редактирования, используя следующий код:

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

Изменения аналогичны внесенным в модель страницы Create. В приведенном выше коде `PopulateDepartmentsDropDownList` передает идентификатор кафедры, по которому выбирается кафедра, заданная в раскрывающемся списке.

Обновите файл *Pages/Courses/Edit.cshtml*, используя следующую разметку:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

Приведенная выше разметка вносит следующие изменения:

* Отображает идентификатор курса. Как правило, первичный ключ сущности не отображается. Первичные ключи для пользователей обычно не имеют значения. В этом случае в качестве первичного ключа используется номер курса.
* Изменяет заголовок с **DepartmentID** на **Department**.
* Заменяет `"ViewBag.DepartmentID"` на `DepartmentNameSL` (из базового класса).

На этой странице содержится скрытое поле (`<input type="hidden">`) с номером курса. Добавление вспомогательной функции тега `<label>` с `asp-for="Course.CourseID"` не избавляет от необходимости использовать это скрытое поле. `<input type="hidden">` необходимо, чтобы включить номер курса в отправляемые данные при нажатии пользователем кнопки **Save** (Сохранить).

Протестируйте обновленный код. Создайте, измените и удалите курс.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Добавление AsNoTracking в модели страниц Details и Delete

Применение [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) позволяет повысить производительность в тех сценариях, где не требуется отслеживание. Добавьте `AsNoTracking` в модели страниц Delete и Details. Следующий код отображает обновленную модель страницы Delete:

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Обновите метод `OnGetAsync` в файле *Pages/Courses/Details.cshtml.cs*:

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Изменение страниц Delete и Details

Обновите страницу Razor Pages Delete, используя следующую разметку:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Выполните те же изменения для страницы Details.

### <a name="test-the-course-pages"></a>Тестирование страниц курса

Протестируйте страницы создания, редактирования и сведений, после чего удалите их.

## <a name="update-the-instructor-pages"></a>Обновление страниц преподавателя

В следующих разделах обновляются страницы преподавателя.

### <a name="add-office-location"></a>Добавить расположения кабинета

При редактировании записи преподавателя может потребоваться обновить назначенный преподавателю кабинет. Сущность `Instructor` имеет отношение "один к нулю или к одному" с сущностью `OfficeAssignment`. Код преподавателя должен обрабатывать следующее:

* Если пользователь удаляет назначение кабинета, удаляется сущность `OfficeAssignment`.
* Если пользователь вводит назначение пустого кабинета, создается новая сущность `OfficeAssignment`.
* Если пользователь изменяет назначение кабинета, обновляется сущность `OfficeAssignment`.

Обновите модель страницы редактирования преподавателей, используя следующий код:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

Предыдущий код:

- Получает текущую сущность `Instructor` из базы данных, используя безотложную загрузку для свойства навигации `OfficeAssignment`.
- Обновляет извлеченную сущность `Instructor`, используя значения из связывателя модели. `TryUpdateModel` позволяет предотвратить [чрезмерную передачу данных](xref:data/ef-rp/crud#overposting).
- Если расположение кабинета пусто, `Instructor.OfficeAssignment` получает значение NULL. Если `Instructor.OfficeAssignment` имеет значение NULL, связанная строка в таблице `OfficeAssignment` удаляется.

### <a name="update-the-instructor-edit-page"></a>Обновление страницы редактирования преподавателя

Обновите файл *Pages/Instructors/Edit.cshtml*, указав расположение кабинета:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Убедитесь, что можно изменить расположение кабинета для преподавателя.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Добавление назначений курсов на страницу редактирования преподавателя

Преподаватели могут вести любое число курсов. В этом разделе вы добавите возможность изменять назначения курсов. На следующем рисунке показана обновленная страница редактирования преподавателя:

![Страница редактирования преподавателя с курсами](update-related-data/_static/instructor-edit-courses.png)

Между сущностями `Course` и `Instructor` существует отношение "многие ко многим". Для добавления и удаления отношений можно добавлять сущности в список объединенного набора сущностей `CourseAssignments` и удалять их из него.

С помощью флажков можно изменять курсы, которым назначен преподаватель. Флажок отображается для каждого курса в базе данных. Для курсов, которым назначен преподаватель, флажок установлен. Пользователь может устанавливать и снимать флажки, изменяя назначения курсов. Если число курсов было бы гораздо больше:

* В таком сценарии, скорее всего, применялся бы другой пользовательский интерфейс для отображения курсов.
* Способ управления объединением сущностей для создания или удаления отношений при этом не изменился бы.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Добавление классов для поддержки страниц создания и редактирования преподавателя

Создайте файл *SchoolViewModels/AssignedCourseData.cs* со следующим кодом:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

Класс `AssignedCourseData` содержит данные для создания флажков, определяющих назначенные преподавателю курсы.

Создайте базовый класс *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*:

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

Базовый класс `InstructorCoursesPageModel` будет использоваться для моделей страниц редактирования и создания. `PopulateAssignedCourseData` считывает все сущности `Course` для заполнения списка `AssignedCourseDataList`. Для каждого курса код задает `CourseID`, название, а также сведения о назначении курсу преподавателя. Для реализации эффективного поиска используется класс [HashSet](/dotnet/api/system.collections.generic.hashset-1).

### <a name="instructors-edit-page-model"></a>Модель страницы редактирования преподавателей

Обновите модель страницы редактирования преподавателя, используя следующий код:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

Приведенный выше код обрабатывает изменения в назначении кабинета.

Обновите представление Razor преподавателя:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> При вставке кода в Visual Studio разрывы строк изменяются, нарушая код. Один раз нажмите клавиши CTRL+Z, чтобы отменить автоматическое форматирование. При нажатии клавиш CTRL+Z разрывы строк исправляются, благодаря чему код приобретает показанный здесь вид. Выравнивать отступы необязательно, однако строки `@</tr><tr>`, `@:<td>`, `@:</td>` и `@:</tr>` должны находиться на одной строке, как показано здесь. Выделите блок нового кода и три раза нажмите клавишу TAB, чтобы выровнять его с существующим кодом. Чтобы проголосовать за эту ошибку или проверить ее статус, воспользуйтесь [этой ссылкой](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

Приведенный выше код создает таблицу HTML с тремя столбцами. Каждый столбец содержит флажок и заголовок с номером и названием курса. Все флажки имеют одинаковые имена ("selectedCourses"). Поскольку используется одно и то же имя, связыватель модели интерпретирует их как группу. Атрибуту значения для каждого флажка присвоен `CourseID`. При отправке страницы связыватель модели передает массив, содержащий значения `CourseID` только для выбранных флажков.

При первичной отрисовке для курсов, назначенных преподавателю, отображаются установленные флажки.

Запустите приложение и протестируйте обновленную страницу редактирования преподавателя. Измените некоторые назначения курсов. Изменения отражаются на странице указателя.

Примечание. Описываемый здесь подход к редактированию данных курсов для преподавателя эффективен при ограниченном числе курсов. Для коллекций большего размера более практичным и эффективным было бы применение другого пользовательского интерфейса и другого метода обновления.

### <a name="update-the-instructors-create-page"></a>Обновление страницы создания преподавателей

Обновите модель страницы создания преподавателя, используя следующий код:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

Приведенный выше код аналогичен коду в файле *Pages/Instructors/Edit.cshtml.cs*.

Обновите страницу создания преподавателя Razor Pages, используя следующую разметку:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Протестируйте страницу создания преподавателя.

## <a name="update-the-delete-page"></a>Обновление страницы удаления

Обновите страницу "Delete" (Удаление) с помощью следующего кода:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

Приведенный выше код вносит следующие изменения:

* Использует упреждающую загрузку для свойства навигации `CourseAssignments`. Требуется включить `CourseAssignments`, иначе они не будут удалены при удалении преподавателя. Чтобы избежать необходимости считывать их, настройте каскадное удаление в базе данных.

* Если преподаватель, которого требуется удалить, назначен в качестве администратора любой из кафедр, удаляется назначение преподавателя из таких кафедр.

> [!div class="step-by-step"]
> [Назад](xref:data/ef-rp/read-related-data)
> [Вперед](xref:data/ef-rp/concurrency)
