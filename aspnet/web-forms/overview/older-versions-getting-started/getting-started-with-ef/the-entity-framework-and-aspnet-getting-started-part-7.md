---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms - часть 7 | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложений веб-форм ASP.NET, с помощью Entity Framework. Пример приложения является...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb1abe52b913785f47b7f8ec9822ed9db5ee8a05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823411"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms - часть 7
====================
по [том Дайкстра](https://github.com/tdykstra)

> Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010. Сведения об этой серии руководств см. в разделе [в первом учебнике серии](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Использование хранимых процедур

В предыдущем учебнике было реализовано шаблон таблица на иерархию наследования. Этом учебнике показано, как использовать хранимые процедуры для получения большего контроля над доступа к базе данных.

Платформа Entity Framework позволяет указать, следует использовать хранимые процедуры для доступа к базе данных. Для любого типа сущности можно указать хранимую процедуру для создания, обновления или удаления сущностей этого типа. Затем в модели данных можно добавить ссылки на хранимые процедуры, которые можно использовать для выполнения задач, таких как извлечение наборов сущностей.

Использование хранимых процедур является общим требованием для доступа к базе данных. В некоторых случаях администратор базы данных может потребоваться, что все доступа к базе данных проходят через хранимые процедуры, по соображениям безопасности. В других случаях может потребоваться создать бизнес-логики в некоторые процессы Entity Framework, используемых при обновлении базы данных. Например при каждом удалении сущности можно скопировать его в базу данных архива. Или всякий раз, когда строка обновляется для записи строки в таблицу журнала, записываемых, выполнившей изменение. Такого рода задач можно выполнять в хранимую процедуру, которая вызывается каждый раз, когда Entity Framework удаляет сущность или обновляет сущность.

Как и в предыдущем руководстве вы не создадите новые страницы. Вместо этого вам предстоит изменить способ, Entity Framework обращается к базе данных для некоторых страниц, вы уже создали.

В этом руководстве вы создадите хранимых процедур в базе данных для вставки `Student` и `Instructor` сущностей. Вы добавите их в модель данных, и потребуется указать, что платформа Entity Framework следует использовать их для добавления `Student` и `Instructor` сущностей в базу данных. Вы также создадите хранимую процедуру, которая используется для извлечения `Course` сущностей.

## <a name="creating-stored-procedures-in-the-database"></a>Создание хранимых процедур в базе данных

(Если вы используете *файл School.mdf* файл из проекта, можно загрузить с этим руководством, можно пропустить этот раздел, поскольку уже существуют хранимые процедуры.)

В **обозревателя серверов**, разверните *файл School.mdf*, щелкните правой кнопкой мыши **хранимые процедуры**и выберите **добавить новую хранимую процедуру**.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Скопируйте следующие инструкции SQL и вставьте их в окно хранимой процедуры, заменив скелет хранимой процедуры.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![Image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` сущности имеют четыре свойства: `PersonID`, `LastName`, `FirstName`, и `EnrollmentDate`. База данных автоматически создает значение идентификатора, а хранимая процедура принимает параметры для других трех. Хранимая процедура возвращает значение ключа записи новой строки, чтобы платформа Entity Framework можно отслеживать, что в версии сущности, которую он сохраняет в памяти.

Сохраните и закройте окно хранимой процедуры.

Создание `InsertInstructor` хранимую процедуру таким же образом, с помощью следующих инструкций SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Создание `Update` хранимых процедур для `Student` и `Instructor` сущности также. (База данных уже содержит `DeletePerson` хранимую процедуру, которая работает в обоих `Instructor` и `Student` сущностей.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

В этом руководстве необходимо сопоставить все три функции--insert, update и delete — для каждого типа сущности. Платформа Entity Framework версии 4 позволяет сопоставить только один или два из этих функций для хранимых процедур без сопоставления с другими, за одним исключением: Если сопоставить функцию обновления, но не функцию удаления, Entity Framework приведет к возникновению исключения при вы Попытка удалить сущность. В Entity Framework версии 3.5, можно было не такой большой объем гибкость при сопоставлении хранимых процедур: Если сопоставить одну функцию нужно было сопоставить все три.

Чтобы создать хранимую процедуру, которая считывает, а не обновляет данные, создать новый, выбирает все `Course` сущностей, с помощью следующих инструкций SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Добавление модели данных хранимые процедуры

Теперь определенные хранимые процедуры в базе данных, но они должны быть добавлены в модель данных, чтобы сделать их доступными на платформу Entity Framework. Откройте *SchoolModel.edmx*, щелкните правой кнопкой мыши область конструктора и выберите **обновить модель из базы данных**. В **добавить** вкладке **Выбор объектов базы данных** диалогового окна разверните узел **хранимые процедуры**, выберите только что созданный хранимые процедуры и `DeletePerson` Хранимая процедура, а затем нажмите кнопку **Готово**.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Сопоставление хранимых процедур

В конструкторе моделей, щелкните правой кнопкой мыши `Student` сущности и выберите **сопоставление хранимых процедур**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

**Сведения о сопоставлении** появится окно, в котором можно указать хранимые процедуры, которые Entity Framework следует использовать для вставки, обновления и удаления сущностей этого типа.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Задайте **вставить** функцию **InsertStudent**. В окне отображаются список параметров хранимых процедур, каждый из которых должен быть сопоставлен со свойством сущности. Два из них сопоставляются автоматически, так как имена совпадают. Отсутствует свойство сущности с именем `FirstName`, поэтому необходимо вручную выбрать `FirstMidName` из приведены свойства доступных сущностей в списке. (Это так, как вы изменили имя `FirstName` свойства `FirstMidName` в первом руководстве.)

[![Image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

В том же **сведения о сопоставлении** окна, карты `Update` функцию `UpdateStudent` хранимой процедуры (Убедитесь, что указано `FirstMidName` как значение параметра для `FirstName`, как это было сделано для `Insert` Хранимая процедура) и `Delete` функцию `DeletePerson` хранимой процедуры.

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Выполните ту же процедуру, чтобы сопоставить insert, update и delete хранимых процедур для инструкции выделялись на `Instructor` сущности.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Для хранимых процедур, которые считывают вместо того, чтобы обновить данные, использовать **браузер моделей** окно, чтобы сопоставить хранимую процедуру с сущностью вводить его возвращает. В конструкторе моделей, щелкните правой кнопкой мыши область конструктора и выберите **браузер моделей**. Откройте **SchoolModel.Store** узел и откройте **хранимые процедуры** узла. Щелкните правой кнопкой мыши `GetCourses` хранимой процедуры и выберите **добавить импорт функции**.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

В **добавить импорт функции** диалогового **возвращает коллекцию** выберите **сущностей**, а затем выберите `Course` как тип сущности возвращаются. Когда все будет готово, нажмите кнопку **ОК**. Сохраните и закройте *.edmx* файл.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>С помощью вставки, обновления и удаления хранимых процедур

Хранимые процедуры для вставки, обновления и удаления данных используются платформой Entity Framework автоматически после их добавления в модель данных и сопоставить их в соответствующие сущности. Теперь вы можете запустить *StudentsAdd.aspx* страницы, и каждый раз при создании нового учащегося, Entity Framework будет использовать `InsertStudent` хранимую процедуру для добавления новой строки к `Student` таблицы.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Запустите *Students.aspx* страницы и нового учащегося появится в списке.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Изменить имя, чтобы убедиться, что функция обновления работает и удалите учащегося, чтобы убедиться, что функция удаления работает.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Использование Select хранимых процедур

Платформа Entity Framework не запускаются автоматически хранимых процедур например `GetCourses`, и не может использовать их с помощью `EntityDataSource` элемента управления. Чтобы воспользоваться ими, их можно вызывать из кода.

Откройте *InstructorsCourses.aspx.cs* файла. `PopulateDropDownLists` Метод использует запрос LINQ to Entities для получения всех сущностей course, чтобы он мог циклического прохода по списку и определить, какие из них назначается преподавателя и какие из них не были назначены:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Замените следующий код:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

На странице теперь используется `GetCourses` хранимой процедуры для получения списка всех курсов. Запустите страницу, чтобы убедиться, что оно работает, как и раньше.

(Свойства навигации сущностей, полученные с помощью хранимой процедуры не могут быть заполнены автоматически с данными, относящимися к этим сущностям, в зависимости от `ObjectContext` параметры по умолчанию. Дополнительные сведения см. в разделе [загрузка связанных объектов](https://msdn.microsoft.com/library/bb896272.aspx) в библиотеке MSDN.)

В следующем руководстве вы узнаете, как использовать функциональные возможности платформы динамических данных для упрощения программы тестирования данных форматирования и проверки правила и. Вместо указания на каждой веб-страницу правил, таких как строки формата данных и того, является ли поле является обязательным, можно указать такие правила в метаданных модели данных, и они автоматически применяются на каждой странице.

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-8.md)
