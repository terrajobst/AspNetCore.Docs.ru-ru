---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Чтение связанных данных с Entity Framework в приложении ASP.NET MVC | Документация Майкрософт
author: tdykstra
description: /AJAX/tutorials/using-AJAX-Control-Toolkit-Controls-and-Control-Extenders-VB
ms.author: aspnetcontent
ms.date: 11/07/2014
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4147feda2b78eefa2f5e280e587759585da738b3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839143"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Чтение связанных данных с Entity Framework в приложении ASP.NET MVC
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем учебном курсе мы завершили модели School. В этом руководстве можно прочитать и отображения связанных данных — то есть данные, которые Entity Framework загружает в свойства навигации.

На следующих рисунках изображены страницы, с которыми вы будете работать.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Отложенная, упреждающая и явная загрузка связанных данных

Существует несколько способов, что платформа Entity Framework может загружать связанные данные в свойства навигации сущности:

- *Отложенная загрузка*. При первом чтении сущности связанные данные не извлекаются. Однако при первой попытке доступа к свойству навигации необходимые для этого свойства навигации данные извлекаются автоматически. Это приводит к отправке нескольких запросов к базе данных — один для самой сущности и один каждый раз, что связанные данные для сущности должны извлекаться. `DbContext` Класс включает отложенной загрузки по умолчанию. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Безотложная загрузка*. При чтении сущности связанные данные извлекаются вместе с ней. Обычно такая загрузка представляет собой одиночный запрос с соединением, который получает все необходимые данные. Безотложная загрузка указывается с помощью `Include` метод.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Явная загрузка*. Это похоже на отложенной загрузки, за исключением того, что вы явно не извлечете связанные данные в коде; Это не происходит автоматически при доступе к свойству навигации. Загружать связанные данные вручную путем получения записи диспетчер состояния объекта для сущности "и" вызов [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) метод для коллекций или [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) метод для свойства, которые содержат одной сущности. (В следующем примере, если вы хотите загрузить свойство навигации администратора, нужно заменить `Collection(x => x.Courses)` с `Reference(x => x.Administrator)`.) Обычно используется явная загрузка только в том случае, если вы включили отложенной загрузки из системы.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Так как они не извлекают сразу же значения свойств, отложенная загрузка и явная загрузка также оба называются *отложенная загрузка*.

### <a name="performance-considerations"></a>Особенности производительности

Если известно, что связанные данные потребуются для каждой полученной сущности, то безотложная загрузка обычно обеспечивает наилучшую производительность, поскольку одиночный запрос к базе данных обычно эффективнее нескольких отдельных запросов для каждой полученной сущности. Например в приведенных выше примерах Предположим, что на каждом факультете есть десять связанных курсов. Пример безотложной загрузки приведет запрос одним (join) и одного цикла приема-передачи в базу данных. Отложенная загрузка и явная загрузка примеров оба приведет одиннадцать запросы и одиннадцать приема-передачи в базу данных. При высокой задержке дополнительные циклы приема-передачи данных особенно сильно влияют на производительность.

С другой стороны в некоторых сценариях отложенная загрузка является более эффективным. Безотложная загрузка может привести к очень сложного соединения для создаваемого, который SQL Server не сможет эффективно обработать. Или, если вам нужно получить доступ к свойствам навигации сущности только для подмножества набора сущностей обработка, отложенная загрузка может работать лучше, так как Безотложная загрузка будет получено больше данных, чем необходимо. Если важна производительность, то для выбора наилучшего решения рекомендуется протестировать производительность для обоих случаев.

