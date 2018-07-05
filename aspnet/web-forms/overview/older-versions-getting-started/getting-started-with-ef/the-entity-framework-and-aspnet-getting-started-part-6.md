---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 6 | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложений веб-форм ASP.NET, с помощью Entity Framework. Пример приложения является...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 99861197e00a3f2f6811ef13136fac63b993ef32
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372110"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 6
====================
по [том Дайкстра](https://github.com/tdykstra)

> Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010. Сведения об этой серии руководств см. в разделе [в первом учебнике серии](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Реализация наследования типа «одна таблица на иерархию»

В предыдущем учебном курсе вы работали с связанных данных путем добавления и удаления связей, а также посредством добавления объекта, имеющего отношение к существующей сущности. В этом учебнике демонстрируется, как реализовать наследование в модели данных.

В объектно ориентированного программирования, можно использовать наследование для упрощения работы с связанных классов. Например, можно создать `Instructor` и `Student` классы, производные от `Person` базового класса. Те же виды структуры наследования между сущностями можно создать в Entity Framework.

В этой части руководства не создаст новый веб-страницы. Вместо этого вы добавите производных сущности в модель данных и изменение существующих страниц для использования новых сущностей.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Таблица на иерархию и таблица на тип наследования

Базу данных можно хранить сведения о связанных объектах, в одной таблице или в нескольких таблицах. Например, в `School` базы данных, `Person` таблица содержит сведения об учащихся и преподавателей в одной таблице. Некоторые из столбцов применяются только к преподавателям (`HireDate`), некоторые только к учащимся (`EnrollmentDate`) и некоторые к обоим (`LastName`, `FirstName`).

[![Image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Можно настроить Entity Framework для создания `Instructor` и `Student` сущности, которые наследуют от `Person` сущности. Такая модель создания структуры наследования сущностей из одной таблицы базы данных называется *таблица на иерархию* наследования (TPH).

Для курсов `School` база данных использует другой шаблон. Онлайн-курсы и очные курсы, хранятся в отдельных таблицах, каждый из которых имеет внешний ключ, который указывает на `Course` таблицы. Сведения, общие для обоих типов курсов хранятся только в `Course` таблицы.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Настройка модели данных Entity Framework, чтобы `OnlineCourse` и `OnsiteCourse` сущности являются производными от `Course` сущности. Такая модель формирования структуры наследования сущностей из отдельных таблиц для каждого типа, с каждой отдельной таблице, Возвращаясь к таблице, в которой хранятся данные, общие для всех типов называется *одна таблица на тип* наследование (TPT).

TPH наследования как правило, обеспечивают более высокую производительность на платформе Entity Framework, чем шаблоны наследования TPT, так как шаблоны TPT может привести к сложные запросы на соединение. В этом пошаговом руководстве демонстрируется реализация МОДЕЛИ наследования. Это можно сделать, выполнив следующие действия:

- Создание `Instructor` и `Student` типы сущностей, которые являются производными от `Person`.
- Перемещение свойства, которые относятся к производные сущности из `Person` сущность производные сущности.
- Задать ограничения для свойств в производных типах.
- Сделать `Person` сущности абстрактной сущности.
- Карты каждого производные сущности для `Person` таблице при условии, что указывает, как определить ли `Person` строк представляет, производный тип.

## <a name="adding-instructor-and-student-entities"></a>Добавление Instructor и Student сущностей

Откройте <em>SchoolModel.edmx</em> файл, щелкните правой кнопкой мыши незанятом область в конструкторе выберите <strong>добавить</strong>, а затем выберите <strong>сущности</strong><em>.</em>

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

В **Добавление сущности** диалоговом окне имя сущности `Instructor` и задайте его **базовый тип** равным `Person`.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Нажмите кнопку **ОК**. Конструктор создает `Instructor` сущности, производный от `Person` сущности. Новая сущность еще не содержит какие-либо свойства.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Повторите процедуру для создания `Student` сущность, которая также является производным от `Person`.

Только для преподавателей имеют дат приема на работу, поэтому необходимо переместить это свойство из `Person` сущность `Instructor` сущности. В `Person` сущности, щелкните правой кнопкой мыши `HireDate` свойство и нажмите кнопку **Вырезать**. Щелкните правой кнопкой мыши **свойства** в `Instructor` сущности и нажмите кнопку **вставить**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

Даты найма `Instructor` сущность не может иметь значение null. Щелкните правой кнопкой мыши `HireDate` свойство, нажмите кнопку **свойства**, а затем в **свойства** окна Изменение `Nullable` для `False`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Повторите эту процедуру, чтобы переместить `EnrollmentDate` свойства из `Person` сущность `Student` сущности. Убедитесь, что вы также `Nullable` для `False` для `EnrollmentDate` свойство.

Теперь, когда `Person` сущность имеет только свойства, которые являются общими для `Instructor` и `Student` сущности (кроме свойств навигации, которые вы не перемещаете), сущность может использоваться только как базовая сущность в структуру наследования. Таким образом необходимо убедиться, что он никогда не обрабатываются как независимые сущности. Щелкните правой кнопкой мыши `Person` сущности, выберите **свойства**, а затем в **свойства** окне измените значение свойства **абстрактный** свойства  **Значение true,**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Сопоставление Instructor и Student сущностей в таблицу Person

Теперь вам необходимо сообщить Entity Framework, каким образом для различения `Instructor` и `Student` сущности в базе данных.

Щелкните правой кнопкой мыши `Instructor` сущности и выберите **сопоставление таблицы**. В **сведения о сопоставлении** окно, нажмите кнопку **добавить таблицу или представление** и выберите **Person**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Нажмите кнопку **добавить условие**, а затем выберите **HireDate**.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Изменение **оператор** для **—** и **значение / свойство** для **Not Null**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Повторите эту процедуру для `Students` сущности, указав, что эта сущность сопоставляется `Person` таблицу при `EnrollmentDate` столбца не равно null. Затем сохраните и закройте модели данных.

Выполните сборку проекта для создания новых сущностей как классы и сделать их доступными в конструкторе.

## <a name="using-the-instructor-and-student-entities"></a>С помощью сущностей Student и Instructor

При создании веб-страниц, работающих с данными student и instructor, вы с привязкой к данным, их `Person` набора сущностей и отфильтрованные по `HireDate` или `EnrollmentDate` ограничивает возвращаемые данные для студентов и преподавателей. Тем не менее, теперь при привязке каждый элемент управления источника данных в `Person` набора сущностей, можно указать, что только `Student` или `Instructor` типы сущностей должен быть выбран. Так как для различения учащихся и преподавателей в Entity Framework `Person` набор сущностей, можно удалить `Where` параметры свойств, введенные вручную для этого.

В конструкторе Visual Studio, можно указать тип сущности, `EntityDataSource` управления следует выбрать в **EntityTypeFilter** поле раскрывающегося списка `Configure Data Source` мастера, как показано в следующем примере.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

И в **свойства** окно, можно удалить `Where` предложение значения, которые больше не нужны, как показано в следующем примере.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Тем не менее так как вы изменили разметка для `EntityDataSource` элементов управления, используемых `ContextTypeName` атрибут, не удается запустить **Настройка источника данных** мастер на `EntityDataSource` элементов управления, которые вы уже создали. Таким образом путем изменения разметки вместо этого будет внесите необходимые изменения.

Откройте *Students.aspx* страницы. В `StudentsEntityDataSource` управления, удалите `Where` атрибут и добавьте `EntityTypeFilter="Student"` атрибута. Разметка теперь будет выглядеть примерно так:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Установка `EntityTypeFilter` атрибут гарантирует, что `EntityDataSource` элемент управления будет выбирать только указанного типа сущности. Если вы хотите получить оба `Student` и `Instructor` типов сущностей, вы не устанавливал бы значение этого атрибута. (Вы можете получить несколько типов сущностей с одним `EntityDataSource` управление только в том случае, если вы используете элемент управления для доступа к данным только для чтения. Если вы используете `EntityDataSource` управления для вставки, обновления или удаления сущностей и если набор сущностей, они привязываются к может содержать несколько типов, можно работать только с одного типа сущности, и вам нужно присвоить этому атрибуту.)

Повторите эту процедуру для `SearchEntityDataSource` управления, за исключением того, удалите только часть `Where` атрибут, который выбирает `Student` объектов, а не полностью удалить свойство. Открывающий тег элемента управления, теперь будет выглядеть примерно так:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Запустите страницу, чтобы убедиться, что он по-прежнему работает как и раньше.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Обновите следующие страницы, созданные в предыдущих учебных курсах, чтобы они использовали новый `Student` и `Instructor` объекты, а не `Person` сущностей, затем выполните их, чтобы убедиться, что они работают как раньше:

- В *StudentsAdd.aspx*, добавьте `EntityTypeFilter="Student"` для `StudentsEntityDataSource` элемента управления. Разметка теперь будет выглядеть примерно так: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![Image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- В *About.aspx*, добавьте `EntityTypeFilter="Student"` для `StudentStatisticsEntityDataSource` управления и устранения `Where="it.EnrollmentDate is not null"`. Разметка теперь будет выглядеть примерно так: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![Image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- В *Instructors.aspx* и *InstructorsCourses.aspx*, добавьте `EntityTypeFilter="Instructor"` для `InstructorsEntityDataSource` управления и устранения `Where="it.HireDate is not null"`. Разметка в *Instructors.aspx* теперь аналогичный приведенному ниже: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![Image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Разметка в *InstructorsCourses.aspx* теперь будет выглядеть примерно так:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![Image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

В результате этих изменений улучшили удобство поддержки приложения университета Contoso несколькими способами. После перемещения Выбор и проверка логики за пределы уровня пользовательского интерфейса (*.aspx* разметки) и сделали неотъемлемой частью уровня доступа к данным. Это позволяет изолировать код приложения от изменений, которые можно вносить в будущем в схему базы данных или модели данных. Например можно, что учащиеся могут быть приняты как вспомогательные средства преподавателей и получит Дата приема на работу. Затем можно добавить новое свойство для различения учащихся, преподавателей и обновление модели данных. Код в веб-приложение не потребуется изменять за исключением случаев, где вы хотите показывать дату приема на работу для учащихся. Еще одним преимуществом добавления `Instructor` и `Student` сущностей является, то, что ваш код легче понятным, чем когда ссылка на `Person` объекты, которые были фактически учащихся и преподавателей.

Теперь вы увидели один из способов реализации дочерние в Entity Framework. В следующем руководстве вы узнаете, как использовать хранимые процедуры, чтобы более точно управлять как Entity Framework получает доступ к базе данных.

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-7.md)
