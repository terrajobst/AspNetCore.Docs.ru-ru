---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Чтение связанных данных с Entity Framework в приложении ASP.NET MVC (5 из 10) | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4767b015db0bad09942802827ce54162687fcabc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835415"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Чтение связанных данных с Entity Framework в приложении ASP.NET MVC (5 из 10)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Серии руководств можно начать с самого начала или [Загрузите начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если вы столкнулись с проблемами, не удается устранить, [скачать завершенного глава](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы, сравнивая код, чтобы полный код. Некоторые распространенные ошибки и способы их устранения, см. в разделе [ошибки и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем учебном курсе мы завершили модели School. В этом руководстве можно прочитать и отображения связанных данных — то есть данные, которые Entity Framework загружает в свойства навигации.

На следующих рисунках изображены страницы, с которыми вы будете работать.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Отложенная, упреждающая и явная загрузка связанных данных

Существует несколько способов, что платформа Entity Framework может загружать связанные данные в свойства навигации сущности:

- *Отложенная загрузка*. При первом чтении сущности связанные данные не извлекаются. Однако при первой попытке доступа к свойству навигации необходимые для этого свойства навигации данные извлекаются автоматически. Это приводит к отправке нескольких запросов к базе данных — один для самой сущности и один каждый раз, что связанные данные для сущности должны извлекаться. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Безотложная загрузка*. При чтении сущности связанные данные извлекаются вместе с ней. Обычно такая загрузка представляет собой одиночный запрос с соединением, который получает все необходимые данные. Безотложная загрузка указывается с помощью `Include` метод.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Явная загрузка*. Это похоже на отложенной загрузки, за исключением того, что вы явно не извлечете связанные данные в коде; Это не происходит автоматически при доступе к свойству навигации. Загружать связанные данные вручную путем получения записи диспетчер состояния объекта для сущности "и" вызов `Collection.Load` метод для коллекций или `Reference.Load` метод для свойства, которые содержат одну сущность. (В следующем примере, если вы хотите загрузить свойство навигации администратора, нужно заменить `Collection(x => x.Courses)` с `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Так как они не извлекают сразу же значения свойств, отложенная загрузка и явная загрузка также оба называются *отложенная загрузка*.

Как правило, если вы знаете связанные данные потребуются для каждой сущности, полученного, упреждающая загрузка предлагает наилучшую производительность, поскольку одиночный запрос к базе данных обычно эффективнее нескольких отдельных запросов для каждой полученной сущности. Например в приведенных выше примерах Предположим, что на каждом факультете есть десять связанных курсов. Пример безотложной загрузки приведет запрос одним (join) и одного цикла приема-передачи в базу данных. Отложенная загрузка и явная загрузка примеров оба приведет одиннадцать запросы и одиннадцать приема-передачи в базу данных. При высокой задержке дополнительные циклы приема-передачи данных особенно сильно влияют на производительность.

С другой стороны в некоторых сценариях отложенная загрузка является более эффективным. Безотложная загрузка может привести к очень сложного соединения для создаваемого, который SQL Server не сможет эффективно обработать. Или, если вам нужно получить доступ к свойствам навигации сущности только для подмножества набора сущностей обработка, отложенная загрузка может работать лучше, так как Безотложная загрузка будет получено больше данных, чем необходимо. Если важна производительность, то для выбора наилучшего решения рекомендуется протестировать производительность для обоих случаев.

Обычно используется явная загрузка только в том случае, если вы включили отложенной загрузки из системы. Вам следует включить отложенная загрузка один сценарий вызывается во время сериализации. Отложенная загрузка и сериализация не следует смешивать хорошо, и если не соблюдать осторожность, что можно опрашивать значительно больше данных, чем планировалось, если отложенная загрузка включена. Сериализация обычно работает с помощью каждого свойства в экземпляре типа. Доступ к свойству запустит отложенную загрузку, а сериализации этих отложенной загрузки сущностей. В процессе сериализации затем обращается к каждое свойство сущности отложенной загрузки, что может привести к еще больше отложенной загрузки и сериализации. Во избежание этой реакции цепи минимизируется включения отложенной загрузки из системы, прежде чем сериализации сущности.

Класс контекста базы данных по умолчанию выполняет отложенную загрузку. Чтобы отключить отложенную загрузку двумя способами:

- Для свойства навигации конкретного, пропустить `virtual` ключевое слово при объявлении свойства.
- Для всех свойств навигации, задайте `LazyLoadingEnabled` для `false`. Например можно поместить следующий код в конструктор класса контекста: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Отложенная загрузка может маскировать код, вызывающий проблемы с производительностью. Например код, который не указывает eager или явной загрузки, но обрабатывает большое количество сущностей и использует несколько свойств навигации в каждой итерации может быть неэффективны (из-за множество обращений к базе данных). Приложение, которое хорошо работать в разработку с помощью на локальный сервер SQL могут возникать проблемы с производительностью при перемещении базы данных SQL Azure из-за отложенной загрузки и увеличению времени задержки. Профилирование с реалистичные тестовые нагрузки запросов к базе данных, поможет определить, подходит ли отложенная загрузка. Дополнительные сведения см. в разделе [раскрытие стратегий Entity Framework: загрузка связанных данных](https://msdn.microsoft.com/magazine/hh205756.aspx) и [с помощью Entity Framework для сокращения задержек в сети для SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Создание, название отдела отображается страница индекса курсов

`Course` Сущность включает свойство навигации, которое содержит `Department` сущность Department факультета, присвоенный курса. Для отображения имени назначенной кафедры в списке курсов, необходимо получить `Name` свойства из `Department` сущность, которая находится в `Course.Department` свойство навигации.

Создайте контроллер с именем `CourseController` для `Course` тип сущности, используя те же параметры, которые вы выполняли ранее для `Student` контроллера, как показано на следующем рисунке (за исключением, что в отличие от образа, класс контекста находится в пространстве имен DAL, не моделей пространство имен):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Откройте *Controllers\CourseController.cs* и просмотрите `Index` метод:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

В автоматически сформированном шаблоне установлена безотложная загрузка свойства навигации `Department` при помощи метода `Include`.

Откройте *Views\Course\Index.cshtml* и замените существующий код следующим кодом. Изменения выделены:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Мы внесли следующие изменения в код шаблона:

- Изменен заголовок из **индекс** для **курсы**.
- Переместить строку ссылки слева.
- Добавлен столбец под заголовком **номер** , в котором показано `CourseID` значение свойства. (По умолчанию шаблоне отсутствуют первичные ключи, так как обычно они не имеют смысла для конечных пользователей. Однако в этом случае первичный ключ имеет смысл и вы хотите показать ее.)
- Изменить заголовок последнего столбца из **DepartmentID** (имя внешнего ключа для `Department` сущности) к **отдел**.

Обратите внимание на то, что для последнего столбца, шаблонный код отображает `Name` свойство `Department` сущность, которая загружается в `Department` свойство навигации:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Откройте страницу (выберите **курсы** вкладку на домашней странице университета Contoso) для просмотра списка с названиями кафедр.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Создание страница индекса преподавателей, с отображением курсов и регистрации

В этом разделе вы создадите контроллера и просмотра для `Instructor` сущности, чтобы отобразить страницу Instructors Index:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Эта страница считывает и отображает связанные данные следующим образом:

- Список преподавателей отображает связанные данные из `OfficeAssignment` сущности. Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному. Вы будете использовать безотложную загрузку для `OfficeAssignment` сущностей. Как упоминалось ранее, безотложная загрузка обычно эффективнее при получении связанных данных для всех строк главной таблицы. В нашем случае мы хотим отобразить принадлежность к кабинету для каждого преподавателя.
- Когда пользователь выбирает преподавателя, связанные с `Course` сущности отображаются. Между сущностями `Instructor` и `Course` действует связь многие ко многим. Вы будете использовать безотложную загрузку для `Course` сущности и связанных с ними `Department` сущностей. Таким образом отложенная загрузка может быть более эффективным, поскольку нам требуются курсы только для выбранного преподавателя. Этот пример, однако, показывает, как использовать безотложную загрузку для свойств навигации сущностей, которые сами находятся в свойствах навигации.
- Когда пользователь выбирает курс, связанные данные из `Enrollments` отображается набор сущностей. Между сущностями `Course` и `Enrollment` действует связь один ко многим. Вам предстоит добавить явную загрузку для `Enrollment` сущности и связанных с ними `Student` сущностей. (Явная загрузка необязательно, поскольку отложенная загрузка включена, но это показано, как для выполнения явной загрузки.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Создание модели представления для представления Instructor Index

Страница индекса преподавателей показано три разных таблиц. Таким образом, мы создаем модель представления, которая включает три свойства, каждое из которых содержит данные из одной таблицы.

В *ViewModels* папке создайте *InstructorIndexData.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Добавление стиля для выбранных строк

Чтобы пометить выбранные строки требуется другой цвет фона. Чтобы создать стиль для этого пользовательского интерфейса, добавьте следующий выделенный код в раздел `/* info and errors */` в *Content\Site.css*, как показано ниже:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Создание контроллера и представлений Instructor

Создание `InstructorController` контроллера, как показано на следующем рисунке:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Откройте *Controllers\InstructorController.cs* и добавьте `using` инструкции для `ViewModels` пространство имен:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Шаблонный код в `Index` метод указывает упреждающую загрузку только для `OfficeAssignment` свойство навигации:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Замените `Index` метод следующим кодом, чтобы загрузить дополнительные данные, относящиеся к и поместить его в модели представления:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Этот метод принимает необязательные данные маршрута (`id`) и параметр строки запроса (`courseID`), содержат значения идентификатора выбранного преподавателя и выбранного курса и передает все необходимые данные в представление. Параметры передаются гиперссылками **Select** на странице.

> [!TIP]
> 
> **Данные маршрута**
> 
> Данные маршрута — это данные, которые обнаруживаются связывателем модели в сегмент URL-адреса, указанные в таблице маршрутизации. Например, задает маршрут по умолчанию `controller`, `action`, и `id` сегментов:
> 
> маршруты. MapRoute)  
>  Имя: «По умолчанию»  
>  URL-адрес: «{controller} / {action} / {id}»,  
>  значения по умолчанию: new {контроллера = «Home» action = «Index», идентификатор = UrlParameter.Optional}  
> );
> 
> В следующий URL-адрес соответствует маршрут по умолчанию `Instructor` как `controller`, `Index` как `action` и 1 в качестве `id`; это значения данных маршрута.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> «? courseID = 2021» — это значения строки запроса. Связыватель модели также будет работать, если передать `id` значения строки запроса:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> URL-адреса создаются с `ActionLink` инструкций в представлении Razor. В следующем коде `id` параметр соответствует маршруту по умолчанию, поэтому `id` добавляется в качестве данных маршрута.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> В следующем коде `courseID` не соответствует параметру в маршруте по умолчанию, поэтому он добавляется как строку запроса.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Код начинается с создания экземпляра модели представления и помещения его в список преподавателей. Код указывает упреждающую загрузку для `Instructor.OfficeAssignment` и `Instructor.Courses` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Второй `Include` метод загружает курсов, и для каждого курса, который загружается как Безотложная загрузка и для `Course.Department` свойства навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Как упоминалось ранее, Безотложная загрузка не является обязательным, но сделано для повышения производительности. Так как представление всегда требует `OfficeAssignment` сущности, это более эффективно извлекать ее в одном запросе. `Course` При выборе преподавателя на веб-странице, поэтому Безотложная загрузка лучше, чем отложенной загрузки только в том случае, если эта страница отображается с курсами чем без чаще требуются сущностей.

Если был выбран идентификатор преподавателя, выбранный преподаватель извлекается из списка преподавателей в модели представления. Модель представления `Courses` свойство затем загружается с `Course` сущностей из этого преподавателя `Courses` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where` Метод возвращает коллекцию, но в этом случае критерии, передан в метод только в одном `Instructor` возвращаемых сущностей. `Single` Метод преобразует коллекцию в один `Instructor` сущность, которая предоставляет доступ к этой сущности `Courses` свойство.

Использовании [единый](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) метод коллекции, когда известно, что коллекция будет иметь только один элемент. `Single` Метод вызывает исключение, если передаваемая ему коллекция пустая или если имеется более одного элемента. В качестве альтернативы [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), который возвращает значение по умолчанию (`null` в данном случае), если коллекция пуста. Тем не менее, при этом по-прежнему в сумме получится исключение (из-за попытки найти `Courses` свойство `null` ссылку), и сообщение об исключении нелегко будет понять причину проблемы. При вызове `Single` метод, можно также передать в `Where` условие вместо вызова метода `Where` метод отдельно:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

вместо следующего кода:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Далее, если был выбран курс, то он получается из списка курсов модели представления. Затем модели представления `Enrollments` загружается свойство `Enrollment` сущностей из этого курса `Enrollments` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Изменение представления Instructor индекса

В *Views\Instructor\Index.cshtml*, замените существующий код следующим кодом. Изменения выделены:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Мы внесли следующие изменения в существующий код:

- Изменили класс модели на `InstructorIndexData`.
- Изменили заголовок страницы с **Index** на **Instructors**.
- Переместить столбцы строки ссылку слева.
- Удалить **FullName** столбца.
- Добавлен **Office** столбец, отображающий `item.OfficeAssignment.Location` только если `item.OfficeAssignment` не равно null. (Так как это связь один к нулю или одному, может быть связанным `OfficeAssignment` сущности.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Добавили код, который будет динамически добавлять `class="selectedrow"` для `tr` элемент выбранного преподавателя. Это задает цвет фона для выбранной строки, с помощью класса CSS, который был создан ранее. ( `valign` Атрибута будут полезны в этом руководстве, при добавлении столбца нескольких строк в таблицу.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Добавлено новое `ActionLink` с меткой **выберите** сразу перед другими ссылками в каждой строке, что приводит идентификатор выбранного преподавателя отправку `Index` метод.

Запустите приложение и выберите **преподавателей** вкладки. На странице отображается `Location` свойства связанных `OfficeAssignment` сущности и пустую таблицу ячейки при отсутствии без связанных `OfficeAssignment` сущности.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

В *Views\Instructor\Index.cshtml* файл, после закрывающего `table` элемента (в конце файла), добавьте следующий выделенный код. Откроется список курсов, связанных с преподавателем, при выборе преподавателя.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Этот код считывает свойство `Courses` модели представления для отображения списка курсов. Он также предоставляет `Select` гиперссылку, которая отправляет идентификатор выбранного курса `Index` метода действия.

> [!NOTE]
> *.Css* файл кэшируется в браузерах. Если вы не видите изменений, при запуске приложения, выполните обновление жесткого (удерживайте нажатой клавишу CTRL, щелкая мышью **обновить** кнопку, или нажмите клавиши CTRL + F5).


Откройте страницу и выберите преподавателя. Вы увидите сетку, которая отображает курсы, назначенные выбранному преподавателю, и для каждого курса отобразится имя связанного факультета.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

После только что добавленного блока кода добавьте следующий код. Он отображает список студентов, которые зачислены на курс при выборе этого курса.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Этот код считывает `Enrollments` свойство модели представления для отображения списка студентов, зарегистрированных в этом курсе.

Откройте страницу и выберите преподавателя. Затем выберите курс, чтобы увидеть список зачисленных студентов и их оценки.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Добавление явная загрузка

Откройте *InstructorController.cs* и посмотрите, как `Index` метод получает список регистраций для выбранного курса:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

При получении списка преподавателей, мы указали безотложную загрузку для `Courses` свойство навигации и `Department` свойства каждого курса. А затем поместить `Courses` коллекции, в модели представления, и теперь вы обращаетесь к `Enrollments` свойства навигации от одной сущности в этой коллекции. Так как вы не указали безотложную загрузку для `Course.Enrollments` свойство навигации, отображаются данные из этого свойства на странице, в результате отложенной загрузки.

Если отключить отложенную загрузку без изменения кода в любом другом `Enrollments` свойство будет иметь значение null, независимо от того, какое количество регистраций, пришлось курса. В этом случае, чтобы загрузить `Enrollments` свойство, пришлось бы указать упреждающую или явную загрузку. Вы уже узнали, как для выполнения безотложной загрузки. Чтобы ознакомиться с примером явную загрузку, замените `Index` метод следующим кодом, который явно загружает `Enrollments` свойство. Изменить код, выделяются.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

После получения выбранной `Course` сущности, новый код явным образом загружает этого курса `Enrollments` свойство навигации:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

А затем явным образом загружает каждую `Enrollment` связанная сущность `Student` сущности:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Обратите внимание, что используется `Collection` метод для загрузки свойства коллекции, но для свойства, которое содержит только одну сущность, используйте `Reference` метод. Страница индекса преподавателей можно выполнить, и вы увидите никакой разницы в том, что отображено на странице, несмотря на то, что вы изменили способ извлечения данных.

## <a name="summary"></a>Сводка

Теперь вы использовали все три способа (отложенная, упреждающая и явная) загружать связанные данные в свойства навигации. В следующем руководстве вы узнаете, как обновлять связанные данные.

Ссылки на другие ресурсы Entity Framework можно найти в [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Вперед](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
