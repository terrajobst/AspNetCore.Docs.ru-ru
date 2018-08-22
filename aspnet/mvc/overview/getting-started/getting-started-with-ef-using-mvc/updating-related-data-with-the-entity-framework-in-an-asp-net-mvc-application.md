---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Обновление связанных данных с Entity Framework в приложении ASP.NET MVC | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio...
ms.author: riande
ms.date: 05/01/2015
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e7f5fd725a0d151f19f49be9ceaf52b049d459c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837275"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Обновление связанных данных с Entity Framework в приложении ASP.NET MVC
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем руководстве вы отобразили связанные данные; в этом руководстве описано обновление связанных данных. Для большинства связей это можно сделать путем обновления полей внешнего ключа или свойства навигации. Для связей многие ко многим Entity Framework не предоставляет таблицы соединения напрямую, поэтому можно добавлять и удалять сущности и из соответствующих свойств навигации.

На следующих рисунках изображены некоторые из страниц, с которыми вы будете работать.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Редактирования преподавателя с курсами](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Настройка страниц создания и редактирования для курсов

Создаваемая сущность курса должна иметь связь с существующей кафедрой. Чтобы упростить эту задачу, шаблонный код включает методы контроллеров, а также представления "Create" (Создание) и "Edit" (Редактирование) с раскрывающимся списком для выбора кафедры. Наборы с раскрывающимся списком `Course.DepartmentID` свойство внешнего ключа, и это все, что нужно Entity Framework для загрузки `Department` свойство навигации с соответствующим `Department` сущности. Вы будете использовать этот шаблонный код, немного его изменив, чтобы добавить обработку ошибок и сортировку раскрывающегося списка.

В *CourseController.cs*, удалить четыре `Create` и `Edit` методы и замените их следующим кодом:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Добавьте следующий `using` инструкцию в начало файла:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList` Метод возвращает список всех кафедр, отсортированных по имени, создает `SelectList` коллекции для раскрывающегося списка и передает ее в представление в `ViewBag` свойство. Этот метод принимает необязательный параметр `selectedDepartment`, позволяющий вызывающему коду указать элемент, который будет выбран при отрисовке раскрывающегося списка. Представление передаст имя `DepartmentID` для [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) вспомогательный метод и вспомогательное приложение затем пользуется `ViewBag` для объекта `SelectList` с именем `DepartmentID`.

`HttpGet` `Create` Вызовы методов `PopulateDepartmentsDropDownList` метод без установки выбранного элемента, так как для нового курса отдел еще не установлено:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit` Метод задает выбранный элемент, на основе идентификатора кафедры, который уже назначен редактируемому курсу:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost` Методов для обоих `Create` и `Edit` также включить код, который задает выбранный элемент, когда они повторно отображают страницу после ошибки:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Этот код гарантирует, что при страницы для отображения сообщения об ошибке, выбор остается выбранным.

Представлений курса уже сформированные с помощью раскрывающихся списков для поля department, но вы не хотите DepartmentID заголовок для этого поля, чтобы сделать следующий выделенный изменить на *Views\Course\Create.cshtml* файл Изменение подписи.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Сделать то же изменение в *Views\Course\Edit.cshtml*.

Обычно шаблон не позволяет формировать первичный ключ, поскольку значение ключа генерируется базой данных и не может быть изменено и не имеющее смысл значение, которое будет отображаться для пользователей. Для сущностей Course шаблон содержит текстовое поле для `CourseID` поле, так как он осознает, что `DatabaseGeneratedOption.None` атрибут означает, что пользователю нужно будет ввести значение первичного ключа. Но он не понимает, так как номер имеет смысл необходимость см. в других представлениях, поэтому необходимо добавить вручную.

В *Views\Course\Edit.cshtml*, добавьте поле номера курса перед **Title** поля. Так как он является первичным ключом, он отображается, но его нельзя изменить.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Уже есть скрытое поле (`Html.HiddenFor` вспомогательный) для номера курса в представлении редактирования. Добавление *Html.LabelFor* вспомогательный не устраняет потребность в скрытое поле, так как оно не вызывает номер курса, которые будут включены в отправленные данные, когда пользователь щелкает **Сохранить** на страницы "Edit".

В *Views\Course\Delete.cshtml* и *Views\Course\Details.cshtml*, измените заголовок имя отдел из «Name» для «Отдел» и добавьте поле номера курса перед **Title**  поля.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Запустите **создать** страницы (на страницу индекса курсов и нажмите кнопку **Create New**) и введите данные для нового курса:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Нажмите кнопку **Создать**. Новый курс, добавляемый в список отображается страница индекса курсов. Название кафедры в списке страницы индекса поступает из свойства навигации, показывая, что связь установлена правильно.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Запустите **изменить** страницы (на страницу индекса курсов и нажмите кнопку **изменить** на курс).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Измените данные на странице и нажмите кнопку **Save** (Сохранить). Отображение страницы индекса курсов с обновленными данными.

## <a name="adding-an-edit-page-for-instructors"></a>Добавление страницы редактирования для преподавателей

При редактировании записи преподавателя может потребоваться обновить назначенный преподавателю кабинет. `Instructor` Сущность имеет связь один к нулю или одному с `OfficeAssignment` сущность, которая означает, что необходимо обрабатывать следующие ситуации:

- Если пользователь сбрасывает назначение кабинета, которое изначально имело значение, необходимо удалить и удалить `OfficeAssignment` сущности.
- Если пользователь вводит значение для назначения кабинета, которое изначально было пустым, необходимо создать новый `OfficeAssignment` сущности.
- Если пользователь изменяет значение для назначения кабинета, необходимо изменить значение в существующем `OfficeAssignment` сущности.

Откройте *InstructorController.cs* и просмотрите `HttpGet` `Edit` метод:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Шаблонный код здесь не требуется. Выполняется настройка данных для раскрывающегося списка, но необходимые представляет собой текстовое поле. Замените этот метод следующий код:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Этот код удаляет `ViewBag` инструкции и добавляет безотложную загрузку для связанного `OfficeAssignment` сущности. Не удается выполнить безотложную загрузку с `Find` метод, поэтому `Where` и `Single` методы вместо них используются для выбора преподавателя.

Замените `HttpPost` `Edit` метод следующим кодом. обрабатывающая обновления назначения кабинета:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Ссылка на `RetryLimitExceededException` требует `using` оператор; добавить, щелкните правой кнопкой мыши `RetryLimitExceededException`и нажмите кнопку **устранить** - **с помощью System.Data.Entity.Infrastructure**.

![Разрешить исключений при повторе](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Этот код выполняет следующее:

- Изменяет имя метода для `EditPost` поскольку подпись является теперь так же, как `HttpGet` метод ( `ActionName` атрибут указывает, что по-прежнему используется /Edit/ URL-адрес).
- Получает текущую сущность `Instructor` из базы данных, используя безотложную загрузку для свойства навигации `OfficeAssignment`. Это так же, как описано в `HttpGet` `Edit` метод.
- Обновляет извлеченную сущность `Instructor`, используя значения из связывателя модели. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) позволяет перегруженный метод, используемый *белого списка* свойства, которые необходимо включить. Это предотвращает от чрезмерной передачи данных, как описано в [втором учебном курсе](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Если расположение кабинета пусто, задает `Instructor.OfficeAssignment` свойство равным null, чтобы связанные строки в `OfficeAssignment` таблица будет удалена.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Сохраняет изменения в базу данных.

В *Views\Instructor\Edit.cshtml*после `div` элементы для **Дата приема на работу** поле, добавьте новое поле для редактирования расположения кабинета:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Откройте страницу (выберите **преподавателей** вкладке и нажмите кнопку **изменить** для преподавателя). Измените значение **Office Location** (Расположение кабинета) и нажмите кнопку **Save** (Сохранить).

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Добавление назначений курсов для преподавателя «изменение»

Преподаватели могут вести любое число курсов. Теперь вы улучшите страницу редактирования преподавателя, добавив возможность изменять назначения курсов с помощью группы флажков, как показано на следующем снимке экрана:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Связь между `Course` и `Instructor` сущности — многие ко многим, это означает, что у вас нет прямого доступа к свойств внешнего ключа, которые являются в соединяемой таблице. Вместо этого добавления и удаления сущностей из `Instructor.Courses` свойство навигации.

Пользовательский интерфейс, позволяющий изменить назначенные для преподавателя курсы, представляет собой группу флажков. Отображается флажок для каждого курса в базе данных, а флажки для курсов, назначенных данному преподавателю, установлены. Пользователь может устанавливать и снимать флажки, изменяя назначения курсов. Если число курсов было бы гораздо больше, может потребоваться использовать другой метод представления данных в представлении, но используется один и тот же метод управления свойства навигации для создания или удаления связей.

Чтобы предоставить данные в представлении для списка флажков, нужно использовать класс модели представления. Создание *AssignedCourseData.cs* в *ViewModels* папку и замените существующий код следующим кодом:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

В *InstructorController.cs*, замените `HttpGet` `Edit` метод следующим кодом. Изменения выделены.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Код добавляет безотложную загрузку для свойства навигации `Courses` и вызывает новый метод `PopulateAssignedCourseData` для предоставления сведений массиву флажков с помощью класса модели представления `AssignedCourseData`.

Код в `PopulateAssignedCourseData` метод считывает всю `Course` класс модели сущностей, чтобы загрузить список курсов с помощью представления. Для каждого курса код проверяет, существует ли этот курс в свойстве навигации `Courses` преподавателя. Чтобы создать эффективную подстановку при проверке, назначен ли курс преподавателю, назначаемые курсы помещаются в [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) коллекции. `Assigned` Свойству `true` курсы назначается преподавателя. Представление будет использовать это свойство, чтобы определить, какие флажки нужно отображать как выбранные. Наконец, список передается в представление в `ViewBag` свойство.

Добавьте код, выполняемый, когда пользователь нажимает кнопку **Save** (Сохранить). Замените `EditPost` метод следующим кодом, который вызывает новый метод, который обновляет `Courses` свойство навигации `Instructor` сущности. Изменения выделены.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Сигнатура метода теперь отличается от `HttpGet` `Edit` метод, поэтому имя метода изменяется с `EditPost` обратно `Edit`.

Так как представление не содержит коллекцию `Course` сущностей, связыватель модели не может автоматически обновлять `Courses` свойство навигации. Вместо использования связывателя модели для обновления `Courses` свойство навигации, это можно сделать в новом `UpdateInstructorCourses` метод. Поэтому нужно исключить свойство `Courses` из привязки модели. Это не требует изменений в код, который вызывает [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) так, как вы используете *списка разрешенных* перегрузки и `Courses` отсутствует в списке include.

Если никакие флажки выбраны, код в `UpdateInstructorCourses` инициализирует `Courses` свойства навигации с пустой коллекции:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

После этого код в цикле проходит по всем курсам в базе данных и сравнивает каждый из них с теми, которые сейчас назначены преподавателю, в противоположность тем, которые были выбраны в представлении. Чтобы упростить эффективную подстановку, последние две коллекции хранятся в объектах `HashSet`.

Если флажок для курса был установлен, но курс отсутствует в свойстве навигации `Instructor.Courses`, этот курс добавляется в коллекцию в свойстве навигации.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Если флажок для курса не был установлен, но курс присутствует в свойстве навигации `Instructor.Courses`, этот курс удаляется из свойства навигации.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

В *Views\Instructor\Edit.cshtml*, добавьте **курсы** поле с массивом флажков, добавив следующий код сразу после `div` элементы для `OfficeAssignment` поля и Прежде чем `div` элемент для **Сохранить** кнопки:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

После вставки кода, если разрывы строк и отступы выглядят, как здесь, вручную все исправить, чтобы он выглядел так, как показано здесь. Выравнивать отступы необязательно, однако строки `@</tr><tr>`, `@:<td>`, `@:</td>` и `@</tr>` должны находиться на одной строке, как показано здесь. В противном случае возникает ошибка времени выполнения.

Этот код создает таблицу HTML с тремя столбцами. Каждый столбец содержит флажок, за которым идет заголовок с номером и названием курса. Все флажки имеют тем же именем («selectedCourses»), который уведомляет связывателя модели, в котором они должны рассматриваться как группу. `value` Атрибут для каждого флажка присваивается значение `CourseID.` при отправке страницы связыватель модели передает массив в контроллер, который состоит из `CourseID` значения только флажки установлены.

При первичной отрисовке флажки для курсов, назначенных преподавателю, имеют `checked` атрибутов, которые выбирает их (открывается установленными).

После изменения назначения курсов, необходимо иметь возможность проверить изменения, когда возвращается сайт `Index` страницы. Таким образом необходимо добавить столбец к таблице на этой странице. В этом случае не нужно использовать `ViewBag` объекта, так как сведения, которые вы хотите отобразить уже `Courses` свойство навигации `Instructor` сущность, которая передается на страницу, что и у модели.

В *Views\Instructor\Index.cshtml*, добавьте **курсы** заголовок сразу после **Office** заголовок, как показано в следующем примере:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Затем добавьте новую ячейку данных сразу после в ячейку сведений расположение office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Запустите **Instructor Index** страницу, чтобы увидеть курсов, назначенных каждой преподавателя:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Нажмите кнопку **изменить** на преподавателя, чтобы открыть страницы "Edit".

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Измените некоторые назначения курсов и нажмите кнопку **Сохранить**. Вносимые вами изменения отражаются на странице индекса.

 Примечание: К редактированию данных курсов для преподавателя, описываемый здесь подход работает также в том случае, когда ограниченном числе курсов. Для коллекций большего размера следовало бы применять другой пользовательский интерфейс и другой метод обновления.  
 

## <a name="update-the-deleteconfirmed-method"></a>Обновите метод DeleteConfirmed

В *InstructorController.cs*, удалить `DeleteConfirmed` метод и вставьте следующий код на его месте.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Этот код вносит следующие изменения:

- Если преподавателю, назначенный в качестве администратора любой отдел, удаляется назначение преподавателя из этого отдела. Без этого кода будет получена ошибки ссылочной целостности при попытка удалить лектора, который был назначен в качестве администратора для определенного подразделения.

Этот код не обрабатывает ситуацию, в которой одного преподавателя, назначенные в качестве администратора для нескольких отделов. В последнем руководстве вы добавите код, который предотвращает возникновение этого сценария.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Добавление расположения кабинета и курсов на страницу создания

В *InstructorController.cs*, удалить `HttpGet` и `HttpPost` `Create` методов, а затем добавьте следующий код на их место:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Этот код аналогичен показанным для методов Edit за исключением того, что изначально никакие курсы не выбраны. `HttpGet` `Create` Вызовы методов `PopulateAssignedCourseData` метод не в том случае, поскольку могут быть выбраны в курсы, чтобы предоставить пустую коллекцию для `foreach` цикл в представлении (в противном случае код представления создавал исключение, исключение null reference ).

Метод HttpPost Create добавляет каждый выбранный курс в свойство навигации Courses, прежде чем код шаблона, которое проверяет наличие ошибок проверки и добавить нового преподавателя в базу данных. Курсы добавляются даже при наличии ошибок модели таким образом, при наличии ошибок модели (например, пользователь ввел недопустимую дату) таким образом, когда страница отображается повторно с сообщением об ошибке, автоматически восстанавливаются параметры курса, которые были внесены.

Обратите внимание, что для добавления курсов в свойство навигации `Courses` нужно инициализировать это свойство как пустую коллекцию:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Это можно сделать не только в коде контроллера, но и в модели Instructor, изменив метод получения свойств для автоматического создания коллекции, если она не существует, как показано в следующем примере:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

При подобном изменении свойства `Courses` можно удалить код явной инициализации свойства в контроллере.

В *Views\Instructor\Create.cshtml*, добавьте текстовое поле для расположения office и курс флажки после поле даты приема на работу и перед **отправить** кнопки.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

После вставки кода, исправьте разрывы строк и отступы, как это делалось ранее для страницы "Edit".

Откройте страницу создать и добавить преподавателя.

![Создание преподавателя с курсами](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Обработка транзакций

Как описано в [руководстве основные функциональные возможности CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), по умолчанию Entity Framework реализует транзакции неявно. См. в сценариях, где требуется дополнительный контроль — например, если требуется включить операции, выполняемые вне платформы Entity Framework в транзакции-- [работа с транзакциями](https://msdn.microsoft.com/data/dn456843) на сайте MSDN.

## <a name="summary"></a>Сводка

Теперь вы завершили этот введение в работу со связанными данными. До сих в этих руководствах вы работали с кодом, который выполняет синхронный ввод-вывод. Можно сделать приложение более эффективного использования ресурсов веб-сервера путем реализации асинхронного кода, и вам необходимо в следующем учебном курсе.

Оставьте свои отзывы на том, как вам понравилось, и этот учебник, и что можно улучшить. Можно также запросить новые темы на [показать мне как с помощью кода](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
