---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Обновление связанных данных с Entity Framework в приложении ASP.NET MVC (6 из 10) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: dde9d5022823b252e59949144e3021a53c0bdd3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371676"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Обновление связанных данных с Entity Framework в приложении ASP.NET MVC (6 из 10)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Серии руководств можно начать с самого начала или [Загрузите начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если вы столкнулись с проблемами, не удается устранить, [скачать завершенного глава](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы, сравнивая код, чтобы полный код. Некоторые распространенные ошибки и способы их устранения, см. в разделе [ошибки и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем руководстве вы отобразили связанные данные; в этом руководстве описано обновление связанных данных. Для большинства связей это можно сделать путем обновления полей внешнего ключа. Для связей многие ко многим Entity Framework не предоставляет таблицы соединения напрямую, поэтому необходимо явно добавить и удалить сущности и из соответствующих свойств навигации.

На следующих рисунках изображены страницы, с которыми вы будете работать.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Настройка страниц создания и редактирования для курсов

Создаваемая сущность курса должна иметь связь с существующей кафедрой. Чтобы упростить эту задачу, шаблонный код включает методы контроллеров, а также представления "Create" (Создание) и "Edit" (Редактирование) с раскрывающимся списком для выбора кафедры. Наборы с раскрывающимся списком `Course.DepartmentID` свойство внешнего ключа, и это все, что нужно Entity Framework для загрузки `Department` свойство навигации с соответствующим `Department` сущности. Вы будете использовать этот шаблонный код, немного его изменив, чтобы добавить обработку ошибок и сортировку раскрывающегося списка.

