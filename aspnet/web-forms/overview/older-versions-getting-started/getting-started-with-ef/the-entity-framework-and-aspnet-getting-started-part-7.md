---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: "Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4, веб-формы - часть 7 | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, использующий Entity Framework. Это образец приложения..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: aeea122636f5235364e6a40cb6e041b1fe221317
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Приступая к работе с базой данных Entity Framework 4.0 сначала и форм ASP.NET 4 веб - часть 7
====================
По [Tom Dykstra](https://github.com/tdykstra)

> Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010. Сведения о учебника серии см [в первом учебнике ряда](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Использование хранимых процедур

В предыдущем учебнике реализован шаблон таблица на иерархию наследования. Этого учебника показано, как использовать хранимые процедуры, чтобы получить больший контроль над доступ к базе данных.

Платформа Entity Framework позволяет указать, следует использовать хранимые процедуры для доступа к базе данных. Для любого типа сущности можно указать хранимую процедуру можно использовать для создания, обновления и удаления сущностей этого типа. Затем в модели данных можно добавить ссылки на хранимые процедуры, которые можно использовать для выполнения задач, таких как извлечение наборов сущностей.

Использование хранимых процедур является общим требованием для доступа к базе данных. В некоторых случаях администратор базы данных может потребоваться, что все доступ к базе данных проходят через хранимые процедуры, по соображениям безопасности. В других случаях может потребоваться построения бизнес-логики в некоторые процессы, которые платформа Entity Framework использует при обновлении базы данных. Например при каждом удалении сущности можно скопировать его в архивную базу данных. Или при каждом обновлении строки может потребоваться записать строку в таблицу журнала, записываемых, кто внес эти изменения. Можно выполнять следующие виды задач в хранимую процедуру, которая вызывается при любом Entity Framework удаляет сущность или обновляет сущность.

Как в предыдущем учебнике вы не создадите новые страницы. Вместо этого мы изменим Entity Framework доступа к базе данных для некоторых страниц, которые уже созданы.

В этом учебнике вы создадите хранимых процедур в базе данных для вставки `Student` и `Instructor` сущности. Вы добавите их в модель данных, и потребуется указать, что платформа Entity Framework должна использовать их для добавления `Student` и `Instructor` сущностей в базу данных. Вы также создадите хранимую процедуру, которая используется для извлечения `Course` сущностей.

## <a name="creating-stored-procedures-in-the-database"></a>Создание хранимых процедур в базе данных

(Если вы используете *файл School.mdf* файл из проекта, можно загрузить с помощью этого учебника, можно пропустить этот раздел, поскольку уже существуют хранимые процедуры.)

В **обозревателя серверов**, разверните *файл School.mdf*, щелкните правой кнопкой мыши **хранимых процедур**и выберите **добавить новую хранимую процедуру**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Скопируйте следующие инструкции SQL и вставьте их в окне хранимой процедуры, заменив каркас хранимой процедуры.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student`сущности имеют четыре свойства: `PersonID`, `LastName`, `FirstName`, и `EnrollmentDate`. База данных автоматически создает значение идентификатора, а хранимая процедура принимает параметры для других трех. Хранимая процедура возвращает значение ключа записи новой строки, чтобы платформа Entity Framework можно хранить список, в версии сущности, он сохраняет в памяти.

Сохраните и закройте окно хранимой процедуры.

Создание `InsertInstructor` хранимую процедуру таким же образом с помощью следующих инструкций SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Создание `Update` хранимых процедур для `Student` и `Instructor` сущности также. (База данных уже содержит `DeletePerson` хранимую процедуру, которая работает в обоих `Instructor` и `Student` сущности.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

В этом учебнике необходимо сопоставить все три функции--insert, update и delete — для каждого типа сущности. Entity Framework версии 4 позволяет сопоставить только один или два из этих функций с хранимыми процедурами без сопоставления другими, за одним исключением: Если сопоставить функции обновления, но не функцию удаления, Entity Framework будет вызывать исключение при вы Попытка удалить сущность. В Entity Framework версии 3.5 не было такой большой объем гибкость при сопоставлении хранимых процедур: при сопоставлении одной функции нужно было сопоставить все три.

Чтобы создать хранимую процедуру, которая считывает, а не обновляет данные, создайте один, который выбирает все `Course` сущности, с помощью следующих инструкций SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Добавление модели данных хранимые процедуры

Теперь определенных хранимых процедур в базе данных, но они должны быть добавлены в модель данных, чтобы сделать их доступными на платформу Entity Framework. Откройте *SchoolModel.edmx*, щелкните правой кнопкой мыши область конструктора и выберите **обновление модели из базы данных**. В **добавить** вкладке **Выбор объектов базы данных** диалогового окна разверните **хранимых процедур**, выберите только что созданный хранимые процедуры и `DeletePerson` Хранимая процедура, а затем нажмите кнопку **Готово**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Сопоставление хранимых процедур

Щелкните правой кнопкой мыши в конструкторе моделей, `Student` сущность и выберите **сопоставление хранимых процедур**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**Сведения о сопоставлении** появится окно, в котором можно указать хранимые процедуры, которые платформа Entity Framework следует использовать для вставки, обновления и удаления сущностей этого типа.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Задать **вставить** функция **InsertStudent**. В окне отображается список параметров хранимых процедур, каждый из которых должны сопоставляться со свойством сущности. Два из них автоматически сопоставляются потому, что имена одинаковы. Отсутствует свойство сущности с именем `FirstName`, поэтому необходимо вручную выбрать `FirstMidName` из раскрывающегося списка, который отображает доступную сущность свойства. (Это, поскольку вы изменили имя `FirstName` свойства `FirstMidName` в первом руководстве.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

В том же **сведения о сопоставлении** окна, карты `Update` функция `UpdateStudent` хранимой процедуры (Убедитесь, что указан `FirstMidName` как значение параметра для `FirstName`, как это было сделано для `Insert` Хранимая процедура) и `Delete` функция `DeletePerson` хранимой процедуры.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Выполните ту же процедуру, чтобы сопоставить insert, update и delete хранимых процедур для инструкторов для `Instructor` сущности.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Для хранимых процедур с указано, а не обновление данных, используйте **браузер моделей** окна, чтобы сопоставить хранимую процедуру для сущности типа возвращает. В конструкторе моделей щелкните правой кнопкой мыши область конструктора и выберите **браузер моделей**. Откройте **SchoolModel.Store** узел и откройте **хранимых процедур** узла. Щелкните правой кнопкой мыши `GetCourses` хранимой процедуры и выберите **добавьте импорт функции**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

В **добавьте импорт функции** диалогового **возвращает коллекцию** выберите **сущностей**и выберите `Course` как тип сущности возвращается. Когда закончите, нажмите кнопку **ОК**. Сохраните и закройте *.edmx* файла.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Использование инструкции Insert, Update и Delete хранимых процедур

Хранимые процедуры для вставки, обновления и удаления данных используются платформой Entity Framework автоматически после их добавления в модель данных и сопоставить их в соответствующие сущности. Теперь можно запустить *StudentsAdd.aspx* страницы, и каждый раз при создании нового студента, Entity Framework будет использовать `InsertStudent` хранимую процедуру, чтобы добавить новую строку `Student` таблицы.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Запустите *Students.aspx* страницы и нового студента появится в списке.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Измените имя, чтобы проверить работоспособность функции обновления, а затем удалите студента для проверки работы функцию удаления.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Выберите хранимые процедуры с помощью

Платформа Entity Framework не запускается автоматически хранимых процедур например `GetCourses`, и не могут использовать их с `EntityDataSource` элемента управления. Чтобы использовать их, вызывать их из кода.

Откройте *InstructorsCourses.aspx.cs* файла. `PopulateDropDownLists` Метод использует запрос LINQ-сущностей для получения всех сущностей курс, чтобы он циклическую обработку списка и определить, какие из них назначен инструктор и какие из них не были назначены:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Замените следующий код:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

На странице теперь используется `GetCourses` хранимой процедуры для получения списка всех курсов. Запустите страницу, чтобы убедиться, что он работает, как раньше.

(Свойства навигации сущностей, полученные с помощью хранимой процедуры может не будут заполнены автоматически данные, связанные с этими сущностями, в зависимости от `ObjectContext` параметры по умолчанию. Дополнительные сведения см. в разделе [загрузка связанных объектов](https://msdn.microsoft.com/library/bb896272.aspx) в библиотеке MSDN.)

В следующем уроке будет рассмотрено использование функциональных возможностей платформы динамических данных для упрощения правила программы и тестирования данных форматирования и проверки. Вместо указания для каждого правила веб-страницы, такие как строки формата данных и ли поле является обязательным, можно указать такие правила в метаданных модели данных и выполняется автоматически, они применены на каждой странице.

>[!div class="step-by-step"]
[Назад](the-entity-framework-and-aspnet-getting-started-part-6.md)
[Вперед](the-entity-framework-and-aspnet-getting-started-part-8.md)
