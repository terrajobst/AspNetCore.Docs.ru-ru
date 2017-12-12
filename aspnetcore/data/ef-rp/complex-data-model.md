---
title: "Страниц Razor с основными EF - модели данных — 5 8."
author: rick-anderson
description: "В этом учебнике добавляйте дополнительные сущности и связи и настроить модель данных, указав форматирование, проверки и правила сопоставления базы данных."
keywords: "Заметок к данным ASP.NET Core, Entity Framework Core,"
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c2761f79fa4836d29541526782969bb0fd47778b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a>Создание сложных данных модели - Core EF учебнику страниц Razor (5 8)

По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Предыдущих учебниках работал с моделью данных, созданный из трех сущностей. В этом учебнике:

* Добавляются дополнительные сущности и связи.
* Модель данных настраивается путем указания форматирования, проверки и правила сопоставления базы данных.

Классы сущностей для завершенной модели показан на следующем рисунке:

![Схема Entity](complex-data-model/_static/diagram.png)

Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).

## <a name="customize-the-data-model-with-attributes"></a>Настройка модели данных с атрибутами

В этом разделе модели данных настраивается с помощью атрибутов.

### <a name="the-datatype-attribute"></a>Атрибут типа данных

Время страницы студента в настоящее время регистрации даты. Как правило поля даты для отображения только даты и не времени.

Обновление *Models/Student.cs* на следующий выделенный код:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

[DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) атрибут задает тип данных, который является более точным определением, чем встроенный тип базы данных. В этом случае следует отображать только дата не даты и времени. [Перечисление DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) предоставляет для многих типов данных, таких как дата, время, PhoneNumber, валюты, EmailAddress, и т. д. `DataType` Атрибута также позволяет приложению автоматически предоставлять функции для определенного типа. Пример:

* `mailto:` Ссылка создается автоматически для `DataType.EmailAddress`.
* Средство выбора даты предоставляется для `DataType.Date` в большинстве браузеров.

`DataType` Атрибут выдает HTML 5 `data-` (произносится данных тире) атрибутов, которые потребляют браузеров HTML 5. `DataType` Атрибуты не имеют проверки.

`DataType.Date` не задает формат отображаемой даты. По умолчанию, поле «Дата» отображается согласно форматы по умолчанию на сервере [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).

С помощью атрибута `DisplayFormat` можно явно указать формат даты:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Параметр указывает, что форматирование должен также применяться к изменения пользовательского интерфейса. Некоторые поля не следует использовать `ApplyFormatInEditMode`. Например символ валюты обычно не должен отображаться в поле ввода текста.

`DisplayFormat` Атрибут может использоваться сама по себе. Обычно рекомендуется использовать `DataType` атрибутом `DisplayFormat` атрибута. `DataType` Атрибут передает семантику данных, а не как отображать его на экране. `DataType` Атрибут предоставляет следующие преимущества, которые недоступны в `DisplayFormat`:

* Браузер может включить функции HTML5. Например отображение элемента управления календаря, локализованными обозначение денежной единицы, ссылки электронной почты, входной проверки на стороне клиента, и т. д.
* По умолчанию браузер отображает данные, используя правильный формат на основе языкового стандарта.

