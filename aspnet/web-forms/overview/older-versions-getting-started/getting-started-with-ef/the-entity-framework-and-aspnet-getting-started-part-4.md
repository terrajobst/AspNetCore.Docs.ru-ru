---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 4 | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложений веб-форм ASP.NET, с помощью Entity Framework. Пример приложения является...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 9783e30fdabe17958690d178ef95616cb72a8164
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380332"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 4
====================
по [том Дайкстра](https://github.com/tdykstra)

> Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010. Сведения об этой серии руководств см. в разделе [в первом учебнике серии](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Работа со связанными данными

В предыдущем учебном курсе мы использовали `EntityDataSource` управления для фильтрации, сортировки и группирования данных. В этом руководстве вы отображения и обновление связанных данных.

Вы создадите страницу преподавателей, в которой отображается список преподавателей. При выборе преподавателя, появится список курсов, проводимых этого преподавателя. При выборе курса, отобразятся сведения для курса и список студентов, зачисленных на курс. Можно изменить имя преподавателя, дата приема на работу и назначение кабинета. Назначение кабинета — это набор отдельных сущностей, доступный через свойство навигации.

Подробные данные в разметке или в коде можно связать основных данных. В этой части руководства вы используете оба метода.

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Отображение и обновление связанных сущностей в элементе управления GridView

Создание нового веб-страницы с именем *Instructors.aspx* , использующий *Site.Master* главную страницу и добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Эта разметка создает `EntityDataSource` элемента управления, который выбирает преподавателей и позволяет выполнять обновления. `div` Элемент настраивает разметки для подготовки к просмотру в левой части, таким образом, можно добавить столбец справа позже.

Между `EntityDataSource` разметки и закрывающей `</div>` , добавьте следующую разметку, которая создает `GridView` управления и `Label` элемент управления, который будет использоваться для сообщения об ошибках:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Это `GridView` управления включает выбор строк, выделяет цветом светло-серый фон выделенной строки и определяет обработчики (которые будет создано позже) для `SelectedIndexChanged` и `Updating` события. Он также указывает `PersonID` для `DataKeyNames` свойство, таким образом, значение ключа выбранной строки можно передать в другой элемент управления, который вам предстоит добавить позже.

Последний столбец содержит преподавателю кабинет, который хранится в свойстве навигации `Person` сущности так, как он поступает из связанной сущности. Обратите внимание, что `EditItemTemplate` элемент указывает `Eval` вместо `Bind`, так как `GridView` управления не удается непосредственно привязать к свойствам навигации, чтобы обновить их. Мы обновим кабинет в коде. Чтобы сделать это, вам потребуется ссылка `TextBox` элемента управления и вы получите и сохраним в `TextBox` элемента управления `Init` событий.

Следуя `GridView` элемент управления является `Label` элемента управления, который используется для сообщения об ошибках. Элемент управления `Visible` свойство `false`, и состояние представления отключено, так, чтобы отображались метки будут только когда код делает его видимым в ответ на ошибку.

Откройте *Instructors.aspx.cs* файл и добавьте следующие `using` инструкции:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Добавьте поле закрытый класс сразу после объявления имя разделяемого класса для хранения ссылки на текстовое поле назначения office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Добавление заглушку для `SelectedIndexChanged` обработчик событий, что вы добавите код для использования в будущем. Также добавьте обработчик для назначения кабинета `TextBox` элемента управления `Init` событий, чтобы можно было хранить ссылку на `TextBox` элемента управления. Вы используете эту ссылку для получения значения, введенные для обновления сущности, связанной со свойством навигации пользователем.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Вы будете использовать `GridView` элемента управления `Updating` событием, обновляют `Location` связанного `OfficeAssignment` сущности. Добавьте следующий обработчик для `Updating` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Этот код выполняется в том случае, когда пользователь щелкает **обновление** в `GridView` строки. Код использует LINQ to Entities для получения `OfficeAssignment` сущности, связанной с текущим `Person` сущность, с помощью `PersonID` выбранной строки из аргумента события.

Код затем принимает одно из следующих действий в зависимости от значения в `InstructorOfficeTextBox` управления:

- Если текстовое поле имеет значение, а не `OfficeAssignment` сущности для обновления, он создает его.
- Если текстовое поле имеет значение, а `OfficeAssignment` сущности, он обновляет `Location` значение свойства.
- Если текстовое поле будет пустым и исключение `OfficeAssignment` сущность существует, она удаляет сущность.

После этого он сохраняет изменения в базу данных. При возникновении исключения, он отображает сообщение об ошибке.

Откройте страницу.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Нажмите кнопку **изменить** и изменить все поля для текстовых полей.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Изменить какие-либо из этих значений, включая **кабинет**. Нажмите кнопку **обновления** и вы увидите изменения отражаются в списке.

## <a name="displaying-related-entities-in-a-separate-control"></a>Отображение связанных сущностей в отдельном элементе управления

Преподаватель можно обучить одну или несколько курсов, поэтому мы добавим `EntityDataSource` управления и `GridView` элемента управления, чтобы получить список курсов, связанный с любой преподаватель выбран в преподавателей, которые `GridView` элемента управления. Чтобы создать заголовок и `EntityDataSource` управления для сущностей курсы, добавьте следующую разметку между сообщение об ошибке `Label` управления и закрытие `</div>` тега:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where` Параметр содержит значение `PersonID` преподавателя, строка выбрана в `InstructorsGridView` элемента управления. `Where` Свойство содержит подзапроса выборки команду, которая возвращает все связанные `Person` сущностей из `Course` сущности `People` свойство навигации и выбирает `Course` сущности, только если один из связанных `Person`сущности содержит выбранный `PersonID` значение.

Для создания `GridView` управления. Добавьте следующую разметку сразу после `CoursesEntityDataSource` управления (перед закрывающей скобкой `</div>` тега):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Так как никакие курсы не отображается, если не преподаватель выбран, `EmptyDataTemplate` элемент включен.

Откройте страницу.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Выберите контролировать назначаемые курсы преподавателя и курса или курсы отображаются в списке. (Обратите внимание: несмотря на то, что схема базы данных допускает несколько курсов, тестовых данных, предоставленные с базой данных без преподавателя фактически имеет более чем одного курса. Можно добавить курсы в базу данных вручную с помощью **обозревателя серверов** окна или *CoursesAdd.aspx* страницы, что вы добавите в следующем руководстве.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView` Элемент управления отображает только несколько полей курса. Чтобы отобразить все сведения для курса, вы используете `DetailsView` управления курса, выбранного пользователем. В *Instructors.aspx*, добавьте следующую разметку после закрывающего `</div>` тега (Убедитесь, что вы поместите эту разметку **после** div закрывающий тег не перед ним):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Эта разметка создает `EntityDataSource` элемента управления, привязанного к `Courses` набора сущностей. `Where` Выбирает курс с помощью `CourseID` значение выбранной строки в курсы `GridView` элемента управления. Разметка определяет обработчик для `Selected` событие, которое будет использоваться в дальнейшем для отображения оценок учащихся, который является еще один уровень ниже в иерархии.

В *Instructors.aspx.cs*, создайте следующие заглушки для `CourseDetailsEntityDataSource_Selected` метод. (Вы будете добавлять постепенно заглушка позже в этом руководстве; сейчас, вам необходимо, чтобы страницы будет скомпилировать и запустить).

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Откройте страницу.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Изначально нет сведений курс, так как не курс выбран. Выберите преподавателя контролировать назначен курс, а затем выберите курс, чтобы просмотреть подробные сведения.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>С помощью управления EntityDataSource «Selected» событий для отображения связанных данных

Наконец вы хотите отображать все зачисленных студентов и их оценок для выбранного курса. Чтобы сделать это, вы используете `Selected` событие `EntityDataSource` элемент управления привязан к курса `DetailsView`.

В *Instructors.aspx*, добавьте следующую разметку после `DetailsView` управления:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Эта разметка создает `ListView` элемент управления, который отображает список студентов и их оценки для выбранного курса. Источник данных не указывается, так как вы будете databind элемента управления, в коде. `EmptyDataTemplate` Элемент предоставляет сообщение, отображаемое при выборе не курса — в этом случае есть учащиеся, не для отображения. `LayoutTemplate` Элемент создает таблицу HTML для отображения списка и `ItemTemplate` указывает столбцы для отображения. Идентификатор студента и его оценку учащегося берутся из `StudentGrade` сущности, а также имя учащегося — от `Person` сущность, которая делает доступными в Entity Framework `Person` свойство навигации `StudentGrade` сущности.

В *Instructors.aspx.cs*, замените усеченные `CourseDetailsEntityDataSource_Selected` метод следующим кодом:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Аргумент события для этого события содержит выбранные данные в виде коллекции, который будет иметь нуль элементов, если ничего не выделено или один элемент Если `Course` выбрана сущность. Если `Course` выбрана сущность, в коде используется `First` метод для преобразования коллекции в один объект. Затем он получает `StudentGrade` сущностей из свойства навигации, преобразует их в коллекцию и привязывает `GradesListView` элемента управления в коллекцию.

Этого достаточно для отображения оценок, но вы хотели бы убедиться, что сообщение в шаблон с пустыми данными отображается при первом отображении страницы, и каждый раз, когда курс не выбран. Чтобы сделать это, создайте следующий метод, который необходимо вызвать из двух мест:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Этот новый метод из `Page_Load` метод для отображения времени шаблона первый пустые данные, эта страница отображается. И вызовите его из `InstructorsGridView_SelectedIndexChanged` метод так, как это событие создается при выборе преподавателя, что означает новые курсы будут загружены в курсы `GridView` элемента управления и ни одна не выбрано. Ниже приведены два вызова.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Откройте страницу.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Выберите преподавателя, имеет назначенный курс, а затем выберите курс.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Теперь вы видели несколько способов работы со связанными данными. В следующем учебнике, вы узнаете, как добавить связи между существующими сущностями как удалить связи и как добавить новую сущность, имеющий отношение к существующей сущности.

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-5.md)
