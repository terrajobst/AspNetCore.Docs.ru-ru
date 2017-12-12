---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: "Создание более сложные модели данных для приложения ASP.NET MVC (4 из 10) | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 350c2e4e92c8a53d22dd2500330281b4003a05e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Создание более сложные модели данных для приложения ASP.NET MVC (4 из 10)
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о учебника серии см [в первом учебнике ряда](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы путем сравнения код для завершения кода. Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущих занятий вы работали с простая модель данных, созданный из трех сущностей. В этом учебнике вы добавите дополнительные сущности и связи и будет настроить модель данных, указав форматирование, проверки и правила сопоставления базы данных. Вы увидите два способа настройки модели данных: путем добавления атрибутов к классам сущностей и путем добавления кода в класс контекста базы данных.

После завершения классы сущностей составляют модель данных, завершенные, показан на следующем рисунке:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Настройка модели данных с помощью атрибутов

В этом разделе вы увидите, как настроить модель данных с помощью атрибутов, укажите параметры форматирования, проверки и правила сопоставления базы данных. Затем в некоторых из следующих разделов вы создадите полный `School` модели данных, добавляя атрибуты к классам уже создана и создание новых классов для остальных типов сущностей в модели.

### <a name="the-datatype-attribute"></a>Атрибут типа данных

Для дат регистрации студентов все веб-страниц в настоящее время отображения времени и даты, несмотря на то, что все, что важна для этого поля является датой. Используя атрибуты данных заметок, ее можно создать, позволяющим формат отображения в каждого представления, отображающего данные изменения кода. Пример этого процесса, что вы добавите атрибут `EnrollmentDate` свойства `Student` класса.

В *Models\Student.cs*, добавьте `using` инструкции для `System.ComponentModel.DataAnnotations` пространства имен и добавьте `DataType` и `DisplayFormat` атрибуты `EnrollmentDate` свойства, как показано в следующем примере:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

[DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибут используется для указания типа данных, который является более точным определением, чем встроенный тип базы данных. В этом случае нам нужен только для отслеживания даты, не даты и времени. [Перечисление DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) предоставляет для многих типов данных, таких как *даты, времени, PhoneNumber, валюты, EmailAddress* и многое другое. Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении. Например `mailto:` связи могут создаваться для [DataType.EmailAddress](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx), и элемент выбора даты, которые могут быть предоставлены для [DataType.Date](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) в браузерах, поддерживающих [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты выдает HTML 5 [от данных](http://ejohn.org/blog/html-5-data-attributes/) (произносится *тире данных*) атрибутов, которые можно понять браузеров HTML 5. [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибуты не имеют каких-либо проверок.

`DataType.Date` не задает формат отображаемой даты. По умолчанию, поле данных отображается в соответствии с форматы по умолчанию на сервере [CultureInfo](https://msdn.microsoft.com/en-us/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

С помощью атрибута `DisplayFormat` можно явно указать формат даты:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode` Параметр указывает, что заданное форматирование должен также быть применяется, когда значение отображается в текстовом поле для редактирования. (Не имеет смысла, для некоторых полей, например, для значений валют, может потребоваться обозначение денежной единицы в текстовом поле для редактирования.)

Можно использовать [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) атрибут сам, но обычно имеет смысл использовать [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) также атрибут. `DataType` Передает атрибут *семантику* данных как отличие от Подготовка к просмотру его на экране и предоставляет следующие преимущества, которые вы не получаете с `DisplayFormat`:

- Браузер может включить функции HTML5 (например для отображения элемента управления календаря, локализованными обозначение денежной единицы, ссылок по электронной почте, и т. д.).
- По умолчанию браузер будет отображаться с использованием правильного формата на основе данных вашей [языкового стандарта](https://msdn.microsoft.com/en-us/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) атрибута можно включить MVC выбрать шаблон справа поля для отображения данных ( [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Если используется сама использует шаблон строки). Дополнительные сведения см. в разделе Брэд Вилсон [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Хотя предназначено для MVC 2, в этой статье по-прежнему применяется к текущей версии ASP.NET MVC.)

Если вы используете `DataType` атрибут с полем даты, необходимо указать `DisplayFormat` атрибута также для того, чтобы убедиться в правильном отображении поля в браузерах Chrome. Дополнительные сведения см. в разделе [этот поток StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Снова запустить страницу индекса студентов и обратите внимание раз больше не отображаются для дат регистрации. Также будет иметь значение true для любого представления, который использует `Student` модели.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>Атрибут StringLengthAttribute

Также можно указать правила проверки данных и сообщения с помощью атрибутов. Предположим, что вы хотите убедиться, что пользователь не ввел более 50 символов для имени. Чтобы добавить это ограничение, добавьте [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибуты `LastName` и `FirstMidName` свойства, как показано в следующем примере:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибут не предотвратить ввода пробелы в имени пользователя. Можно использовать [регулярное выражение](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) атрибутов для применения ограничений входных данных. Например следующий код требуются первого символа в записываются прописными буквами и остальные символы преобразуются в алфавитном порядке.

`[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]`

[MaxLength](https://msdn.microsoft.com/en-us/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) атрибут предоставляет аналогичные функциональные возможности для [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) атрибута, но не предоставляет клиентской проверки.

Запустите приложение и нажмите кнопку **учащихся** вкладки. Появиться следующая ошибка:

*Модель резервного контекст «SchoolContext» изменилось с момента создания базы данных. Рассмотрите возможность обновления базы данных с помощью Code First Migrations ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Модель базы данных изменилось в результате которого требует изменения в схеме базы данных и обнаружила, что платформа Entity Framework. Миграция будет использоваться для обновления схемы без потери данных, добавленные в базу данных с помощью пользовательского интерфейса. При изменении данных, которая была создана с `Seed` метод, который будет изменен обратно в исходное состояние из-за [AddOrUpdate](https://msdn.microsoft.com/en-us/library/hh846520(v=vs.103).aspx) метод, который вы используете в `Seed` метод. ([AddOrUpdate](https://msdn.microsoft.com/en-us/library/hh846520(v=vs.103).aspx) эквивалентен операцией «вставки-обновления» в терминологии связанных баз данных.)

В пакет Диспетчер консоли (PMC), введите следующие команды:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration MaxLengthOnNames` Команда создает файл с именем  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Этот файл содержит код, который будет обновлять базу данных в соответствии с текущей моделью данных. Отметка времени, добавляемый в начало имени файла миграции используется платформой Entity Framework для упорядочения для миграции. После создания нескольких миграций и, если удалить базу данных или развертывании проекта с помощью миграций, все миграций применяются в порядке, в котором они были созданы.

Запустите **создать** страницы и введите имя длиной более 50 символов. Как только вы более 50 символов, клиентская проверка немедленно отобразится сообщение об ошибке.

![Ошибка val стороны клиента](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Атрибут столбца

Атрибуты используются для управления как классы и свойства сопоставляются в базу данных. Предположим, что вы использовали имя `FirstMidName` -имя поля, так как поле также может содержать отчество. Но необходимо сделать столбец базы данных могут называться `FirstName`, так как в это имя привыкшим пользователей, которые будет записывать нерегламентированные запросы к базе данных. Чтобы это сопоставление, можно использовать `Column` атрибута.

`Column` Атрибут указывает, что при создании базы данных в столбце `Student` таблицу, которая сопоставляет `FirstMidName` свойство с именем `FirstName`. Другими словами, когда ваш код ссылается `Student.FirstMidName`, данные получаются из или обновлены в `FirstName` столбец `Student` таблицы. Если не указать имена столбцов, они являются присваивается то же имя, как и имя свойства.

Добавить с помощью инструкции для [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.aspx) и атрибут имени столбца для `FirstMidName` свойства, как показано в следующий выделенный код:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Добавление [атрибут столбца](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) изменения модели, резервное SchoolContext, поэтому он не будет соответствовать базе данных. Введите следующие команды в PMC, чтобы создать еще один процесс миграции:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

В **обозревателя серверов** (**обозреватель баз данных** при использовании Express для веб-сайта), дважды щелкните *студента* таблицы.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Ниже приведен имя исходного столбца, в котором он находился перед применением первых двух миграции. В дополнение к имени столбца, изменение с `FirstMidName` для `FirstName`, два столбца были изменены из `MAX` длиной до 50 символов.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Вы также можете базы данных с помощью изменения сопоставления [Fluent API](https://msdn.microsoft.com/en-us/data/jj591617), как вы увидите далее в этом учебнике.

> [!NOTE]
> При попытке скомпилировать до завершения создания всех этих классов сущностей, могут возникнуть ошибки компилятора.


## <a name="create-the-instructor-entity"></a>Создать сущность инструктора

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Создание *Models\Instructor.cs*, заменив шаблон код следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Обратите внимание, что некоторые свойства в `Student` и `Instructor` сущности. В [реализации наследования](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) учебника далее в этой серии будет рефакторинг с использованием наследования для устранения этой избыточности.

### <a name="the-required-and-display-attributes"></a>Необходимая и атрибуты отображения

Атрибуты на `LastName` свойство указать, что это обязательное поле, что заголовок для текстового поля должен «Last Name» (а не именем свойства, которое будет «LastName» без пробела) и что значение не может превышать 50 символов.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

[StringLength атрибута](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) задает максимальную длину в базе данных и предоставляет на стороне клиента и на стороне сервера проверки ASP.NET MVC. Можно также указать минимальной длины строки в этом атрибуте, но минимальное значение не оказывает влияния на схему базы данных. [Обязательный атрибут](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) не требуются для типов значений, например даты и времени, int, double и число с плавающей запятой. Типы значений нельзя задать значение null, по сути они требуются. Можно удалить [обязательный атрибут](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) и замените его минимальную длину параметра `StringLength` атрибута:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

Несколько атрибутов можно поместить на одной строке, можно также написать класс инструктора следующим образом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>Полное имя вычисляемого свойства

`FullName`— Вычисляемое свойство, которое возвращает значение, которое создается путем объединения двух свойств. Поэтому он имеет только `get` метод доступа и нет `FullName` столбец создается в базе данных.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Курсы и свойства навигации OfficeAssignment

`Courses` И `OfficeAssignment` свойства — это свойства навигации. Как было описано ранее, обычно они определяются как [виртуальных](https://msdn.microsoft.com/en-us/library/9fkccyh4(v=vs.110).aspx) , чтобы они могут воспользоваться преимуществами платформы Entity Framework, называемую [отложенную загрузку](https://msdn.microsoft.com/en-us/magazine/hh205756.aspx). Кроме того, если свойство навигации может содержать несколько сущностей, его тип должен реализовывать [ICollection&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/92t2ye13.aspx) интерфейса. (Например [IList&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/5y536ey6.aspx) , но не определяет [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/9eekhta0.aspx) из-за `IEnumerable<T>` не реализует [добавить ](https://msdn.microsoft.com/en-us/library/63ywd54z.aspx).

Инструктор можно обучить любое количество курсов, поэтому `Courses` определяется как совокупность `Course` сущностей. Наш бизнес-правила, состояния инструктор только может иметь не более одного офиса, так `OfficeAssignment` определяется как один `OfficeAssignment` сущности (которые могут быть `null` Если office не назначено).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Создать сущность OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Создание *Models\OfficeAssignment.cs* следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Постройте проект, сохраняющий изменения и подтверждает, что не было выполнено любой копии и вставьте ошибок, которое компилятор может перехватить.

### <a name="the-key-attribute"></a>Ключевой атрибут

Имеется отношение "один к нулю или одному" между `Instructor` и `OfficeAssignment` сущности. Office назначения существует только относительно инструктора, которому присвоено, и поэтому ее первичный ключ также является его внешний ключ к `Instructor` сущности. Платформа Entity Framework не удалось распознать автоматически, но `InstructorID` первичной ключа сущности, так как его имя не соответствует соглашениям `ID` или *classname* `ID` соглашение об именовании. Таким образом `Key` атрибут используется для идентификации в качестве ключа:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Можно также использовать `Key` атрибута, если сущность имеет собственный первичный ключ, но требуется имя свойства, отличные от `classnameID` или `ID`. По умолчанию EF обрабатывает ключ как без формирования базы данных, так как столбец для идентифицирующего отношения.

### <a name="the-foreignkey-attribute"></a>Атрибут ForeignKey

При отсутствии отношением один к нулю или одному или однозначное соответствие между двумя сущностями (такие как между `OfficeAssignment` и `Instructor`), EF не может работать какому концу отношения является участником и какая является зависимым. Один к одному отношениями свойством навигации ссылки в каждом классе другому классу. [Атрибута ForeignKey](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) может применяться к классу зависимых для установления связи. Если не указан [атрибута ForeignKey](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), возникает следующая ошибка при попытке создать миграции:

Не удалось определить основной конец ассоциации между типами «ContosoUniversity.Models.OfficeAssignment» и «ContosoUniversity.Models.Instructor». Основной конец ассоциации должен быть явно настроен с помощью заметок к данным или fluent API связи.

Далее в этом учебнике мы покажем, как настроить эту связь с fluent API.

### <a name="the-instructor-navigation-property"></a>Свойство навигации инструктора

`Instructor` Сущность имеет значение NULL `OfficeAssignment` свойство навигации (поскольку инструктор не может иметь назначение office) и `OfficeAssignment` сущность имеет не допускающий `Instructor` свойство навигации (так как не может присваиваться office существует без инструктор-- `InstructorID` не допускает значение NULL). Когда `Instructor` сущность имеет связанный с ним `OfficeAssignment` сущностей, каждая сущность имеет ссылку на другую переменную его свойства навигации.

Можно поместить `[Required]` атрибут для свойства навигации инструктора, чтобы указать, должен быть связанные инструктора, что не нужно делать, так как не допускающий InstructorID внешний ключ (который также является ключом к этой таблице).

## <a name="modify-the-course-entity"></a>Изменение сущности курса

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

В *Models\Course.cs*, замените код, добавленный ранее следующий код:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Сущность курс имеет свойство внешнего ключа `DepartmentID` указывающая на связанный с ним `Department` сущности и он имеет `Department` свойства навигации. Платформа Entity Framework не требуется добавлять свойство внешнего ключа в модель данных при наличии свойства навигации для связанной сущности. EF автоматически создает внешние ключи в базе данных, где они необходимы. Однако наличие внешнего ключа в модели данных могут выполнять обновления проще и эффективнее. Например, когда выборки сущностью курса для редактирования, `Department` сущности имеет значение null, если он не загружается, поэтому при обновлении сущности курса пришлось бы получить `Department` сущности. При свойстве внешнего ключа `DepartmentID` включено в модель данных, нет необходимости получить `Department` сущности перед обновлением.

### <a name="the-databasegenerated-attribute"></a>Атрибут DatabaseGenerated

[Атрибута DatabaseGenerated](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) с [нет](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) параметр на `CourseID` свойство указывает, что значения первичного ключа предоставленного пользователем, а не созданное базой данных.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

По умолчанию платформа Entity Framework предполагается генерировать значений первичного ключа в базе данных. Это требуется для большинства сценариев. Однако для `Course` сущностей, будет использовать курса определяемый пользователем номер серии 1000 для одного подразделения ряд 2000 для разных отделов и т. д.

### <a name="foreign-key-and-navigation-properties"></a>Внешний ключ и свойств навигации

Свойства внешнего ключа и свойства навигации в `Course` сущности отражают следующие связи:

- Курс назначается одного подразделения, то есть `DepartmentID` внешнего ключа и `Department` свойства навигации по причинам, перечисленным выше. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Курс может иметь любое количество студентов, зарегистрированных в нем, так что `Enrollments` свойство навигации — это коллекция: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Может обучения по нескольким инструкторов, поэтому `Instructors` свойство навигации — это коллекция: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Создание сущности «отдел»

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Создание *Models\Department.cs* следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Атрибут столбца

Ранее вы использовали [атрибут столбца](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) изменять сопоставления имени столбца. В коде `Department` сущности, `Column` атрибут используется для изменения SQL сопоставления типов данных, чтобы столбец будет определяться с помощью SQL Server [money](https://msdn.microsoft.com/en-us/library/ms179882.aspx) типа в базе данных:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Сопоставление столбцов обычно не является обязательным, поскольку Entity Framework обычно выбирает соответствующий тип данных SQL Server, на основе типа CLR, определяемый для свойства. Среда CLR `decimal` сопоставляется SQL Server с типом `decimal` типа. Однако в этом случае известно, что столбец будет удерживать суммы в валюте и [money](https://msdn.microsoft.com/en-us/library/ms179882.aspx) больше подходит для этого типа данных.

### <a name="foreign-key-and-navigation-properties"></a>Внешний ключ и свойств навигации

Свойства внешнего ключа и навигации отражают следующие связи:

- Отдел может или не может быть администратор и администратор всегда инструктор. Поэтому `InstructorID` свойство включается в качестве внешнего ключа в `Instructor` сущности, а вопросительный знак добавляется после `int` введите обозначение, — отмечает свойство как допускающие значение NULL. Свойство навигации называется `Administrator` , но содержит `Instructor` сущности: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Отдел может иметь несколько курсов, так что `Courses` свойство навигации: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

 > [!NOTE]
 > По соглашению платформа Entity Framework позволяет каскадное удаление для внешних ключей, не допускающие значения NULL, а также для связи многие ко многим. Это может привести к циклической cascade delete правила, которые вызывает исключение во время выполнения кода инициализации. Например, если не определен `Department.InstructorID` свойство как допускающие значение NULL, можно получить следующее сообщение об исключении при выполняется инициализатор: «ссылочную связь приведет к циклической ссылки, что не разрешено.» При необходимости бизнес-правила `InstructorID` свойство как не допускающие пришлось бы использовать для отключения каскадное удаление в отношении следующих fluent API: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>Изменение сущности студента

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

В *Models\Student.cs*, замените код, добавленный ранее следующий код. Изменения выделены.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>Сущность регистрации

 В *Models\Enrollment.cs*, замените код, добавленный ранее следующий код

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Внешний ключ и свойств навигации

Свойства внешнего ключа, а также свойства навигации отражают следующие связи:

- Для одного курса, является запись регистрации, поэтому `CourseID` свойство внешнего ключа и `Course` свойство навигации: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Для одного студента, является запись регистрации, поэтому `StudentID` свойство внешнего ключа и `Student` свойство навигации: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Многие ко многим

Имеется отношение "многие ко многим" между `Student` и `Course` сущности и `Enrollment` сущности функционирует как многие ко многим Соединяемая таблица *с полезными данными* в базе данных. Это означает, что `Enrollment` таблица содержит дополнительные данные помимо внешние ключи для соединяемых таблиц (в данном случае первичный ключ и `Grade` свойство).

Ниже показано, как будут выглядеть эти связи в схеме сущности. (Эта схема была создана с помощью [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); Создание схемы не является частью учебника, он просто используется здесь как пример.)

![Ученик Course_many для many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Каждая линия связи имеет 1 на одном конце и звездочку (\*) в другое, указывающее, один ко многим.

Если `Enrollment` таблицы не были включены сведения об оценках, его потребуется только содержат два внешних ключа `CourseID` и `StudentID`. В этом случае он будет соответствовать многие ко многим Соединяемая таблица *без полезных данных* (или *таблицы присоединения*) в базе данных и создать класс модели для него вообще не требуется. `Instructor` И `Course` сущности, имеют этот тип связи "многие ко многим", и как можно видеть, между ними отсутствует класс сущности:

![Инструктора Course_many для many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Соединяемая таблица является обязательным в базе данных, однако, как показано на следующей схеме базы данных:

![Инструктора Course_many для many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Платформа Entity Framework автоматически создает `CourseInstructor` таблицы и чтения и обновить ее косвенно, чтение и обновление `Instructor.Courses` и `Course.Instructors` свойства навигации.

## <a name="entity-diagram-showing-relationships"></a>Отображение отношений объектов схемы

Ниже показана схема, создаваемая Entity Framework Power Tools для завершенной модели School.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Помимо строк многие ко-многим (\* для \*) и один ко многим связей (от 1 до \*), здесь можно увидеть на линию связи один к нулю или одному (1 к нулю или одному) между `Instructor` и `OfficeAssignment` сущности и линию связи нуль или один ко многим (от 0 до 1 для \*) между сущностями инструктора и отдел.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Настройка модели данных путем добавления кода в контексте базы данных

Далее добавим новые сущности для `SchoolContext` класса и настраивать некоторые сопоставления с помощью [fluent API](https://msdn.microsoft.com/en-us/data/jj591617) вызовов. (API — «fluent» так, как часто используемые проводить ряда вызовов метода вместе в одной инструкции).

В этом учебнике fluent API используется только для сопоставления с базой данных, нельзя выполнить с помощью атрибутов. Тем не менее можно также использовать fluent API для указания форматирования, проверки и правила сопоставления, которые можно сделать с помощью атрибутов, большинство. Некоторые атрибуты, такие как `MinimumLength` не может применяться с помощью плавного API. Как упоминалось ранее, `MinimumLength` не изменяет схему, применяется правило проверки стороны клиента и сервера

Некоторые разработчики предпочитают использовать fluent API, они могут обновлять свои классы сущностей «очистить». Могут быть использованы смешанные атрибуты и fluent API, если требуется, и существует ряд настроек, можно только с помощью плавного API, но в целом рекомендуется выбрать один из этих двух подходов и использовать согласованно, насколько возможно.

Чтобы добавить новые сущности для данных модели и выполнения сопоставления с базой данных, не были выполнены с помощью атрибутов, замените код в *DAL\SchoolContext.cs* следующим кодом:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Оператор new в [OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) метод настраивает многие ко многим соединенной таблице:

- Для связи "многие ко многим" между `Instructor` и `Course` сущностей, код задает имена таблиц и столбцов для таблицы объединения. Код сначала можно настроить многие ко многим для вас без этого кода, но если вы не вызываете его, вы получите имена по умолчанию такие как `InstructorInstructorID` для `InstructorID` столбца.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Ниже приведен пример как вы мог использоваться fluent API вместо атрибутов для указания связь между `Instructor` и `OfficeAssignment` сущности:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Сведения о действиях инструкций «fluent API» в фоновом см [Fluent API](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) записи блога.

## <a name="seed-the-database-with-test-data"></a>Начальное значение базы данных с тестовыми данными

Замените код в *Migrations\Configuration.cs* файла следующим кодом для предоставления данных начального значения для новых сущностей, которые вы создали.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Как видно в первом руководстве, большая часть кода просто обновляет или создает новые объекты сущности и загружает данные в свойства, необходимые для тестирования. Обратите внимание, как `Course` сущности, которая имеет отношение многие ко многим с `Instructor` обрабатывается сущности:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

При создании `Course` объекта инициализации `Instructors` свойство навигации как пустую коллекцию с помощью кода `Instructors = new List<Instructor>()`. Это позволяет добавить `Instructor` сущностей, относящихся к этому `Course` с помощью `Instructors.Add` метод. Если не удалось создать пустой список, невозможно добавить эти связи, так как `Instructors` свойство будет иметь значение null и не пришлось бы `Add` метод. Инициализация списка можно также добавить в конструктор.

## <a name="add-a-migration-and-update-the-database"></a>Добавьте миграции и обновления базы данных

PMC, введите следующую команду `add-migration` команды:

`PM> add-Migration Chap4`

При попытке обновить базу данных на этом этапе, появится следующая ошибка:

*Конфликт инструкции ALTER TABLE с ограничением FOREIGN KEY «FK\_dbo. Курс\_dbo. Отдел\_DepartmentID». Конфликт произошел в таблицу в базе данных «ContosoUniversity», «dbo. Отдела», столбец «DepartmentID».*

Изменить &lt; *timestamp&gt;\_Chap4.cs* файл и внесите следующие изменения в код (мы добавим в инструкции SQL и изменить `AddColumn` инструкции):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Убедитесь, что закомментируйте или удалите существующую `AddColumn` строки при добавлении нового или приведет к ошибке при вводе `update-database` команд.)

Иногда при выполнении миграции с существующими данными, необходимо вставить данные заглушки в базу данных для удовлетворения ограничения внешнего ключа, и, что делаем сейчас. Созданный код добавляет не допускающий `DepartmentID` внешний ключ к `Course` таблицы. Если уже имеются строки в `Course` таблицы при выполнении кода, `AddColumn` операция окончится неудачей, так как SQL Server не знает, какое значение в столбце, который не может иметь значение null. Таким образом вы изменили код, чтобы предоставить значения по умолчанию для нового столбца и создания заглушки подразделение с именем «Temp» в качестве подразделения по умолчанию. В результате, если существуют `Course` строках, если этот код запускается, они будут быть связаны в отдел «Temp».

При `Seed` выполняется метод, он будет вставлять строки в `Department` относится существующие таблицы и ее `Course` этих новых строк `Department` строк. Если вы не добавляли курсов в пользовательском Интерфейсе, затем больше не потребуется отдела «Temp» или значение по умолчанию на `Course.DepartmentID` столбца. Чтобы обеспечить возможность того, что кто-то добавили курсы с помощью приложения, также требуется обновить `Seed` код метода, чтобы убедиться, что все `Course` строк (не только те, которые вставляются при предыдущих запусках `Seed` метод) имеет Допустимые `DepartmentID` значения перед удалением по умолчанию значение из столбца и удалить отдел «Temp».

После завершения редактирования &lt; *timestamp&gt;\_Chap4.cs* , введите `update-database` в PMC для выполнения миграции.

> [!NOTE]
> Можно получить другие ошибки при переносе данных и внесения изменений схемы. Если возникли ошибки миграции, не удается устранить, либо можно изменить строку подключения в *Web.config* файл или удалить базу данных. Самым простым подходом является переименовать базу данных, в *Web.config* файла. Например, измените имя базы данных на CU\_проверить, как показано ниже:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  С помощью новой базы данных нет данных для переноса и `update-database` команда гораздо больше шансов завершиться без ошибок. Инструкции по удалению базы данных см. в разделе [как удалить базу данных из Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).


Открыть базу данных в **обозревателя серверов** выполнял ранее, и разверните **таблиц** узел, чтобы увидеть, что все таблицы были созданы. (При наличии **обозревателя серверов** откройте из более ранних времени, щелкните **обновление** кнопки.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Не удалось создать класс модели для `CourseInstructor` таблицы. Как упоминалось ранее, это Соединяемая таблица для связи "многие ко многим" между `Instructor` и `Course` сущности.

Щелкните правой кнопкой мыши `CourseInstructor` таблицы и выберите **Показать таблицу данных** для убедитесь в наличии данных в нем в результате использования `Instructor` сущностей, которые вы добавили `Course.Instructors` свойство навигации.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Сводка

Теперь у вас есть более сложные модели данных и соответствующей базой данных. В этом руководстве вы узнаете о дополнительных сведений о различные способы доступа к взаимосвязанных данных.

Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Назад](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Вперед](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
