---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fbfbc667b4031c67827dc158ffca4d1b4dbfb23e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394298"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms
====================
по [том Дайкстра](https://github.com/tdykstra)

> Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010. Пример приложения веб-сайт вымышленного университета Contoso. На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей.
> 
> Этом руководстве показаны примеры на языке C#. [Загружаемый пример](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) содержит код в C# и Visual Basic.
> 
> ## <a name="database-first"></a>Сначала базы данных
> 
> Существует три способа, можно работать с данными в Entity Framework: *Database First*, *Model First*, и *Code First*. Это руководство предназначено для первой базы данных. Сведения о различиях между этими рабочими процессами и рекомендации о том, как выбрать наиболее подходящий для вашего сценария, см. в разделе [рабочие процессы разработки Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Веб-формы
> 
> В этой серии руководств используется модель веб-форм ASP.NET и предполагается, что вы знаете, как работать с веб-форм ASP.NET в Visual Studio. Если вы не сделать, см. в разделе [Приступая к работе с веб-форм ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Если вы предпочитаете работать с платформой ASP.NET MVC, см. в разделе [Приступая к работе с платформой Entity Framework, с помощью ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
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


## <a name="overview"></a>Обзор

Приложение, которое вы будете создавать в этих учебниках веб-сайт простой университета.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Пользователи приложения могут просматривать и обновлять сведения об учащихся, курсах и преподавателях. Ниже приведены некоторые из экранов, которые будут созданы.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Создание веб-приложения

Чтобы начать работу с учебником, откройте Visual Studio и создайте новый проект веб-приложения ASP.NET с помощью **веб-приложение ASP.NET** шаблона:

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Этот шаблон создает проект веб-приложения, который уже включает таблицу стилей и главные страницы:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Откройте *Site.Master* и измените «My ASP.NET Application» на «Contoso University».

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Найти *меню* управления с именем `NavigationMenu` и замените следующую разметку, которая добавляет пункты меню для страниц, вы создадите.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Откройте *Default.aspx* и изменить `Content` управления с именем `BodyContent` к этому:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Теперь у вас есть простая домашняя страница, со ссылками на различных страницах, которые будут создаваться:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Создание базы данных

Эти учебники, будет использоваться в конструкторе моделей Entity Framework для автоматического создания модели данных, в зависимости от существующей базы данных (часто называемые *сначала база данных* подход). Другой способ, который не рассматривается в этой серии руководств является создание модели данных вручную и затем конструктору генерировать скрипты, которые создают базы данных ( *моделей в первую очередь* подход).

Для базы данных первый метод, используемый в этом руководстве следующим шагом является добавление базы данных сайта. Самым простым способом является сначала загрузить проект, который переходит к этому учебнику. Щелкните правой кнопкой мыши *приложения\_данных* папку, выберите **добавить существующий элемент**и выберите *файл School.mdf* файл базы данных из загруженного проекта.

Альтернативным вариантом является следуйте инструкциям, приведенным в [Создание образца базы данных School](https://msdn.microsoft.com/library/bb399731.aspx). Загрузите базу данных или создайте его, скопируйте *файл School.mdf* файл из следующей папки в приложение *приложения\_данных* папку:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Это расположение *.mdf* файла предполагается, что вы используете SQL Server 2008 Express.)

При создании базы данных из скрипта, выполните следующие действия, чтобы создать диаграмму базы данных.

1. В **обозревателя серверов**, разверните **подключения к данным**, разверните *файл School.mdf*, щелкните правой кнопкой мыши **диаграмм баз данных**и выберите **Добавить новую схему**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Выберите все таблицы и нажмите кнопку **добавить**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server создает диаграмму базы данных, содержит таблицы, столбцы в таблицы и связи между таблицами. Вы можете переместить таблицы решения для организации их, как вам нравится.
3. Сохраните схему как «SchoolDiagram» и закройте его.

При загрузке *файл School.mdf* файл, который переходит к этому учебнику, можно просмотреть диаграмму базы данных, дважды щелкнув **SchoolDiagram** под **диаграмм баз данных** в **Обозревателя серверов**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Диаграмма выглядит примерно так (таблицы может быть в разных расположениях от приведенных здесь):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Создание модели данных Entity Framework

Теперь можно создать модель данных Entity Framework из этой базы данных. Можно создать модель данных в корневой папке приложения, но в этом руководстве вы будете поместите его в папку с именем *DAL* (для уровня доступа к данным).

В **обозревателе решений**, добавить папку проекта с именем *DAL* (Убедитесь, что он находится в проекте, не находится в решении).

Щелкните правой кнопкой мыши *DAL* папку, а затем выберите **добавить** и **новый элемент**. В разделе **установленные шаблоны**выберите **данных**выберите **ADO.NET Entity Data Model** шаблона, назовите его *SchoolModel.edmx*, и Нажмите кнопку **добавить**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Запустится мастер модели EDM. На первом шаге мастера **создать из базы данных** выбран по умолчанию. Нажмите кнопку **Далее**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

В **Выбор подключения к данным** шагов, оставьте значения по умолчанию и нажмите кнопку **Далее**. По умолчанию выбран базы данных School, и параметр подключения сохраняется в *Web.config* файл в формате **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

В **Choose Your Database Objects** шага мастера, выберите все таблицы, за исключением `sysdiagrams` (который был создан для диаграммы, созданные в более ранних версий) и нажмите кнопку **Готово**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

После завершения создания модели, Visual Studio отображает графическое представление объектов Entity Framework (сущности), соответствующие таблицы базы данных. (Как и в диаграмму базы данных, расположение отдельных элементов может отличаться от показанного на рисунке. Можно перетаскивать элементы решения для сопоставления на рисунке, если вы хотите.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Изучение модели данных Entity Framework

Вы увидите, что на схеме сущности очень похож на схеме базы данных, с помощью нескольких различий. Одно различие заключается в добавлении символы в конце каждой ассоциации, которые указывают тип ассоциации (связи между таблицами, называются ассоциаций сущностей в модели данных):

- Сопоставление один к нулю или одному представляется «1» и «от 0 до 1».

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    В этом случае `Person` сущность может или не могут быть связаны с `OfficeAssignment` сущности. `OfficeAssignment` Сущности должно быть связано с `Person` сущности. Другими словами инструктор может или не может назначаться для office и любой office можно назначить только одного преподавателя.
- Представленный связь один ко многим «1» и "\*«.

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    В этом случае `Person` сущность может или не могут быть связаны `StudentGrade` сущностей. Объект `StudentGrade` сущности должен быть связан с одним `Person` сущности. `StudentGrade` сущности представляют фактически курсы, зарегистрированных в этой базе данных; Если учащийся зарегистрировано на курс и еще не корпоративного уровня, `Grade` свойство имеет значение null. Другими словами учащийся не могут быть зарегистрированы в курсов еще, могут быть зарегистрированы в один курс или могут быть зарегистрированы в нескольких курсах. Каждый корпоративного класса в зарегистрированных курс применяется к только один учащийся.
- Представленный связь многие ко многим "\*«и»\*«.

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    В этом случае `Person` сущность может или не могут быть связаны `Course` сущности и обратное также верно: `Course` сущность может или не могут быть связаны `Person` сущностей. Другими словами инструктор могут вести несколько курсов и курс могут вести несколько преподавателей. (В этой базе данных, эта связь применяется только к преподавателей; он не содержит ссылок учащихся на курсы. Учащиеся, связанных с курсы в таблице StudentGrades.)

Еще одно различие между схемой базы данных и модели данных — это дополнительный **свойства навигации** раздел для каждой сущности. Свойство навигации сущности ссылается на связанные сущности. Например `Courses` свойство в `Person` сущность содержит коллекцию всех `Course` сущностей, которые связаны `Person` сущности.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Еще еще одно различие между базы данных и модель данных — отсутствие `CourseInstructor` ассоциации таблицы, которая используется в базе данных, чтобы связать `Person` и `Course` таблицы в связи многие ко многим. Свойства навигации позволяют получить связанные `Course` сущностей из `Person` сущности и связанных `Person` сущностей из `Course` сущности, поэтому нет необходимости для представления таблицы ассоциации в модели данных.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Для целей данного учебника, предположим, что `FirstName` столбец `Person` таблица фактически содержит пользователя имя и отчество. Чтобы изменить имя поля, чтобы отразить это, но администратору базы данных (DBA) не может потребоваться изменить базу данных. Можно изменить имя `FirstName` свойства в модели данных, оставив эквивалентное свою базу данных без изменений.

В конструкторе щелкните правой кнопкой мыши **FirstName** в `Person` сущности, а затем выберите **Переименовать**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Введите новое имя «FirstMidName». Это позволяет изменить способ ссылки на столбец в коде без изменения базы данных.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Браузер моделей предоставляет еще один способ просмотреть структуру базы данных, структуры модели данных и их сопоставление. Чтобы увидеть его, щелкните правой кнопкой мыши пустую область в конструкторе сущностей и нажмите кнопку **браузер моделей**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**Браузер моделей** панель содержит представление дерева. ( **Браузер моделей** области может быть закреплено с **обозревателе решений** области.) **SchoolModel** узел представляет структуру модели данных и **SchoolModel.Store** узел представляет структуру базы данных.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Разверните **SchoolModel.Store** для просмотра таблиц, разверните **таблицы / представления** для просмотра таблиц, а затем разверните **курс** для просмотра столбцов в таблице.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Разверните **SchoolModel**, разверните **типы сущностей**и затем разверните **курс** узел, чтобы просмотреть сущности и свойства в сущностях.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

В любом конструкторе или **браузер моделей** области, вы увидите, как платформа Entity Framework относится объекты из двух моделей. Щелкните правой кнопкой мыши `Person` сущности и выберите **сопоставление таблицы**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Откроется **сведения о сопоставлении** окна. Обратите внимание на то, что это окно позволяет увидеть, что столбец базы данных `FirstName` сопоставляется `FirstMidName`, который является то, что вы переименовали его, чтобы в модели данных.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Платформа Entity Framework использует XML для хранения сведений о базе данных, модели данных и сопоставления между ними. *SchoolModel.edmx* файл является фактически XML-файл, содержащий данную информацию. Конструктор отображает данные в графическом формате, но также можно просмотреть файл в формате XML щелкните правой кнопкой мыши *.edmx* файл **обозревателе решений**, затем **открыть с помощью**и выбрав **редактор (текстовый) XML**. (В конструкторе моделей и редактор XML являются два разных способа открытия и работа с тот же файл, поэтому не может иметь конструктора откройте и откройте файл в редакторе XML, в то же время.)

Теперь вы создали веб-сайт, базу данных и модель данных. В следующем пошаговом руководстве вы начнете работу с данными, с помощью модели данных и ASP.NET `EntityDataSource` элемента управления.

> [!div class="step-by-step"]
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-2.md)
