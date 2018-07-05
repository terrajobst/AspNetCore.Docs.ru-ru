---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 5 | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложений веб-форм ASP.NET, с помощью Entity Framework. Пример приложения является...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 0d0ae8385af96cf5e5951ac85f5cf6ae3c201a4d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366680"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 5
====================
по [том Дайкстра](https://github.com/tdykstra)

> Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010. Сведения об этой серии руководств см. в разделе [в первом учебнике серии](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Работа со связанными данными, продолжение

В предыдущем руководстве, когда вы начали использовать `EntityDataSource` для работы со связанными данными управления. Отображения нескольких уровней иерархии и изменения данных в свойствах навигации. В этом учебнике будет продолжать работать со связанными данными, путем добавления и удаления связей, а также посредством добавления новой сущности, имеющее отношения к существующей сущности.

Вы создадите страницу, которая добавляет курсов, назначенных отделов. Подразделение уже существует, и при создании нового курса, в то же время будет установить связь между ним и отдела.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Вы также создадите страницы, которая работает с отношением «многие ко многим», назначив преподавателя для курса (Добавление отношения между двумя сущностями, выбранными) или удаление преподавателя из курса (удаление связи между двумя сущностями, которые Выберите). В базе данных, Добавление отношения между преподавателя и курса результатов в новой строке, добавляемый `CourseInstructor` таблицу ассоциации; удаление связи включает в себя удаление строки из `CourseInstructor` таблицу ассоциации. Тем не менее, это делается в Entity Framework, установив свойства навигации, не обращаясь к `CourseInstructor` таблицы явным образом.

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Добавление сущности со связью с существующей сущностью

Создание нового веб-страницы с именем *CoursesAdd.aspx* , использующий *Site.Master* главную страницу и добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Эта разметка создает `EntityDataSource` элемент управления, который выбирает курсы, позволяющем Вставка, и, указывающий обработчик для `Inserting` событий. Вы используете обработчик для обновления `Department` свойство навигации, при создании нового `Course` создания сущности.

Разметка также создает `DetailsView` элемент управления, используемый для добавления новых `Course` сущностей. Разметка использует граничных полей для `Course` свойства сущности. Необходимо ввести `CourseID` значение, так как это не является полем созданный системой идентификатор. Вместо этого он является номер курса, который должен быть указан вручную, при создании курса.

Используйте шаблон поля для `Department` свойство навигации, так как свойства навигации не может использоваться с `BoundField` элементов управления. Поля шаблона предоставляет стрелку раскрывающегося списка для выбора подразделения. Стрелку раскрывающегося списка привязан к `Departments` набор с помощью сущностей `Eval` вместо `Bind`, еще раз, так как нельзя напрямую привязать свойства навигации для их обновления. Укажите обработчик для `DropDownList` элемента управления `Init` событий это хранить ссылку на элемент управления для использования в коде, который обновляет `DepartmentID` внешний ключ.

В *CoursesAdd.aspx.cs* сразу после объявления разделяемого класса, добавьте поле класса для хранения ссылки на `DepartmentsDropDownList` управления:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Добавить обработчик для `DepartmentsDropDownList` элемента управления `Init` событий, чтобы можно было сохранить ссылку на элемент управления. Это позволяет получить значение, введенное пользователем и использовать его для обновления `DepartmentID` значение `Course` сущности.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Добавить обработчик для `DetailsView` элемента управления `Inserting` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Когда пользователь щелкает `Insert`, `Inserting` событие возникает перед вставкой новой записи. Получает код в обработчике `DepartmentID` из `DropDownList` управления и использует его для задания значения, который будет использоваться для `DepartmentID` свойство `Course` сущности.

Платформа Entity Framework позаботится о добавлении этот курс поможет вам `Courses` свойство навигации связанного `Department` сущности. Он также добавляет в отдел `Department` свойство навигации `Course` сущности.

Откройте страницу.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Введите идентификатор, заголовок, число кредитов и выбрать отдел, а затем нажмите кнопку **вставить**.

Запустите *Courses.aspx* страницы и выберите одному отделу, чтобы увидеть новый курс.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Работа со связями многие ко многим

Связь между `Courses` набор сущностей и `People` набор сущностей представляет связь многие ко многим. Объект `Course` сущность имеет навигационное свойство `People` , может содержать ноль, один или несколько связанных `Person` сущностей (представляющих преподавателей, назначенный на изучение курса). И `Person` сущность имеет навигационное свойство `Courses` , может содержать ноль, один или несколько связанных `Course` сущностей (представляющий курсы назначен обучение, преподаватель). Одного преподавателя может научить несколько курсов, и один курс могут вести несколько преподавателей. В этом разделе пошагового руководства вы будете добавлять и удалять связи между `Person` и `Course` сущности путем обновления связанных сущностей свойства навигации.

Создание нового веб-страницы с именем *InstructorsCourses.aspx* , использующий *Site.Master* главную страницу и добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Эта разметка создает `EntityDataSource` элемент управления, который извлекает имя и `PersonID` из `Person` сущностей для преподавателей. Объект `DropDrownList` привязан элемент управления `EntityDataSource` элемента управления. `DropDownList` Управления задает обработчик для `DataBound` событий. Вы будете использовать этот обработчик для databind двух раскрывающихся списков, содержащих курсы.

Разметка также создает следующую группу элементов управления, используемых для назначения курсов для выбранного преподавателя:

- Объект `DropDownList` управления для выбора курс, чтобы назначить. Этот элемент управления будет заполняться курсы, которые в настоящее время не назначены выбранного преподавателя.
- Объект `Button` управления для инициирования назначения.
- Объект `Label` управления для отображения сообщения об ошибке, если назначение завершается сбоем.

И, наконец разметка также создает группу элементов управления, используемых для удаления курс из выбранного преподавателя.

В *InstructorsCourses.aspx.cs*, добавьте с помощью инструкции:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Добавьте метод для заполнения двух раскрывающихся списков, содержащих курсы:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Этот код возвращает все курсы из `Courses` сущности набор и возвращает курсы из `Courses` свойство навигации `Person` сущности для выбранного преподавателя. Затем определяет назначаются этого преподавателя курсы и отобразятся соответствующие раскрывающиеся списки.

Добавить обработчик для `Assign` кнопки `Click` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Этот код получает `Person` возвращает сущности для выбранного преподавателя, `Course` сущность для выбранного курса и добавляет выбранного курса `Courses` свойство навигации instructor `Person` сущности. Затем сохраняет изменения в базу данных и повторно заполняет раскрывающиеся списки, поэтому можно немедленно увидеть результаты.

Добавить обработчик для `Remove` кнопки `Click` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Этот код получает `Person` возвращает сущности для выбранного преподавателя, `Course` сущность для выбранного курса и удаляет выбранного курса из `Person` сущности `Courses` свойство навигации. Затем сохраняет изменения в базу данных и повторно заполняет раскрывающиеся списки, поэтому можно немедленно увидеть результаты.

Добавьте код для `Page_Load` метод, который гарантирует, что сообщения об ошибках не отображаются, когда отсутствуют ошибки, отчет, и добавьте обработчики для `DataBound` и `SelectedIndexChanged` события преподавателей стрелку раскрывающегося списка для заполнения раскрывающихся списков курсы:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Откройте страницу.

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Выберите преподавателя. <strong>Назначить курс</strong> раскрывающемся списке отображаются курсы, лектор не преподавать, и <strong>удалите курс</strong> раскрывающемся списке отображаются курсы, которые уже назначен преподаватель. В <strong>назначить курс</strong> раздела, выберите курс и выберите команду <strong>назначить</strong>. Перемещает курса <strong>удалите курс</strong> стрелку раскрывающегося списка. Выберите курс, в <strong>удалите курс</strong> раздела и нажмите кнопку <strong>удалить</strong><em>.</em> Перемещает курса <strong>назначить курс</strong> стрелку раскрывающегося списка.

Теперь вы узнали некоторые дополнительные способы работы со связанными данными. В следующем учебнике вы узнаете, как использование наследования в модели данных для более удобного обслуживания приложения.

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-6.md)