Отложенная загрузка может маскировать код, вызывающий проблемы с производительностью. Например код, который не указывает eager или явной загрузки, но обрабатывает большое количество сущностей и использует несколько свойств навигации в каждой итерации может быть неэффективны (из-за множество обращений к базе данных). Приложение, которое хорошо работать в разработку с помощью на локальный сервер SQL могут возникать проблемы с производительностью при перемещении базы данных SQL Azure из-за отложенной загрузки и увеличению времени задержки. Профилирование с реалистичные тестовые нагрузки запросов к базе данных, поможет определить, подходит ли отложенная загрузка. Дополнительные сведения см. в разделе [раскрытие стратегий Entity Framework: загрузка связанных данных](https://msdn.microsoft.com/magazine/hh205756.aspx) и [с помощью Entity Framework для сокращения задержек в сети для SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Отключить отложенную загрузку до сериализации

Если оставить отложенная загрузка включена во время сериализации, может опрашивать значительно больше данных, чем планировалось. Сериализация обычно работает с помощью каждого свойства в экземпляре типа. Доступ к свойству запустит отложенную загрузку, а сериализации этих отложенной загрузки сущностей. В процессе сериализации затем обращается к каждое свойство сущности отложенной загрузки, что может привести к еще больше отложенной загрузки и сериализации. Во избежание этой реакции цепи минимизируется включения отложенной загрузки из системы, прежде чем сериализации сущности.

Сериализация может также быть осложняется прокси-классы, которые использует Entity Framework, как описано в [расширенные сценарии учебника](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Один из способов избежать сериализации — для сериализации объектов передачи данных (DTO) вместо объекта сущности, как показано в [с помощью веб-API с Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) руководства.

Если вы не используете DTO, можно отключить отложенную загрузку и избежать проблем с прокси-сервера, [отключение создания прокси-сервера](https://msdn.microsoft.com/data/jj592886.aspx).

Ниже приведены некоторые другие [способа отключить отложенную загрузку](https://msdn.microsoft.com/data/jj574232):

- Для свойства навигации конкретного, пропустить `virtual` ключевое слово при объявлении свойства.
- Для всех свойств навигации, задайте `LazyLoadingEnabled` для `false`, поместите следующий код в конструктор класса контекста: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Создание страницы курсы, название отдела отображает

`Course` Сущность включает свойство навигации, которое содержит `Department` сущность Department факультета, присвоенный курса. Для отображения имени назначенной кафедры в списке курсов, необходимо получить `Name` свойства из `Department` сущность, которая находится в `Course.Department` свойство навигации.

Создайте контроллер с именем `CourseController` (не CoursesController) для `Course` тип сущности, используя те же параметры для **контроллер MVC 5 с представлениями, использующий Entity Framework** шаблон, который было показано ранее в `Student` контроллера, как показано на следующем рисунке:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Откройте *Controllers\CourseController.cs* и просмотрите `Index` метод:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

В автоматически сформированном шаблоне установлена безотложная загрузка свойства навигации `Department` при помощи метода `Include`.

Откройте *Views\Course\Index.cshtml* и замените код шаблона следующим кодом. Изменения выделены:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Мы внесли следующие изменения в код шаблона:

- Изменен заголовок из **индекс** для **курсы**.
- Добавлен столбец **Number** (Номер), отображающий значение свойства `CourseID`. По умолчанию шаблоне отсутствуют первичные ключи, так как обычно они не имеют смысла для конечных пользователей. Однако в нашем случае первичный ключ имеет смысл, и мы хотим его отобразить.
- Переместить **отдел** столбец справа от оператора и изменить его заголовок. Шаблон правильно выбрали `Name` свойства из `Department` сущности, но здесь в страницу курса, заголовок столбца должно быть **отдел** вместо **имя**.

Обратите внимание на то, что для столбце "Отдел", шаблонный код отображает `Name` свойство `Department` сущность, которая загружается в `Department` свойство навигации:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Откройте страницу (выберите **курсы** вкладку на домашней странице университета Contoso) для просмотра списка с названиями кафедр.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Создание страницы "Instructors" с отображением курсов и регистрации

В этом разделе вы создадите контроллера и просмотра для `Instructor` сущности, чтобы отобразить страницу Instructors:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Эта страница считывает и отображает связанные данные следующим образом:

- Список преподавателей отображает связанные данные из `OfficeAssignment` сущности. Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному. Вы будете использовать безотложную загрузку для `OfficeAssignment` сущностей. Как упоминалось ранее, безотложная загрузка обычно эффективнее при получении связанных данных для всех строк главной таблицы. В нашем случае мы хотим отобразить принадлежность к кабинету для каждого преподавателя.
- Когда пользователь выбирает преподавателя, связанные с `Course` сущности отображаются. Между сущностями `Instructor` и `Course` действует связь многие ко многим. Вы будете использовать безотложную загрузку для `Course` сущности и связанных с ними `Department` сущностей. Таким образом отложенная загрузка может быть более эффективным, поскольку нам требуются курсы только для выбранного преподавателя. Этот пример, однако, показывает, как использовать безотложную загрузку для свойств навигации сущностей, которые сами находятся в свойствах навигации.
- Когда пользователь выбирает курс, связанные данные из `Enrollments` отображается набор сущностей. Между сущностями `Course` и `Enrollment` действует связь один ко многим. Вам предстоит добавить явную загрузку для `Enrollment` сущности и связанных с ними `Student` сущностей. (Явная загрузка необязательно, поскольку отложенная загрузка включена, но это показано, как для выполнения явной загрузки.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Создание модели представления для представления Instructor Index

На странице преподавателей отображаются три разных таблиц. Таким образом, мы создаем модель представления, которая включает три свойства, каждое из которых содержит данные из одной таблицы.

В *ViewModels* папке создайте *InstructorIndexData.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Создание контроллера и представлений Instructor

Создание `InstructorController` (не InstructorsController) контроллер с действиями чтения и записи EF, как показано на следующем рисунке:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Откройте *Controllers\InstructorController.cs* и добавьте `using` инструкции для `ViewModels` пространство имен:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Шаблонный код в `Index` метод указывает упреждающую загрузку только для `OfficeAssignment` свойство навигации:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Замените `Index` метод следующим кодом, чтобы загрузить дополнительные данные, относящиеся к и поместить его в модели представления:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Этот метод принимает необязательные данные маршрута (`id`) и параметр строки запроса (`courseID`), содержат значения идентификатора выбранного преподавателя и выбранного курса и передает все необходимые данные в представление. Параметры передаются гиперссылками **Select** на странице.

Код начинается с создания экземпляра модели представления и помещения его в список преподавателей. Код указывает упреждающую загрузку для `Instructor.OfficeAssignment` и `Instructor.Courses` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

Второй `Include` метод загружает курсов, и для каждого курса, который загружается как Безотложная загрузка и для `Course.Department` свойства навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Как упоминалось ранее, Безотложная загрузка не является обязательным, но сделано для повышения производительности. Так как представление всегда требует `OfficeAssignment` сущности, это более эффективно извлекать ее в одном запросе. `Course` При выборе преподавателя на веб-странице, поэтому Безотложная загрузка лучше, чем отложенной загрузки только в том случае, если эта страница отображается с курсами чем без чаще требуются сущностей.

Если был выбран идентификатор преподавателя, выбранный преподаватель извлекается из списка преподавателей в модели представления. Модель представления `Courses` свойство затем загружается с `Course` сущностей из этого преподавателя `Courses` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where` Метод возвращает коллекцию, но в этом случае критерии, передан в метод только в одном `Instructor` возвращаемых сущностей. `Single` Метод преобразует коллекцию в один `Instructor` сущность, которая предоставляет доступ к этой сущности `Courses` свойство.

Использовании [единый](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) метод коллекции, когда известно, что коллекция будет иметь только один элемент. `Single` Метод вызывает исключение, если передаваемая ему коллекция пустая или если имеется более одного элемента. В качестве альтернативы [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), который возвращает значение по умолчанию (`null` в данном случае), если коллекция пуста. Тем не менее, при этом по-прежнему в сумме получится исключение (из-за попытки найти `Courses` свойство `null` ссылку), и сообщение об исключении нелегко будет понять причину проблемы. При вызове `Single` метод, можно также передать в `Where` условие вместо вызова метода `Where` метод отдельно:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

вместо следующего кода:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Далее, если был выбран курс, то он получается из списка курсов модели представления. Затем модели представления `Enrollments` загружается свойство `Enrollment` сущностей из этого курса `Enrollments` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Изменение представления Instructor индекса

В *Views\Instructor\Index.cshtml*, замените код шаблона следующим кодом. Изменения выделены:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Мы внесли следующие изменения в существующий код:

- Изменили класс модели на `InstructorIndexData`.
- Изменили заголовок страницы с **Index** на **Instructors**.
- Добавлен **Office** столбец, отображающий `item.OfficeAssignment.Location` только если `item.OfficeAssignment` не равно null. (Так как это связь один к нулю или одному, может быть связанным `OfficeAssignment` сущности.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Добавили код, который будет динамически добавлять `class="success"` для `tr` элемент выбранного преподавателя. Этот параметр задает цвет фона для выбранной строки с помощью [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) класса. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Добавлено новое `ActionLink` с меткой **выберите** сразу перед другими ссылками в каждой строке, что приводит идентификатор выбранного преподавателя отправку `Index` метод.

Запустите приложение и выберите **преподавателей** вкладки. На странице отображается `Location` свойства связанных `OfficeAssignment` сущности и пустую таблицу ячейки при отсутствии без связанных `OfficeAssignment` сущности.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

В *Views\Instructor\Index.cshtml* файл, после закрывающего `table` элемента (в конце файла), добавьте следующий код. Этот код отображает список связанных с преподавателем курсов, когда преподаватель выбран.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Этот код считывает свойство `Courses` модели представления для отображения списка курсов. Он также предоставляет `Select` гиперссылку, которая отправляет идентификатор выбранного курса `Index` метода действия.

Откройте страницу и выберите преподавателя. Вы увидите сетку, которая отображает курсы, назначенные выбранному преподавателю, и для каждого курса отобразится имя связанного факультета.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

После только что добавленного блока кода добавьте следующий код. Он отображает список студентов, которые зачислены на курс при выборе этого курса.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Этот код считывает `Enrollments` свойство модели представления для отображения списка студентов, зарегистрированных в этом курсе.

Откройте страницу и выберите преподавателя. Затем выберите курс, чтобы увидеть список зачисленных студентов и их оценки.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Добавление явная загрузка

Откройте *InstructorController.cs* и посмотрите, как `Index` метод получает список регистраций для выбранного курса:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

При получении списка преподавателей, мы указали безотложную загрузку для `Courses` свойство навигации и `Department` свойства каждого курса. А затем поместить `Courses` коллекции, в модели представления, и теперь вы обращаетесь к `Enrollments` свойства навигации от одной сущности в этой коллекции. Так как вы не указали безотложную загрузку для `Course.Enrollments` свойство навигации, отображаются данные из этого свойства на странице, в результате отложенной загрузки.

Если отключить отложенную загрузку без изменения кода в любом другом `Enrollments` свойство будет иметь значение null, независимо от того, какое количество регистраций, пришлось курса. В этом случае, чтобы загрузить `Enrollments` свойство, пришлось бы указать упреждающую или явную загрузку. Вы уже узнали, как для выполнения безотложной загрузки. Чтобы ознакомиться с примером явную загрузку, замените `Index` метод следующим кодом, который явно загружает `Enrollments` свойство. Изменить код, выделяются.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

После получения выбранной `Course` сущности, новый код явным образом загружает этого курса `Enrollments` свойство навигации:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

А затем явным образом загружает каждую `Enrollment` связанная сущность `Student` сущности:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Обратите внимание, что используется `Collection` метод для загрузки свойства коллекции, но для свойства, которое содержит только одну сущность, используйте `Reference` метод.

Теперь запустите страница индекса преподавателей, и вы увидите никакой разницы в том, что отображено на странице, несмотря на то, что вы изменили способ извлечения данных.

## <a name="summary"></a>Сводка

Теперь вы использовали все три способа (отложенная, упреждающая и явная) загружать связанные данные в свойства навигации. В следующем руководстве вы узнаете, как обновлять связанные данные.

Оставьте свои отзывы на том, как вам понравилось, и этот учебник, и что можно улучшить. Можно также запросить новые темы на [показать мне как с помощью кода](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Вперед](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
