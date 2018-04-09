---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Создание более сложные модели данных для приложения ASP.NET MVC | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fd8bf6502b0dd261505a86a2ed86d4c3f42e8755
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application"></a>Создание более сложные модели данных для приложения ASP.NET MVC
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью Entity Framework 6 Code First и Visual Studio 2013. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


В предыдущих занятий вы работали с простая модель данных, созданный из трех сущностей. В этом учебнике вы добавите дополнительные сущности и связи и будет настроить модель данных, указав форматирование, проверки и правила сопоставления базы данных. Вы увидите два способа настройки модели данных: путем добавления атрибутов к классам сущностей и путем добавления кода в класс контекста базы данных.

По завершении работы классы сущностей сформируют готовую модель данных, приведенную на следующем рисунке:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Настройка модели данных с использованием атрибутов

В этом разделе вы узнаете, как настроить модель данных с помощью атрибутов, которые указывают правила форматирования, проверки и сопоставления базы данных. Затем в некоторых из следующих разделов вы создадите полный `School` модели данных, добавляя атрибуты к классам уже создана и создание новых классов для остальных типов сущностей в модели.

### <a name="the-datatype-attribute"></a>Атрибут типа данных

Сейчас для дат зачисления студентов учащихся все веб-страницы отображают время и дату, хотя для этого поля достаточно одной даты. Используя атрибуты заметок к данным, вы можете внести в код одно изменение, позволяющее исправить формат отображения в каждом представлении, где отображаются эти данные. Чтобы рассмотреть соответствующий пример, вы добавите атрибут в свойство `EnrollmentDate` класса `Student`.

