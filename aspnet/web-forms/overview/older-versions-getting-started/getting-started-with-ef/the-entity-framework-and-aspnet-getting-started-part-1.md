---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: "Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4 Web Forms | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения показано, как создавать приложения веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: ae2fddc81f6f4da866ec0719a0e74516bdd2a4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4 Web Forms
====================
По [Tom Dykstra](https://github.com/tdykstra)

> Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010. Образец приложения это веб-сайт для вымышленной компании Contoso университета. Он включает функции, такие как допуском студента, создание курса и инструктора назначения.
> 
> Учебник показаны примеры на языке C#. [Загружаемые примеры](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) содержит код на C# и Visual Basic.
> 
> ## <a name="database-first"></a>Сначала базы данных
> 
> Существует три способа, можно работать с данными в Entity Framework: *Database First*, *Model First*, и *Code First*. Этот учебник предназначен для первой базы данных. Сведения о различиях между этими рабочими процессами и рекомендации о том, как выбирать наиболее подходящий для вашего сценария см. в разделе [процессов разработки Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>веб-формы
> 
> Этот учебник ряд использует модель веб-форм ASP.NET и предполагается, что вы знаете, как работать с веб-форм ASP.NET в Visual Studio. Если отсутствует [Приступая к работе с веб-форм ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Если вы предпочитаете работать с ASP.NET MVC, см. раздел [Приступая к работе с платформой Entity Framework, с помощью ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
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


## <a name="overview"></a>Обзор

Приложение, которое вы будете построения в этих учебниках является простой университета веб-сайта.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Пользователи могут просматривать и обновлять студента курса и инструктора сведения. Ниже приведены несколько экранов, которые будут созданы.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Создание веб-приложения

Чтобы запустить учебник, откройте Visual Studio и создайте новый проект веб-приложения ASP.NET с помощью **веб-приложение ASP.NET** шаблона:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Этот шаблон создает проект веб-приложения, который уже включает таблицу стилей и главные страницы:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Откройте *Site.Master* и измените «Мое приложение ASP.NET» на «Contoso University».

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Найти *меню* управления с именем `NavigationMenu` и замените следующую разметку, которая добавляет пункты меню для страниц, которые будут создаваться.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Откройте *Default.aspx* и изменить `Content` управления с именем `BodyContent` к этому:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Теперь у вас есть простой Домашняя страница со ссылками на различные страницы, которые будут создаваться:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Создание базы данных

Для этих учебников конструктор модели данных Entity Framework будет использоваться для автоматического создания модели данных на основе существующей базы данных (часто называемые *сначала база данных* подход). Альтернативы, не соответствует этой серии учебника является создание модели данных вручную и затем конструктора создания скрипта, создайте базу данных ( *моделей в первую очередь* подход).

Метод сначала база данных, используемые в этом учебнике следующим шагом является добавление базы данных на сайт. Проще всего сначала загрузить проект, который переходит к этому учебнику. Щелкните правой кнопкой мыши *приложения\_данные* выберите **Добавление существующего элемента**и выберите *файл School.mdf* файла базы данных, загруженном проекте.

Альтернативой является следуйте инструкциям в [Создание образца базы данных School](https://msdn.microsoft.com/library/bb399731.aspx). Загрузите базу данных или создать его, скопируйте *файл School.mdf* файл из следующей папки приложения *приложения\_данные* папки:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Это расположение *.mdf* файл предполагается вы используете SQL Server 2008 Express.)

При создании базы данных с помощью сценария, выполните следующие действия, чтобы создать диаграмму базы данных.

1. В **обозревателя серверов**, разверните **подключения к данным**, разверните *файл School.mdf*, щелкните правой кнопкой мыши **диаграммы базы данных**и выберите **Добавить новую диаграмму**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Выберите все таблицы и нажмите кнопку **добавить**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server создает диаграмму базы данных, содержит таблицы, столбцы в таблицы и связи между таблицами. Можно перемещать таблицы, чтобы упорядочить их следования.
3. Сохраните схему как «SchoolDiagram» и закройте его.

При загрузке *файл School.mdf* файл, который переходит к этому учебнику диаграммы базы данных можно просмотреть двойным щелчком **SchoolDiagram** под **диаграмм баз данных** в **Обозревателя серверов**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Диаграмма выглядит примерно следующим образом (таблицы может быть в разных местах от показанной здесь):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Создание модели данных Entity Framework

Теперь можно создавать модели данных Entity Framework, из этой базы данных. Можно создать модель данных в корневой папке приложения, но в этом учебнике будет поместить его в папку с именем *DAL* (для доступа к данным).

В **обозревателе решений**, добавьте в проект папку с именем *DAL* (Убедитесь, что он находится в проекте, находящиеся вне решения).

Щелкните правой кнопкой мыши *DAL* папки, а затем выберите **добавить** и **новый элемент**. В разделе **установленные шаблоны**выберите **данные**выберите **ADO.NET Entity Data Model** шаблона, назовите его *SchoolModel.edmx*, и Нажмите кнопку **добавить**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Запустится мастер моделей EDM. На первом шаге мастера **создать из базы данных** параметр выбран по умолчанию. Нажмите кнопку **Далее**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

В **Выбор подключения к данным** шагов, оставьте значения по умолчанию и нажмите кнопку **Далее**. Базы данных School выбран по умолчанию и параметр подключения сохраняется в *Web.config* файл с именем **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

В **Выбор объектов базы данных** шаг мастера, выберите все таблицы, за исключением `sysdiagrams` (который был создан для диаграммы, созданные в более ранних версий) и нажмите кнопку **Готово**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

После завершения создания модели, Visual Studio отображает графическое представление объектов Entity Framework (сущности), которые соответствуют таблицам базы данных. (Как и в диаграмме базы данных, расположение отдельных элементов может отличаться от показанного на рисунке. Можно перетаскивать элементы решения для соответствия на рисунке, если требуется.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Изучение модели данных Entity Framework

Вы увидите, что схема entity очень похожа на диаграмме базы данных с помощью нескольких различия. Единственное отличие заключается в добавлении символы в конце каждой связи, указывающие тип ассоциации (связи между таблицами, называются сопоставления сущностей в модели данных):

- Сопоставления один к нулю или одному представлен «1» и «от 0 до 1».

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    В этом случае `Person` сущности может или не может быть связано с `OfficeAssignment` сущности. `OfficeAssignment` Сущности должно быть связано с `Person` сущности. Другими словами инструктор может или не может назначаться для office и любые office можно назначить только один инструктора.
- Представленный связь один ко многим «1» и "\*«.

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    В этом случае `Person` сущности могут или не могут быть связаны `StudentGrade` сущностей. Объект `StudentGrade` сущность должна быть связана с одним `Person` сущности. `StudentGrade`сущности представляют фактически зарегистрированных курсов в этой базе данных; Если зарегистрировано студент курсов, а не уровень еще, `Grade` свойство имеет значение null. Другими словами не могут быть зарегистрированы в курсов студент еще, могут быть зарегистрированы в одной курса или могут быть зарегистрированы в нескольких курсах. Каждый уровень в курсе зарегистрированных применяется только один студент.
- Представленный ассоциации многие ко многим "\*«и»\*».

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    В этом случае `Person` сущности могут или не могут быть связаны `Course` сущностей и обратное также верно: `Course` сущности могут или не могут быть связаны `Person` сущностей. Другими словами инструктор может обучения нескольких курсах, и может обучения по нескольким инструкторов. (В этой базе данных, эту связь применяется только к инструкторов; он не связан учащихся курсов. Студенты, связаны курсы по таблице StudentGrades.)

Еще одно отличие от диаграммы базы данных и модели данных является дополнительной **свойства навигации** раздел для каждой сущности. Свойства навигации сущности содержит ссылки на связанные сущности. Например `Courses` свойство в `Person` сущность содержит коллекцию всех `Course` сущностей, которые относятся к, `Person` сущности.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Еще еще одно отличие от базы данных и модели данных — отсутствие `CourseInstructor` связь таблицы, используемые в базе данных для связи с `Person` и `Course` таблицы в связи многие ко многим. Свойства навигации позволяют получить связанные `Course` сущностей из `Person` сущности и связанных `Person` сущностей из `Course` сущности, поэтому нет необходимости для представления таблицы связи в модели данных.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

В целях этого учебника Предположим, что `FirstName` столбец `Person` таблицы содержит фактическую человека имя и отчество. Вы хотите изменить имя поля в соответствии с этим, но администратор базы данных (DBA) не может потребоваться изменить базу данных. Можно изменить имя `FirstName` свойства в модели данных, оставив эквивалентные свою базу данных без изменений.

В конструкторе щелкните правой кнопкой мыши **FirstName** в `Person` сущности, а затем выберите **переименование**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Введите новое имя «FirstMidName». Это позволяет изменить способ ссылки на столбец в коде без изменения базы данных.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Браузер моделей предоставляет еще один способ просмотра структуры базы данных, структуры данных модели и сопоставления между ними. Чтобы увидеть ее, щелкните правой кнопкой мыши пустую область в конструкторе сущностей и нажмите кнопку **браузер моделей**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**Браузер моделей** отображаются в виде дерева. ( **Браузер моделей** области могут быть закреплены с **обозревателе решений** области.) **SchoolModel** узел представляет структуру модели данных и **SchoolModel.Store** узел представляет структуру базы данных.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Разверните **SchoolModel.Store** для просмотра таблиц, разверните **таблицы / представления** для просмотра таблиц, а затем разверните **курса** для просмотра всех столбцов в таблице.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Разверните **SchoolModel**, разверните **типов сущностей**и разверните **курса** узел, чтобы просмотреть объекты и свойства в сущностях.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

В любом конструкторе или **браузер моделей** области можно показать связь объектов две модели Entity Framework. Щелкните правой кнопкой мыши `Person` сущность и выберите **сопоставление таблиц**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

При этом откроется **сведения о сопоставлении** окна. Обратите внимание, что данное окно позволяет видеть, что столбец базы данных `FirstName` сопоставляется `FirstMidName`, который является то, что вы переименовали ее в модели данных.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Платформа Entity Framework использует XML для хранения сведений о базе данных, модели данных и сопоставления между ними. *SchoolModel.edmx* файл является фактически XML-файл, содержащий данную информацию. Конструктор отображает данные в графическом виде, но можно также просмотреть файл как XML, щелкнув правой кнопкой мыши *.edmx* файла в **обозревателе решений**, а затем выбрав **открыть с помощью**и выбрав **редактор XML (текстовый)**. (Конструктор моделей данных и редактор XML являются только два разных способа открытия и работы с тот же файл, поэтому не могут иметь конструктор откройте и откройте файл в редакторе XML, в то же время.)

Теперь вы создали веб-сайт, базы данных и модели данных. В следующем пошаговом руководстве будет приступить к работе с данными с помощью модели данных и ASP.NET `EntityDataSource` элемента управления.

>[!div class="step-by-step"]
[Вперед](the-entity-framework-and-aspnet-getting-started-part-2.md)