Дополнительные сведения см. в разделе [ \<ввода > Вспомогательные тега документации](xref:mvc/views/working-with-forms#the-input-tag-helper).

Запустите приложение. Перейдите на страницу индекса учащихся. Время больше не отображается. Каждый представление, использующее `Student` модели отображается дата без времени.

![Отображение даты без времени страницы индекса студентов](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Атрибут StringLength

С помощью атрибутов можно указать правила проверки данных и сообщений об ошибках проверки. [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) атрибут указывает минимальную и максимальную длину символов, разрешенных в поле данных. `StringLength` Атрибута также предоставляет проверки на стороне клиента и на стороне сервера. Минимальное значение не оказывает влияния на схему базы данных.

Обновление `Student` модели с помощью следующего кода:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Предыдущий код допускается использование не более 50 символов. `StringLength` Атрибута не запрещает ввод символы-разделители для имени пользователя. [Регулярное выражение](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) атрибут используется для применения ограничений входных данных. Например следующий код требует первого символа в записываются прописными буквами и остальные символы преобразуются в алфавитном порядке:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

Запуск приложения:

* Перейдите на страницу учащихся.
* Выберите **создать новый**и введите имя длиной более 50 символов.
* Выберите **создать**, проверка на стороне клиента показано сообщение об ошибке.

![Студенты индексная страница, отображающий строку ошибки длины](complex-data-model/_static/string-length-errors.png)

В **обозреватель объектов SQL Server** (SSOX), откройте в конструкторе таблиц учащихся, дважды щелкнув **студента** таблицы.

![Таблица учащихся в SSOX до миграции](complex-data-model/_static/ssox-before-migration.png)

На предыдущем рисунке показана схемы для `Student` таблицы. Имя поля имеют тип `nvarchar(MAX)` , так как миграция не была запущена на базу данных. При выполнении миграции далее в этом учебнике, имя поля становятся `nvarchar(50)`.

### <a name="the-column-attribute"></a>Атрибут столбца

Атрибуты можно управлять как классы и свойства сопоставляются в базу данных. В этом разделе `Column` атрибут используется для сопоставления имени `FirstMidName` свойства «FirstName» в базе данных.

При создании базы данных, имена свойств в модели используются для имен столбцов (только если `Column` используется атрибут).

`Student` Модель использует `FirstMidName` для имени первого поля, так как поле также может содержать отчество.

Обновление *Student.cs* файл с следующий выделенный код:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

С помощью предыдущего изменения `Student.FirstMidName` в приложении сопоставляется `FirstName` столбец `Student` таблицы.

Добавление `Column` атрибут изменяет модель `SchoolContext`. Модель `SchoolContext` больше не соответствует базе данных. Если приложение запускается перед применением миграций, создается следующее исключение:

```SQL
SqlException: Invalid column name 'FirstName'.
```
Обновление базы данных:

* Выполните построение проекта.
* Откройте окно командной строки в папке проекта. Введите следующие команды для создания новой миграции и обновления базы данных:

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

`dotnet ef migrations add ColumnFirstName` Команда создает сообщение об ошибке:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

Создается предупреждение, так как имя поля, становятся более 50 символов. Если имя в базе данных имеет более 50 символов, 51 до последнего символа будет потеряна.

* Тестирование приложения.

Откройте таблицу студента в SSOX:

![Студенты таблицы в SSOX после миграции](complex-data-model/_static/ssox-after-migration.png)

До применения миграции, имя столбцы были типа [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Столбцы имен стали `nvarchar(50)`. Имя столбца отличается от `FirstMidName` для `FirstName`.

> [!Note]
> В следующем разделе сборка приложения на некоторых этапах приводит к возникновению ошибки компилятора. Укажите инструкции, когда для сборки приложения.

## <a name="student-entity-update"></a>Обновление сущности студента

![Сущность Student](complex-data-model/_static/student-entity.png)

Обновление *Models/Student.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Обязательный атрибут

`Required` Атрибут делает обязательные поля имя свойства. `Required` Атрибут не нужен для запретом типов, таких как типы значений (`DateTime`, `int`, `double`и т. д.). Типы, которые не может быть неопределенным автоматически обрабатываются как обязательные поля.

`Required` Атрибута может быть заменено минимальную длину параметра в `StringLength` атрибута:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Атрибут отображения

`Display` Атрибут указывает заголовок для текстовых полей таблицу «Имя», «Last Name», «Полное имя» и «Дата регистрации». По умолчанию заголовки размером не разделение слов, например «Фамилия».

### <a name="the-fullname-calculated-property"></a>Свойство FullName вычисляется

`FullName`— Вычисляемое свойство, которое возвращает значение, которое создается путем объединения двух свойств. `FullName`не может быть задано, оно имеет только метод доступа get. Не `FullName` столбец создается в базе данных.

## <a name="create-the-instructor-entity"></a>Создать сущность инструктора

![Сущность инструктора](complex-data-model/_static/instructor-entity.png)

Создание *Models/Instructor.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Обратите внимание, что некоторые свойства в `Student` и `Instructor` сущности. В этом учебнике реализация наследования далее в этой серии этот код оптимизирован во избежание избыточность.

Несколько атрибутов может быть на одной строке. `HireDate` Атрибутов может быть записан следующим образом:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Свойства навигации CourseAssignments и OfficeAssignment

`CourseAssignments` И `OfficeAssignment` свойства — это свойства навигации.

Инструктор можно обучить любое количество курсов, поэтому `CourseAssignments` определен как коллекция.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Если свойство навигации содержит несколько сущностей:

* Он должен быть типом списка, где записи может быть добавленные, удаленные и обновлены.

Следующие типы свойств навигации.

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Если `ICollection<T>` указан, создает EF Core `HashSet<T>` коллекции по умолчанию.

`CourseAssignment` Сущности см. в разделе на связи, многие ко многим.

Contoso университета бизнес-правил, инструктор может иметь не более одного офиса состояния. `OfficeAssignment` Свойство содержит один `OfficeAssignment` сущности. `OfficeAssignment`имеет значение null, если office не назначено.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Создать сущность OfficeAssignment

![OfficeAssignment сущности](complex-data-model/_static/officeassignment-entity.png)

Создание *Models/OfficeAssignment.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Ключевой атрибут

`[Key]` Атрибут используется для идентификации свойства как первичный ключ (PK) при имя свойства стоит Кроме classnameID или идентификатор.

Имеется отношение "один к нулю или одному" между `Instructor` и `OfficeAssignment` сущности. Назначение office существует только относительно инструктора, которую он назначен. `OfficeAssignment` Первичного ключа является также его внешний ключ (FK) `Instructor` сущности. Ядро EF не распознает автоматически `InstructorID` как PK из `OfficeAssignment` из-за:

* `InstructorID`не Следуйте соглашению об именах идентификатор или classnameID.

Таким образом `Key` атрибут используется для идентификации `InstructorID` как первичного ключа:

```csharp
[Key]
public int InstructorID { get; set; }
```

По умолчанию EF Core считает ключ без формирования базы данных, так как столбец используется идентифицирующего отношения.

### <a name="the-instructor-navigation-property"></a>Свойство навигации инструктора

`OfficeAssignment` Свойства навигации для `Instructor` сущности допускает значения NULL из-за:

* Ссылочных типов (такие, как классы, допускающие значение NULL).
* Инструктор не может иметь назначение office.


`OfficeAssignment` Сущность имеет не допускающий `Instructor` свойства навигации из-за:

* `InstructorID`является запретом.
* Назначение office не может существовать без инструктор.

Когда `Instructor` сущность имеет связанный с ним `OfficeAssignment` сущностей, каждая сущность содержит ссылку на другую переменную его свойства навигации.

`[Required]` Атрибут может применяться к `Instructor` свойство навигации:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Предыдущий код указывает, что должен быть связанные инструктора. Приведенный выше код не требуется из-за `InstructorID` внешний ключ (который также является PK) допускают значение NULL.

## <a name="modify-the-course-entity"></a>Изменение сущности курса

![Сущности курса](complex-data-model/_static/course-entity.png)

Обновление *Models/Course.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

`Course` Сущность имеет внешний ключ (FK) свойство `DepartmentID`. `DepartmentID`Указывает на связанный `Department` сущности. `Course` Сущность имеет `Department` свойства навигации.

EF Core не требует свойства внешнего ключа для модели данных, если модель имеет свойство навигации для связанной сущности.

Везде, где они нужны FKs автоматически создает EF Core в базе данных. Создает основной EF [затемнять свойства](https://docs.microsoft.com/ef/core/modeling/shadow-properties) для автоматически созданных FKs. Наличие внешнего ключа в модели данных могут выполнять обновления проще и эффективнее. Например, рассмотрим модель где свойства внешнего ключа `DepartmentID` — *не* включены. При выборке сущности курса для изменения:

* `Department` Сущности имеет значение null, если он явно не загружен.
* Для обновления сущности курса `Department` сущности должно быть выбрано.

Если свойство внешнего ключа `DepartmentID` включено в модели данных, нет необходимости выбирать `Department` сущности перед обновлением.

### <a name="the-databasegenerated-attribute"></a>Атрибут DatabaseGenerated

`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Атрибут указывает, что первичного ключа является предоставляемый приложением, а не созданное базой данных.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

По умолчанию EF Core предполагается генерировать значений первичного ключа в базе данных. DB созданный PK значений обычно является лучшим подходом. Для `Course` сущностей, пользователь указывает первичный ключ. Например курс число, например 1000 серии для отдела математические и 2000 для английского языка отдела.

`DatabaseGenerated` Атрибут также может использоваться для создания значения по умолчанию. Например базы данных может автоматически создавать поля даты для записи даты строки был создан или обновлен. Дополнительные сведения см. в разделе [созданные свойства](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Свойства внешнего ключа и навигации

Внешний ключ (FK) свойства и свойства навигации в `Course` сущности отражают следующие связи:

Курс назначается одного подразделения, то есть `DepartmentID` внешнего ключа и `Department` свойства навигации.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Курс может иметь любое количество студентов, зарегистрированных в нем, так что `Enrollments` свойство навигации — это коллекция:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Может обучения по нескольким инструкторов, поэтому `CourseAssignments` свойство навигации — это коллекция:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment`объясняется [позже](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Создание сущности «отдел»

![Сущность Department](complex-data-model/_static/department-entity.png)

Создание *Models/Department.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Атрибут столбца

Ранее `Column` атрибут был применен, чтобы изменить сопоставление имени столбца. В коде `Department` сущности, `Column` атрибута можно изменить сопоставление типов данных SQL. `Budget` Столбца определяется с помощью типа money SQL Server в базе данных:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Сопоставление столбцов обычно не является обязательным. EF Core обычно выбирает соответствующий тип данных SQL Server, на основе типа CLR для свойства. Среда CLR `decimal` сопоставляется SQL Server с типом `decimal` типа. `Budget`для денежных единиц и тип данных money больше подходит для валюты.

### <a name="foreign-key-and-navigation-properties"></a>Свойства внешнего ключа и навигации

Свойства внешнего ключа и навигации отражают следующие связи:

* Отдел может или не может иметь администратора.
* Администратор всегда является инструктор. Поэтому `InstructorID` свойство включается в качестве внешнего ключа для `Instructor` сущности.

Свойство навигации называется `Administrator` , но содержит `Instructor` сущности:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Вопросительный знак (?) в приведенном выше коде указывает, что свойство имеет значение NULL.

Отдел может иметь несколько курсов, то есть свойство навигации курсы:

```csharp
public ICollection<Course> Courses { get; set; }
```

Примечание: По соглашению EF Core позволяет каскадное удаление для запретом FKs и многие ко многим связи. Каскадное удаление может привести к циклической каскадное удаление правил. Циклическая каскадного удаления правила вызывает исключение при добавлении миграции.

Например если `Department.InstructorID` свойства не был определен как допускающая значение NULL:

* EF Core настраивает правила cascade delete для удаления инструктора при удалении подразделения.
* Удаление инструктора при удалении подразделения не подобное поведение.

При необходимости бизнес-правила `InstructorID` свойства быть не значение NULL, используйте следующую инструкцию fluent API:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

Предыдущий код отключает каскадное удаление в отношении инструктора отдела.

## <a name="update-the-enrollment-entity"></a>Обновление сущности регистрации

Запись регистрации — для одного курса, выполняемое один студент.

![Сущности регистрации](complex-data-model/_static/enrollment-entity.png)

Обновление *Models/Enrollment.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Свойства внешнего ключа и навигации

Свойства внешнего ключа и свойства навигации отражают следующие связи:

Для одного курса является запись регистрации, поэтому `CourseID` свойства внешнего ключа и `Course` свойство навигации:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Для один студент является запись регистрации, поэтому `StudentID` свойства внешнего ключа и `Student` свойство навигации:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Многие ко многим

Имеется отношение "многие ко многим" между `Student` и `Course` сущности. `Enrollment` Сущности функционирует как многие ко многим Соединяемая таблица *с полезными данными* в базе данных. «С полезными данными» означает, что `Enrollment` таблица содержит дополнительные данные помимо FKs для соединяемых таблиц (в данном случае PK и `Grade`).

Ниже показано, как будут выглядеть эти связи в схеме сущности. (Эта схема была создана с помощью EF Power Tools для EF 6.x. Создание схемы не является частью учебника.)

![Курс студента много множественных связей](complex-data-model/_static/student-course.png)

Каждая линия связи имеет 1 на одном конце и звездочку (*) в другом, указывающее один ко многим.

Если `Enrollment` таблицы не были включены сведения об оценках, его потребуется только содержат два FKs (`CourseID` и `StudentID`). Многие ко многим Соединяемая таблица без полезных данных иногда называют таблицы присоединения (ЧИСТАЯ).

`Instructor` И `Course` сущности имеют связь «многие ко многим» с помощью таблицы присоединения.

Примечание: Неявное соединение таблиц для связи многие ко многим, но основные EF не поддерживает 6.x EF. Дополнительные сведения см. в разделе [многие ко многим связей в версии 2.0 основные EF](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>Сущность CourseAssignment

![CourseAssignment сущности](complex-data-model/_static/courseassignment-entity.png)

Создание *Models/CourseAssignment.cs* следующим кодом:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Инструктора курсов

![M:M инструктора курсов](complex-data-model/_static/courseassignment.png)

Связь многие ко многим инструктора-курсы:

* Требуется соединяемой таблице, который должен быть представлен набор сущностей.
* Является таблицы присоединения (таблицы без полезных данных).

Обычно имя сущности объединения `EntityName1EntityName2`. Например, инструктора-курсы соединенной таблице с помощью этого шаблона является `CourseInstructor`. Тем не менее рекомендуется использовать имя, которое описывает связь.

Модели создаются простой и увеличиваться. Для включения полезных данных часто изменяться полезных данных нет соединения (PJTs). С помощью сущности описательное имя, имя не нужно изменять при изменении таблицы соединения. В идеальном случае сущности объединения бы естественным имени (возможно одно слово) в бизнес-среде. Например книги и клиенты могут быть связаны с соединения сущности, называемой оценок. В отношении многие ко многим инструктора-курсы `CourseAssignment` предпочтительнее, чем `CourseInstructor`.

### <a name="composite-key"></a>Составной ключ

FKs не допускают значения NULL. Два FKs в `CourseAssignment` (`InstructorID` и `CourseID`) совместно однозначно определяют каждую строку `CourseAssignment` таблицы. `CourseAssignment`не требует выделенной системы. `InstructorID` И `CourseID` свойства функционировать как составной первичный ключ. — Единственный способ для указания составных первичных EF Core с *fluent API*. В следующем разделе показано, как настроить составной первичный ключ.

Обеспечивает составного ключа.

* Для одного курса допускается несколько строк.
* Для одного инструктора допускается несколько строк.
* Не допускается несколько строк для одной и той же инструктора и курса.

`Enrollment` Сущности объединения определяет собственный первичного ключа, возможны повторяющиеся значения такого рода. Для предотвращения таких повторяющиеся значения:

* Добавьте уникальный индекс по полям внешнего ключа, или
* Настройка `Enrollment` с основной составного ключа, аналогично `CourseAssignment`. Дополнительные сведения см. в разделе [индексы](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Обновить контекст базы данных

Добавьте следующий выделенный код в *Data/SchoolContext.cs*:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Предыдущий код добавляет новые сущности и настраивает `CourseAssignment` сущности составной первичный ключ.

## <a name="fluent-api-alternative-to-attributes"></a>Fluent API вместо атрибутов

`OnModelCreating` Метод в предыдущем коде используется *fluent API* можно настроить поведение EF Core. API-Интерфейс называется «fluent», поскольку часто используемые проводить ряда вызовов метода вместе в одной инструкции. [После кода](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) является примером fluent API:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

В этом учебнике fluent API используется только для сопоставления базы данных, которое не может выполняться с атрибутами. Однако fluent API можно указать большинство форматирование, проверки и правила сопоставления, которые можно сделать с помощью атрибутов.

Некоторые атрибуты, такие как `MinimumLength` не может применяться с помощью плавного API. `MinimumLength`не изменяйте схему, он применяется только минимальная длина правила проверки.

Некоторые разработчики предпочитают использовать fluent API, они могут обновлять свои классы сущностей «очистить». Можно одновременно атрибуты и fluent API. Существует несколько конфигураций, которые можно будет только с помощью плавного API (с указанием составного первичного ключа). Существует несколько конфигураций, которые можно будет только с атрибутами (`MinimumLength`). Рекомендуется с помощью плавного API или атрибуты:

* Выберите один из этих двух подходов.
* Выбранный подход постоянно максимально используйте.

Некоторые атрибуты, используемые в этом учебнике используются для:

* Только для проверки (например, `MinimumLength`).
* EF основная конфигурация (например, `HasKey`).
* Проверка и EF основные конфигурации (например, `[StringLength(50)]`).

Дополнительные сведения об атрибутах и fluent API см. в разделе [методы конфигурации](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Отображение отношений объектов схемы

Ниже показана схема, создаваемая EF Power Tools для завершенной модели School.

![Схема Entity](complex-data-model/_static/diagram.png)

На предыдущей схеме показаны:

* Несколько строк один ко многим (от 1 до \*).
* Линия связи один к нулю или одному (1 к нулю или одному) между `Instructor` и `OfficeAssignment` сущности.
* Линия связи нуль или один ко многим (от 0 до 1 для *) между `Instructor` и `Department` сущности.

## <a name="seed-the-db-with-test-data"></a>Начальное значение DB тестовыми данными

Обновление кода в *Data/DbInitializer.cs*:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Предыдущий код предоставляет данные начальное значение для новых сущностей. Большая часть кода создает новые объекты сущности и загружает данные образца. Образец данных используется для проверки. Приведенный выше код создает следующие связи многие ко многим:

* `Enrollments`
* `CourseAssignment`

Примечание: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) будет поддерживать [заполнение данными](https://github.com/aspnet/EntityFrameworkCore/issues/629).

## <a name="add-a-migration"></a>Добавить миграции

Выполните построение проекта. Откройте окно командной строки в папке проекта и введите следующую команду:

```console
dotnet ef migrations add ComplexDataModel
```

Эта команда отображает предупреждение о возможной потере данных.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Если `database update` команда выполняется, возникает следующая ошибка:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

При выполнении миграции с существующими данными, могут существовать ограничения внешнего ключа, которые не выполнены для существующих данных. В этом учебнике создается новый DB, поэтому отсутствие нарушений ограничений внешнего ключа. В разделе [исправление ограничения внешнего ключа с помощью предыдущих версий данных](#fk) инструкции для устранения нарушения внешнего ключа на текущей базы данных.

## <a name="change-the-connection-string-and-update-the-db"></a>Измените строку подключения и обновления базы данных

Код в обновленной `DbInitializer` добавляет данные начальное значение для новых сущностей. Чтобы принудительно EF основных компонентов для создания новой пустой базы данных:

* Измените имя строки подключения базы данных в *appsettings.json* для ContosoUniversity3. Новое имя должно быть имя, которое не было использовано на компьютере.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* Кроме того удаление базы данных с помощью:

    * **Обозреватель объектов SQL Server** (SSOX).
    * `database drop` Команду CLI:

   ```console
   dotnet ef database drop
   ```

Запустите `database update` в командную строку:

```console
dotnet ef database update
```

Предыдущая команда выполняется миграция.

Запустите приложение. Приложение выполняется под управлением `DbInitializer.Initialize` метод. `DbInitializer.Initialize` Заполняет новой базы данных.

Откройте базу данных в SSOX:

* Разверните **таблиц** узла. Будут отображены созданные таблицы.
* Если ранее был открыт SSOX, нажмите кнопку **обновление** кнопки.

![Таблицы в SSOX](complex-data-model/_static/ssox-tables.png)

Изучите **CourseAssignment** таблицы:

* Щелкните правой кнопкой мыши **CourseAssignment** таблицы и выберите **данные представления**.
* Проверьте **CourseAssignment** таблица содержит данные.

![Данные CourseAssignment в SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Исправление ограничения внешнего ключа с помощью данных из прежних версий

Этот раздел является необязательным.

При выполнении миграции с существующими данными, могут существовать ограничения внешнего ключа, которые не выполнены для существующих данных. С рабочими данными необходимо выполнить действия для переноса существующих данных. В этом разделе приведен пример исправления нарушения ограничений внешнего ключа. Не внести эти изменения кода без резервной копии. Не внести эти изменения кода, если выполнено предыдущего раздела и изменения в базу данных.

*{Timestamp}_ComplexDataModel.cs* файл содержит следующий код:

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

Предыдущий код добавляет не допускающий `DepartmentID` внешнего ключа для `Course` таблицы. Базы данных из предыдущего учебника содержит строки в `Course`, поэтому для этой таблицы не может быть обновлена миграции.

Чтобы сделать `ComplexDataModel` миграции работы с существующими данными:

* Изменить код, чтобы предоставить новый столбец (`DepartmentID`) значение по умолчанию.
* Создайте фиктивный подразделение с именем «Temp» в качестве подразделения по умолчанию.

### <a name="fix-the-foreign-key-constraints"></a>Исправьте ограничения внешнего ключа

Обновление `ComplexDataModel` классы `Up` метод:

* Откройте *{timestamp}_ComplexDataModel.cs* файл.
* Закомментировать строку кода, который добавляет `DepartmentID` столбец `Course` таблицы.

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Добавьте следующий выделенный код. Новый код идет после `.CreateTable( name: "Department"` блока:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

С предыдущие изменения, существующие `Course` строки будут связаны с отделом «Temp» после `ComplexDataModel` `Up` метода.

Так же производственного приложения.

* Включает код или сценарии, чтобы добавить `Department` строк и связанных с `Course` строк к новому `Department` строк.
* Не используйте отдела «Temp» или значение по умолчанию для `Course.DepartmentID `.

Далее в этом учебнике описывается взаимосвязанных данных.

>[!div class="step-by-step"]
[Назад](xref:data/ef-rp/migrations)
[Вперед](xref:data/ef-rp/read-related-data)