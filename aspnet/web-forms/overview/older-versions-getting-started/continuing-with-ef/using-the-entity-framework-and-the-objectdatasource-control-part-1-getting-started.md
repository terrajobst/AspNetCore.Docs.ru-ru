---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'С помощью Entity Framework 4.0 и элемент управления ObjectDataSource, часть 1: Приступая к работе | Документация Майкрософт'
author: tdykstra
description: В этой серии руководств основана на веб-приложение университета Contoso, созданный по началу работы с этой серии руководств Entity Framework. Если yo...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 5eaeaa0aa474e1aed86954e6c10dd1703b938944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836697"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>С помощью Entity Framework 4.0 и элемент управления ObjectDataSource, часть 1: Приступая к работе
====================
по [том Дайкстра](https://github.com/tdykstra)

> В этой серии руководств основан на веб-приложение университета Contoso, которая создается с [Приступая к работе с Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) серии руководств. Если вы не прошли предыдущих учебных курсах, в качестве отправной точки для этого учебника вы можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , он был создан. Вы также можете [загрузить приложение](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , созданный путем завершения серии руководств.
> 
> Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010. Пример приложения веб-сайт вымышленного университета Contoso. На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей.
> 
> Этом руководстве показаны примеры на языке C#. [Загружаемый пример](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) содержит код в C# и Visual Basic.
> 
> ## <a name="database-first"></a>Сначала базы данных
> 
> Существует три способа, можно работать с данными в Entity Framework: *Database First*, *Model First*, и *Code First*. Это руководство предназначено для первой базы данных. Сведения о различиях между этими рабочими процессами и рекомендации о том, как выбрать наиболее подходящий для вашего сценария, см. в разделе [рабочие процессы разработки Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Веб-формы
> 
> Как и серии Приступая к работе этой серии руководств используется модель веб-форм ASP.NET и предполагается, что вы знаете, как работать с веб-форм ASP.NET в Visual Studio. Если вы не сделать, см. в разделе [Приступая к работе с веб-форм ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Если вы предпочитаете работать с платформой ASP.NET MVC, см. в разделе [Приступая к работе с платформой Entity Framework, с помощью ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Версии программного обеспечения
> 
> | **В этом руководстве показано** | **Также работает с** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express для Web. Руководства не был протестирован с более поздними версиями Visual Studio. Существует множество различий в выборе пунктов меню, диалоговые окна и шаблоны. |
> | .NET 4 | .NET 4.5 имеет обратную совместимость с .NET 4, но руководства не был протестирован с .NET 4.5. |
> | Entity Framework 4 | Руководства не был протестирован с более поздними версиями платформы Entity Framework. Начиная с Entity Framework 5, по умолчанию использует EF `DbContext API` , был введен с версии 4.1 платформы EF. Элемент управления EntityDataSource разрабатывалась для использования `ObjectContext` API. Сведения об использовании управления EntityDataSource управления `DbContext` API, см. в разделе [этой записи блога](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Вопросы
> 
> Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework и LINQ to Entities форум](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), или [ StackOverflow.com](http://stackoverflow.com/).


`EntityDataSource` Управления позволяет очень быстро создавать приложения, но она обычно требует поддерживать значительный объем бизнес-логику и логику доступа к данным в вашей *.aspx* страниц. Если ожидается, что приложение сложными и требует текущего обслуживания, вы заранее вкладывать больше времени разработки для создания *n уровневых* или *многоуровневой* структуры приложения Это упрощает управление системой. Для реализации этой архитектуры можно отделить слой представления от уровня бизнес-логики (BLL) и уровня доступа к данным (DAL). Один из способов реализации этой структуры является использование `ObjectDataSource` вместо элемента `EntityDataSource` элемента управления. При использовании `ObjectDataSource` элемента управления, можно реализовать собственный код для доступа к данным, а затем вызывать ее в *.aspx* страниц с помощью элемента управления, обладающей множеством одной и той же функции, как другие элементы управления источника данных. Это позволяет сочетать преимущества подход n уровневых преимущества применения элемента управления веб-форм для доступа к данным.

`ObjectDataSource` Управления обеспечивает большую гибкость в других способов. Так, как написать собственный код доступа к данным, это упрощает не только чтения, вставки, обновления или удаления определенного типа сущности, которые являются задачи, `EntityDataSource` элемент управления предназначен для выполнения. Например можно выполнять ведение журнала каждый раз при обновлении сущности, архивация данных каждый раз при удалении сущности, или автоматически проверка и обновление связанных данных при необходимости при вставке строки со значением внешнего ключа.

## <a name="business-logic-and-repository-classes"></a>Бизнес-логики и классы репозитория

`ObjectDataSource` Работы элемента управления путем вызова класса, который создается. Этот класс включает методы для получения и обновления данных, и укажите имена в этих методах `ObjectDataSource` элемента управления в разметке. Во время подготовки к просмотру или обработки обратной передачи `ObjectDataSource` вызывает методы, которые вы указали.

Помимо основные операции CRUD, класса, который создается для использования с `ObjectDataSource` управления может потребоваться выполнение бизнес-логики при `ObjectDataSource` чтения или обновления данных. Например при обновлении отдел, может потребоваться проверить, что нет других отделов нет того же администратора, так как один пользователь не может быть администратор более одного отдела.

В некоторых `ObjectDataSource` документации, такие как [Общие сведения о классе ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), элемент управления вызывает класс называется *бизнес-объект* , включает в себя бизнес-логику и логику доступа к данным . В этом руководстве вы создадите отдельные классы для бизнес-логики и логики доступа к данным. Класс, инкапсулирующий логику доступа к данным, называется *репозитория*. Класс бизнес-логики включает в себя бизнес логики методы и методы доступа к данным, но методы доступа к данным вызова репозитория для выполнения задач доступа к данным.

Также вы создадите уровень абстракции между BLL и DAL, который упрощает автоматическое модульное тестирование BLL. Этот уровень абстракции реализуется путем создания интерфейса и с помощью интерфейса, при создании экземпляра репозитория в классе бизнес логики. Это позволяет указать класс бизнес логики со ссылкой на любой объект, реализующий интерфейс репозитория. Для нормальной работы необходимо предоставить объект хранилища, который работает с Entity Framework. Для тестирования, необходимо предоставить объект хранилища, который работает с данными, хранящимися в виде, можно легко управлять, например класс переменные, определенные как коллекции.

Ниже показана разница класс бизнес логики, который включает логику доступа к данным без репозитория и только один репозиторий.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Начну с создания веб-страниц, в котором `ObjectDataSource` непосредственно к репозиторию привязан элемент управления, так как он выполняет только базовый доступ к данным задачи. В следующем руководстве вы создадите класс бизнес-логики с логикой проверки и привязать `ObjectDataSource` управления для этого класса, а не класс репозитория. Также вы создадите модульные тесты для логики проверки. В третьем учебнике этой серии вы добавите функций в приложение сортировки и фильтрации.

Pages, созданные в этом руководстве работает со `Departments` набор сущностей модели данных, созданный в [серии руководств, Приступая к работе](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[!["Image01"](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Обновление базы данных и модели данных

Будет работы с этим руководством, сделав два изменения в базу данных, для которых требуется соответствующие изменения в модель данных, созданная в [Приступая к работе с Entity Framework и веб-форм](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) учебники. В одном из этих учебников внесены изменения в конструкторе вручную, чтобы синхронизировать модель данных с базой данных после изменения базы данных. В этом руководстве используется конструктор **обновить модель из базы данных** средство для автоматического обновления данных модели.

### <a name="adding-a-relationship-to-the-database"></a>Добавление отношения к базе данных

В Visual Studio, откройте веб-приложение университета Contoso, созданный в [Приступая к работе с Entity Framework и веб-форм](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) серии руководств, а затем откройте `SchoolDiagram` диаграммы базы данных.

Если взглянуть на `Department` таблицы в схеме базы данных, вы увидите, что у него есть `Administrator` столбца. Этот столбец является внешним ключом к `Person` таблицы, но нет связи по внешнему ключу определен в базе данных. Необходимо создать связь и обновление модели данных Entity Framework может автоматически обрабатывать эту связь.

В диаграмме базы данных щелкните правой кнопкой мыши `Department` таблицу и команду **связи**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

В **связи по внешнему ключу** выберите **добавить**, затем нажмите кнопку с многоточием возле **Спецификация таблиц и столбцов**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

В **таблицы и столбцы** диалогового окна поле, установите таблицы первичного ключа и поле `Person` и `PersonID`и задайте таблицы внешнего ключа и полю `Department` и `Administrator`. (При этом в имени связи будет изменен `FK_Department_Department` для `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Нажмите кнопку **ОК** в **таблицы и столбцы** выберите **закрыть** в **связи по внешнему ключу** поле и сохраните изменения. При появлении запроса необходимо выполнить сохранение `Person` и `Department` таблиц, нажмите кнопку **Да**.

> [!NOTE]
> Если вы удалили `Person` строки, которые относятся к данным, уже `Administrator` столбца, не можно сохранить это изменение. В этом случае следует использовать редактор таблиц в **обозревателя серверов** чтобы убедиться в том, что `Administrator` значение в каждой `Department` строка содержит идентификатор записи, которая действительно существует в `Person` таблицы.
> 
> После сохранения изменений, вы не сможете удалить строку из `Person` таблица, если этот пользователь является администратором отдела. В рабочем приложении обеспечит сообщение об ошибке при ограничении базы данных не допускает удаление, или следует указать приведет к каскадному удалению. Пример указания приведет к каскадному удалению, см. в разделе [платформы Entity Framework и ASP.NET — часть 2 Приступая к работе](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Добавление представления в базу данных

В новом *Departments.aspx* страницы, вы создадите, необходимо предоставить стрелку раскрывающегося списка преподавателей, с именами в формате «Фамилия, имя», чтобы пользователи могли выбирать администраторам отдела. Чтобы упростить это сделать, будет создано представление в базе данных. Представление будет состоять из только что данных, необходимых в раскрывающемся списке: полное имя (правильный формат) и ключа записи.

В **обозревателя серверов**, разверните *файл School.mdf*, щелкните правой кнопкой мыши **представления** и выберите **добавить новое представление**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Нажмите кнопку **закрыть** при **добавить таблицу** диалоговое окно появляется и вставьте следующую инструкцию SQL на панели SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Сохранить представление в виде `vInstructorName`.

### <a name="updating-the-data-model"></a>Обновление модели данных

В *DAL* откройте *SchoolModel.edmx* файл, щелкните правой кнопкой мыши область конструктора и выберите **обновить модель из базы данных**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

В **Choose Your Database Objects** выберите **добавить** и выберите только что созданное представление.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Нажмите кнопку **Готово**.

В конструкторе, вы увидите созданный средство `vInstructorName` сущности и создает новое сопоставление между `Department` и `Person` сущностей.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> В **вывода** и **список ошибок** windows, может появиться предупреждающее сообщение, информирующее о том, что средство автоматически создается первичный ключ для нового `vInstructorName` представления. Это нормальная ситуация.


При ссылке на новый `vInstructorName` сущности в коде, вы не хотите использовать префикс строчные «v» к нему правило базы данных. Таким образом переименует объект и набор сущностей в модели.

Откройте **модели обозревателя**. Вы увидите `vInstructorName` типа сущности и представления.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

В разделе **SchoolModel** (не **SchoolModel.Store**), щелкните правой кнопкой мыши **vInstructorName** и выберите **свойства**. В **свойства** измените **имя** свойство «InstructorName» и изменение **имя набора сущностей** свойства «InstructorNames».

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Сохраните и закройте модели данных и затем перестройте проект.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Используя класс репозитория и элемент управления ObjectDataSource

Создайте новый файл класса в *DAL* папки, назовите его *SchoolRepository.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Этот код предоставляет один `GetDepartments` метод, возвращающий все сущности в `Departments` набора сущностей. Так как вы знаете, что вам необходим доступ к `Person` свойство навигации для каждой строки получаемой, указать безотложную загрузку для свойства с помощью `Include` метод. Класс также реализует интерфейс `IDisposable` интерфейс, чтобы высвободить подключения к базе данных при удалении объекта.

> [!NOTE]
> Распространенной практикой является создание класса репозиторий для каждого типа сущности. В этом руководстве используется один класс репозитория для нескольких типов сущностей. Дополнительные сведения о шаблон репозитория, см. в записях в [блоге группы Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) и [блога Джули Лерман](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


`GetDepartments` Возвращает `IEnumerable` объекта вместо `IQueryable` объекта, чтобы гарантировать, что возвращенной коллекции можно использовать даже в том случае, после удаления самого объекта репозитория. `IQueryable` Объекта может привести к доступ к базе данных каждый раз, когда осуществляется, но к моменту, элемент управления с привязкой к данным пытается выполнить отрисовку данных можно было бы удалить объекта репозитория. Может вернуть другой тип коллекции, такие как `IList` вместо объекта `IEnumerable` объекта. Тем не менее, возвращая `IEnumerable` объект гарантирует, что вы сможете выполнять задачи обработки типичных только для чтения список таких как `foreach` циклы и запросы LINQ, но невозможно добавить или удалить элементы в коллекции, которая может подразумеваться, что такие изменения будет сохранить в базе данных.

Создание *Departments.aspx* страницы, использующей *Site.Master* главную страницу и добавьте следующую разметку в `Content` управления с именем `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Эта разметка создает `ObjectDataSource` элемента управления, использующего класс репозитория, который вы только что создали, и `GridView` управления для отображения данных. `GridView` Управления задает **изменить** и **удалить** команды, но вы еще не добавили код для поддержки их еще.

Используйте несколько столбцов `DynamicField` управляет таким образом, можно воспользоваться преимуществами автоматического данных форматирования и проверки функциональности. Для их работы, необходимо вызвать `EnableDynamicData` метод в `Page_Init` обработчик событий. (`DynamicControl` элементы управления не используются в `Administrator` поле, так как они не работают со свойствами навигации.)

`Vertical-Align="Top"` Атрибутов может быть важно позже при добавлении столбца, имеющего вложенный `GridView` элемента управления в сетку.

Откройте *Departments.aspx.cs* файл и добавьте следующие `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Затем добавьте следующий обработчик для страницы `Init` событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

В *DAL* папки, создайте новый файл класса с именем *Department.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Этот код добавляет метаданные в модель данных. Указывает, что `Budget` свойство `Department` сущности фактически представляет валюту, несмотря на то, что он имеет тип данных `Decimal`, и указывает, что значение должно быть между 0 и 1,000,000.00 долл. США. Он также указывает, что `StartDate` свойства должен иметь формат даты в формате мм/дд/гггг.

Запустите *Departments.aspx* страницы.

[!["Image01"](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Обратите внимание, что, несмотря на то, что вы не указали строку формата в *Departments.aspx* разметки страницы для **бюджета** или **Дата начала** столбцы, по умолчанию, валюты и даты применения форматирования к ним по `DynamicField` элементы управления, с помощью метаданных, которое вы использовали в *Department.cs* файла.

## <a name="adding-insert-and-delete-functionality"></a>Добавление Insert и функции удаления

Откройте *SchoolRepository.cs*, добавьте следующий код для создания `Insert` метод и `Delete` метод. Код также содержит метод с именем `GenerateDepartmentID` Далее записей значение ключа для использования, который вычисляет `Insert` метод. Это необходимо, так как база данных не настроена для вычисления это автоматически для `Department` таблицы.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Метод Attach

`DeleteDepartment` Вызовы методов `Attach` метода для того чтобы повторно установить ссылку, которая сохраняется в контекст объекта диспетчера состояния объектов между сущностями в памяти и базе данных строки, он представляет. Это должно произойти до вызова метода `SaveChanges` метод.

Термин *контекст объекта* ссылается на класс Entity Framework, который является производным от `ObjectContext` класс, который позволяет получить доступ к наборам сущностей и сущностей. В коде для этого проекта с именем класса `SchoolEntities`, и его экземпляр, который всегда имеет имя `context`. Контекст объекта *диспетчер состояния объекта* — это класс, производный от `ObjectStateManager` класса. Объект-контакт использует диспетчер состояния объекта для хранения объектов сущностей, так и для отслеживания ли каждого из них в соответствии с его соответствующей строки таблицы или строк в базе данных.

При чтении сущности, контекст объекта сохраняется в диспетчере состояния объектов и отслеживает ли это представление объекта синхронизации с базой данных. Например если изменить значение свойства, флаг имеет значение, свойство, которое вы изменили больше не является синхронизацию с базой данных. При последующем вызове `SaveChanges` метода, контекст объекта будет знать, что делать в базе данных, поскольку диспетчер состояния объекта знает именно то, что текущее состояние сущности и состояния базы данных различаются.

Тем не менее этот процесс обычно не работает в веб-приложения, так как экземпляр контекста объекта, который считывает сущность, а также все, что в его диспетчер состояния объекта, ликвидируется после отрисовки страницы. Экземпляр контекста объекта, который необходимо применить изменения — это новый, который создается для обработки обратной передачи. В случае использования `DeleteDepartment` метод, `ObjectDataSource` управления повторно создает исходную версию сущности из значений в состоянии представления, но это пересоздается `Department` сущность не существует в диспетчере состояния объекта. Если вызван `DeleteObject` метод эта сущность создана повторно, вызов бы невозможна, поскольку контекст объекта не знает, является ли сущность синхронизации с базой данных. Однако, при вызове `Attach` метод повторно устанавливает же отслеживания между значениями и повторно созданной сущности в базе данных, которое изначально было сделано автоматически после считывания сущности в более ранних экземпляра контекста объекта.

Бывают ситуации, при требуется контекст объекта для отслеживания сущностей в диспетчере состояния объекта и флаги, чтобы предотвратить это, можно установить. Это примеры приведены в последующих руководствах этой серии.

### <a name="the-savechanges-method"></a>Метод SaveChanges

Этот класс простой репозиторий иллюстрирует основные принципы для выполнения операций CRUD. В этом примере `SaveChanges` метод вызывается сразу после каждого обновления. В рабочем приложении может потребоваться вызвать `SaveChanges` метода из отдельный метод, чтобы получить больший контроль над обновлением базы данных. (В конце следующего руководства вы найдете ссылку на технический документ, посвященный единицы рабочий шаблон, который является одним из подходов к координации связанных обновлений.) Обратите внимание, что в примере `DeleteDepartment` метод не содержит код для обработки конфликтов параллелизма; код для этого будет добавлен в следующем руководстве этой серии.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Извлечение имен преподавателя для выбора при вставке

Пользователи должны иметь возможность выбрать администратора из списка преподавателей в раскрывающемся списке при создании нового подразделения. Таким образом, добавьте следующий код, чтобы *SchoolRepository.cs* создать метод для извлечения списка преподавателей в режиме, который был создан ранее:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Создание страницы для вставки отделов

Создание *DepartmentsAdd.aspx* страницы, использующей *Site.Master* страницы и добавьте следующую разметку в `Content` управления с именем `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Эта разметка создает два `ObjectDataSource` контролирует, один для вставки нового `Department` сущности и одно — для получения имен преподавателя для `DropDownList` элемент управления, который используется для выбора администраторам отдела. Разметка создает `DetailsView` управления для ввода нового подразделения и он определяет обработчик для элемента управления `ItemInserting` событий, которые можно использовать `Administrator` значение внешнего ключа. После завершения `ValidationSummary` управления для отображения сообщения об ошибках.

Откройте *DepartmentsAdd.aspx.cs* и добавьте следующие `using` инструкции:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Добавьте следующую переменную класса и методы:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init` Метод включает функциональные возможности платформы динамических данных. Обработчик `DropDownList` элемента управления `Init` событие сохраняет ссылку на элемент управления и обработчик для `DetailsView` элемента управления `Inserting` событий использует эту ссылку, чтобы получить `PersonID` значение выбранного преподавателя и обновления `Administrator` свойства внешнего ключа `Department` сущности.

Запустить эту страницу, добавьте сведения для нового отдела и щелкните **вставить** ссылку.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Введите значения для другого нового отдела. Введите число больше значения 1,000,000.00 в **бюджета** поля и вкладки в следующее поле. В поле отображается звездочка, и если навести указатель мыши на него, вы увидите сообщение об ошибке, введенный в метаданных для этого поля.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Нажмите кнопку **вставить**, и вы увидите сообщение об ошибке `ValidationSummary` элемента управления в нижней части страницы.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Затем закройте браузер и откройте *Departments.aspx* страницы. Добавить возможность удаления для *Departments.aspx* страницы путем добавления `DeleteMethod` атрибут `ObjectDataSource` элемента управления и `DataKeyNames` атрибут `GridView` элемента управления. Открывающие теги для этих элементов управления, теперь будет выглядеть примерно так:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Откройте страницу.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Удаление подразделения, добавленные при выполнении *DepartmentsAdd.aspx* страницы.

## <a name="adding-update-functionality"></a>Добавление функциональных возможностей обновления

Откройте *SchoolRepository.cs* и добавьте следующие `Update` метод:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

При нажатии кнопки **обновление** в *Departments.aspx* странице `ObjectDataSource` управления создает два `Department` сущностей для передачи `UpdateDepartment` метод. Один из них содержит исходные значения, которые сохранены в состоянии представления, а другой — новые значения, которые были введены в `GridView` элемента управления. Код в `UpdateDepartment` передает метод `Department` сущность, которая содержит исходные значения для `Attach` метод, чтобы установить отслеживание между сущностями и в базе данных. Затем код передает `Department` сущность, которая содержит новые значения для `ApplyCurrentValues` метод. Контекст объекта сравнивает старое и новое значения. Если новое значение отличается от старое, контекст объекта изменяет значение свойства. `SaveChanges` Метод обновляет только измененные столбцы в базе данных. (Тем не менее, если функция обновления для этой сущности были сопоставлены с хранимой процедурой, вся строка будет обновлено независимо от того, что столбцы были изменены.)

Откройте *Departments.aspx* файл и добавьте следующие атрибуты, чтобы `DepartmentsObjectDataSource` управления:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Это причины старые значения сохраняются в состоянии представления, чтобы их можно сравнивать с новыми значениями в `Update` метод.
- `OldValuesParameterFormatString="orig{0}"`   
 Это информирует элемент управления, который называется исходный параметр значения `origDepartment` .

Разметку для открывающего тега `ObjectDataSource` управления теперь аналогичный приведенному ниже:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Добавить `OnRowUpdating="DepartmentsGridView_RowUpdating"` атрибут `GridView` элемента управления. Вы будет использовать для задания `Administrator` значение свойства на основе строки пользователь выбирает в раскрывающемся списке. `GridView` Теперь открывающий тег аналогичный приведенному ниже:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Добавить `EditItemTemplate` управления `Administrator` столбец `GridView` управления сразу же после `ItemTemplate` управления для этого столбца:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Это `EditItemTemplate` управления аналогичен `InsertItemTemplate` контролировать *DepartmentsAdd.aspx* страницы. Разница заключается в том, что начальное значение элемента управления задается с помощью `SelectedValue` атрибута.

Прежде чем `GridView` управления, добавьте `ValidationSummary` управления, как это делалось *DepartmentsAdd.aspx* страницы.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Откройте *Departments.aspx.cs* и сразу после объявления разделяемого класса, добавьте следующий код, чтобы создать частное поле для ссылки на `DropDownList` управления:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Затем добавьте обработчики для `DropDownList` элемента управления `Init` событий и `GridView` элемента управления `RowUpdating` событий:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Обработчик `Init` событие сохраняет ссылку на `DropDownList` элемента управления в поле класса. Обработчик `RowUpdating` событий использует ссылки для получения значения, введенные пользователем и применить его к `Administrator` свойство `Department` сущности.

Используйте *DepartmentsAdd.aspx* странице для добавления нового отдела, а затем запустите *Departments.aspx* и нажмите кнопку **изменить** в строке, вы добавили.

> [!NOTE]
> Вы не сможете изменить строки, которые не был добавлен (то есть, которые уже были в базе данных), из-за недопустимых данных в базе данных; Администраторы для строк, которые были созданы с базой данных являются студентами. Если попытаться изменить один из них, вы получите страницу ошибки, который сообщает об ошибке, например `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

При вводе недопустимого **бюджета** сумму, а затем нажмите кнопку **обновление**, вы увидите же звездочка и сообщение об ошибке, можно было увидеть в *Departments.aspx* страницы.

Измените значение поля или выберите другой администратор и нажмите кнопку **обновления**. Изменение отображается.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

На этом завершается введение в использование `ObjectDataSource` управления для основных CRUD (Создание, чтение, обновление и удаление) операций с помощью Entity Framework. Вы создали простой n уровневого приложения, но по-прежнему тесно связана бизнес-логики на уровень доступа к данным, что осложняет автоматическое тестирование модулей. В следующем учебном курсе будет показано, как реализовать шаблон repository, чтобы упростить модульное тестирование.

> [!div class="step-by-step"]
> [Вперед](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
