---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Чтение данных, связанных с платформой Entity Framework в приложении ASP.NET MVC | Документы Microsoft"
author: tdykstra
description: /AJAX/tutorials/using-AJAX-Control-Toolkit-Controls-and-Control-Extenders-VB
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f4912bb3113a8f9cdae4211e055a7e317ab2aff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Чтение связанных данных с платформой Entity Framework в приложении ASP.NET MVC
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio 2013. Сведения о учебника серии см [в первом учебнике ряда](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущем учебнике завершить модели данных School. В этом учебнике предстоит чтения и отображения связанных данных, то есть данных, загружающий Entity Framework в свойствах навигации.

На следующих рисунках страницы, вы будете работать с.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Отложенная, упреждающая и явная загрузка связанных данных

Существует несколько способов, что платформа Entity Framework можно загрузить связанные данные в свойства навигации сущности:

- *Отложенная загрузка*. При считывании сущности связанные данные не получены. Однако первой попытке доступа к свойству навигации, данные, необходимые для этого свойства навигации автоматически извлекается. Это приведет к несколько запросов, отправляемые в базу данных — один для самой сущности и один каждый раз, что связанные данные для сущности необходимо извлечь. `DbContext` Класс включает отложенную загрузку по умолчанию. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Безотложная загрузка*. При чтении сущности связанные данные извлекаются вместе с ним. Обычно в результате запроса одного соединения, который получает все данные, которые необходимы. Безотложная загрузка задаются с помощью `Include` метод.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Явная загрузка*. Это похоже на отложенную загрузку, за исключением того, что явно получать взаимосвязанные данные в коде; Это не происходит автоматически при доступе к свойству навигации. Загрузить связанные данные вручную путем получения записи диспетчер состояния объекта сущности и вызова [Collection.Load](https://msdn.microsoft.com/en-us/library/gg696220(v=vs.103).aspx) метод для коллекций или [Reference.Load](https://msdn.microsoft.com/en-us/library/gg679166(v=vs.103).aspx) метод для свойств, которые содержат одной сущности. (В следующем примере, если вы хотите загрузить свойство навигации администратору следует заменить `Collection(x => x.Courses)` с `Reference(x => x.Administrator)`.) Обычно следует использовать явную загрузку только в том случае, если вы включили отложенной загрузки из системы.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Так как они не извлекают немедленно значения свойств, отложенную загрузку и явная загрузка также оба называются *отложенная загрузка*.

### <a name="performance-considerations"></a>Особенности производительности

Если известно, что требуются связанные данные для каждой сущности, которые получены, активная загрузка часто обеспечивает оптимальную производительность, поскольку одного запроса, отправляемые в базу данных обычно более эффективно, чем отдельные запросы для каждой сущности, которые получены. Например в приведенных выше примерах Предположим, что каждый отдел содержит десять связанных курсов. В примере упреждающую приведет к запрос одним (join) и одним кругового пути к базе данных. Отложенная загрузка и примеры явная загрузка оба приведет одиннадцать запросов и одиннадцать циклов приема-передачи в базу данных. При высокой задержки, дополнительных циклов обращения к базе данных, особенно к падению производительности.

С другой стороны в некоторых сценариях отложенную загрузку более эффективна. Безотложная загрузка может привести к очень сложного соединения для создаваемого, который SQL Server не может эффективно обработать. Или если необходимо получить доступ к свойствам навигации сущности только для подмножества сущностей обработка, отложенная загрузка будет работать лучше, поскольку упреждающую бы получить больше данных, чем необходимо. Если важна производительность, рекомендуется проверить производительность оба способа, чтобы выбрать наиболее подходящую.

Отложенная загрузка могут маскировать код, вызывающий проблемы с производительностью. Например код, который не указывает eager или явной загрузки, но обрабатывает большое количество сущностей и использует несколько свойств навигации в каждой итерации можно очень Неэффективная (из-за множество циклов приема-передачи в базу данных). Приложение, выполняющее подходят для разработки приложений с использованием локального сервера SQL могут возникнуть проблемы производительности при перемещении базы данных SQL Azure из-за отложенной загрузки и увеличивают задержку. Профилирование с загрузкой реалистичных тестовых запросов баз данных поможет определить, подходит ли отложенную загрузку. Дополнительные сведения см. [тайны стратегии Entity Framework: загрузка связанных данных](https://msdn.microsoft.com/en-us/magazine/hh205756.aspx) и [платформы Entity Framework, чтобы уменьшить задержки в сети в SQL Azure](https://msdn.microsoft.com/en-us/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Отключить отложенную загрузку перед выполнением сериализации

Если оставить отложенной загрузки включена во время сериализации, может оказаться запрос данных значительно больше, чем планировалось. Сериализация обычно работает с помощью каждого свойства в экземпляре типа. Доступ к свойству инициирует отложенную загрузку и сериализуются этих сущностей, отложенной загрузки. В процессе сериализации затем обращается к каждое свойство сущности отложенной загрузки, может привести к еще более отложенную загрузку и сериализации. Во избежание этого реакции цепи минимизируется включите отложенной загрузки из системы, прежде чем сериализации сущности.

Сериализация может также быть осложняется прокси-классы, используемые Entity Framework, как описано в [дополнительные сценарии учебника](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Один из способов избежать проблем при сериализации является сериализовать объекты передачи данных (DTO) вместо объекта сущности, как показано в [с помощью веб-API с Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) учебника.

Если вы не используете DTO, можно отключить отложенную загрузку и избежать проблем с прокси-сервера, [отключение создания прокси-сервера](https://msdn.microsoft.com/en-US/data/jj592886.aspx).

Ниже приведены некоторые другие [способа отключить отложенную загрузку](https://msdn.microsoft.com/en-US/data/jj574232):

- Свойства навигации определенного опустить `virtual` ключевого слова при объявлении свойства.
- Все свойства навигации, задайте `LazyLoadingEnabled` для `false`, поместите следующий код в конструктор класса контекста: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Создание страницы курсов, название отдела отображает

`Course` Сущность включает свойство навигации, которое содержит `Department` сущности отдела, назначенный курса. Чтобы отобразить имя подразделения, назначенный в списке курсов, которые необходимо получить `Name` свойство из `Department` сущность, которая находится в `Course.Department` свойство навигации.

Создать контроллер с именем `CourseController` (не CoursesController) для `Course` типа сущности, используя те же параметры для **контроллер MVC 5 с представлениями, использующий Entity Framework** scaffolder, которые были ранее для `Student` контроллера, как показано на следующем рисунке:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Откройте *Controllers\CourseController.cs* и посмотрите на `Index` метод:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Автоматическое формирование шаблонов указал упреждающую для `Department` свойство навигации с помощью `Include` метод.

Откройте *Views\Course\Index.cshtml* и замените код шаблона с помощью следующего кода. Изменения выделены:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Для формирования шаблонов код внесли следующие изменения:

- Изменить заголовок из **индекс** для **курсы**.
- Добавлен **номер** столбца, отображающего `CourseID` значение свойства. По умолчанию первичные ключи не являются формирования шаблонов, так как обычно они не имеют смысла для конечных пользователей. Однако в этом случае первичный ключ имеет смысл, и вы хотите отображать их.
- Переместить **отдел** столбца справа и изменить его заголовка. Scaffolder правильно выбрать для отображения `Name` свойство из `Department` сущности, однако в данном случае на странице курса, должен быть заголовком столбца **отдел** вместо **имя**.

Обратите внимание, что в столбце отдел формирования шаблонов код отображает `Name` свойство `Department` сущность, которая загружается в `Department` свойство навигации:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Запустите страницу (выберите **курсы** вкладку на домашней странице Contoso университета), чтобы просмотреть список с именами отдела.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Создание страницы инструкторов курсов и регистрации

В этом разделе вы создадите контроллера и просмотр для `Instructor` сущности, чтобы отобразить страницу «преподаватели»:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Эта страница считывает и взаимосвязанные данные отображаются следующим образом:

- В списке инструкторов отображаются связанные данные из `OfficeAssignment` сущности. `Instructor` И `OfficeAssignment` сущности находятся в отношении один к нулю или одному. Вы воспользуетесь упреждающую для `OfficeAssignment` сущностей. Как упоминалось ранее, упреждающую обычно более эффективна при необходимости связанные данные для всех извлеченных строк таблицы первичного. В этом случае будет отображаться office назначения для всех отображаемых инструкторов.
- Когда пользователь выбирает инструктор, связанные с `Course` сущности отображаются. `Instructor` И `Course` сущности находятся в отношении многие ко многим. Вы воспользуетесь упреждающую для `Course` сущности и связанных с ними `Department` сущностей. В этом случае отложенную загрузку могут оказаться эффективными, поскольку требуется курсы только для выбранных инструктора. Однако в этом примере показано, как использовать упреждающую свойств навигации в сущностях, которые сами в свойствах навигации.
- Когда пользователь выбирает курса, связанные с данными из `Enrollments` отображается набор сущностей. `Course` И `Enrollment` сущности находятся в отношении один ко многим. Вы добавите явная загрузка для `Enrollment` сущности и связанных с ними `Student` сущностей. (Явная загрузка необязательно, поскольку отложенная загрузка включена, но это показывает, как сделать явная загрузка.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Создать модель представлений для представление Index инструктора

На странице инструкторов отображаются три разных таблиц. Таким образом вы создадите модель представления, которая включает три свойства каждого хранения данных для одной из таблиц.

В *ViewModels* папке создайте *InstructorIndexData.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Создание контроллера инструктора и представления

Создание `InstructorController` (не InstructorsController) контроллер с действиями чтения и записи EF, как показано на следующем рисунке:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Откройте *Controllers\InstructorController.cs* и добавьте `using` инструкции для `ViewModels` пространство имен:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Код формирования шаблонов в `Index` метод задает упреждающую только для `OfficeAssignment` свойство навигации:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Замените `Index` метод следующий код, чтобы загрузить дополнительные данные, связанные с и поместить его в модели представления:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Этот метод принимает необязательный маршрутизации данных (`id`) и параметр строки запроса (`courseID`), укажите идентификатор значения выбранного инструктора и выбранного курса и передает все необходимые данные в представлении. Параметры, предоставляемые **выберите** гиперссылок на странице.

Код начинается с создания экземпляра модели представления и помещения его в списке инструкторов. В коде задается упреждающую для `Instructor.OfficeAssignment` и `Instructor.Courses` свойства навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

Второй `Include` метод загружает курсов, а для каждого курса, которая загружается как упреждающую и для `Course.Department` свойства навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Как упоминалось ранее, упреждающую не является обязательным, но сделано для повышения производительности. Так как представление всегда требует `OfficeAssignment` сущности, это значительно эффективнее извлекать, в одном запросе. `Course`сущности являются обязательными при выборе инструктора на веб-странице, упреждающую лучше, чем отложенную загрузку только в том случае, если эта страница отображается чаще с курсом выбран чем без.

Выделенный код преподавателя выбранного инструктора извлекается из списка инструкторов в модели представления. Модель представления `Courses` свойство затем загружен с `Course` сущностей из этого инструктора `Courses` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where` Метод возвращает коллекцию, но в этом случае условия, переданные в результат метода только один `Instructor` возвращаемых сущностей. `Single` Метод преобразует коллекцию в один `Instructor` сущности, которая предоставляет доступ к этой сущности `Courses` свойство.

Вы используете [одного](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.single.aspx) метод в коллекции, если известно коллекции будет иметь только один элемент. `Single` Метод создает исключение, если возвращается пустая коллекция, переданы в него, или если существует более одного элемента. Альтернативным вариантом является [SingleOrDefault](https://msdn.microsoft.com/en-us/library/bb342451.aspx), который возвращает значение по умолчанию (`null` в этом случае), если коллекция пуста. Тем не менее, в этом случае, по-прежнему приведет к исключению (из найти `Courses` свойство `null` ссылка), и сообщение об исключении менее четко указывает на причину проблемы. При вызове `Single` метод, можно также передать в `Where` условие вместо вызова метода `Where` метод отдельно:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

вместо следующего кода:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Далее Если был выбран курса, выбранного курса извлекается из списка курсов в модели представления. Затем модели представления `Enrollments` заполняемые свойства `Enrollment` сущностей из этого курса `Enrollments` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Изменить представление Index инструктора

В *Views\Instructor\Index.cshtml*, замените код шаблона в следующий код. Изменения выделены:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Следующие изменения, внесенные в существующий код:

- Класс модели, чтобы изменить `InstructorIndexData`.
- Изменить заголовок страницы из **индекс** для **инструкторов**.
- Добавлен **Office** столбец, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не имеет значение null. (Так как один к нулю или одному связь, может быть связана `OfficeAssignment` сущности.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Добавленный код, который будет динамически добавлять `class="success"` для `tr` инструктора выбранного элемента. Это задает цвет фона для выбранной строки с помощью [начальной загрузки](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) класса. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Добавлен новый `ActionLink` с меткой **выберите** непосредственно перед другие ссылки в каждой строке, что вызывает код выбранного преподавателя отправку `Index` метод.

Запустите приложение и выберите **инструкторов** вкладки. На странице отображается `Location` свойства связанных `OfficeAssignment` сущностей и пустую таблицу ячейки при наличии нет связанных `OfficeAssignment` сущности.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

В *Views\Instructor\Index.cshtml* файла после закрывающего `table` элемента (в конце файла), добавьте следующий код. Этот код отображает список связанных с инструктор при выборе инструктор курсов.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Этот код считывает `Courses` свойство модели представления для отображения списка курсов. Он также предоставляет `Select` гиперссылку, которая отправляет идентификатор курса для `Index` метода действия.

Запустите страницу и выберите инструктор. Вы увидите на таблице, которая отображает курсов, которые назначены выбранной инструктора, и для каждого курса вы увидите имя подразделения, назначенный.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

После только что добавленного блока кода добавьте следующий код. Это список учащихся, которые участвуют в курса при выборе этого курса.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Этот код считывает `Enrollments` свойство модели представления для отображения списка учащихся, зарегистрированных в этом курсе.

Запустите страницу и выберите инструктор. Выберите курс, чтобы просмотреть список зарегистрированных студентов и их оценки.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Добавление явная загрузка

Откройте *InstructorController.cs* и посмотрите на том, как `Index` метод возвращает список регистраций для выбранного курса:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

При получении списка инструкторов указанной упреждающую для `Courses` свойства навигации и для `Department` свойства каждого курса. А затем поместить `Courses` коллекции в модели представления, и теперь требуется получить доступ к `Enrollments` свойства навигации от одной сущности в этой коллекции. Поскольку вы не указали упреждающую для `Course.Enrollments` свойство навигации отображаются данные из этого свойства на странице в результате отложенную загрузку.

Если отключить отложенную загрузку без изменения кода другим способом, `Enrollments` свойство будет иметь значение null, независимо от того, какое количество регистраций пришлось курса. В этом случае для загрузки `Enrollments` свойства, нужно будет указать упреждающую или явная загрузка. Вы уже знаете, как сделать Безотложная загрузка. Чтобы увидеть пример явная загрузка, замените `Index` метод следующим кодом, который явно загружает `Enrollments` свойство. Изменения кода, выделяются.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

После получения выбранного `Course` сущности, новый код явно загружает этот курс `Enrollments` свойство навигации:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Затем он явно загружает каждый `Enrollment` связанная сущность `Student` сущности:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Обратите внимание, что вы используете `Collection` метод, чтобы загрузить свойство коллекции, но для свойства, которое содержит только одну сущность, используйте `Reference` метод.

Теперь запустите страницу индекса инструктора и вы увидите не отличается от отображаемых на странице, несмотря на то, что вы изменили способ загрузки данных.

## <a name="summary"></a>Сводка

Теперь вы использовали все три способа (отложенной упреждающая и явные) для загрузки связанных данных в свойствах навигации. В следующем уроке будет рассмотрено обновления связанных данных.

Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить. Можно также запросить новые разделы на [показать мне как с код](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступа к данным ASP.NET - рекомендуется использовать ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Назад](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[Вперед](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
