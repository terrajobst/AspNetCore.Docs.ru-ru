---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4, веб-формы - часть 6 | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, использующий Entity Framework. Это образец приложения...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: b76be25501275ba676c9a9acca8e73333439ee70
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Приступая к работе с базой данных Entity Framework 4.0 сначала и форм ASP.NET 4 веб - часть 6
====================
по [Tom Dykstra](https://github.com/tdykstra)

> Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010. Сведения о учебника серии см [в первом учебнике ряда](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Реализация наследования типа «одна таблица на иерархию»

В предыдущем учебнике вы работали с взаимосвязанных данных путем добавления и удаления связей, а также посредством добавления новой сущности, имеющих отношение к существующей сущности. В этом учебнике демонстрируется, как реализовать наследование в модели данных.

В объектно ориентированного программирования, можно использовать наследование для упрощения работы с связанных классов. Например, можно создать `Instructor` и `Student` классы, производные от `Person` базового класса. Те же самые виды структуры наследования между сущностями можно создать на платформе Entity Framework.

В этой части руководства не будет создать новый веб-страницы. Вместо этого мы добавим производные сущности в модель данных и изменение существующих страниц для использования новых сущностей.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Таблица иерархию и таблица на тип наследования

Базы данных можно хранить сведения о связанных объектов в одной таблице или в нескольких таблицах. Например, в `School` базы данных, `Person` таблица содержит сведения о учащихся и инструкторов в одной таблице. Некоторые столбцы применяются только к инструкторов (`HireDate`), некоторые только для учащихся (`EnrollmentDate`) и некоторые к обоим (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Можно настроить Entity Framework для создания `Instructor` и `Student` сущности, которые наследуют от `Person` сущности. Эта модель создания структура наследования сущностей из одной таблицы базы данных называется *таблица на иерархию* наследование (TPH).

Для курсов `School` база данных использует другой шаблон. Курсов в сети и на предприятии курсы хранятся в отдельных таблицах, каждая из которых имеет внешний ключ, который указывает на `Course` таблицы. Сведения, общие для обоих типов курса хранятся только в `Course` таблицы.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Настройка модели данных Entity Framework, чтобы `OnlineCourse` и `OnsiteCourse` сущности являются производными от `Course` сущности. Эта модель создания структура наследования сущностей из отдельных таблиц для каждого типа, с каждой отдельной таблице, возвращаясь в таблицу, в которой хранятся данные, общие для всех типов называется *одна таблица на тип* наследования (TPT).

TPH схемы наследования обычно работы с большей производительностью в платформе Entity Framework, чем TPT схемы наследования, поскольку шаблоны TPT может привести к запросы сложного соединения. В этом пошаговом руководстве показано, как реализовать наследование TPH. Вам предстоит выполнить, выполнив следующие действия.

- Создание `Instructor` и `Student` типов сущностей, которые являются производными от `Person`.
- Перемещение свойств, которые относятся к сущностям, производного от `Person` сущность производные сущности.
- Задать ограничения для свойства в производных типах.
- Сделать `Person` сущности абстрактные сущности.
- Карты каждый производный сущность `Person` таблицы при условии, что указывает способ определения ли `Person` строка представляет производный тип.

## <a name="adding-instructor-and-student-entities"></a>Добавление инструктора и студента сущностей

Откройте <em>SchoolModel.edmx</em> файл, щелкните правой кнопкой мыши незанятое область в конструкторе выберите <strong>добавить</strong>, а затем выберите <strong>сущности</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

В **добавить сущность** диалоговом имя сущности `Instructor` и задайте его **базовый тип** для параметра `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Нажмите кнопку **ОК**. Конструктор создает `Instructor` сущность, которая является производным от `Person` сущности. Новая сущность еще не все свойства.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Повторите процедуру для создания `Student` сущность, которая также является производным от `Person`.

Только инструкторов имеют дат приема на работу, поэтому необходимо перенести это свойство из `Person` сущность `Instructor` сущности. В `Person` сущности, щелкните правой кнопкой мыши `HireDate` свойство и нажмите кнопку **Вырезать**. Щелкните правой кнопкой мыши **свойства** в `Instructor` сущности и нажмите кнопку **вставить**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Даты найма `Instructor` сущность не может иметь значение null. Щелкните правой кнопкой мыши `HireDate` свойства, нажмите кнопку **свойства**, а затем в **свойства** окно Изменение `Nullable` для `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Повторите процедуру, чтобы переместить `EnrollmentDate` свойство из `Person` сущность `Student` сущности. Убедитесь, что также задать `Nullable` для `False` для `EnrollmentDate` свойства.

Теперь, когда `Person` сущность имеет свойства, которые являются общими для `Instructor` и `Student` сущностей (Помимо свойств навигации, которые вы не перемещаете), сущность может использоваться только как базовая сущность в структуру наследования. Таким образом необходимо убедиться, что никогда не обрабатываются как независимая сущность. Щелкните правой кнопкой мыши `Person` сущности, выберите **свойства**, а затем в **свойства** окна измените значение **абстрактный** свойства  **Значение true,**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Сопоставление инструктора и студента сущности в таблице Person

Теперь необходимо сообщить платформе Entity Framework способ различения `Instructor` и `Student` сущности в базе данных.

Щелкните правой кнопкой мыши `Instructor` сущность и выберите **сопоставление таблиц**. В **сведения о сопоставлении** окно, нажмите кнопку **добавить таблицу или представление** и выберите **лицо**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Нажмите кнопку **добавить условие**, а затем выберите **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Изменение **оператор** для **—** и **значение и свойство** для **Not Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Повторите процедуру для `Students` сущности, указав, что соответствует этой сущности `Person` таблицу при `EnrollmentDate` столбец не имеет значение null. Затем сохраните и закройте модели данных.

Выполните построение проекта для создания новых сущностей, как классы и сделать их доступными в конструкторе.

## <a name="using-the-instructor-and-student-entities"></a>С помощью инструктора и студента сущностей

При создании веб-страниц, работающих с данными учащихся и инструкторов, вы с привязкой к данным их `Person` набора сущностей и фильтрации на `HireDate` или `EnrollmentDate` свойства, чтобы ограничить возвращаемые данные студентов или инструкторов. Однако теперь при привязке каждого элемента управления источника данных для `Person` набора сущностей, можно указать, что только `Student` или `Instructor` должен быть выбран типов сущностей. Поскольку Entity Framework знает, как для различения студентов и преподавателей в `Person` набора сущностей, можно удалить `Where` значения свойств, введенные вручную для этого.

В конструкторе Visual Studio можно указать тип сущности, который `EntityDataSource` управления следует выбрать в **EntityTypeFilter** поле со списком из `Configure Data Source` мастера, как показано в следующем примере.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

И в **свойства** окна можно удалить `Where` предложение значения, которые больше не нужны, как показано в следующем примере.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Тем не менее так, как вы изменили разметку для `EntityDataSource` элементы управления для использования `ContextTypeName` атрибут, не может выполнить **Настройка источника данных** мастер на `EntityDataSource` элементов управления, которые уже созданы. Таким образом изменив разметки будет вносить необходимые изменения.

Откройте *Students.aspx* страницы. В `StudentsEntityDataSource` управления, удалите `Where` атрибута и добавьте `EntityTypeFilter="Student"` атрибута. Разметка теперь будет выглядеть примерно так:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Установка `EntityTypeFilter` атрибут гарантирует, что `EntityDataSource` элемент управления будет выбирать только указанного типа сущности. Если вы хотите получить оба `Student` и `Instructor` типов сущностей, не надо устанавливать этот атрибут. (Вы можете получить несколько типов сущностей с одним `EntityDataSource` управление только в том случае, если элемент управления используется для доступа к данным только для чтения. Если вы используете `EntityDataSource` управления для вставки, обновления или удаления сущностей и, если он привязан к набору сущностей могут содержать несколько типов, могут работать только с одного типа сущности, и необходимо указать значение этого атрибута.)

Повторите процедуру для `SearchEntityDataSource` управления, за исключением извлечения только часть `Where` атрибут, который выбирает `Student` сущности, а не полностью удалить свойство. Открывающий тег элемента управления теперь будет выглядеть примерно так:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Запустите страницу, чтобы убедиться, что все работает, как раньше.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Обновите следующие страницы, созданные в предыдущих учебниках, чтобы они использовали новый `Student` и `Instructor` объекты, а не `Person` сущностей, выполните их, чтобы убедиться, что они работают, как раньше:

- В *StudentsAdd.aspx*, добавьте `EntityTypeFilter="Student"` для `StudentsEntityDataSource` элемента управления. Разметка теперь будет выглядеть примерно так: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- В *About.aspx*, добавьте `EntityTypeFilter="Student"` для `StudentStatisticsEntityDataSource` управления и устранения `Where="it.EnrollmentDate is not null"`. Разметка теперь будет выглядеть примерно так: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- В *Instructors.aspx* и *InstructorsCourses.aspx*, добавьте `EntityTypeFilter="Instructor"` для `InstructorsEntityDataSource` управления и устранения `Where="it.HireDate is not null"`. Разметка *Instructors.aspx* теперь приведенной в следующем примере: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Разметка *InstructorsCourses.aspx* теперь будет выглядеть примерно так:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

В результате этих изменений улучшили удобства поддержки приложения Contoso университета несколькими способами. Перемещено Выбор и проверка логики из уровня пользовательского интерфейса (*.aspx* разметки) и представляет собой неотъемлемой частью уровня доступа к данным. Это позволяет изолировать код приложения от изменений, которые можно вносить в будущем в схему базы данных или моделью данных. Например может решить, что студенты могут быть приняты как учителей помогает и таким образом, будет получена Дата приема на работу. Затем можно добавить новое свойство для различения учащихся, преподавателей и обновление модели данных. Никакого кода в веб-приложения необходимо изменить, за исключением того, где вы хотели Показать дату найма для учащихся. Еще одним преимуществом добавления `Instructor` и `Student` сущности — что код является более легко понятным, чем при ее называют `Person` объекты, которые были фактически студентов или инструкторов.

Теперь вы знаете, как реализовать шаблон наследования в платформе Entity Framework. В этом руководстве вы узнаете, как использовать хранимые процедуры для более строгий контроль над как Entity Framework получает доступ к базе данных.

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-7.md)
