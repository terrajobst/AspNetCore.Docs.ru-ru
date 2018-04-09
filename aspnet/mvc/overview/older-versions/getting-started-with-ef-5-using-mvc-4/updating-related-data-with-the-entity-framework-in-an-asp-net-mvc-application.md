---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Обновление данных, связанных с платформой Entity Framework в приложении ASP.NET MVC (6 из 10) | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 227a7fed0ced884db591f0375454d6d0a62518f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Обновление данных, связанных с платформой Entity Framework в приложении ASP.NET MVC (6 из 10)
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы путем сравнения код для завершения кода. Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем учебнике отображаются связанные данные; в этом учебнике будет показано обновление связанных данных. Для большинства связей это можно сделать, обновляя соответствующие поля внешнего ключа. Для связи многие ко многим Entity Framework не предоставляют соединяемой таблице напрямую, поэтому необходимо явно добавить и удалить сущности в и из соответствующих свойств навигации.

На следующих рисунках изображены страницы, с которыми вы будете работать.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Настройка страниц создания и редактирования для курсов

Создаваемая сущность курса должна иметь связь с существующей кафедрой. Чтобы упростить эту задачу, шаблонный код включает методы контроллеров, а также представления "Create" (Создание) и "Edit" (Редактирование) с раскрывающимся списком для выбора кафедры. Задает раскрывающегося списка `Course.DepartmentID` свойство внешнего ключа, и это все платформы Entity Framework требуется для загрузки `Department` свойство навигации с помощью соответствующих `Department` сущности. Вы будете использовать этот шаблонный код, немного его изменив, чтобы добавить обработку ошибок и сортировку раскрывающегося списка.