В *Models\Student.cs*, добавьте `using` инструкции для `System.ComponentModel.DataAnnotations` пространства имен и добавьте `DataType` и `DisplayFormat` атрибуты `EnrollmentDate` свойства, как показано в следующем примере:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибут используется для указания типа данных, который является более точным определением, чем встроенный тип базы данных. В этом случае требуется отслеживать только дату, а не дату и время. [Перечисление DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) предоставляет для многих типов данных, таких как *даты, времени, PhoneNumber, валюты, EmailAddress* и многое другое. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении. Например `mailto:` связи могут создаваться для [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), и элемент выбора даты, которые могут быть предоставлены для [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) в браузерах, поддерживающих [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты выдает HTML 5 [от данных](http://ejohn.org/blog/html-5-data-attributes/) (произносится *тире данных*) атрибутов, которые можно понять браузеров HTML 5. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты не имеют каких-либо проверок.

`DataType.Date` не задает формат отображаемой даты. По умолчанию, поле данных отображается в соответствии с форматы по умолчанию на сервере [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

С помощью атрибута `DisplayFormat` можно явно указать формат даты:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode` Параметр указывает, что заданное форматирование должен также быть применяется, когда значение отображается в текстовом поле для редактирования. (Не имеет смысла, для некоторых полей, например, для значений валют, может потребоваться обозначение денежной единицы в текстовом поле для редактирования.)

Можно использовать [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут сам, но обычно имеет смысл использовать [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) также атрибут. `DataType` Передает атрибут *семантику* данных как отличие от Подготовка к просмотру его на экране и предоставляет следующие преимущества, которые вы не получаете с `DisplayFormat`:

- Поддержка функций HTML5 в браузере (отображение элемента управления календарем, соответствующего языковому стандарту символа валюты, ссылок электронной почты, проверки на стороне клиента и т. д.).
- По умолчанию браузер будет отображаться с использованием правильного формата на основе данных вашей [языкового стандарта](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибута можно включить MVC выбрать шаблон справа поля для отображения данных ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) использует шаблон строки). Дополнительные сведения см. в разделе Брэд Вилсон [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Хотя предназначено для MVC 2, в этой статье по-прежнему применяется к текущей версии ASP.NET MVC.)

Если вы используете `DataType` атрибут с полем даты, необходимо указать `DisplayFormat` атрибута также для того, чтобы убедиться в правильном отображении поля в браузерах Chrome. Дополнительные сведения см. в разделе [этот поток StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Дополнительные сведения об обработке другие форматы даты в MVC см. в [введение MVC 5: изучение изменить методы и изменить представление](../introduction/examining-the-edit-methods-and-edit-view.md) и поиска на странице &quot;интернационализации&quot;.

Снова запустить страницу индекса студентов и обратите внимание раз больше не отображаются для дат регистрации. Также будет иметь значение true для любого представления, который использует `Student` модели.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>Атрибут StringLengthAttribute

С помощью атрибутов также можно указать правила проверки данных и сообщения об ошибках проверки. [StringLength атрибута](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) задает максимальную длину в базе данных и предоставляет на стороне клиента и на стороне сервера проверки ASP.NET MVC. В этом атрибуте также можно указать минимальную длину строки, но это минимальное значение не влияет на схему базы данных.

Предположим, вы хотите сделать так, чтобы пользователи не вводили больше 50 символов для имени. Чтобы добавить это ограничение, добавьте [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибуты `LastName` и `FirstMidName` свойства, как показано в следующем примере:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибут не предотвратить ввода пробелы в имени пользователя. Можно использовать [регулярное выражение](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) атрибутов для применения ограничений входных данных. Например следующий код требуются первого символа в записываются прописными буквами и остальные символы преобразуются в алфавитном порядке.

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) атрибут предоставляет аналогичные функциональные возможности для [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибута, но не предоставляет клиентской проверки.

Запустите приложение и нажмите кнопку **учащихся** вкладки. Появиться следующая ошибка:

*Модель резервного контекст «SchoolContext» изменилось с момента создания базы данных. Рассмотрите возможность обновления базы данных с помощью Code First Migrations ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Модель базы данных изменилось в результате которого требует изменения в схеме базы данных и обнаружила, что платформа Entity Framework. Миграция будет использоваться для обновления схемы без потери данных, добавленные в базу данных с помощью пользовательского интерфейса. При изменении данных, которая была создана с `Seed` метод, который будет изменен обратно в исходное состояние из-за [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) метод, который вы используете в `Seed` метод. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) эквивалентен операцией «вставки-обновления» в терминологии связанных баз данных.)

Введите в консоли диспетчера пакетов (PMC) следующие команды:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration` Команда создает файл с именем  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Он содержит в методе `Up` код, который обновит базу данных в соответствии с текущей моделью данных. Команда `update-database` запустила этот код.

Отметка времени, добавляемый в начало имени файла миграции используется платформой Entity Framework для упорядочения для миграции. Можно создать несколько миграции перед выполнением `update-database` команду, а затем все миграций применяются в порядке, в котором они были созданы.

Запустите **создать** страницы и введите имя длиной более 50 символов. При нажатии кнопки **Create** (Создать) проверка на стороне клиента отображает сообщение об ошибке.

![Ошибка val стороны клиента](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Атрибут столбца

Вы также можете использовать атрибуты, чтобы управлять сопоставлением классов и свойств с базой данных. Предположим, что вы использовали имя `FirstMidName` для поля имени, так как это поле также может содержать отчество. Но вам нужно, чтобы столбец базы данных назывался `FirstName`, так как к этому имени привыкли пользователи, которые будут составлять нерегламентированные запросы к базе данных. Чтобы выполнить это сопоставление, можно использовать атрибут `Column`.

Атрибут `Column` указывает, что при создании базы данных столбец таблицы `Student`, сопоставляемый со свойством `FirstMidName`, будет называться `FirstName`. Другими словами, когда ваш код ссылается на `Student.FirstMidName`, данные будут браться из столбца `FirstName` таблицы `Student` или обновляться в нем. Если не указать имена столбцов, они являются присваивается то же имя, как и имя свойства.

В *Student.cs* файл, добавьте `using` инструкции для [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) и добавьте атрибут имени столбца для `FirstMidName` свойства, как показано в следующий выделенный код:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Добавление [атрибут столбца](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) изменения модели, резервное SchoolContext, поэтому он не будет соответствовать базе данных. Введите следующие команды в PMC, чтобы создать еще один процесс миграции:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

В **обозревателя серверов**откройте *студента* конструктора таблиц, дважды щелкнув *студента* таблицы.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Ниже приведен имя исходного столбца, в котором он находился перед применением первых двух миграции. В дополнение к имени столбца, изменение с `FirstMidName` для `FirstName`, два столбца были изменены из `MAX` длиной до 50 символов.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Вы также можете базы данных с помощью изменения сопоставления [Fluent API](https://msdn.microsoft.com/data/jj591617), как вы увидите далее в этом учебнике.

> [!NOTE]
> Если попытаться выполнить компиляцию до создания всех классов сущностей в следующих разделах, могут возникнуть ошибки компилятора.


## <a name="complete-changes-to-the-student-entity"></a>Внесите изменения в сущность Student

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

В *Models\Student.cs*, замените код, добавленный ранее следующий код. Изменения выделены.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>Обязательный атрибут

[Обязательный атрибут](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) делает обязательные поля имя свойства. `Required attribute` Не требуются для типов значений, например даты и времени, int, double и число с плавающей запятой. Типы значений нельзя задать значение null, поэтому по своей природе обрабатываются как обязательные поля. Можно удалить [обязательный атрибут](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) и замените его минимальную длину параметра `StringLength` атрибута:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>Атрибут отображения

Атрибут `Display` указывает, что заголовки для текстовых полей должны иметь вид "First Name" (Имя), "Last Name" (Фамилия), "Full Name" (Полное имя) и "Enrollment Date" (Дата зачисления) вместо имени свойства в каждом экземпляре (в котором не используется пробел для разделения слов).

### <a name="the-fullname-calculated-property"></a>Полное имя вычисляемого свойства

`FullName` — это вычисляемое свойство, которое возвращает значение, созданное путем объединения двух других свойств. Поэтому он имеет только `get` метод доступа и нет `FullName` столбец создается в базе данных.

## <a name="create-the-instructor-entity"></a>Создание сущности Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Создание *Models\Instructor.cs*, заменив шаблон код следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Обратите внимание, что некоторые свойства являются одинаковыми в сущностях `Student` и `Instructor`. В руководстве по [реализации наследования](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) далее в этой серии вы выполните рефакторинг данного кода, чтобы устранить избыточность.

Несколько атрибутов можно поместить на одной строке, можно также написать класс инструктора следующим образом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Курсы и свойства навигации OfficeAssignment

`Courses` и `OfficeAssignment` — это свойства навигации. Как было описано ранее, обычно они определяются как [виртуальных](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) , чтобы они могут воспользоваться преимуществами платформы Entity Framework, называемую [отложенную загрузку](https://msdn.microsoft.com/magazine/hh205756.aspx). Кроме того, если свойство навигации может содержать несколько сущностей, его тип должен реализовывать [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) интерфейса. Например [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) , но не определяет [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) из-за `IEnumerable<T>` не реализует [добавить ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Инструктор можно обучить любое количество курсов, поэтому `Courses` определяется как совокупность `Course` сущностей.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Наш бизнес-правила, состояния инструктор только может иметь не более одного офиса, так `OfficeAssignment` определяется как один `OfficeAssignment` сущности (которые могут быть `null` Если office не назначено).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Создать сущность OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Создание *Models\OfficeAssignment.cs* следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Постройте проект, сохраняющий изменения и подтверждает, что не было выполнено любой копии и вставьте ошибок, которое компилятор может перехватить.

### <a name="the-key-attribute"></a>Ключевой атрибут

Имеется отношение "один к нулю или одному" между `Instructor` и `OfficeAssignment` сущности. Office назначения существует только относительно инструктора, которому присвоено, и поэтому ее первичный ключ также является его внешний ключ к `Instructor` сущности. Платформа Entity Framework не удалось распознать автоматически, но `InstructorID` первичной ключа сущности, так как его имя не соответствует соглашениям `ID` или *classname* `ID` соглашение об именовании. Таким образом, атрибут `Key` используется для определения ее в качестве ключа:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Можно также использовать `Key` атрибута, если сущность имеет собственный первичный ключ, но требуется имя свойства, отличные от `classnameID` или `ID`. По умолчанию EF обрабатывает ключ как без формирования базы данных, так как столбец для идентифицирующего отношения.

### <a name="the-foreignkey-attribute"></a>Атрибут ForeignKey

При отсутствии отношением один к нулю или одному или однозначное соответствие между двумя сущностями (такие как между `OfficeAssignment` и `Instructor`), EF не может работать какому концу отношения является участником и какая является зависимым. Один к одному отношениями свойством навигации ссылки в каждом классе другому классу. [Атрибута ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) может применяться к классу зависимых для установления связи. Если не указан [атрибута ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), возникает следующая ошибка при попытке создать миграции:

*Не удалось определить основной конец ассоциации между типами «ContosoUniversity.Models.OfficeAssignment» и «ContosoUniversity.Models.Instructor». Основной конец ассоциации должен быть явно настроен с помощью заметок к данным или fluent API связи.*

Далее в этом учебнике вы увидите, как настроить эту связь с fluent API.

### <a name="the-instructor-navigation-property"></a>Свойство навигации инструктора

`Instructor` Сущность имеет значение NULL `OfficeAssignment` свойство навигации (поскольку инструктор не может иметь назначение office) и `OfficeAssignment` сущность имеет не допускающий `Instructor` свойство навигации (так как не может присваиваться office существует без инструктор-- `InstructorID` не допускает значение NULL). Когда `Instructor` сущность имеет связанный с ним `OfficeAssignment` сущностей, каждая сущность имеет ссылку на другую переменную его свойства навигации.

Можно поместить `[Required]` атрибут для свойства навигации инструктора, чтобы указать, должен быть связанные инструктора, что не нужно делать, так как не допускающий InstructorID внешний ключ (который также является ключом к этой таблице).

## <a name="modify-the-course-entity"></a>Изменение сущности Course

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

В *Models\Course.cs*, замените код, добавленный ранее следующий код:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Сущность курс имеет свойство внешнего ключа `DepartmentID` указывающая на связанный с ним `Department` сущности и он имеет `Department` свойства навигации. Платформа Entity Framework не требует добавлять свойство внешнего ключа в модель данных при наличии свойства навигации для связанной сущности. EF автоматически создает внешние ключи в базе данных, где они необходимы. Однако наличие внешнего ключа в модели данных позволяет сделать обновления проще и эффективнее. Например, когда выборки сущностью курса для редактирования, `Department` сущности имеет значение null, если он не загружается, поэтому при обновлении сущности курса пришлось бы получить `Department` сущности. При свойстве внешнего ключа `DepartmentID` включено в модель данных, нет необходимости получить `Department` сущности перед обновлением.

### <a name="the-databasegenerated-attribute"></a>Атрибут DatabaseGenerated

[Атрибута DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) с [нет](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) параметр на `CourseID` свойство указывает, что значения первичного ключа предоставленного пользователем, а не созданное базой данных.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

По умолчанию платформа Entity Framework предполагается генерировать значений первичного ключа в базе данных. Именно это и требуется для большинства сценариев. Однако для `Course` сущностей, будет использовать курса определяемый пользователем номер серии 1000 для одного подразделения ряд 2000 для разных отделов и т. д.

### <a name="foreign-key-and-navigation-properties"></a>Внешний ключ и свойств навигации

Свойства внешнего ключа и свойства навигации в `Course` сущности отражают следующие связи:

- Курс назначается одной кафедре, поэтому по указанным выше причинам имеется внешний ключ `DepartmentID` и свойство навигации `Department`. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- На курс может быть зачислено любое количество учащихся, поэтому свойство навигации `Enrollments` является коллекцией: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Курс могут вести несколько преподавателей, поэтому свойство навигации `Instructors` является коллекцией: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Создание сущности «отдел»

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Создание *Models\Department.cs* следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Атрибут столбца

Ранее вы использовали [атрибут столбца](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) изменять сопоставления имени столбца. В коде `Department` сущности, `Column` атрибут используется для изменения SQL сопоставления типов данных, чтобы столбец будет определяться с помощью SQL Server [money](https://msdn.microsoft.com/library/ms179882.aspx) типа в базе данных:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Сопоставление столбцов обычно не является обязательным, поскольку Entity Framework обычно выбирает соответствующий тип данных SQL Server, на основе типа CLR, определяемый для свойства. Тип `decimal` среды CLR сопоставляется с типом `decimal` SQL Server. Однако в этом случае известно, что столбец будет удерживать суммы в валюте и [money](https://msdn.microsoft.com/library/ms179882.aspx) больше подходит для этого типа данных. Дополнительные сведения о типах данных CLR и как они совпадают с типами данных SQL Server см. в разделе [SqlClient для Entity Framework](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Внешний ключ и свойств навигации

Свойства внешнего ключа и навигации отражают следующие связи:

- Кафедра может иметь или не иметь администратора, и администратор всегда является преподавателем. Поэтому `InstructorID` свойство включается в качестве внешнего ключа в `Instructor` сущности, а вопросительный знак добавляется после `int` введите обозначение, — отмечает свойство как допускающие значение NULL. Свойство навигации называется `Administrator` , но содержит `Instructor` сущности: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Отдел может иметь несколько курсов, так что `Courses` свойство навигации: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > По соглашению Entity Framework разрешает каскадное удаление для внешних ключей, не допускающих значение null, и связей многие ко многим. Это может привести к циклическим правилам каскадного удаления, которые вызывают исключение при попытке добавить миграцию. Например, если не определен `Department.InstructorID` свойство как допускающие значение NULL, получить следующее сообщение об исключении: «ссылочную связь приведет к циклической ссылки, что не разрешено.» При необходимости бизнес-правила `InstructorID` значение отличное от NULL, необходимо выполнить инструкцию fluent API для отключения каскадное удаление в отношении: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modify-the-enrollment-entity"></a>Изменение сущности регистрации

![Enrollment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

 В *Models\Enrollment.cs*, замените код, добавленный ранее следующий код

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Внешний ключ и свойств навигации

Свойства внешнего ключа и навигации отражают следующие связи:

- Запись зачисления предназначена для одного курса, поэтому доступно свойство первичного ключа `CourseID` и свойство навигации `Course`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Запись зачисления предназначена для одного учащегося, поэтому доступно свойство первичного ключа `StudentID` и свойство навигации `Student`: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Связи многие ко многим

Имеется отношение "многие ко многим" между `Student` и `Course` сущности и `Enrollment` сущности функционирует как многие ко многим Соединяемая таблица *с полезными данными* в базе данных. Это означает, что `Enrollment` таблица содержит дополнительные данные помимо внешние ключи для соединяемых таблиц (в данном случае первичный ключ и `Grade` свойство).

На следующем рисунке показано, как выглядят эти связи на схеме сущностей. (Эта схема была создана с помощью [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); Создание схемы не является частью учебника, он просто используется здесь как пример.)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Каждая линия связи имеет 1 на одном конце и звездочку (\*) в другое, указывающее, один ко многим.

Если `Enrollment` таблицы не были включены сведения об оценках, его потребуется только содержат два внешних ключа `CourseID` и `StudentID`. В этом случае он будет соответствовать многие ко многим Соединяемая таблица *без полезных данных* (или *таблицы присоединения*) в базе данных и создать класс модели для него вообще не требуется. `Instructor` И `Course` сущности, имеют этот тип связи "многие ко многим", и как можно видеть, между ними отсутствует класс сущности:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Соединяемая таблица является обязательным в базе данных, однако, как показано на следующей схеме базы данных:

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Платформа Entity Framework автоматически создает `CourseInstructor` таблицы и чтения и обновить ее косвенно, чтение и обновление `Instructor.Courses` и `Course.Instructors` свойства навигации.

## <a name="entity-diagram-showing-relationships"></a>Схема сущностей, показывающая связи

Ниже показана схема, создаваемая средствами Entity Framework Power Tools для завершенной модели School.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

Помимо строк многие ко-многим (\* для \*) и один ко многим связей (от 1 до \*), здесь можно увидеть на линию связи один к нулю или одному (1 к нулю или одному) между `Instructor` и `OfficeAssignment` сущности и линию связи нуль или один ко многим (от 0 до 1 для \*) между сущностями инструктора и отдел.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Настройка модели данных путем добавления кода в контексте базы данных

Далее добавим новые сущности для `SchoolContext` класса и настраивать некоторые сопоставления с помощью [fluent API](https://msdn.microsoft.com/data/jj591617) вызовов. API «fluent», так как он часто используется проводить ряда вызовов метода вместе в одной инструкции, как показано в следующем примере:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

В этом учебнике fluent API используется только для сопоставления с базой данных, нельзя выполнить с помощью атрибутов. Однако текучий API позволяет задать большинство правил форматирования, проверки и сопоставления, которые можно указать с помощью атрибутов. Некоторые атрибуты, такие как `MinimumLength`, невозможно применить с текучим API. Как упоминалось ранее, `MinimumLength` не изменяет схему, применяется правило проверки стороны клиента и сервера

Некоторые разработчики предпочитают использовать текучий API монопольно, чтобы оставить свои классы сущностей "чистыми". Атрибуты и текучий API можно смешивать, и существует несколько конфигураций, которые можно реализовать только с помощью текучего API. На практике рекомендуется выбрать один из этих двух подходов и использовать его максимально согласованно.

Чтобы добавить новые сущности для данных модели и выполнения сопоставления с базой данных, не были выполнены с помощью атрибутов, замените код в *DAL\SchoolContext.cs* следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Оператор new в [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) метод настраивает многие ко многим соединенной таблице:

- Для связи "многие ко многим" между `Instructor` и `Course` сущностей, код задает имена таблиц и столбцов для таблицы объединения. Код сначала можно настроить многие ко многим для вас без этого кода, но если вы не вызываете его, вы получите имена по умолчанию такие как `InstructorInstructorID` для `InstructorID` столбца.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Ниже приведен пример как вы мог использоваться fluent API вместо атрибутов для указания связь между `Instructor` и `OfficeAssignment` сущности:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Сведения о действиях инструкций «fluent API» в фоновом см [Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) записи блога.

## <a name="seed-the-database-with-test-data"></a>Заполнение базы данных тестовыми данными

Замените код в *Migrations\Configuration.cs* файла следующим кодом для предоставления данных начального значения для новых сущностей, которые вы создали.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Как видно в первом руководстве, большая часть кода просто обновляет или создает новые объекты сущности и загружает данные в свойства, необходимые для тестирования. Обратите внимание, как `Course` сущности, которая имеет отношение многие ко многим с `Instructor` обрабатывается сущности:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

При создании `Course` объекта инициализации `Instructors` свойство навигации как пустую коллекцию с помощью кода `Instructors = new List<Instructor>()`. Это позволяет добавить `Instructor` сущностей, относящихся к этому `Course` с помощью `Instructors.Add` метод. Если не удалось создать пустой список, невозможно добавить эти связи, так как `Instructors` свойство будет иметь значение null и не пришлось бы `Add` метод. Инициализация списка можно также добавить в конструктор.

## <a name="add-a-migration-and-update-the-database"></a>Добавьте миграции и обновления базы данных

PMC, введите следующую команду `add-migration` команду (не `update-database` еще command):

`add-Migration ComplexDataModel`

Если попытаться выполнить команду `update-database` на этом этапе (пока этого делать не нужно), возникнет следующая ошибка:

*Конфликт инструкции ALTER TABLE с ограничением FOREIGN KEY «FK\_dbo. Курс\_dbo. Отдел\_DepartmentID». Конфликт произошел в таблицу в базе данных «ContosoUniversity», «dbo. Отдела», столбец «DepartmentID».*

Иногда при выполнении миграции с существующими данными, необходимо вставить данные заглушки в базу данных для удовлетворения ограничения внешнего ключа, и это необходимо сделать сейчас. Код, созданный в ComplexDataModel `Up` метод добавляет не допускающий `DepartmentID` внешний ключ к `Course` таблицы. Так как уже существует строки в `Course` таблицы при выполнении кода, `AddColumn` операция завершится ошибкой, так как SQL Server не знает, какое значение в столбце, который не может иметь значение null. Поэтому нужно изменить код, чтобы предоставить значение по умолчанию для нового столбца и создания заглушки подразделение с именем «Temp» в качестве подразделения по умолчанию. В результате существующие `Course` строк будут все связаны с отделом «Temp» после `Up` метода. Их можно связать со правильный отделов `Seed` метод.

Изменить &lt; *timestamp&gt;\_ComplexDataModel.cs* закомментировать строку кода, который добавляет столбец DepartmentID в таблицу Course и добавьте следующий выделенный код (комментариями Строка также выделяется):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

При `Seed` выполняется метод, он будет вставлять строки в `Department` относится существующие таблицы и ее `Course` этих новых строк `Department` строк. Если вы не добавляли курсов в пользовательском Интерфейсе, затем больше не потребуется отдела «Temp» или значение по умолчанию на `Course.DepartmentID` столбца. Чтобы обеспечить возможность того, что кто-то добавили курсы с помощью приложения, также требуется обновить `Seed` код метода, чтобы убедиться, что все `Course` строк (не только те, которые вставляются при предыдущих запусках `Seed` метод) имеет Допустимые `DepartmentID` значения перед удалением по умолчанию значение из столбца и удалить отдел «Temp».

После завершения редактирования &lt; *timestamp&gt;\_ComplexDataModel.cs* , введите `update-database` в PMC для выполнения миграции.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Можно получить другие ошибки при переносе данных и внесения изменений схемы. Если вы получаете ошибки миграции, которые не удается устранить, измените имя базы данных в строке подключения или удалите базу данных. Самым простым подходом является переименовать базу данных, в *Web.config* файла. В следующем примере показано имя изменено на CU\_теста:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
> 
> С помощью новой базы данных нет данных для переноса и `update-database` команда гораздо больше шансов завершиться без ошибок. Инструкции по удалению базы данных см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
> 
> В случае неудачи другой, можно попробовать что это повторную инициализацию базы данных, введя следующую команду в PMC:
> 
> `update-database -TargetMigration:0`


Открыть базу данных в **обозревателя серверов** выполнял ранее, и разверните **таблиц** узел, чтобы увидеть, что все таблицы были созданы. (При наличии **обозревателя серверов** откройте из более ранних времени, щелкните **обновление** кнопки.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Не удалось создать класс модели для `CourseInstructor` таблицы. Как упоминалось ранее, это Соединяемая таблица для связи "многие ко многим" между `Instructor` и `Course` сущности.

Щелкните правой кнопкой мыши `CourseInstructor` таблицы и выберите **Показать таблицу данных** для убедитесь в наличии данных в нем в результате использования `Instructor` сущностей, которые вы добавили `Course.Instructors` свойство навигации.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="summary"></a>Сводка

Теперь у вас есть более сложная модель данных и соответствующая база данных. В этом руководстве вы узнаете о дополнительных сведений о различные способы доступа к взаимосвязанных данных.

Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить. Можно также запросить новые разделы на [показать мне как с код](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступа к данным ASP.NET - рекомендуется использовать ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
