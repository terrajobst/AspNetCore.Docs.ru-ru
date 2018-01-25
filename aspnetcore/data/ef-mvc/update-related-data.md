---
title: "Данные - 7 из 10, относящиеся к ASP.NET Core MVC с основными EF - обновление"
author: tdykstra
description: "В этом учебнике будет обновить связанные данные, обновив внешних ключевых полей и свойств навигации."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 3cdd36ae03824645e09f97cae85cc55956679390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a>Обновление связанных данных - Core EF учебнику ASP.NET Core MVC (7, 10)

По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio. Сведения о учебника серии см [в первом учебнике ряда](intro.md).

В предыдущем учебнике отображаются связанные данные; в этом учебнике будет обновить связанные данные, обновив внешних ключевых полей и свойств навигации.

Некоторые страницы, которые вы будете работать с рисунках ниже.

![Страница изменения курса](update-related-data/_static/course-edit.png)

![Страница изменения инструктора](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Настроить создание и редактирование страниц для курсов

При создании новой сущности курса, он должен иметь отношение к отдела. Чтобы упростить эту задачу, формирования шаблонов код включает создание и изменение представления, включающие раскрывающегося списка для выбора подразделение и методов контроллера. Наборы раскрывающегося списка `Course.DepartmentID` свойство внешнего ключа, и это все платформы Entity Framework требуется для загрузки `Department` свойство навигации с соответствующие сущность Department. Будет использовать формирования шаблонов кода, но изменить их немного на добавление обработки ошибок и сортировка раскрывающегося списка.

В *CoursesController.cs*, удалить четыре метода Создание и изменение и замените их следующим кодом:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

После `Edit` метод HttpPost, создайте новый метод, который загружает данные отдела для раскрывающегося списка.

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

`PopulateDepartmentsDropDownList` Метод возвращает список всех отделов, отсортированных по имени, создает `SelectList` коллекции для раскрывающегося списка и передает в представление коллекции `ViewBag`. Этот метод принимает необязательный `selectedDepartment` параметр, который позволяет вызывающему коду указать элемент, который будет установлен при отображении раскрывающегося списка. Представление будет передать имя «DepartmentID» `<select>` тег вспомогательный метод и вспомогательный объект для поиска в становится известен `ViewBag` для объекта `SelectList` с именем «DepartmentID».

HttpGet `Create` вызовы метода `PopulateDepartmentsDropDownList` метод без установки выбранного элемента, так как для нового курса отделе еще не установлено:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

HttpGet `Edit` метод задает выбранный элемент, на основе идентификатора отдела, которые уже назначен редактируемого курса:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Для обоих методов HttpPost `Create` и `Edit` также включать код, который задает элемент, выбранный при их отображении страницы после возникновения ошибки. Это гарантирует, что если отобразится страница для отображения сообщения об ошибках, был выбран любой отдел остается выбранным.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Добавите. AsNoTracking подробности методы и Delete

Для оптимизации производительности, сведения по курсу и удаление страниц, добавьте `AsNoTracking` вызывает в `Details` и HttpGet `Delete` методы.

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>Изменение представлений курса

В *Views/Courses/Create.cshtml*, добавьте параметр «Выберите отдела», чтобы **отдел** раскрывающемся списке, измените заголовок из **DepartmentID** для  **Отдел**и добавить сообщение проверки.

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

В *Views/Courses/Edit.cshtml*, внести такое же изменение поля отдела, которые были в *Create.cshtml*.

Кроме того, в *Views/Courses/Edit.cshtml*, добавить числовое поле курс перед **заголовок** поля. Так как номер является первичным ключом, он отображается, но его нельзя изменить.

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

Скрытое поле уже существует (`<input type="hidden">`) для номер в режиме редактирования. Добавление `<label>` вспомогательный тег не устраняют необходимость в скрытое поле, так как оно не вызывает должны быть включены в данные, когда пользователь щелкает номер **Сохранить** на **изменить** страницы.

В *Views/Courses/Delete.cshtml*добавьте курса числовое поле в верхней и сделать department, идентификатор и название отдела.

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

В *Views/Courses/Details.cshtml*, внести такое же изменение, которые были для *Delete.cshtml*.

### <a name="test-the-course-pages"></a>Тестирование страниц курса

Запустите приложение, выберите **курсы** щелкните **создать новый**и введите данные для нового курса:

![Страница создания курса](update-related-data/_static/course-create.png)

Нажмите кнопку **Создать**. Страница индекса курсы отображается новый курс, добавляемый в список. Имя подразделения в список страниц индекса поступает из свойства навигации, показывающая связь была установлена правильно.

Нажмите кнопку **изменить** на курс страницы индекса курсов.

![Страница изменения курса](update-related-data/_static/course-edit.png)

Измените данные на странице и нажмите кнопку **Сохранить**. Страница индекса курсы отображается с данными курс был обновлен.

## <a name="add-an-edit-page-for-instructors"></a>Добавить страницу редактирования для преподавателей

При редактировании запись инструктора, необходимо иметь возможность обновить назначение инструктора office. Сущность инструктора имеет отношение "один к нулю или одному" с OfficeAssignment сущностью, которая означает, что код для обработки в следующих ситуациях:

* Если пользователь удаляет назначение office и было до значения, удалите OfficeAssignment сущности.

* Если пользователь вводит значение назначения office и он изначально был пуст, создайте новую сущность OfficeAssignment.

* Если пользователь изменяет значение назначения office, измените значение в существующей сущности OfficeAssignment.

### <a name="update-the-instructors-controller"></a>Обновление контроллера преподавателей

В *InstructorsController.cs*, измените код в HttpGet `Edit` метода, так что он загружает сущность инструктора `OfficeAssignment` свойства навигации и вызывает `AsNoTracking`:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

Замените HttpPost `Edit` метод следующим кодом для обработки обновлений office назначения:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

Код выполняет следующие задачи.

-  Изменяет имя метода для `EditPost` поскольку подпись теперь является таким же, как HttpGet `Edit` метод ( `ActionName` атрибут указывает, что `/Edit/` по-прежнему используется URL-адрес).

-  Возвращает текущую сущность инструктора из базы данных с помощью упреждающая загрузка для `OfficeAssignment` свойства навигации. Это то же самое, как выполнялись HttpGet `Edit` метод.

-  Обновляет полученную сущность инструктора значениями из связывателя модели. `TryUpdateModel` Перегрузка позволяет белого списка включаемых свойств. Это предотвращает чрезмерное учета, как описано в статье [второй учебника](crud.md).

    <!-- Snippets don't play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   Если отсутствует в расположении офиса, задает свойство Instructor.OfficeAssignment значение null, чтобы связанные строки в таблице OfficeAssignment будут удалены.

    <!-- Snippets don't play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- Сохраняет изменения в базу данных.

### <a name="update-the-instructor-edit-view"></a>Обновить представление изменить инструктора

В *Views/Instructors/Edit.cshtml*, Добавление нового поля для редактирования в расположении офиса в конце перед **Сохранить** кнопки:

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Запустите приложение, выберите **инструкторов** , а затем щелкните **изменить** на инструктор. Изменение **офиса** и нажмите кнопку **Сохранить**.

![Страница изменения инструктора](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Добавить назначения курса инструктора изменить страницу

Преподаватели могут обучить любое количество курсов. Теперь будет улучшить страница инструктора изменить, добавив возможность изменения курса назначения с помощью группы флажков, как показано на следующем снимке экрана:

![Страница изменения инструктора курсы](update-related-data/_static/instructor-edit-courses.png)

Связь между сущностями Course и инструктора — многие ко многим. Чтобы добавить и удалить связи, можно добавлять и удалять сущности в и из набора сущностей CourseAssignments соединения.

Пользовательский Интерфейс, позволяющий изменить какие курсы инструктор назначен составляет несколько флажков. Отображается флажок для каждого курса в базе данных, а те, которые инструктора в данный момент назначен выбраны. Пользователь может установите или снимите флажки, чтобы изменить назначения курса. Если количество курсов были значительно больше, возможно, следует использовать другой метод представления данных в представлении, но используется один и тот же метод управления сущности объединения для создания и удаления связей.

### <a name="update-the-instructors-controller"></a>Обновление контроллера преподавателей

Для предоставления данных представления для списка флажков, потребуется использовать класс модели представления.

Создание *AssignedCourseData.cs* в *SchoolViewModels* папки и замените существующий код следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

В *InstructorsController.cs*, замените HttpGet `Edit` метод следующим кодом. Изменения выделены.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

Код добавляет упреждающую для `Courses` свойства навигации и вызывает новый `PopulateAssignedCourseData` метод для предоставления сведений о флажок массив с помощью `AssignedCourseData` просматривать класс модели.

Код в `PopulateAssignedCourseData` метод считывает все курса сущности для загрузки списка курсов, используя класс модели представления. Для каждого курса код проверяет, существует ли курса инструктора `Courses` свойство навигации. Чтобы создать эффективную подстановку при проверке, назначена ли курса инструктора, курсы, назначенные инструктора помещаются в `HashSet` коллекции. `Assigned` Задано значение true для курсов инструктора назначается. Представление будет использовать это свойство для определения, какие проверки поля должны отображаться как выбранные. Наконец, список передается в представление `ViewData`.

Добавьте код, который выполняется, когда пользователь щелкает **Сохранить**. Замените `EditPost` метод со следующими кода и добавьте новый метод, который обновляет `Courses` свойства навигации сущности инструктора.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

Сигнатура метода теперь отличается от HttpGet `Edit` метод, поэтому имя метода изменений из `EditPost` к `Edit`.

Так как представление не содержит коллекцию сущностей курса, связыватель модели не может автоматически обновлять `CourseAssignments` свойство навигации. Вместо использования связыватель модели для обновления `CourseAssignments` свойство навигации, выполните в новом `UpdateInstructorCourses` метод. Поэтому необходимо исключить `CourseAssignments` свойству из привязки модели. Это не требует изменений в код, который вызывает `TryUpdateModel` так, как вы используете перегрузки список утвержденных и `CourseAssignments` отсутствует в списке include.

Если проверка не были выбраны поля, код в `UpdateInstructorCourses` инициализирует `CourseAssignments` свойство навигации с пустой коллекции и возвращает:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

Код реализован цикл просмотра всех курсов в базе данных и проверяет каждый курс от тех, которые в настоящее время назначен инструктора и те, которые были выбраны в представлении. Чтобы упростить эффективный уточняющие запросы, последние две коллекции хранятся в `HashSet` объектов.

Если был установлен флажок для курса, но курса не находится в `Instructor.CourseAssignments` свойство навигации курса добавляется в коллекцию в свойстве навигации.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Если флажок для курса не была выбрана, но курса в `Instructor.CourseAssignments` свойство навигации, курса удаляется из свойства навигации.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Обновление представлений инструктора

В *Views/Instructors/Edit.cshtml*, добавьте **курсы** поля с помощью массива флажки, добавив следующий код сразу после `div` элементы для **Office**  поля и перед `div` элемент для **Сохранить** кнопки.

<a id="notepad"></a>
> [!NOTE] 
> При вставке кода в Visual Studio, разрывы строк будет изменяться в способом, нарушающим код.  Нажмите один раз сочетание клавиш Ctrl + Z для отмены автоматического форматирования.  Это устранит разрывы, чтобы они выглядят как показано ниже. Отступы не должен быть идеальным, но `@</tr><tr>`, `@:<td>`, `@:</td>`, и `@:</tr>` строки должны быть в одной строке показанного или вы получите ошибку во время выполнения. С блоком выбран новый код нажмите клавишу Tab трижды Чтобы выровнять новый код с существующим кодом. Можно проверить состояние этой проблемы [здесь](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Этот код создает таблицу HTML, который содержит три столбца. В каждом столбце является типа "флажок" следуют подпись, состоящая из номер и название. Флажки, все имеют одинаковые имена («selectedCourses»), который уведомляет связывателя модели, если они следует рассматривать как группу. Недопустимое значение атрибута каждого флажка присвоено значение `CourseID`. При отправке страницы связывателя модели передает массив, состоящий из контроллера `CourseID` значения только флажки установлены.

При первоначальном отображении флажки тех, которые назначены инструктора курсов проверки атрибутов, который выбирает их (открывается, когда их возвращен).

Запустите приложение, выберите **инструкторов** и нажмите кнопку **изменить** на инструктор, чтобы увидеть **изменить** страницы.

![Страница изменения инструктора курсы](update-related-data/_static/instructor-edit-courses.png)

Некоторые курса назначения и нажмите кнопку Сохранить. Вносимые изменения отражаются на странице индекса.

> [!NOTE] 
> Подходом, который используется здесь для изменения данных курса инструктора удобно использовать, когда имеется ограниченное число курсов. Для коллекций, которые могут быть значительно большими другой пользовательский Интерфейс и другой метод обновления не требуются.

## <a name="update-the-delete-page"></a>Обновление страницы удаления

В *InstructorsController.cs*, удалить `DeleteConfirmed` метод и вставьте следующий код на его месте.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Этот код выполняет следующие изменения:

* Загрузка для eager выполняет `CourseAssignments` свойство навигации.  Необходимо включить это или EF не будет знать о связанных `CourseAssignment` сущности и не удалите их.  Чтобы избежать необходимости читать их здесь выполняется настройка базы данных каскадное удаление.

* Если администратор любые отделы назначается инструктора для удаления, удаляет инструктора назначения из тех отделов.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Добавление страницы создания офиса и курсов

В *InstructorsController.cs*, удалите HttpGet и HttpPost `Create` методы, а затем добавьте следующий код вместо них:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Это тот же способом, что для `Edit` выбранных методов, за исключением того, которые изначально не курсов. HttpGet `Create` вызовы метода `PopulateAssignedCourseData` метод не потому, что может быть курсов, однако в выбранных заказ для пустой коллекции для обеспечения `foreach` цикла в представлении (в противном случае просмотр кода, вызывает исключение null reference).

HttpPost `Create` метод добавляет каждого выбранного курса для `CourseAssignments` свойство навигации, прежде чем он проверяет наличие ошибок проверки и добавляет новый инструктор в базу данных. Курсы добавляются, даже если есть какие-либо курс параметры, которые были внесены автоматическое восстановление при наличии ошибки модели (например, пользователь с ключами произошла недопустимая дата), и отобразится страница с сообщением об ошибке, ошибки модели.

Обратите внимание на то, чтобы иметь возможность добавить курсы для `CourseAssignments` свойство навигации было нужно инициализировать свойство как пустой коллекции:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Вместо этого код контроллера вы могли сделать инструктора модели, изменив метод считывания свойства для автоматического создания коллекции, если он не существует, как показано в следующем примере:

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

При изменении `CourseAssignments` свойства таким образом, можно удалить код инициализации явное свойство в контроллере.

В *Views/Instructor/Create.cshtml*, добавьте текстовое поле расположения office и установите флажки для курсов перед кнопки "Отправить". В случае страницы правки [форматировании Если Visual Studio переформатирует код при вставке](#notepad).

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Проверьте, выполнение приложения и создание инструктор. 

## <a name="handling-transactions"></a>Обработка транзакций

Как описано в статье [CRUD учебника](crud.md), Entity Framework неявно реализует транзакции. См. в сценариях, где требуется дополнительный контроль — например, если вы хотите включить действия, выполненные за пределами платформы Entity Framework транзакции-- [транзакции](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="summary"></a>Сводка

Введение в работу со связанными данными завершена. В следующем уроке вы увидите, как обрабатывать конфликты параллелизма.

>[!div class="step-by-step"]
[Назад](read-related-data.md)
[Вперед](concurrency.md)  
