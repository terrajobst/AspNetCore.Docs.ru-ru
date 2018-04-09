---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4, веб-формы - часть 4 | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, использующий Entity Framework. Это образец приложения...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6bea5f4faeb0a9c11a406a7e4e87c4929eda6a18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4 Web Forms — часть 4
====================
по [Tom Dykstra](https://github.com/tdykstra)

> Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010. Сведения о учебника серии см [в первом учебнике ряда](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Работа со связанными данными

В предыдущем учебнике используется `EntityDataSource` управления для фильтрации, сортировки и группировки данных. В этом учебнике будет отображать и обновлять связанные данные.

Вы создадите инструкторов страницы со списком инструкторов. При выборе инструктор появиться список при изучении этого инструктора курсов. При выборе курса будут отображаться сведения курса и список учащихся, зарегистрированных в этом курсе. Можно изменить имя инструктора, дата приема на работу и назначение office. Назначение office — это набор отдельную сущность, доступ через свойство навигации.

Подробные данные в разметке или в коде, можно связать основных данных. В этой части руководства мы используем оба метода.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Отображение и обновление связанных сущностей в элементе управления GridView

Создать новый веб-страницу с именем *Instructors.aspx* , использующий *Site.Master* главную страницу и добавьте следующую разметку, чтобы `Content` управления с именем `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Эта разметка создает `EntityDataSource` элемента управления, который выбирает инструкторов и включает обновления. `div` Элемент настраивает разметки для подготовки к просмотру в левой части экрана, чтобы столбец справа можно добавить позднее.

Между `EntityDataSource` разметки и закрывающей `</div>` , добавьте следующую разметку, которая создает `GridView` управления и `Label` элемента управления, который будет использоваться для сообщения об ошибках:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Это `GridView` управления включает выбор строк, выделяет выбранную строку с светло-серым фоном и определяет обработчики (которые вы создадите в более поздней версии) для `SelectedIndexChanged` и `Updating` события. Кроме того, задается `PersonID` для `DataKeyNames` свойства, чтобы значение ключа выбранной строки могут передаваться на другой элемент управления, которую вы впоследствии добавите.

Последний столбец содержит назначение инструктора office, которое хранится в свойстве навигации `Person` сущности, так как он поступает из связанной сущности. Обратите внимание, что `EditItemTemplate` элемент указывает `Eval` вместо `Bind`, так как `GridView` управления не удается выполнить привязку непосредственно к свойствам навигации для их обновления. Мы обновим office назначения в коде. Чтобы сделать это, вам потребуется ссылку на `TextBox` управления и будет получить и сохранить, в `TextBox` элемента управления `Init` событий.

После `GridView` управления `Label` управления, используемого для сообщений об ошибках. Элемент управления `Visible` свойство `false`, и состояние просмотра отключено, чтобы метка будет отображаться только если код не сделает ее видимой в ответ на ошибку.

Откройте *Instructors.aspx.cs* файл и добавьте следующее `using` инструкции:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Добавьте поле закрытый класс сразу после объявления имя разделяемого класса для хранения ссылки на текстовое поле назначения office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Добавление заглушки для `SelectedIndexChanged` обработчик событий, необходимо добавить код для использования в дальнейшем. Также добавьте обработчик для назначения office `TextBox` элемента управления `Init` событий, чтобы можно было сохранить ссылку `TextBox` элемента управления. Эта ссылка будет использоваться для получения значения, введенные для обновления сущности, связанной со свойством навигации пользователем.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Вы воспользуетесь `GridView` элемента управления `Updating` событий для обновления `Location` свойства связанного `OfficeAssignment` сущности. Добавьте следующий обработчик `Updating` событий:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Этот код выполняется, когда пользователь щелкает **обновление** в `GridView` строк. Код использует LINQ to Entities для получения `OfficeAssignment` сущности, связанной с текущим `Person` сущности, с помощью `PersonID` выбранной строки из аргумента события.

Затем код принимает одно из следующих действий в зависимости от значения в `InstructorOfficeTextBox` управления:

- Если текстовое поле имеет значение, а не `OfficeAssignment` сущности для обновления, он создает ее.
- Если текстовое поле имеет значение, а `OfficeAssignment` сущности, он обновляет `Location` значение свойства.
- Если текстовое поле будет пустым и исключение `OfficeAssignment` сущность существует, он удаляет сущность.

После этого он сохраняет изменения в базу данных. При возникновении исключения, он отображает сообщение об ошибке.

Запустите страницу.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Нажмите кнопку **изменить** и измените все поля для текстовых полей.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Изменить какие-либо из этих значений, включая **Office назначения**. Нажмите кнопку **обновление** и вы увидите изменения отражаются в списке.

## <a name="displaying-related-entities-in-a-separate-control"></a>Отображение связанных сущностей в отдельном элементе управления

Каждый инструктора можно обучить один или несколько курсов, поэтому мы добавим `EntityDataSource` управления и `GridView` управления Список курсов, связанный с любой инструктора выбран инструкторов `GridView` элемента управления. Чтобы создать заголовок и `EntityDataSource` управления для сущностей курсов, добавьте следующую разметку между сообщение об ошибке `Label` управления и закрытие `</div>` тег:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

`Where` Параметр содержит значение `PersonID` из которого строка выбирается в инструктора `InstructorsGridView` элемента управления. `Where` Свойство содержит подзапроса выборки команду, которая возвращает все связанные `Person` сущностей из `Course` сущности `People` свойства навигации и выбирает `Course` сущность, только если один из связанных `Person`сущности содержит выбранного `PersonID` значение.

Для создания `GridView` управления. Добавьте следующую разметку, следующий сразу за `CoursesEntityDataSource` управления (перед закрывающим тегом `</div>` тега):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Так как не курсы будет отображаться, если нет инструктора выбран, `EmptyDataTemplate` элемент включен.

Запустите страницу.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Выберите инструктор, имеющий один или несколько курсов, которые назначены и курса или курсы появляются в списке. (Обратите внимание: несмотря на то, что схема базы данных допускает несколько курсов, тестовых данных, поставляемых вместе с базы данных не инструктора фактически имеет более одного курса. Можно добавить курсы в базу данных вручную с помощью **обозревателя серверов** окна или *CoursesAdd.aspx* страницу, которая будет добавлен в этом руководстве.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

`CoursesGridView` Элемент управления отображает несколько полей курса. Для отображения всех сведений для курса, потребуется использовать `DetailsView` управления курса, выбранного пользователем. В *Instructors.aspx*, добавьте следующую разметку после закрывающего `</div>` тег (Убедитесь, что поместить Эта разметка **после** div закрывающий тег не ранее):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Эта разметка создает `EntityDataSource` управления, к которому привязан `Courses` набора сущностей. `Where` Свойство выбирает курсе с помощью `CourseID` значение выбранной строки в курсы `GridView` элемента управления. Разметка определяет обработчик `Selected` события, которая позднее будет использоваться для отображения оценок учащихся, то есть еще один уровень ниже в иерархии.

В *Instructors.aspx.cs*, создайте следующие заглушки для `CourseDetailsEntityDataSource_Selected` метод. (Вы будете заполнить заглушка позже в данном руководстве, на данном этапе необходимо, чтобы страница будет скомпилировать и запустить.)

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Запустите страницу.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Изначально нет сведений курс, так как нет курса выбран. Выберите инструктор, имеющего назначенный курса, а затем выберите курс, чтобы просмотреть сведения.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>С помощью управления EntityDataSource «выбран» события для отображения связанных данных

Наконец, вам необходимо для отображения всех зарегистрированных студентов и их оценки выбранного курса. Для этого потребуется использовать `Selected` событие `EntityDataSource` элемент управления привязан к мере `DetailsView`.

В *Instructors.aspx*, добавьте следующую разметку после `DetailsView` управления:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Эта разметка создает `ListView` элемент управления, отображающий список студентов и их оценки выбранного курса. Источник данных не указывается, так как вы будете databind элемент управления в коде. `EmptyDataTemplate` Элемент предоставляет сообщение, отображаемое при выборе не курса — в этом случае существуют не учащихся для отображения. `LayoutTemplate` Создает таблицу HTML, чтобы отобразить список, а `ItemTemplate` указывает столбцы для отображения. Идентификатор студента и уровень студентов взяты из `StudentGrade` сущности, а также имя студента — от `Person` сущность, которая обеспечивает Entity Framework в `Person` свойство навигации `StudentGrade` сущности.

В *Instructors.aspx.cs*, замените усеченные `CourseDetailsEntityDataSource_Selected` метод следующим кодом:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

Аргумент события для этого события содержит выбранные данные в виде коллекции, которая будет содержать нуль элементов, если ничего не выбрано или один элемент Если `Course` выбрана сущность. Если `Course` выбрать сущность, код использует `First` способов преобразования объекта в коллекцию. Затем он возвращает `StudentGrade` сущностей из свойства навигации, преобразует их в коллекцию и привязывает `GradesListView` элемента управления в коллекцию.

Этого достаточно для отображения уровней, но вы хотели бы убедиться, что сообщение в пустом шаблоне данных отображается при первом отображении страницы, и каждый раз, когда не выбран курса. Чтобы сделать это, создайте следующий метод, который будет вызван из двух мест:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Этот новый метод из `Page_Load` метод для отображения времени шаблона первый пустые данные, эта страница отображается. И вызов его из `InstructorsGridView_SelectedIndexChanged` метод так, как это событие создается при выборе инструктор, означающее новых курсов, загружаются в курсы `GridView` элемента управления и ни одна не выбрано. Ниже приведены два вызова.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Запустите страницу.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Выберите, имеет назначенный курса инструктора, а затем выберите курса.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Теперь вы знаете несколько способов работы со связанными данными. В этом руководстве вы узнаете, как добавление связей между существующими сущностями, как удалить связи и как добавить новую сущность, имеющий отношение к существующей сущности.

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-5.md)