В *CourseController.cs*, удалить четыре `Edit` и `Create` методов и замените их следующим кодом:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList` Метод возвращает список всех отделов, отсортированных по имени, создает `SelectList` коллекции для раскрывающегося списка и передает в представление коллекции `ViewBag` свойство. Этот метод принимает необязательный параметр `selectedDepartment`, позволяющий вызывающему коду указать элемент, который будет выбран при отрисовке раскрывающегося списка. Представление будет передать имя `DepartmentID` для [ `DropDownList` вспомогательный](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), и затем знает вспомогательный объект для поиска в `ViewBag` для объекта `SelectList` с именем `DepartmentID`.

`HttpGet` `Create` Вызовы метода `PopulateDepartmentsDropDownList` метод без установки выбранного элемента, так как для нового курса отдела не будет установлено еще:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit` Метод задает выбранный элемент, на основе идентификатора отдела, которые уже назначен редактируемого курса:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost` Методы для обоих `Create` и `Edit` также включать код, который задает элемент, выбранный при их отображении страницы после возникновения ошибки:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Этот код гарантирует, что если отобразится страница для отображения сообщения об ошибках, был выбран любой отдел остается выбранным.

В *Views\Course\Create.cshtml*, добавьте выделенный код для создания нового числового поля курс перед **заголовок** поля. Как описано в предыдущих учебник, поля основного ключа не формирования шаблонов по умолчанию, но этого первичного ключа применяется, поэтому пользователю необходимо иметь возможность вводить значение ключа.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

В *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, и *Views\Course\Details.cshtml*, добавить числовое поле курс перед **заголовка**  поля. Так как он является первичным ключом, он отображается, но его нельзя изменить.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Запустите **создать** страницы (отображать страницу индекса курса и щелкните **создать новый**) и введите данные для нового курса:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Нажмите кнопку **Создать**. Страница индекса курса отображается новый курс, добавляемый в список. Название кафедры в списке страницы индекса поступает из свойства навигации, показывая, что связь установлена правильно.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Запустите **изменить** страницы (отображать страницу индекса курса и щелкните **изменить** на курс).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Измените данные на странице и нажмите кнопку **Save** (Сохранить). С данными обновленный курс отображается страница индекса курса.

## <a name="adding-an-edit-page-for-instructors"></a>Добавление страницы правки для преподавателей

При редактировании записи преподавателя может потребоваться обновить назначенный преподавателю кабинет. `Instructor` Сущность имеет отношение "один к нулю или одному" с `OfficeAssignment` сущности, которая означает должен обрабатывать следующие ситуации:

- Если пользователь удаляет назначение office и было до значения, необходимо удалить и удалите `OfficeAssignment` сущности.
- Если пользователь вводит значение назначения office, и он изначально был пуст, необходимо создать новый `OfficeAssignment` сущности.
- Если пользователь изменяет значение назначения office, необходимо изменить значение в существующем `OfficeAssignment` сущности.

Откройте *InstructorController.cs* и посмотрите на `HttpGet` `Edit` метод:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Здесь код формирования шаблонов не самое интересное. Настраивает данные для раскрывающегося списка, но необходимые представляет собой текстовое поле. Замените этот метод следующий код:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Этот код удаляет `ViewBag` инструкции и добавляет упреждающую для связанного `OfficeAssignment` сущности. Не удается выполнить упреждающую с `Find` метод, поэтому `Where` и `Single` методов вместо них используются для выбора инструктора.

Замените `HttpPost` `Edit` метод следующим кодом. обрабатывающая назначения обновлений office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Этот код выполняет следующее:

- Получает текущую сущность `Instructor` из базы данных, используя безотложную загрузку для свойства навигации `OfficeAssignment`. Это то же самое, как это требовалось в `HttpGet` `Edit` метод.
- Обновляет извлеченную сущность `Instructor`, используя значения из связывателя модели. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) используется перегрузка метода позволяет *белого списка* свойства, которые необходимо включить. Это предотвращает чрезмерное учета, как описано в статье [второй учебника](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Если отсутствует в расположении офиса, задает `Instructor.OfficeAssignment` свойство в значение null, чтобы связанные строки в `OfficeAssignment` таблица будет удалена.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Сохраняет изменения в базу данных.

В *Views\Instructor\Edit.cshtml*после того, как `div` элементы для **Дата приема на работу** поле, Добавление нового поля для редактирования в расположении офиса:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Запустите страницу (выберите **инструкторов** и нажмите кнопку **изменить** на инструктор). Измените значение **Office Location** (Расположение кабинета) и нажмите кнопку **Save** (Сохранить).

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Добавление назначения курса преподавателя редактировать страницу

Преподаватели могут вести любое число курсов. Теперь вы улучшите страницу редактирования преподавателя, добавив возможность изменять назначения курсов с помощью группы флажков, как показано на следующем снимке экрана:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Связь между `Course` и `Instructor` сущности — многие ко многим, это означает, что нет прямого доступа к таблице соединения. Вместо этого добавления и удаления сущностей с `Instructor.Courses` свойство навигации.

Пользовательский интерфейс, позволяющий изменить назначенные для преподавателя курсы, представляет собой группу флажков. Отображается флажок для каждого курса в базе данных, а флажки для курсов, назначенных данному преподавателю, установлены. Пользователь может устанавливать и снимать флажки, изменяя назначения курсов. Если количество курсов были значительно больше, возможно, следует использовать другой метод представления данных в представлении, но используется один и тот же метод управления свойства навигации для создания и удаления связей.

Чтобы предоставить данные в представлении для списка флажков, нужно использовать класс модели представления. Создание *AssignedCourseData.cs* в *ViewModels* папки и замените существующий код следующим кодом:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

В *InstructorController.cs*, замените `HttpGet` `Edit` метод следующим кодом. Изменения выделены.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Код добавляет безотложную загрузку для свойства навигации `Courses` и вызывает новый метод `PopulateAssignedCourseData` для предоставления сведений массиву флажков с помощью класса модели представления `AssignedCourseData`.

Код в `PopulateAssignedCourseData` метод считывает все `Course` класс модели сущности для загрузки списка курсов, с помощью представления. Для каждого курса код проверяет, существует ли этот курс в свойстве навигации `Courses` преподавателя. Чтобы создать эффективную подстановку при проверке, назначена ли курса инструктора, курсы, назначенные инструктора помещаются в [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) коллекции. `Assigned` Свойству `true` курсы инструктора назначается. Представление будет использовать это свойство, чтобы определить, какие флажки нужно отображать как выбранные. Наконец, список передается в представление `ViewBag` свойство.

Добавьте код, выполняемый, когда пользователь нажимает кнопку **Save** (Сохранить). Замените `HttpPost` `Edit` метод следующий код, который вызывает новый метод, который обновляет `Courses` свойство навигации `Instructor` сущности. Изменения выделены.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Поскольку представление не имеет коллекцию `Course` сущностей, связыватель модели не может автоматически обновлять `Courses` свойство навигации. Вместо связыватель модели для обновления свойства навигации курсов, вы сможете сделать, в новый `UpdateInstructorCourses` метод. Поэтому нужно исключить свойство `Courses` из привязки модели. Это не требует изменений в код, который вызывает [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) так, как вы используете *список утвержденных* перегрузки и `Courses` отсутствует в списке include.

Если проверка не были выбраны поля, код в `UpdateInstructorCourses` инициализирует `Courses` свойство навигации с пустой коллекции:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

После этого код в цикле проходит по всем курсам в базе данных и сравнивает каждый из них с теми, которые сейчас назначены преподавателю, в противоположность тем, которые были выбраны в представлении. Чтобы упростить эффективную подстановку, последние две коллекции хранятся в объектах `HashSet`.

Если флажок для курса был установлен, но курс отсутствует в свойстве навигации `Instructor.Courses`, этот курс добавляется в коллекцию в свойстве навигации.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Если флажок для курса не был установлен, но курс присутствует в свойстве навигации `Instructor.Courses`, этот курс удаляется из свойства навигации.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

В *Views\Instructor\Edit.cshtml*, добавьте **курсы** поля с помощью массива флажки, добавив выделенный ниже код сразу после `div` элементы для `OfficeAssignment` поле:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Этот код создает таблицу HTML с тремя столбцами. Каждый столбец содержит флажок, за которым идет заголовок с номером и названием курса. Все флажки совпадают («selectedCourses»), который уведомляет связывателя модели, они должны рассматриваться как группа. `value` Атрибут каждого флажка присвоено значение `CourseID.` при отправке страницы связывателя модели передает массив, состоящий из контроллера `CourseID` значения только флажки установлены.

При первоначальном отображении флажки тех, которые назначены инструктора курсов имеют `checked` атрибутов, которые выбирает их (отображаются их возвращенных).

После изменения курса назначения, необходимо иметь возможность проверить внесенные изменения при возвращении сайта `Index` страницы. Таким образом необходимо добавить столбец к таблице на этой странице. В этом случае нет необходимости использовать `ViewBag` объекта, так как сведения, которые необходимо отобразить уже `Courses` свойство навигации `Instructor` сущность, которая передается на страницу, что и модель.

В *Views\Instructor\Index.cshtml*, добавьте **курсы** заголовок сразу после **Office** заголовок, как показано в следующем примере:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Затем добавьте новую ячейку сведений сразу после ячеек с подробными расположение office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Запустите **индекс инструктора** страницу, чтобы просмотреть курсы, назначенные каждой инструктора:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Нажмите кнопку **изменить** на инструктор для просмотра страницы правки.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Измените некоторые курса назначения и нажмите кнопку **Сохранить**. Вносимые вами изменения отражаются на странице индекса.

 Примечание: Подходов, использованных для изменения данных курса инструктора удобно использовать, когда имеется ограниченное число курсов. Для коллекций большего размера следовало бы применять другой пользовательский интерфейс и другой метод обновления.  
 

## <a name="update-the-delete-method"></a>Метод удаления обновления

Измените код в метод удаления HttpPost, таким образом, чтобы запись назначения office (если таковые имеются) удаляются при удалении инструктора:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Если вы попытаетесь удалить инструктор, назначенный для отдела, от имени администратора, вы сможете получить ошибки целостности ссылок. В разделе [текущей версии этого учебника](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) для дополнительный код, который автоматически удалит инструктора отдела, которой назначается инструктора с правами администратора.

## <a name="summary"></a>Сводка

Это введение в работу со связанными данными завершена. Пока этих учебников вы сделали полный диапазон из этих операций, но еще не обработаны проблем параллелизма. Следующее руководство вводят темы параллелизма, описываются параметры для его обработки и добавить параллелизма, обработку кода CRUD, которые уже были написаны для одного типа сущности.

Ссылки на другие ресурсы Entity Framework, можно найти в конце [последнего учебнике этой серии](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Назад](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