В *CourseController.cs*, удалить четыре `Edit` и `Create` методы и замените их следующим кодом:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList` Метод возвращает список всех кафедр, отсортированных по имени, создает `SelectList` коллекции для раскрывающегося списка и передает ее в представление в `ViewBag` свойство. Этот метод принимает необязательный параметр `selectedDepartment`, позволяющий вызывающему коду указать элемент, который будет выбран при отрисовке раскрывающегося списка. Представление передаст имя `DepartmentID` для [ `DropDownList` вспомогательный](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), и тогда знает вспомогательное приложение для поиска `ViewBag` для объекта `SelectList` с именем `DepartmentID`.

`HttpGet` `Create` Вызовы методов `PopulateDepartmentsDropDownList` метод без установки выбранного элемента, так как для нового курса отдел еще не установлено:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit` Метод задает выбранный элемент, на основе идентификатора кафедры, который уже назначен редактируемому курсу:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost` Методов для обоих `Create` и `Edit` также включить код, который задает выбранный элемент, когда они повторно отображают страницу после ошибки:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Этот код гарантирует, что при страницы для отображения сообщения об ошибке, выбор остается выбранным.

В *Views\Course\Create.cshtml*, добавьте выделенный код, чтобы создать новое поле номера курса перед **Title** поля. Как описано в предыдущем руководстве, в шаблоне ключевые поля отсутствуют по умолчанию, но этого первичного ключа применяется, чтобы предоставить пользователю возможность ввести значение ключа.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

В *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, и *Views\Course\Details.cshtml*, добавьте поле номера курса перед **Title**  поля. Так как он является первичным ключом, он отображается, но его нельзя изменить.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Запустите **создать** страницы (на страницу индекса курсов и нажмите кнопку **Create New**) и введите данные для нового курса:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Нажмите кнопку **Создать**. Новый курс, добавляемый в список отображается страница индекса курсов. Название кафедры в списке страницы индекса поступает из свойства навигации, показывая, что связь установлена правильно.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Запустите **изменить** страницы (на страницу индекса курсов и нажмите кнопку **изменить** на курс).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Измените данные на странице и нажмите кнопку **Save** (Сохранить). Отображение страницы индекса курсов с обновленными данными.

## <a name="adding-an-edit-page-for-instructors"></a>Добавление страницы редактирования для преподавателей

При редактировании записи преподавателя может потребоваться обновить назначенный преподавателю кабинет. `Instructor` Сущность имеет связь один к нулю или одному с `OfficeAssignment` сущность, которая означает, что необходимо обрабатывать следующие ситуации:

- Если пользователь сбрасывает назначение кабинета, которое изначально имело значение, необходимо удалить и удалить `OfficeAssignment` сущности.
- Если пользователь вводит значение для назначения кабинета, которое изначально было пустым, необходимо создать новый `OfficeAssignment` сущности.
- Если пользователь изменяет значение для назначения кабинета, необходимо изменить значение в существующем `OfficeAssignment` сущности.

Откройте *InstructorController.cs* и просмотрите `HttpGet` `Edit` метод:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Шаблонный код здесь не требуется. Выполняется настройка данных для раскрывающегося списка, но необходимые представляет собой текстовое поле. Замените этот метод следующий код:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Этот код удаляет `ViewBag` инструкции и добавляет безотложную загрузку для связанного `OfficeAssignment` сущности. Не удается выполнить безотложную загрузку с `Find` метод, поэтому `Where` и `Single` методы вместо них используются для выбора преподавателя.

Замените `HttpPost` `Edit` метод следующим кодом. обрабатывающая обновления назначения кабинета:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Этот код выполняет следующее:

- Получает текущую сущность `Instructor` из базы данных, используя безотложную загрузку для свойства навигации `OfficeAssignment`. Это так же, как описано в `HttpGet` `Edit` метод.
- Обновляет извлеченную сущность `Instructor`, используя значения из связывателя модели. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) позволяет перегруженный метод, используемый *белого списка* свойства, которые необходимо включить. Это предотвращает от чрезмерной передачи данных, как описано в [втором учебном курсе](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Если расположение кабинета пусто, задает `Instructor.OfficeAssignment` свойство равным null, чтобы связанные строки в `OfficeAssignment` таблица будет удалена.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Сохраняет изменения в базу данных.

В *Views\Instructor\Edit.cshtml*после `div` элементы для **Дата приема на работу** поле, добавьте новое поле для редактирования расположения кабинета:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Откройте страницу (выберите **преподавателей** вкладке и нажмите кнопку **изменить** для преподавателя). Измените значение **Office Location** (Расположение кабинета) и нажмите кнопку **Save** (Сохранить).

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Добавление назначений курсов для преподавателя «изменение»

Преподаватели могут вести любое число курсов. Теперь вы улучшите страницу редактирования преподавателя, добавив возможность изменять назначения курсов с помощью группы флажков, как показано на следующем снимке экрана:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Связь между `Course` и `Instructor` сущности — многие ко многим, это означает, что у вас нет прямого доступа к таблице join. Независимо от того, будет вместо этого можно добавлять и удалять сущности и обратно `Instructor.Courses` свойство навигации.

Пользовательский интерфейс, позволяющий изменить назначенные для преподавателя курсы, представляет собой группу флажков. Отображается флажок для каждого курса в базе данных, а флажки для курсов, назначенных данному преподавателю, установлены. Пользователь может устанавливать и снимать флажки, изменяя назначения курсов. Если число курсов было бы гораздо больше, может потребоваться использовать другой метод представления данных в представлении, но используется один и тот же метод управления свойства навигации для создания или удаления связей.

Чтобы предоставить данные в представлении для списка флажков, нужно использовать класс модели представления. Создание *AssignedCourseData.cs* в *ViewModels* папку и замените существующий код следующим кодом:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

В *InstructorController.cs*, замените `HttpGet` `Edit` метод следующим кодом. Изменения выделены.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Код добавляет безотложную загрузку для свойства навигации `Courses` и вызывает новый метод `PopulateAssignedCourseData` для предоставления сведений массиву флажков с помощью класса модели представления `AssignedCourseData`.

Код в `PopulateAssignedCourseData` метод считывает всю `Course` класс модели сущностей, чтобы загрузить список курсов с помощью представления. Для каждого курса код проверяет, существует ли этот курс в свойстве навигации `Courses` преподавателя. Чтобы создать эффективную подстановку при проверке, назначен ли курс преподавателю, назначаемые курсы помещаются в [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) коллекции. `Assigned` Свойству `true` курсы назначается преподавателя. Представление будет использовать это свойство, чтобы определить, какие флажки нужно отображать как выбранные. Наконец, список передается в представление в `ViewBag` свойство.

Добавьте код, выполняемый, когда пользователь нажимает кнопку **Save** (Сохранить). Замените `HttpPost` `Edit` метод следующим кодом, который вызывает новый метод, который обновляет `Courses` свойство навигации `Instructor` сущности. Изменения выделены.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Так как представление не содержит коллекцию `Course` сущностей, связыватель модели не может автоматически обновлять `Courses` свойство навигации. Вместо использования связывателя модели для обновления свойства навигации курсы, это можно сделать в новом `UpdateInstructorCourses` метод. Поэтому нужно исключить свойство `Courses` из привязки модели. Это не требует изменений в код, который вызывает [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) так, как вы используете *списка разрешенных* перегрузки и `Courses` отсутствует в списке include.

Если никакие флажки выбраны, код в `UpdateInstructorCourses` инициализирует `Courses` свойства навигации с пустой коллекции:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

После этого код в цикле проходит по всем курсам в базе данных и сравнивает каждый из них с теми, которые сейчас назначены преподавателю, в противоположность тем, которые были выбраны в представлении. Чтобы упростить эффективную подстановку, последние две коллекции хранятся в объектах `HashSet`.

Если флажок для курса был установлен, но курс отсутствует в свойстве навигации `Instructor.Courses`, этот курс добавляется в коллекцию в свойстве навигации.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Если флажок для курса не был установлен, но курс присутствует в свойстве навигации `Instructor.Courses`, этот курс удаляется из свойства навигации.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

В *Views\Instructor\Edit.cshtml*, добавьте **курсы** поле с массивом флажков, добавив выделенный ниже код сразу после `div` элементы для `OfficeAssignment` поле:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Этот код создает таблицу HTML с тремя столбцами. Каждый столбец содержит флажок, за которым идет заголовок с номером и названием курса. Все флажки имеют тем же именем («selectedCourses»), который уведомляет связывателя модели, в котором они должны рассматриваться как группу. `value` Атрибут для каждого флажка присваивается значение `CourseID.` при отправке страницы связыватель модели передает массив в контроллер, который состоит из `CourseID` значения только флажки установлены.

При первичной отрисовке флажки для курсов, назначенных преподавателю, имеют `checked` атрибутов, которые выбирает их (открывается установленными).

После изменения назначения курсов, необходимо иметь возможность проверить изменения, когда возвращается сайт `Index` страницы. Таким образом необходимо добавить столбец к таблице на этой странице. В этом случае не нужно использовать `ViewBag` объекта, так как сведения, которые вы хотите отобразить уже `Courses` свойство навигации `Instructor` сущность, которая передается на страницу, что и у модели.

В *Views\Instructor\Index.cshtml*, добавьте **курсы** заголовок сразу после **Office** заголовок, как показано в следующем примере:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Затем добавьте новую ячейку данных сразу после в ячейку сведений расположение office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Запустите **Instructor Index** страницу, чтобы увидеть курсов, назначенных каждой преподавателя:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Нажмите кнопку **изменить** на преподавателя, чтобы открыть страницы "Edit".

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Измените некоторые назначения курсов и нажмите кнопку **Сохранить**. Вносимые вами изменения отражаются на странице индекса.

 Примечание: Подход к редактированию данных курсов для преподавателя работает также в том случае, когда ограниченном числе курсов. Для коллекций большего размера следовало бы применять другой пользовательский интерфейс и другой метод обновления.  
 

## <a name="update-the-delete-method"></a>Обновите метод Delete

Измените код в метод HttpPost Delete, чтобы запись назначения office (если таковые имеются) удаляется при удалении преподавателя:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Если вы попытаетесь удалить преподавателя, назначенный в подразделение, от имени администратора, вы получите ошибки ссылочной целостности. См. в разделе [текущей версии этого учебника](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) для дополнительный код, который автоматически удалит преподавателя из любой отдел, где преподавателя присваивается в качестве администратора.

## <a name="summary"></a>Сводка

Теперь вы завершили этот введение в работу со связанными данными. До сих в этих учебниках вы выполнили полный диапазон CRUD-операций, но вы еще не обработаны проблем параллелизма. Следующем учебном курсе будет вводят разделе параллелизма, описываются параметры для его обработки и добавить параллелизма, обработки кода CRUD, который вы уже написали для одного типа сущности.

Ссылки на другие ресурсы Entity Framework можно найти в конце [в последнем руководстве этой серии](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Назад](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
