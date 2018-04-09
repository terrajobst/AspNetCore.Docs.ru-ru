---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Чтение данных, связанных с платформой Entity Framework в приложении ASP.NET MVC (5, 10) | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 831f5e0a8b57907ea012c10c1d1f8ff166f5e88b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Чтение связанных данных с платформой Entity Framework в приложении ASP.NET MVC (5, 10)
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы путем сравнения код для завершения кода. Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем учебнике завершить модели данных School. В этом учебнике предстоит чтения и отображения связанных данных, то есть данных, загружающий Entity Framework в свойствах навигации.

На следующих рисунках изображены страницы, с которыми вы будете работать.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Отложенная, упреждающая и явная загрузка связанных данных

Существует несколько способов, что платформа Entity Framework можно загрузить связанные данные в свойства навигации сущности:

- *Отложенная загрузка*. При первом чтении сущности связанные данные не извлекаются. Однако при первой попытке доступа к свойству навигации необходимые для этого свойства навигации данные извлекаются автоматически. Это приведет к несколько запросов, отправляемые в базу данных — один для самой сущности и один каждый раз, что связанные данные для сущности необходимо извлечь. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Безотложная загрузка*. При чтении сущности связанные данные извлекаются вместе с ней. Обычно такая загрузка представляет собой одиночный запрос с соединением, который получает все необходимые данные. Безотложная загрузка задаются с помощью `Include` метод.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Явная загрузка*. Это похоже на отложенную загрузку, за исключением того, что явно получать взаимосвязанные данные в коде; Это не происходит автоматически при доступе к свойству навигации. Загрузить связанные данные вручную путем получения записи диспетчер состояния объекта сущности и вызова `Collection.Load` метод для коллекций или `Reference.Load` метод для свойств, которые содержат одну сущность. (В следующем примере, если вы хотите загрузить свойство навигации администратору следует заменить `Collection(x => x.Courses)` с `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Так как они не извлекают немедленно значения свойств, отложенную загрузку и явная загрузка также оба называются *отложенная загрузка*.

Как правило если известно, необходимо связанных данных для каждой сущности, полученного, упреждающая загрузка обеспечивает наивысшую производительность, из-за одного запроса, отправляемые в базу данных обычно более эффективно, чем отдельные запросы для каждой сущности, которые получены. Например в приведенных выше примерах Предположим, что каждый отдел содержит десять связанных курсов. В примере упреждающую приведет к запрос одним (join) и одним кругового пути к базе данных. Отложенная загрузка и примеры явная загрузка оба приведет одиннадцать запросов и одиннадцать циклов приема-передачи в базу данных. При высокой задержке дополнительные циклы приема-передачи данных особенно сильно влияют на производительность.

С другой стороны в некоторых сценариях отложенную загрузку более эффективна. Безотложная загрузка может привести к очень сложного соединения для создаваемого, который SQL Server не может эффективно обработать. Или если необходимо получить доступ к свойствам навигации сущности только для подмножества сущностей обработка, отложенная загрузка будет работать лучше, поскольку упреждающую бы получить больше данных, чем необходимо. Если важна производительность, то для выбора наилучшего решения рекомендуется протестировать производительность для обоих случаев.

Обычно следует использовать явную загрузку только в том случае, если вы включили отложенной загрузки из системы. Единственный случай, в случае включения отложенной загрузки — во время сериализации. Отложенная загрузка и сериализация не смешивайте также и включен следует соблюдать осторожность, что может оказаться запрос значительно больше данных, чем планировалось при отложенной загрузки. Сериализация обычно работает с помощью каждого свойства в экземпляре типа. Доступ к свойству инициирует отложенную загрузку и сериализуются этих сущностей, отложенной загрузки. В процессе сериализации затем обращается к каждое свойство сущности отложенной загрузки, может привести к еще более отложенную загрузку и сериализации. Во избежание этого реакции цепи минимизируется включите отложенной загрузки из системы, прежде чем сериализации сущности.

Класс контекста базы данных по умолчанию выполняет отложенную загрузку. Чтобы отключить отложенную загрузку двумя способами.

- Свойства навигации определенного опустить `virtual` ключевого слова при объявлении свойства.
- Все свойства навигации, задайте `LazyLoadingEnabled` для `false`. Например можно поместить следующий код в конструктор класса контекста: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Отложенная загрузка могут маскировать код, вызывающий проблемы с производительностью. Например код, который не указывает eager или явной загрузки, но обрабатывает большое количество сущностей и использует несколько свойств навигации в каждой итерации можно очень Неэффективная (из-за множество циклов приема-передачи в базу данных). Приложение, выполняющее подходят для разработки приложений с использованием локального сервера SQL могут возникнуть проблемы производительности при перемещении базы данных SQL Azure из-за отложенной загрузки и увеличивают задержку. Профилирование с загрузкой реалистичных тестовых запросов баз данных поможет определить, подходит ли отложенную загрузку. Дополнительные сведения см. [тайны стратегии Entity Framework: загрузка связанных данных](https://msdn.microsoft.com/magazine/hh205756.aspx) и [платформы Entity Framework, чтобы уменьшить задержки в сети в SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Создание страницы индекса курсов, название отдела отображает

`Course` Сущность включает свойство навигации, которое содержит `Department` сущности отдела, назначенный курса. Чтобы отобразить имя подразделения, назначенный в списке курсов, которые необходимо получить `Name` свойство из `Department` сущность, которая находится в `Course.Department` свойство навигации.

Создать контроллер с именем `CourseController` для `Course` типа сущности, используя же самые параметры, которые вы выполняли ранее для `Student` контроллера, как показано на следующем рисунке (за исключением, что в отличие от изображения, класс контекста находится в пространстве имен DAL, не моделей пространство имен):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Откройте *Controllers\CourseController.cs* и посмотрите на `Index` метод:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

В автоматически сформированном шаблоне установлена безотложная загрузка свойства навигации `Department` при помощи метода `Include`.

Откройте *Views\Course\Index.cshtml* и замените существующий код следующим кодом. Изменения выделены:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Мы внесли следующие изменения в код шаблона:

- Изменить заголовок из **индекс** для **курсы**.
- Переместить строку ссылки слева.
- Добавлен столбец под заголовком **номер** , показывающий `CourseID` значение свойства. (По умолчанию, первичные ключи не являются формирования шаблонов, так как обычно они не имеют смысла для конечных пользователей. Тем не менее в этом случае имеет значение первичного ключа и вы хотите отображать их.)
- Изменить заголовок последнего столбца из **DepartmentID** (имя внешнего ключа в `Department` сущностей) для **отдел**.

Обратите внимание, что для последнего столбца, формирования шаблонов код отображает `Name` свойство `Department` сущность, которая загружается в `Department` свойство навигации:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Запустите страницу (выберите **курсы** вкладку на домашней странице Contoso университета), чтобы просмотреть список с именами отдела.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Создайте страницу индекса инструкторов курсов и регистрации

В этом разделе вы создадите контроллера и просмотр для `Instructor` сущность для отображения страницы инструкторов индекса:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Эта страница считывает и отображает связанные данные следующим образом:

- В списке инструкторов отображаются связанные данные из `OfficeAssignment` сущности. Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному. Вы воспользуетесь упреждающую для `OfficeAssignment` сущностей. Как упоминалось ранее, безотложная загрузка обычно эффективнее при получении связанных данных для всех строк главной таблицы. В нашем случае мы хотим отобразить принадлежность к кабинету для каждого преподавателя.
- Когда пользователь выбирает инструктор, связанные с `Course` сущности отображаются. Между сущностями `Instructor` и `Course` действует связь многие ко многим. Вы воспользуетесь упреждающую для `Course` сущности и связанных с ними `Department` сущностей. В этом случае отложенную загрузку могут оказаться эффективными, поскольку требуется курсы только для выбранных инструктора. Этот пример, однако, показывает, как использовать безотложную загрузку для свойств навигации сущностей, которые сами находятся в свойствах навигации.
- Когда пользователь выбирает курса, связанные с данными из `Enrollments` отображается набор сущностей. Между сущностями `Course` и `Enrollment` действует связь один ко многим. Вы добавите явная загрузка для `Enrollment` сущности и связанных с ними `Student` сущностей. (Явная загрузка необязательно, поскольку отложенная загрузка включена, но это показывает, как сделать явная загрузка.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Создать модель представлений для представление Index инструктора

Страница индекса инструктора показано три разных таблиц. Таким образом, мы создаем модель представления, которая включает три свойства, каждое из которых содержит данные из одной таблицы.

В *ViewModels* папке создайте *InstructorIndexData.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Добавление стиля для выбранных строк

Чтобы пометить выбранные строки должны другой цвет фона. Для предоставления стиля для этого пользовательского интерфейса, добавьте следующий выделенный код в раздел `/* info and errors */` в *Content\Site.css*, как показано ниже:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Создание контроллера инструктора и представлений

Создание `InstructorController` контроллера, как показано на следующем рисунке:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Откройте *Controllers\InstructorController.cs* и добавьте `using` инструкции для `ViewModels` пространство имен:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Код формирования шаблонов в `Index` метод задает упреждающую только для `OfficeAssignment` свойство навигации:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Замените `Index` метод следующий код, чтобы загрузить дополнительные данные, связанные с и поместить его в модели представления:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Этот метод принимает необязательный маршрутизации данных (`id`) и параметр строки запроса (`courseID`), укажите идентификатор значения выбранного инструктора и выбранного курса и передает все необходимые данные в представлении. Параметры передаются гиперссылками **Select** на странице.

> [!TIP]
> 
> **Данные о маршруте**
> 
> Данные маршрута — это данные, связывателя модели найден в сегмент URL-адреса, указанные в таблице маршрутизации. Например, задает маршрут по умолчанию `controller`, `action`, и `id` сегментов:
> 
> маршруты. MapRoute)  
>  Имя: «Default»  
>  URL-адрес: «{controller} / {action} / {id}»,  
>  значения по умолчанию: new {контроллера = действие «Home» = «Индекс», id = UrlParameter.Optional}  
> );
> 
> В следующий URL-адрес маршрута по умолчанию соответствует `Instructor` как `controller`, `Index` как `action` и 1 в качестве `id`; это значения данных маршрута.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> «? courseID = 2021» имеет значение строки запроса. Связыватель модели также будет работать, если передать `id` как значение строки запроса:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> URL-адреса, созданные `ActionLink` инструкций в представлении Razor. В следующем коде `id` параметр соответствует маршрут по умолчанию, поэтому `id` добавляется в данные маршрута.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> В следующем коде `courseID` не совпадает с параметром в маршрут по умолчанию, он добавляется в виде строки запроса.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Код начинается с создания экземпляра модели представления и помещения его в список преподавателей. В коде задается упреждающую для `Instructor.OfficeAssignment` и `Instructor.Courses` свойства навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Второй `Include` метод загружает курсов, а для каждого курса, которая загружается как упреждающую и для `Course.Department` свойства навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Как упоминалось ранее, упреждающую не является обязательным, но сделано для повышения производительности. Так как представление всегда требует `OfficeAssignment` сущности, это значительно эффективнее извлекать, в одном запросе. `Course` сущности являются обязательными при выборе инструктора на веб-странице, упреждающую лучше, чем отложенную загрузку только в том случае, если эта страница отображается чаще с курсом выбран чем без.

Выделенный код преподавателя выбранного инструктора извлекается из списка инструкторов в модели представления. Модель представления `Courses` свойство затем загружен с `Course` сущностей из этого инструктора `Courses` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where` Метод возвращает коллекцию, но в этом случае условия, переданные в результат метода только один `Instructor` возвращаемых сущностей. `Single` Метод преобразует коллекцию в один `Instructor` сущности, которая предоставляет доступ к этой сущности `Courses` свойство.

Вы используете [одного](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) метод в коллекции, если известно коллекции будет иметь только один элемент. `Single` Метод создает исключение, если возвращается пустая коллекция, переданы в него, или если существует более одного элемента. Альтернативным вариантом является [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), который возвращает значение по умолчанию (`null` в этом случае), если коллекция пуста. Тем не менее, в этом случае, по-прежнему приведет к исключению (из найти `Courses` свойство `null` ссылка), и сообщение об исключении менее четко указывает на причину проблемы. При вызове `Single` метод, можно также передать в `Where` условие вместо вызова метода `Where` метод отдельно:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

вместо следующего кода:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Далее, если был выбран курс, то он получается из списка курсов модели представления. Затем модели представления `Enrollments` заполняемые свойства `Enrollment` сущностей из этого курса `Enrollments` свойство навигации.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Изменение представления индекс инструктора

В *Views\Instructor\Index.cshtml*, замените существующий код следующим кодом. Изменения выделены:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Мы внесли следующие изменения в существующий код:

- Изменили класс модели на `InstructorIndexData`.
- Изменили заголовок страницы с **Index** на **Instructors**.
- Переместить ссылку столбцы строки слева.
- Удалить **FullName** столбца.
- Добавлен **Office** столбец, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не имеет значение null. (Так как один к нулю или одному связь, может быть связана `OfficeAssignment` сущности.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Добавленный код, который будет динамически добавлять `class="selectedrow"` для `tr` инструктора выбранного элемента. Это задает цвет фона для строк, выделенных с помощью класса CSS, созданного ранее. ( `valign` Атрибут при добавлении столбцов нескольких строк в таблице будет полезна в этом руководстве.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Добавлен новый `ActionLink` с меткой **выберите** непосредственно перед другие ссылки в каждой строке, что вызывает код выбранного преподавателя отправку `Index` метод.

Запустите приложение и выберите **инструкторов** вкладки. На странице отображается `Location` свойства связанных `OfficeAssignment` сущностей и пустую таблицу ячейки при наличии нет связанных `OfficeAssignment` сущности.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

В *Views\Instructor\Index.cshtml* файла после закрывающего `table` элемента (в конце файла), добавьте следующий выделенный код. Откроется список связанных с инструктор при выборе инструктор курсов.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Этот код считывает свойство `Courses` модели представления для отображения списка курсов. Он также предоставляет `Select` гиперссылку, которая отправляет идентификатор курса для `Index` метода действия.

> [!NOTE]
> *.Css* кэшируется в браузерах. Если вы не видите изменения при запуске приложения, выполните жестких обновления (, удерживая нажатой клавишу CTRL **обновление** кнопку или нажмите клавиши CTRL + F5).


Запустите страницу и выберите инструктор. Вы увидите сетку, которая отображает курсы, назначенные выбранному преподавателю, и для каждого курса отобразится имя связанного факультета.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

После только что добавленного блока кода добавьте следующий код. Он отображает список студентов, которые зачислены на курс при выборе этого курса.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Этот код считывает `Enrollments` свойство модели представления для отображения списка учащихся, зарегистрированных в этом курсе.

Запустите страницу и выберите инструктор. Затем выберите курс, чтобы увидеть список зачисленных студентов и их оценки.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Добавление явная загрузка

Откройте *InstructorController.cs* и посмотрите на том, как `Index` метод возвращает список регистраций для выбранного курса:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

При получении списка инструкторов указанной упреждающую для `Courses` свойства навигации и для `Department` свойства каждого курса. А затем поместить `Courses` коллекции в модели представления, и теперь требуется получить доступ к `Enrollments` свойства навигации от одной сущности в этой коллекции. Поскольку вы не указали упреждающую для `Course.Enrollments` свойство навигации отображаются данные из этого свойства на странице в результате отложенную загрузку.

Если отключить отложенную загрузку без изменения кода другим способом, `Enrollments` свойство будет иметь значение null, независимо от того, какое количество регистраций пришлось курса. В этом случае для загрузки `Enrollments` свойства, нужно будет указать упреждающую или явная загрузка. Вы уже знаете, как сделать Безотложная загрузка. Чтобы увидеть пример явная загрузка, замените `Index` метод следующим кодом, который явно загружает `Enrollments` свойство. Изменения кода, выделяются.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

После получения выбранного `Course` сущности, новый код явно загружает этот курс `Enrollments` свойство навигации:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Затем он явно загружает каждый `Enrollment` связанная сущность `Student` сущности:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Обратите внимание, что вы используете `Collection` метод, чтобы загрузить свойство коллекции, но для свойства, которое содержит только одну сущность, используйте `Reference` метод. Можно запустить страницу индекса инструктора теперь и вы увидите не отличается от отображаемых на странице, несмотря на то, что вы изменили способ загрузки данных.

## <a name="summary"></a>Сводка

Теперь вы использовали все три способа (отложенной упреждающая и явные) для загрузки связанных данных в свойствах навигации. В следующем руководстве вы узнаете, как обновлять связанные данные.

Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Вперед](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
