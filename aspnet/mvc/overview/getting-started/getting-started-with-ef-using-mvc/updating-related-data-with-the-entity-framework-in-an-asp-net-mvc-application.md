---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Обновление данных, связанных с платформой Entity Framework в приложении ASP.NET MVC | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 348940748e3c33ace03d1b8f41615e9814cf6b40
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Обновление данных, связанных с платформой Entity Framework в приложении ASP.NET MVC
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio 2013. Сведения о учебника серии см [в первом учебнике ряда](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем учебнике отображаются связанные данные; в этом учебнике будет показано обновление связанных данных. Для большинства связей это можно сделать путем обновления внешнего ключа поля или свойства навигации. Для связи многие ко многим Entity Framework не предоставляют соединяемой таблице напрямую, чтобы добавлять и удалять сущности в и из соответствующих свойств навигации.

Некоторые страницы, которые вы будете работать с рисунках ниже.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Редактирование инструктора курсы](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Настроить создание и редактирование страниц для курсов

При создании новой сущности курса, он должен иметь отношение к отдела. Чтобы упростить эту задачу, формирования шаблонов код включает создание и изменение представления, включающие раскрывающегося списка для выбора подразделение и методов контроллера. Задает раскрывающегося списка `Course.DepartmentID` свойство внешнего ключа, и это все платформы Entity Framework требуется для загрузки `Department` свойство навигации с помощью соответствующих `Department` сущности. Будет использовать формирования шаблонов кода, но изменить их немного на добавление обработки ошибок и сортировка раскрывающегося списка.

В *CourseController.cs*, удалить четыре `Create` и `Edit` методов и замените их следующим кодом:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Добавьте следующие `using` инструкции в начало файла:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList` Метод возвращает список всех отделов, отсортированных по имени, создает `SelectList` коллекции для раскрывающегося списка и передает в представление коллекции `ViewBag` свойство. Этот метод принимает необязательный `selectedDepartment` параметр, который позволяет вызывающему коду указать элемент, который будет установлен при отображении раскрывающегося списка. Представление будет передать имя `DepartmentID` для [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) вспомогательный метод и вспомогательный объект для поиска в становится известен `ViewBag` для объекта `SelectList` с именем `DepartmentID`.

`HttpGet` `Create` Вызовы метода `PopulateDepartmentsDropDownList` метод без установки выбранного элемента, так как для нового курса отдела не будет установлено еще:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit` Метод задает выбранный элемент, на основе идентификатора отдела, которые уже назначен редактируемого курса:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost` Методы для обоих `Create` и `Edit` также включать код, который задает элемент, выбранный при их отображении страницы после возникновения ошибки:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Этот код гарантирует, что если отобразится страница для отображения сообщения об ошибках, был выбран любой отдел остается выбранным.

С помощью раскрывающихся списков для поля отдел уже формирования шаблонов представлений курса, но вы не хотите DepartmentID заголовок для этого поля, измените следующий выделенный Убедитесь *Views\Course\Create.cshtml* файл Изменение подписи.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Внести такое же изменение в *Views\Course\Edit.cshtml*.

Обычно scaffolder не формировать первичный ключ, поскольку значение ключа создается в базе данных и не может быть изменено и не может применяться значение, отображаемое для пользователей. Для сущностей курса scaffolder содержит текстовое поле для `CourseID` поле, так как он понимает, что это `DatabaseGeneratedOption.None` атрибута означает пользователь должен иметь права введите значение первичного ключа. Но он не понимает, потому что она может применяться необходимость см в других представлениях, поэтому необходимо добавить ее вручную.

В *Views\Course\Edit.cshtml*, добавить числовое поле курс перед **заголовок** поля. Так как он является первичным ключом, он отображается, но его нельзя изменить.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Скрытое поле уже существует (`Html.HiddenFor` вспомогательный) для номер в режиме редактирования. Добавление *Html.LabelFor* вспомогательный не устраняют необходимость в скрытое поле, так как оно не вызывает должны быть включены в данные, когда пользователь щелкает номер **Сохранить** на странице «Изменение».

В *Views\Course\Delete.cshtml* и *Views\Course\Details.cshtml*, измените заголовок имя отдела из «Name» на «Отдел» и добавить числовое поле курс перед **заголовка**  поля.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Запустите **создать** страницы (отображать страницу индекса курса и щелкните **создать новый**) и введите данные для нового курса:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Нажмите кнопку **Создать**. Страница индекса курса отображается новый курс, добавляемый в список. Имя подразделения в список страниц индекса поступает из свойства навигации, показывающая связь была установлена правильно.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Запустите **изменить** страницы (отображать страницу индекса курса и щелкните **изменить** на курс).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Измените данные на странице и нажмите кнопку **Сохранить**. С данными обновленный курс отображается страница индекса курса.

## <a name="adding-an-edit-page-for-instructors"></a>Добавление страницы правки для преподавателей

При редактировании запись инструктора, необходимо иметь возможность обновить назначение инструктора office. `Instructor` Сущность имеет отношение "один к нулю или одному" с `OfficeAssignment` сущности, которая означает должен обрабатывать следующие ситуации:

- Если пользователь удаляет назначение office и было до значения, необходимо удалить и удалите `OfficeAssignment` сущности.
- Если пользователь вводит значение назначения office, и он изначально был пуст, необходимо создать новый `OfficeAssignment` сущности.
- Если пользователь изменяет значение назначения office, необходимо изменить значение в существующем `OfficeAssignment` сущности.

Откройте *InstructorController.cs* и посмотрите на `HttpGet` `Edit` метод:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Здесь код формирования шаблонов не самое интересное. Настраивает данные для раскрывающегося списка, но необходимые представляет собой текстовое поле. Замените этот метод следующий код:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Этот код удаляет `ViewBag` инструкции и добавляет упреждающую для связанного `OfficeAssignment` сущности. Не удается выполнить упреждающую с `Find` метод, поэтому `Where` и `Single` методов вместо них используются для выбора инструктора.

Замените `HttpPost` `Edit` метод следующим кодом. обрабатывающая назначения обновлений office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Ссылка на `RetryLimitExceededException` требует `using` инструкции; Чтобы добавить его, щелкните правой кнопкой мыши `RetryLimitExceededException`и нажмите кнопку **Разрешить** - **с помощью System.Data.Entity.Infrastructure**.

![Разрешить исключение повторной попытки](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Код выполняет следующие задачи.

- Изменяет имя метода для `EditPost` поскольку подпись теперь является таким же, как `HttpGet` метод ( `ActionName` атрибут указывает, что по-прежнему используется /Edit/ URL-адрес).
- Возвращает текущий `Instructor` сущностей из базы данных, используя упреждающую для `OfficeAssignment` свойства навигации. Это то же самое, как это требовалось в `HttpGet` `Edit` метод.
- Обновляет извлеченного `Instructor` сущности значениями из связывателя модели. [TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.108).aspx) используется перегрузка метода позволяет *белого списка* свойства, которые необходимо включить. Это предотвращает чрезмерное учета, как описано в статье [второй учебника](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Если отсутствует в расположении офиса, задает `Instructor.OfficeAssignment` свойство в значение null, чтобы связанные строки в `OfficeAssignment` таблица будет удалена.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Сохраняет изменения в базу данных.

В *Views\Instructor\Edit.cshtml*после того, как `div` элементы для **Дата приема на работу** поле, Добавление нового поля для редактирования в расположении офиса:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Запустите страницу (выберите **инструкторов** и нажмите кнопку **изменить** на инструктор). Изменение **офиса** и нажмите кнопку **Сохранить**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Добавление назначения курса преподавателя редактировать страницу

Преподаватели могут обучить любое количество курсов. Теперь будет улучшить страница инструктора изменить, добавив возможность изменения курса назначения с помощью группы флажков, как показано на следующем снимке экрана:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Связь между `Course` и `Instructor` сущности — многие ко многим, это означает, что у вас прямой доступ к свойства внешнего ключа, которые находятся в соединяемой таблице. Вместо этого добавления и удаления сущностей с `Instructor.Courses` свойство навигации.

Пользовательский Интерфейс, позволяющий изменить какие курсы инструктор назначен составляет несколько флажков. Отображается флажок для каждого курса в базе данных, а те, которые инструктора в данный момент назначен выбраны. Пользователь может установите или снимите флажки, чтобы изменить назначения курса. Если количество курсов были значительно больше, возможно, следует использовать другой метод представления данных в представлении, но используется один и тот же метод управления свойства навигации для создания и удаления связей.

Для предоставления данных представления для списка флажков, потребуется использовать класс модели представления. Создание *AssignedCourseData.cs* в *ViewModels* папки и замените существующий код следующим кодом:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

В *InstructorController.cs*, замените `HttpGet` `Edit` метод следующим кодом. Изменения выделены.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Код добавляет упреждающую для `Courses` свойства навигации и вызывает новый `PopulateAssignedCourseData` метод для предоставления сведений о флажок массив с помощью `AssignedCourseData` просматривать класс модели.

Код в `PopulateAssignedCourseData` метод считывает все `Course` класс модели сущности для загрузки списка курсов, с помощью представления. Для каждого курса код проверяет, существует ли курса инструктора `Courses` свойство навигации. Чтобы создать эффективную подстановку при проверке, назначена ли курса инструктора, курсы, назначенные инструктора помещаются в [HashSet](https://msdn.microsoft.com/en-us/library/bb359438.aspx) коллекции. `Assigned` Свойству `true` курсы инструктора назначается. Представление будет использовать это свойство для определения, какие проверки поля должны отображаться как выбранные. Наконец, список передается в представление `ViewBag` свойство.

Добавьте код, который выполняется, когда пользователь щелкает **Сохранить**. Замените `EditPost` метод следующий код, который вызывает новый метод, который обновляет `Courses` свойство навигации `Instructor` сущности. Изменения выделены.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Сигнатура метода теперь отличается от `HttpGet` `Edit` метод, поэтому имя метода изменений из `EditPost` к `Edit`.

Поскольку представление не имеет коллекцию `Course` сущностей, связыватель модели не может автоматически обновлять `Courses` свойство навигации. Вместо использования связыватель модели для обновления `Courses` свойство навигации, который будет выполняться в новой `UpdateInstructorCourses` метод. Поэтому необходимо исключить `Courses` свойству из привязки модели. Это не требует изменений в код, который вызывает [TryUpdateModel](https://msdn.microsoft.com/en-us/library/dd470908(v=vs.98).aspx) так, как вы используете *список утвержденных* перегрузки и `Courses` отсутствует в списке include.

Если проверка не были выбраны поля, код в `UpdateInstructorCourses` инициализирует `Courses` свойство навигации с пустой коллекции:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Код реализован цикл просмотра всех курсов в базе данных и проверяет каждый курс от тех, которые в настоящее время назначен инструктора и те, которые были выбраны в представлении. Чтобы упростить эффективный уточняющие запросы, последние две коллекции хранятся в `HashSet` объектов.

Если был установлен флажок для курса, но курса не находится в `Instructor.Courses` свойство навигации курса добавляется в коллекцию в свойстве навигации.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Если флажок для курса не была выбрана, но курса в `Instructor.Courses` свойство навигации, курса удаляется из свойства навигации.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

В *Views\Instructor\Edit.cshtml*, добавьте **курсы** поля с помощью массива флажки, добавив следующий код сразу после `div` элементы для `OfficeAssignment` поля и Прежде чем `div` элемент для **Сохранить** кнопки:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

После вставки кода, если разрывы строк и отступы выглядят, как здесь, вручную исправить все данные, чтобы он выглядел как показано ниже. Отступы не должен быть идеальным, но `@</tr><tr>`, `@:<td>`, `@:</td>`, и `@</tr>` строки должны быть в одной строке показанного или вы получите ошибку во время выполнения.

Этот код создает таблицу HTML, который содержит три столбца. В каждом столбце является типа "флажок" следуют подпись, состоящая из номер и название. Все флажки совпадают («selectedCourses»), который уведомляет связывателя модели, они должны рассматриваться как группа. `value` Атрибут каждого флажка присвоено значение `CourseID.` при отправке страницы связывателя модели передает массив, состоящий из контроллера `CourseID` значения только флажки установлены.

При первоначальном отображении флажки тех, которые назначены инструктора курсов имеют `checked` атрибутов, которые выбирает их (отображаются их возвращенных).

После изменения курса назначения, необходимо иметь возможность проверить внесенные изменения при возвращении сайта `Index` страницы. Таким образом необходимо добавить столбец к таблице на этой странице. В этом случае нет необходимости использовать `ViewBag` объекта, так как сведения, которые необходимо отобразить уже `Courses` свойство навигации `Instructor` сущность, которая передается на страницу, что и модель.

В *Views\Instructor\Index.cshtml*, добавьте **курсы** заголовок сразу после **Office** заголовок, как показано в следующем примере:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Затем добавьте новую ячейку сведений сразу после ячеек с подробными расположение office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Запустите **индекс инструктора** страницу, чтобы просмотреть курсы, назначенные каждой инструктора:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Нажмите кнопку **изменить** на инструктор для просмотра страницы правки.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Измените некоторые курса назначения и нажмите кнопку **Сохранить**. Вносимые изменения отражаются на странице индекса.

 Примечание: Подходом, который используется здесь для изменения данных курса инструктора удобно использовать, когда имеется ограниченное число курсов. Для коллекций, которые могут быть значительно большими другой пользовательский Интерфейс и другой метод обновления не требуются.  
 

## <a name="update-the-deleteconfirmed-method"></a>Метод DeleteConfirmed обновления

В *InstructorController.cs*, удалить `DeleteConfirmed` метод и вставьте следующий код на его месте.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Этот код выполняет следующие изменения:

- Если инструктора назначается администратор любое подразделение, удаляет назначение инструктора этого отдела. Без этого кода получаемому ошибки целостности ссылок при попытке удалить инструктора, который был назначен в качестве администратора отдела.

Этот код не обрабатывает ситуацию, в которой один инструктора назначены в качестве администратора для нескольких отделов. В последней учебнике предстоит добавить код, который запрещает этого сценария ситуации.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Добавление страницы создания офиса и курсов

В *InstructorController.cs*, удалить `HttpGet` и `HttpPost` `Create` методы, а затем добавьте следующий код вместо них:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Это тот же способом, что для изменения методов, но изначально выбираются не курсов. `HttpGet` `Create` Вызовы метода `PopulateAssignedCourseData` метод не потому, что может быть курсов, однако в выбранных заказ для пустой коллекции для обеспечения `foreach` цикла в представлении (в противном случае просмотреть код вызывает исключение с пустой ссылкой ).

Метод HttpPost Create добавляет каждого выбранного курса свойство навигации курсы перед кодом шаблона, который проверяет наличие ошибок проверки и добавляет новый инструктор в базу данных. Курсы добавляются, даже если есть ошибки модели, чтобы при наличии ошибок модели (например, пользователь с ключами произошла недопустимая дата), чтобы при отобразится страница с сообщением об ошибке, автоматически восстанавливаются какие-либо курс параметры, которые были внесены.

Обратите внимание на то, чтобы иметь возможность добавить курсы для `Courses` свойство навигации было нужно инициализировать свойство как пустой коллекции:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Вместо этого код контроллера вы могли сделать инструктора модели, изменив метод считывания свойства для автоматического создания коллекции, если он не существует, как показано в следующем примере:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

При изменении `Courses` свойства таким образом, можно удалить код инициализации явное свойство в контроллере.

В *Views\Instructor\Create.cshtml*, добавьте текстовое поле расположения office и курс флажки после поле даты найма и перед **отправить** кнопки.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

После вставки кода исправьте разрывы строк и отступы, как это было ранее для страницы правки.

Откройте страницу Создание и добавление инструктора.

![Создание инструктора курсы](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Обработка транзакций

Как описано в статье [основные функциональные возможности CRUD учебника](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), по умолчанию неявно реализует операции Entity Framework. См. в сценариях, где требуется дополнительный контроль — например, если вы хотите включить действия, выполненные за пределами платформы Entity Framework транзакции-- [работы с транзакциями](https://msdn.microsoft.com/en-US/data/dn456843) на сайте MSDN.

## <a name="summary"></a>Сводка

Это введение в работу со связанными данными завершена. Пока этих учебников вы работали с кодом, который выполняет синхронный ввод-вывод. Можно сделать приложение более эффективное использование ресурсов веб-сервера путем реализации асинхронный код, и это вам предстоит выполнить в следующем уроке.

Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить. Можно также запросить новые разделы на [показать мне как с код](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступа к данным ASP.NET - рекомендуется использовать ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Назад](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Вперед](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
