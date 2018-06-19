---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Использование Entity Framework 4.0 и управления ObjectDataSource, часть 1: Приступая к работе | Документы Microsoft'
author: tdykstra
description: Этот учебник ряд строится на веб-приложение Contoso университета, созданный Приступая к работе с рядами учебника Entity Framework. Если ё...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 6584767418c898913777b3b1549a816679c8430d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892086"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Использование Entity Framework 4.0 и управления ObjectDataSource, часть 1: Приступая к работе
====================
по [Tom Dykstra](https://github.com/tdykstra)

> Этот учебник ряд основан на веб-приложение Contoso университета, созданный [Приступая к работе с Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) учебника рядов. Если не была завершена ранее учебники, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , будет создана. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , создаваемый для завершения учебника ряда.
> 
> Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010. Образец приложения это веб-сайт для вымышленной компании Contoso университета. На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей.
> 
> Учебник показаны примеры на языке C#. [Загружаемые примеры](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) содержит код на C# и Visual Basic.
> 
> ## <a name="database-first"></a>Сначала базы данных
> 
> Существует три способа, можно работать с данными в Entity Framework: *Database First*, *Model First*, и *Code First*. Этот учебник предназначен для первой базы данных. Сведения о различиях между этими рабочими процессами и рекомендации о том, как выбирать наиболее подходящий для вашего сценария см. в разделе [процессов разработки Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Веб-формы
> 
> Как и ряда Приступая к работе этого учебника ряда использует модель веб-форм ASP.NET и предполагается, что вы знаете, как работать с веб-форм ASP.NET в Visual Studio. Если отсутствует [Приступая к работе с веб-форм ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Если вы предпочитаете работать с ASP.NET MVC, см. раздел [Приступая к работе с платформой Entity Framework, с помощью ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Версии программного обеспечения
> 
> | **В этом учебнике показано** | **Также работает с** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express для Web. Учебник не тестировался с более поздними версиями Visual Studio. Существует много различий в выбранных элементов меню, диалоговых окон и шаблоны. |
> | .NET 4 | .NET 4.5 имеет обратную совместимость с .NET 4, но с .NET 4.5 не тестировался учебника. |
> | Entity Framework 4 | Учебник не тестировался с более поздними версиями платформы Entity Framework. Начиная с Entity Framework 5, по умолчанию используется EF `DbContext API` , впервые появилась в EF 4.1. Элемент управления EntityDataSource была разработана для использования `ObjectContext` API. Дополнительные сведения об использовании управления EntityDataSource было управлять с помощью `DbContext` API, в разделе [этой записи блога](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Вопросы
> 
> Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework и LINQ to Entities форум](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), или [ StackOverflow.com](http://stackoverflow.com/).


`EntityDataSource` Управления позволяет создавать приложения, очень быстро, но обычно требуется сохранить значительный объем бизнес-логики и логики доступа к данным в вашей *.aspx* страниц. Если ожидается, что ваше приложение к росту сложности и требует текущего обслуживания, можно заранее вкладывать больше времени разработки для создания *n уровневые* или *многоуровневая* структуры приложения Это более простого в сопровождении. Для реализации этой архитектуры, отделить уровень представления из уровня бизнес-логики (уровень бизнес-ЛОГИКИ) и уровня доступа к данным (DAL). Один из способов реализации этой структуры является использование `ObjectDataSource` управления вместо `EntityDataSource` элемента управления. При использовании `ObjectDataSource` управления, можно реализовать собственный код для доступа к данным, а затем вызовите его в *.aspx* страниц с помощью элемента управления, обладающей множеством одной и той же функции, как другие элементы управления источником данных. Это позволяет сочетать преимущества многоуровневого подхода со всеми преимуществами управления веб-форм для доступа к данным.

`ObjectDataSource` Управления обеспечивает большую гибкость, а также другие способы. Так, как написать собственный код доступа к данным, проще не только чтения, вставки, обновления или удаления определенного типа сущности, которые являются задачами, `EntityDataSource` элемент управления предназначен для выполнения. Например можно выполнить вход каждый раз при обновлении сущности, архивировать данные при каждом удалении сущности или автоматически проверка и обновление связанных данных при необходимости при вставке строки со значением внешнего ключа.

## <a name="business-logic-and-repository-classes"></a>Бизнес-логики и классы репозитория

`ObjectDataSource` Работы элемента управления с помощью вызова класса, который создан. Класс содержит методы для извлечения и обновления данных и укажите имена этих методов для `ObjectDataSource` элемента управления в разметке. Во время подготовки к просмотру или обратной передачи `ObjectDataSource` вызывает методы, которые вы указали.

Помимо основные операции CRUD, класса, который создан для использования с `ObjectDataSource` управления может потребоваться выполнение бизнес-логики при `ObjectDataSource` чтения или обновления данных. Например при обновлении отдел может потребоваться убедитесь, что нет других отделов есть ту же администратора, так как один пользователь не может быть администратором более одного отдела.

В некоторых `ObjectDataSource` документации, таких как [Общие сведения о классе ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), элемент управления вызывает класс, называемый *бизнес-объекта* бизнес-логики и логики доступа к данным . В этом учебнике вы создадите отдельные классы для бизнес-логики и логики доступа к данным. Класс, инкапсулирующий логики доступа к данным, называется *репозитория*. Бизнес логики класса включает бизнес логики методы и методы доступа к данным, но методы доступа к данным вызова репозитория для выполнения задач, доступ к данным.

Также создается уровень абстракции между уровень бизнес-ЛОГИКИ и DAL, упрощающий автоматических модульных тестов для МЕТОДА. Этот уровень абстракции реализуется путем создания интерфейса и с помощью интерфейса, при создании экземпляра репозитория в классе бизнес логики. Это позволяет предоставить класс бизнес логики со ссылкой на любой объект, реализующий интерфейс репозитория. Для нормальной работы необходимо предоставить объект хранилища, который работает с платформой Entity Framework. Для тестирования, необходимо предоставить объект хранилища, который работает с данными, хранящимися в результате которого можно легко управлять, например класс переменные, определенные в коллекции.

Разница между класс бизнес логики, который включает логики доступа к данным без репозитория, а вторая использует репозиторий на следующем рисунке.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Сначала выполняется создание веб-страниц, в котором `ObjectDataSource` непосредственно в репозиторий привязан элемент управления, так как только он выполняет задачи базовый доступ к данным. В следующем уроке будет создать класс бизнес-логики с логикой проверки и привязать `ObjectDataSource` управления для этого класса, а не класс репозитория. Также вы создадите модульные тесты для логики проверки. В третьем учебнике этой серии вы добавите функций для применения сортировки и фильтрации.

Страницы, создайте в этом учебнике работать в `Departments` набора сущностей модели данных, созданную в [начало учебника ряда](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Обновление базы данных и модели данных

Сначала этот учебник, делая два изменения в базе данных, для которых требуется соответствующих изменений в модель данных, созданную в [Приступая к работе с платформой Entity Framework и веб-форм](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) учебники. В одном из этих учебников внесены изменения в конструкторе вручную для синхронизации модели данных с базой данных после изменения базы данных. В этом учебнике будет использоваться конструктор **обновление из базы данных Model** средство для автоматического обновления данных модели.

### <a name="adding-a-relationship-to-the-database"></a>Добавление отношения к базе данных

В Visual Studio откройте веб-приложение Contoso университета, вы создали в [Приступая к работе с платформой Entity Framework и веб-форм](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) учебника рядов, а затем откройте `SchoolDiagram` диаграммы базы данных.

Если взглянуть на `Department` таблицы в схеме базы данных, вы увидите, что она имеет `Administrator` столбца. Этот столбец является внешним ключом для `Person` таблицы, но не связи по внешнему ключу определен в базе данных. Необходимо создать связь и обновление модели данных Entity Framework может автоматически обрабатывать эту связь.

В схеме базы данных, щелкните правой кнопкой мыши `Department` и выберите **связи**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

В **связи по внешним ключам** выберите **добавить**, нажмите кнопку с многоточием для **Спецификация таблиц и столбцов**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

В **таблицы и столбцы** диалогового окна поле, задайте таблице первичного ключа и полю `Person` и `PersonID`и задать таблицы внешнего ключа и полю `Department` и `Administrator`. (При этом имя связи изменится с `FK_Department_Department` для `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Нажмите кнопку **ОК** в **таблицы и столбцы** щелкните **закрыть** в **связи по внешним ключам** и сохраните изменения. Ответ на вопрос, следует ли сохранить `Person` и `Department` , нажмите **Да**.

> [!NOTE]
> Если вы удалили `Person` строки, которые относятся к данным, уже имеется в `Administrator` столбца, не можно сохранить это изменение. В этом случае использовать редактор таблиц в **обозревателя серверов** , чтобы `Administrator` значение в каждой `Department` строка содержит идентификатор записи, которая действительно существует в `Person` таблицы.
> 
> После сохранения изменений, вы не сможете удалить строку из `Person` таблица, если этот пользователь является администратором отдела. В реальном приложении будет предоставлен сообщение об ошибке при ограничения базы данных не допускает удаления или указании каскадное удаление. Пример указания каскадное удаление см. в разделе [платформа Entity Framework и ASP.NET — часть 2 Приступая к работе](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Добавление представления в базу данных

В новом *Departments.aspx* страницы, будет создана, необходимо предоставить список раскрывающегося списка инструкторов, с именами в формате «Фамилия, имя» так, чтобы пользователи могли выбирать Администраторы отдела. Для упрощения этого будет создано представление в базе данных. Представление будет содержать только данные, необходимые для раскрывающегося списка: полное имя (правильно отформатированные) и ключа записи.

В **обозревателя серверов**, разверните *файл School.mdf*, щелкните правой кнопкой мыши **представления** папки, а затем выберите **добавить новое представление**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Нажмите кнопку **закрыть** при **добавить таблицу** диалоговое окно появляется и вставьте следующую инструкцию SQL на панели SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Сохранение представления как `vInstructorName`.

### <a name="updating-the-data-model"></a>Обновление модели данных

В *DAL* откройте *SchoolModel.edmx* файл, щелкните правой кнопкой мыши область конструктора и выберите **обновление модели из базы данных**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

В **Выбор объектов базы данных** выберите **добавить** и выберите только что созданное представление.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Нажмите кнопку **Готово**.

В конструкторе, вы видите, что средство создания `vInstructorName` сущности и новой связи между `Department` и `Person` сущности.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> В **вывода** и **список ошибок** windows, может появиться предупреждающее сообщение, информирующее о том, что средство автоматически создается первичный ключ для нового `vInstructorName` представления. Это нормальная ситуация.


При ссылке на новый `vInstructorName` сущности в коде, вы не хотите использовать префикса строчный символ «v» к нему правило базы данных. Таким образом переименовании сущностью и сущностью, определенную в модели.

Откройте **модели браузера**. Вы видите `vInstructorName` указано как тип сущности и представления.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

В разделе **SchoolModel** (не **SchoolModel.Store**), щелкните правой кнопкой мыши **vInstructorName** и выберите **свойства**. В **свойства** измените **имя** «InstructorName» и измените свойства **имя набора сущностей** свойства «InstructorNames».

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Сохраните и закройте модели данных и затем перестройте проект.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>С помощью класса репозитория и элемент управления ObjectDataSource

Создать новый файл класса в *DAL* папки, назовите его *SchoolRepository.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Этот код предоставляет одну `GetDepartments` метод, возвращающий все сущности в `Departments` набора сущностей. Поскольку вам известно, что вам необходим доступ к `Person` свойство навигации для каждой строки получаемой, указать упреждающая загрузка для этого свойства с помощью `Include` метод. Класс также реализует `IDisposable` интерфейса, чтобы убедиться, что подключение к базе данных освобождается, когда удаляется объект.

> [!NOTE]
> Распространенной практикой является создание класса репозитория для каждого типа сущности. В этом учебнике используется один класс репозиторий для нескольких типов сущностей. Дополнительные сведения о шаблон репозитория см. в записях в [блог группы Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) и [Джули Лерман блог](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


`GetDepartments` Возвращает `IEnumerable` объекта, а не `IQueryable` объекта, для того чтобы убедиться, что возвращаемой коллекции можно использовать, даже после удаления самого объекта репозитория. `IQueryable` Объекта может привести к доступ к базе данных каждый раз, когда осуществляется, но объект репозитория может быть удален при попытке элемента управления с привязкой к данным для отображения данных. Может вернуть другой тип коллекции, такие как `IList` объекта вместо `IEnumerable` объекта. Тем не менее, возвращая `IEnumerable` объекта гарантирует, такие как выполнение задачи обработки типичных только для чтения список `foreach` циклы и запросы LINQ, но нельзя добавить или удалить элементы из коллекции может подразумеваться, что такие изменения будет сохранить в базе данных.

Создание *Departments.aspx* страницы, использующей *Site.Master* главную страницу и добавьте следующую разметку в `Content` управления с именем `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Эта разметка создает `ObjectDataSource` управления, использующий класс репозитория, который вы только что создали, и `GridView` элемента управления для отображения данных. `GridView` Элемент управления задает **изменить** и **удалить** команды, но не был добавлен код, чтобы еще поддерживается.

Используйте несколько столбцов `DynamicField` элементы управления, чтобы воспользоваться преимуществами данных автоматического форматирования и проверки функциональности. Для их работы, необходимо вызвать `EnableDynamicData` метод в `Page_Init` обработчика событий. (`DynamicControl` элементы не используются в `Administrator` поле, так как они не работают со свойствами навигации.)

`Vertical-Align="Top"` Атрибутов может быть важно позже при добавлении столбца, имеющего вложенный `GridView` элемента управления в сетке.

Откройте *Departments.aspx.cs* файл и добавьте следующее `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Затем добавьте следующий обработчик для страницы `Init` событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

В *DAL* папки, создайте новый файл класса с именем *Department.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Этот код добавляет метаданные в модель данных. Указывает, что `Budget` свойство `Department` сущности фактически представляет валюту, несмотря на то, что его тип данных — `Decimal`, оно указывает, что значение должно быть между 0 и 1,000,000.00 $. Также указывает, что `StartDate` свойства должен иметь формат даты в формате мм/дд/гггг.

Запустите *Departments.aspx* страницы.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Обратите внимание, что, несмотря на то, что не было указано в строке формата *Departments.aspx* разметку страницы для **бюджета** или **Дата начала** столбцы, по умолчанию, валюты и даты форматирование, примененных к ним, `DynamicField` элементов управления, используя метаданные, предоставленные в *Department.cs* файла.

## <a name="adding-insert-and-delete-functionality"></a>Добавление Insert и Delete функциональные возможности

Откройте *SchoolRepository.cs*, добавьте следующий код для создания `Insert` метод и `Delete` метод. Код также содержит метод с именем `GenerateDepartmentID` , вычисляет следующей доступной записи значение ключа для использования `Insert` метод. Это необходимо, так как база данных не настроена для вычисления автоматически для `Department` таблицы.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach-метод

`DeleteDepartment` Вызовы метода `Attach` метод, чтобы повторно установить ссылку, которая сохраняется в контексте объекта диспетчер состояния объекта между сущностями в памяти и базу данных строки, он представляет. Это должно произойти перед вызовом метода `SaveChanges` метод.

Термин *контекста объекта* ссылается на класс Entity Framework, который является производным от `ObjectContext` класс, используемый для доступа к наборам сущностей и сущностей. В коде для данного проекта класс с именем `SchoolEntities`, а его экземпляр всегда совпадает с именем `context`. Контекст объекта *диспетчер состояния объекта* — это класс, производный от `ObjectStateManager` класса. Контакт объект использует диспетчер состояния объекта, для хранения объектов сущностей и для отслеживания ли каждый из них в соответствии с его соответствующей строки таблицы или строк в базе данных.

При чтении сущности контекста объекта сохраняет его в диспетчер состояния объекта и отслеживает ли это представление объекта синхронизации с базой данных. Например при изменении значения свойства флаг имеет значение указывает, что свойство, которое вы изменили больше не является синхронизации с базой данных. Затем при вызове `SaveChanges` метод контекст объекта знает, что делать в базе данных, так как диспетчер состояния объекта знает именно то, что текущее состояние сущности и состояние базы данных отличаются.

Тем не менее этот процесс обычно не работает в веб-приложения, так как экземпляр контекста объекта, который считывает сущности, а также все его диспетчер состояния объекта, удаляется после подготовки страницы к просмотру. Экземпляр контекста объекта, который необходимо применить изменения является новым, экземпляр которого создается для обработки обратной передачи. В случае использования `DeleteDepartment` метода `ObjectDataSource` управления повторно создает исходную версию сущности из значений в состоянии представления, но это повторно создать `Department` сущность не существует в диспетчер состояния объекта. Если вызван `DeleteObject` метод в этой сущности будет создана повторно, вызов завершится ошибкой, так как контекст объекта не знаете, является ли сущность синхронизации с базой данных. Однако, при вызове `Attach` метод повторно устанавливает же отслеживания между значениями и повторно созданной сущности в базе данных, изначально было выполнено автоматически после считывания в более ранней версии экземпляра контекста объекта сущности.

Существуют ситуации, когда необходимо исключить из контекста объекта для отслеживания сущностей в диспетчер состояния объекта, и можно задать флаги, чтобы предотвратить это сделать. В качестве примера отображаются на следующих занятиях рассматривается в этой серии.

### <a name="the-savechanges-method"></a>Метод SaveChanges

Этот класс простой репозиторий рассмотрены основные принципы для выполнения операций CRUD. В этом примере `SaveChanges` метод вызывается сразу после каждого обновления. В реальном приложении может потребоваться вызвать `SaveChanges` метод отдельный метод, чтобы получить больший контроль над при обновлении базы данных. (В конце следующем уроке вы найдете ссылку на технический документ, посвященный единицы работы шаблону, который является одним из подходов для координации соответствующие обновления.) Обратите внимание, что в примере `DeleteDepartment` метода не включает код для обработки конфликтов параллелизма; будет добавлен код, чтобы сделать это позднее в этом учебнике этой серии.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Получение инструктора имена для выбора при вставке

Пользователи должны быть доступны для выбора администратор список инструкторов в раскрывающемся списке при создании нового подразделения. Таким образом, добавьте следующий код в *SchoolRepository.cs* Создание метода на получение списка инструкторов, с помощью представления, созданного ранее:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Создание страницы для вставки отделов

Создание *DepartmentsAdd.aspx* страницы, использующей *Site.Master* страницы и добавьте следующую разметку в `Content` управления с именем `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Эта разметка создает два `ObjectDataSource` контролирует, один для вставки новых `Department` сущностей и один для извлечения имен инструктора `DropDownList` элемент управления, используемый для выбора Администраторы отдела. Разметка создает `DetailsView` управления для ввода нового подразделения и он определяет обработчик для элемента управления `ItemInserting` событий, которые можно использовать `Administrator` значение внешнего ключа. По окончании `ValidationSummary` элемента управления для отображения сообщений об ошибках.

Откройте *DepartmentsAdd.aspx.cs* и добавьте следующее `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Добавьте следующую переменную классов и методов:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init` Метод включает функциональные возможности платформы динамических данных. Обработчик `DropDownList` элемента управления `Init` событий сохраняет ссылку на элемент управления и обработчик для `DetailsView` элемента управления `Inserting` событий использует эту ссылку, чтобы получить `PersonID` значение выбранного инструктора и обновления `Administrator` свойство внешнего ключа из `Department` сущности.

Запустите страницу, добавьте сведения для нового подразделения и нажмите кнопку **вставить** ссылку.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Введите значения для другого новое подразделение. Введите число больше 1,000,000.00 в **бюджета** поля и вкладки в следующее поле. В этом поле отображается звездочка, и если навести указатель мыши на его может появиться сообщение об ошибке, введенного в метаданных для этого поля.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Нажмите кнопку **вставить**, и появляется сообщение об ошибке `ValidationSummary` элемента управления в нижней части страницы.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Затем закройте окно браузера и откройте *Departments.aspx* страницы. Реализовать возможность удаления для *Departments.aspx* страницы путем добавления `DeleteMethod` атрибут `ObjectDataSource` управления и `DataKeyNames` атрибут `GridView` элемента управления. Открывающие теги для этих элементов управления, теперь будет выглядеть примерно так:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Запустите страницу.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Удаление подразделения, добавленный при выполнении *DepartmentsAdd.aspx* страницы.

## <a name="adding-update-functionality"></a>Добавление функциональных возможностей обновления

Откройте *SchoolRepository.cs* и добавьте следующее `Update` метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

При нажатии кнопки **обновление** в *Departments.aspx* страницы, `ObjectDataSource` управления создает два `Department` сущностей для передачи `UpdateDepartment` метод. Одна из них содержит исходные значения, которые сохранены в состоянии представления, а другой содержит новые значения, которые были введены в `GridView` элемента управления. Код в `UpdateDepartment` метод передает `Department` сущность, которая содержит исходные значения для `Attach` метод, чтобы установить отслеживания между сущностью и возможности базы данных. Затем код передает `Department` сущность, которая содержит новые значения для `ApplyCurrentValues` метод. Контекст объекта сравнивает старое и новое значения. Если новое значение отличается от старое, контекст объекта изменяется значение свойства. `SaveChanges` Обновляет только измененные столбцы в базе данных. (Тем не менее, если функции обновления для этой сущности были сопоставлены с хранимой процедурой, то вся строка будет обновлено независимо от того, что столбцы были изменены.)

Откройте *Departments.aspx* и добавьте следующие атрибуты, чтобы `DepartmentsObjectDataSource` управления:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Это причины старые значения сохраняются в состоянии представления, чтобы их можно сравнивать с новыми значениями в `Update` метод.
- `OldValuesParameterFormatString="orig{0}"`   
 Это позволяет сообщить элемента управления, имя исходного значения параметра `origDepartment` .

Разметку для открывающего тега элемента `ObjectDataSource` теперь элемент управления похож на приведенный ниже:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Добавить `OnRowUpdating="DepartmentsGridView_RowUpdating"` атрибут `GridView` элемента управления. Который будет использоваться для задания `Administrator` значение свойства на основе строки пользователь выбирает в раскрывающемся списке. `GridView` Теперь открывающий тег приведенной в следующем примере:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Добавить `EditItemTemplate` управления `Administrator` столбец `GridView` управления сразу же после `ItemTemplate` элемента управления для этого столбца:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Это `EditItemTemplate` управления аналогичен `InsertItemTemplate` управления *DepartmentsAdd.aspx* страницы. Различие заключается в том, что исходное значение элемента управления устанавливается с помощью `SelectedValue` атрибута.

Прежде чем `GridView` управления, добавьте `ValidationSummary` управления, как описано в *DepartmentsAdd.aspx* страницы.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Откройте *Departments.aspx.cs* и сразу после объявления разделяемого класса, добавьте следующий код, чтобы создать частное поле для ссылки на `DropDownList` управления:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Затем добавьте обработчики для `DropDownList` элемента управления `Init` событий и `GridView` элемента управления `RowUpdating` событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Обработчик `Init` событий сохраняет ссылку на `DropDownList` элемента управления в поле класса. Обработчик `RowUpdating` событий использует ссылку для получения значения, введенные пользователем и примените его к `Administrator` свойство `Department` сущности.

Используйте *DepartmentsAdd.aspx* страницы для добавления нового отдела, а затем запустите *Departments.aspx* и нажмите кнопку **изменить** на строку, которая была добавлена.

> [!NOTE]
> Вы не сможете изменить строки, которые вы не добавляли (то есть, уже есть в базе данных), из-за недопустимых данных в базе данных. Администраторы для строк, которые были созданы с базой данных, являются учащихся. При попытке изменить один из них, вы получите страницу ошибки, который сообщает об ошибке `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

При вводе недопустимого **бюджета** сумму, а затем нажмите кнопку **обновление**, вы видите же звездочкой и сообщение об ошибке, в *Departments.aspx* страницы.

Измените значение поля или выберите другой администратор и нажмите кнопку **обновление**. Изменение отображается.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

На этом завершается сведения об использовании `ObjectDataSource` элемента управления для основных CRUD (Создание, чтение, обновление и удаление) операций с платформой Entity Framework. После создания простого n уровневого приложения, но по-прежнему тесно связана бизнес-логики на уровень доступа к данным, что осложняет автоматическое тестирование модулей. В этом руководстве вы увидите, как реализовать шаблон репозитория для применения модульного тестирования.

> [!div class="step-by-step"]
> [Вперед](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
