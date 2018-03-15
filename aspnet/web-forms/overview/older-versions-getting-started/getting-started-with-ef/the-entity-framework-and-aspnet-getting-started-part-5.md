---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: "Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4, веб-формы - часть 5 | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, использующий Entity Framework. Это образец приложения..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 5efc5ff367d5da5df060eba0028399af898a69fa
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Приступая к работе с базой данных Entity Framework 4.0 сначала и форм ASP.NET 4 веб - часть 5
====================
по [Tom Dykstra](https://github.com/tdykstra)

> Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010. Сведения о учебника серии см [в первом учебнике ряда](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Продолжение работы со связанными данными

В предыдущем учебнике вы начали использовать `EntityDataSource` для работы со связанными данными управления. Отображения нескольких уровней иерархии и изменения данных в свойствах навигации. В этом учебнике будет продолжать работать со связанными данными путем добавления и удаления связей, а также посредством добавления новой сущности, имеющий отношение к существующей сущности.

Вы создадите страницу, которая добавляет курсов, назначенные отделов. Отделы уже существует, и при создании нового курса, в то же время будет установления связи между ним и отдела.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Вы также создадите страницы, которая работает с отношением «многие ко многим», назначив инструктор курса (Добавление отношения между двумя сущностями, выбранными) или инструктор убирая курса (удаление связи между двумя сущностями, которые Выберите). В базе данных, Добавление отношения между инструктор и курса приводит к новой строки, добавляемые `CourseInstructor` таблице ассоциации; удаление связи включает в себя удаление строки из `CourseInstructor` связи таблицы. Тем не менее, это делается на платформе Entity Framework, установив свойства навигации, не обращаясь к `CourseInstructor` таблицы явным образом.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Добавление сущности со связью существующей сущности

Создать новый веб-страницу с именем *CoursesAdd.aspx* , использующий *Site.Master* главную страницу и добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Эта разметка создает `EntityDataSource` элемента управления, выбирающий курсов, который позволяет включить и, указывающий обработчик для `Inserting` события. Обработчик будет использоваться для обновления `Department` свойство навигации, при создании нового `Course` создания сущности.

Также создает разметку `DetailsView` управления, используемые для добавления новых `Course` сущностей. Разметка использует связанных полей для `Course` свойства сущности. Необходимо ввести `CourseID` значение, так как это не является полем созданный системой идентификатор. Вместо этого он является курса число, которое необходимо указать вручную, при создании курса.

Используйте поле шаблона для `Department` свойство навигации, так как свойства навигации не может использоваться с `BoundField` элементов управления. Поле шаблон предоставляет раскрывающегося списка для выбора подразделение. Раскрывающегося списка привязан к `Departments` сущности устанавливается с помощью `Eval` вместо `Bind`, снова поскольку напрямую привязать свойства навигации для их обновления. Укажите обработчик для `DropDownList` элемента управления `Init` событий, чтобы можно было сохранить ссылку на элемент управления для использования, код, который обновляет `DepartmentID` внешнего ключа.

В *CoursesAdd.aspx.cs* сразу после объявления разделяемого класса, добавьте поле класса для хранения ссылки на `DepartmentsDropDownList` управления:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Добавление обработчика для `DepartmentsDropDownList` элемента управления `Init` событий, чтобы можно было хранить ссылку на элемент управления. Это позволяет получить значение, введенное пользователем и использовать его для обновления `DepartmentID` значение `Course` сущности.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Добавление обработчика для `DetailsView` элемента управления `Inserting` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Когда пользователь щелкает `Insert`, `Inserting` событие возникает перед вставкой новой записи. Возвращает код в обработчике `DepartmentID` из `DropDownList` управления и использует его, чтобы задать значение, которое будет использоваться для `DepartmentID` свойство `Course` сущности.

Платформа Entity Framework позаботится о добавлении этот курс для `Courses` свойство навигации связанного `Department` сущности. Он также добавляет в отдел `Department` свойство навигации `Course` сущности.

Запустите страницу.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Введите идентификатор, заголовок, количество кредиты и подразделения, а затем нажмите кнопку **вставить**.

Запустите *Courses.aspx* и выберите того же отдела, чтобы увидеть новый курс.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Работа со связями многие ко многим

Связь между `Courses` набора сущностей и `People` набор сущностей представляет отношение многие ко многим. Объект `Course` сущность имеет свойство навигации с именем `People` , может содержать ноль, один или несколько связанных `Person` сущностей (представляющий инструкторов, назначенный для изучения этого курса). И `Person` сущность имеет свойство навигации с именем `Courses` , может содержать ноль, один или несколько связанных `Course` сущностей (представляющий курсов, инструктора назначается обучение). Один инструктора может обучения нескольких курсах, и один может обучения по нескольким инструкторов. В этом разделе пошагового руководства будет добавлять и удалять связи между `Person` и `Course` сущности путем обновления свойств навигации связанных сущностей.

Создать новый веб-страницу с именем *InstructorsCourses.aspx* , использующий *Site.Master* главную страницу и добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Эта разметка создает `EntityDataSource` управления, который получает имя и `PersonID` из `Person` сущностей для инструкторов. Объект `DropDrownList` элемент управления привязан к `EntityDataSource` элемента управления. `DropDownList` Управления задает обработчик для `DataBound` события. Вы будете использовать этот обработчик в databind два раскрывающихся списков, отображающие курсов.

Разметка также создает следующую группу элементов управления, используемых для присвоения курса выбранных инструктора:

- Объект `DropDownList` управления для выбора курса для назначения. Этот элемент управления будет заполняться курсов, которые в настоящее время не назначены выбранной инструктора.
- Объект `Button` управления для инициирования назначения.
- Объект `Label` элемента управления, чтобы отобразить сообщение об ошибке, если произойдет сбой назначения.

Наконец разметку также создает группу элементов управления, используемых для удаления из выбранной инструктора курса.

В *InstructorsCourses.aspx.cs*, добавить с помощью инструкции:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Добавьте метод для заполнения два раскрывающихся списков, отображающие курсы:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Этот код получает все курсами `Courses` сущности набор и возвращает курсы из `Courses` свойство навигации `Person` сущность для выбранного инструктора. Он определяет, какие курсы назначены этой инструктора и заполняет раскрывающиеся списки соответствующим образом.

Добавление обработчика для `Assign` кнопки `Click` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Этот код получает `Person` получает сущность для выбранного инструктора `Course` сущность для выбранного курса и добавляет выбранного курса для `Courses` свойство навигации инструктора `Person` сущности. Он сохраняет изменения в базу данных и повторно заполняет раскрывающиеся списки, можно сразу же увидеть результаты.

Добавление обработчика для `Remove` кнопки `Click` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Этот код получает `Person` получает сущность для выбранного инструктора `Course` сущность для выбранного курса и удаляет из выбранного курса `Person` сущности `Courses` свойство навигации. Он сохраняет изменения в базу данных и повторно заполняет раскрывающиеся списки, можно сразу же увидеть результаты.

Добавьте код для `Page_Load` метод, который гарантирует, что сообщения об ошибках не отображаются ошибки, нет отчетов, и добавьте обработчики для `DataBound` и `SelectedIndexChanged` события инструкторов раскрывающегося списка для заполнения курсы раскрывающиеся списки:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Запустите страницу.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Выберите инструктор. **Назначить курса** раскрывающемся списке отображаются курсах, инструктора не предназначено для обучения, и **удалить курс** курсов, которым уже назначен инструктора отображает раскрывающегося списка. В **назначить курса** выберите курс и нажмите кнопку **назначить**. Перемещает курса **удалить курс** раскрывающегося списка. Выберите курс в **удалить курс** и нажмите кнопку **Remove ***.* Перемещает курса **назначить курса** раскрывающегося списка.

Теперь вы знаете, некоторые дополнительные способы работы со связанными данными. В этом руководстве вы узнаете, как использовать наследование в модели данных для повышения удобства поддержки приложения.

>[!div class="step-by-step"]
[Назад](the-entity-framework-and-aspnet-getting-started-part-4.md)
[Вперед](the-entity-framework-and-aspnet-getting-started-part-6.md)
