---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Создание модели данных Entity Framework для приложения ASP.NET MVC (1 из 10) | Документация Майкрософт
author: tdykstra
description: Для Visual Studio 2013, Entity Framework 6 и MVC 5 доступна более новая версия этого цикла руководств. Web приложения университета Contoso образец de...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 037f67d679762a037eaef9f0a4060156b94d97b1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837683"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Создание модели данных Entity Framework для приложения ASP.NET MVC (1 из 10)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Объект [новой версии этой серии руководств](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) доступны для Visual Studio 2013, Entity Framework 6 и MVC 5.
> 
> 
> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 и Visual Studio 2012. В этом примере приложения реализуется веб-сайт вымышленного университета Contoso. На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей. В этой серии руководств описывается построение примера приложения университета Contoso. Вы можете [загрузить готовое приложение](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Существует три способа, можно работать с данными в Entity Framework: *Database First*, *Model First*, и *Code First*. Это руководство предназначено для Code First. Сведения о различиях между этими рабочими процессами и рекомендации о том, как выбрать наиболее подходящий для вашего сценария, см. в разделе [рабочие процессы разработки Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Создание примера приложения на базе [ASP.NET MVC](../../../index.md). Если вы предпочитаете работать с моделью веб-форм ASP.NET, см. в разделе [привязки модели и Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) серии руководств и [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Версии программного обеспечения
> 
> | **В этом руководстве показано** | **Также работает с** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio Express 2012 для Web. Это автоматически устанавливается с помощью пакета SDK Windows Azure, если у вас нет Visual STUDIO 2012 или VS 2012 Express для Web. Visual Studio 2013 должна работать, но руководства не был протестирован с ним и некоторые элементы меню и диалоговые окна отличаются. [Windows Azure SDK версии Visual STUDIO 2013](https://go.microsoft.com/fwlink/p/?linkid=323510) необходим для развертывания Windows Azure. |
> | .NET 4.5 | Большая часть функций, представленных будет работать в .NET 4, но некоторые не будут. Например для поддержки перечисления в EF требуется .NET 4.5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Если вы пропустите шаги развертывания Windows Azure, вам не требуется пакет SDK. При выпуске новой версии пакета SDK, ссылка будет установить новую версию. В этом случае может потребоваться изменить некоторые инструкции для нового пользовательского интерфейса и функций. |
> 
> ## <a name="questions"></a>Вопросы
> 
> Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework и LINQ to Entities форум](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), или [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Благодарности
> 
> См. в последнем руководстве серии для [подтверждений и Примечание о VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Исходная версия этого руководства
> 
> Исходная версия руководства доступна в [EF 4.1 и MVC 3 электронная книга](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Веб-приложение университета Contoso

В рамках этих учебников вы будете создавать приложение, которое представляет собой простой веб-сайт университета.

Пользователи приложения могут просматривать и обновлять сведения об учащихся, курсах и преподавателях. Будет создано несколько экранов.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Стиль пользовательского интерфейса этого сайта практически полностью основан на встроенных шаблонах, поскольку это позволяет сосредоточиться на изучении и использовании возможностей платформы Entity Framework.

## <a name="prerequisites"></a>Предварительные требования

Инструкции и снимки экрана в этом руководстве предполагается, что вы используете [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) или [Visual Studio 2012 Express для Web](https://go.microsoft.com/fwlink/?LinkID=275131), новейшие обновления и пакета Azure SDK для .NET, установлен по состоянию на июль, 2013. Вы можете получить все это с помощью следующей ссылки:

[Пакет Azure SDK для .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Если у вас установлена среда Visual Studio, указанной выше ссылке установит недостающие компоненты. Если у вас нет Visual Studio, ссылку установит Visual Studio 2012 Express для Web. Можно использовать Visual Studio 2013, но некоторые необходимые процедуры и экраны будут отличаться.

## <a name="create-an-mvc-web-application"></a>Создать веб-приложение MVC

Откройте Visual Studio и создайте новый проект C# с именем «ContosoUniversity» с помощью **веб-приложение ASP.NET MVC 4** шаблона. Убедитесь, что целевой **.NET Framework 4.5** (вы будете использовать [ `enum` свойства](https://msdn.microsoft.com/data/hh859576.aspx), которых требуется .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

В **создания проекта ASP.NET MVC 4** диалогового окна выберите **веб-приложение** шаблона.

Оставьте **Razor** системы выбранных представлений и оставить **Создание проекта модульного теста** флажок снят.

Нажмите кнопку **ОК**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Настройка стиля сайта

Выполните незначительную настройку меню, макета и домашней страницы сайта.

Откройте *Views\Shared\\_Layout.cshtml*и замените содержимое файла следующим кодом. Изменения выделены.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Этот код вносит следующие изменения:

- Заменяет экземпляры шаблона «My ASP.NET MVC Application» и «ваша эмблема здесь» с «Contoso University».
- Добавляет несколько ссылки на действия, которые будут использоваться позже в этом руководстве.

В *Views\Home\Index.cshtml*, замените содержимое файла следующим кодом, чтобы исключить абзацев шаблона о ASP.NET и MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

В *Controllers\HomeController.cs*, измените значение `ViewBag.Message` в `Index` методом действия «Welcome to университета Contoso!», как показано в следующем примере:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Нажмите клавиши CTRL + F5 для запуска сайта. Появится домашняя страница с меню.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Создание модели данных

Теперь необходимо создать классы сущностей для приложения университета Contoso. Сначала вы возьмете следующих трех сущностей:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Между сущностями `Student` и `Enrollment`, а также между сущностями `Course` и `Enrollment` существует отношение "один ко многим". Другими словами, учащийся может быть зарегистрирован в любом количестве курсов, а в отдельном курсе может быть зарегистрировано любое количество учащихся.

В следующих разделах создаются классы для каждой из этих сущностей.

> [!NOTE]
> При попытке компиляции проекта до завершения создания всех этих классов сущностей, вы получите ошибки компилятора.


### <a name="the-student-entity"></a>Сущность Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

В *моделей* папке создайте *Student.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Свойство `StudentID` будет использоваться в качестве столбца первичного ключа в таблице базы данных, соответствующей этому классу. По умолчанию Entity Framework интерпретирует свойство с именем `ID` или *classname* `ID` как первичный ключ.

`Enrollments` Свойство *свойство навигации*. Свойства навигации содержат другие сущности, связанные с этой сущностью. В этом случае `Enrollments` свойство `Student` сущность будет содержать все `Enrollment` сущностей, которые связаны `Student` сущности. Другими словами если заданный `Student` строк в базе данных имеет два связанных `Enrollment` строк (значение строки, которые содержат первичный ключ этого учащегося в их `StudentID` внешний ключевой столбец), в котором `Student` сущности `Enrollments` свойство навигации будет содержать две этих `Enrollment` сущностей.

Свойства навигации, обычно определяются как `virtual` таким образом, чтобы они можно воспользоваться преимуществами определенные функциональные возможности Entity Framework, такие как *отложенная загрузка*. (Отложенная загрузка, будет рассматриваться далее в [чтение связанных данных](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) далее в этой серии руководств.

Если свойство навигации может содержать несколько сущностей (как в отношениях "многие ко многим" или "один ко многим"), оно должно иметь тип списка, допускающий добавление, удаление и обновление записей, такой как `ICollection`.

### <a name="the-enrollment-entity"></a>Сущность Enrollment

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

В папке *Models* создайте файл *Enrollment.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

— Свойство Grade [перечисления](https://msdn.microsoft.com/data/hh859576.aspx). Знак вопроса после `Grade` объявление типа указывает, что `Grade` свойство [допускает значения NULL](https://msdn.microsoft.com/library/2cf62fcy.aspx). Корпоративного класса, который имеет значение null отличается от нулевой оценки тем — значение null означает, что это оценка не известна или еще не назначена.

Свойство `StudentID` представляет собой внешний ключ. Ему соответствует свойство навигации `Student`. Сущность `Enrollment` связана с одной сущностью `Student`, поэтому это свойство может содержать одну сущность `Student` (в отличие от представленного ранее свойства навигации `Student.Enrollments`, которое может содержать несколько сущностей `Enrollment`).

Свойство `CourseID` представляет собой внешний ключ. Ему соответствует свойство навигации `Course`. Сущность `Enrollment` связана с одной сущностью `Course`.

### <a name="the-course-entity"></a>Сущности Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

В *моделей* папке создайте *Course.cs*, заменив существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

Свойство `Enrollments` является свойством навигации. Сущность `Course` может быть связана с любым числом сущностей `Enrollment`.

Мы скажем: сведения о [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([параметр DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Нет)] атрибута в следующем учебном курсе. Фактически, этот атрибут позволяет ввести первичный ключ для курса, а не использовать базу данных, чтобы создать его.

## <a name="create-the-database-context"></a>Создание контекста базы данных

Основным классом, который координирует функциональные возможности Entity Framework для заданной модели данных является *контекст базы данных* класса. Этот класс создается путем наследования от [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) класса. В коде указываются сущности, которые включаются в модель данных. Также вы можете настроить реакцию платформы Entity Framework на некоторые события. В этом проекте соответствующий класс называется `SchoolContext`.

Создайте папку с именем *DAL* (для уровня доступа к данным). В этой папке создайте новый файл класса с именем *SchoolContext.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Этот код создает [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) свойство для каждого набора сущностей. В терминологии Entity Framework *набор сущностей* обычно соответствует таблице базы данных и *сущности* соответствует строке в таблице.

`modelBuilder.Conventions.Remove` Инструкции в [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) метод предотвращает имена таблиц имена во множественном числе. Если вы не сделаете этого, созданные таблицы будет называться `Students`, `Courses`, и `Enrollments`. Вместо этого будет имена таблиц `Student`, `Course`, и `Enrollment`. В среде разработчиков нет единого мнения о том, следует ли использовать имена таблиц во множественном числе. В этом руководстве используется существительные в единственном числе, но важно то, что вы можете выбрать любую форму, вы предпочитаете, включая или исключая эту строку кода.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) — это облегченная версия SQL Server Express Database Engine, запускаемая по запросу и работает в пользовательском режиме. LocalDB выполняется в специального режима выполнения SQL Server Express, которая позволяет работать с базами данных как *.mdf* файлов. Как правило, хранятся файлы базы данных LocalDB в *приложения\_данных* папку веб-проекта. Функции пользовательского экземпляра в SQL Server Express также позволяет работать с *.mdf* файлы, но функции пользовательского экземпляра устарело; таким образом, рекомендуется использовать LocalDB для работы с *.mdf* файлов.

Обычно SQL Server Express не используется для рабочих веб-приложений. LocalDB в частности не рекомендуется для использования в рабочей среде с веб-приложение, так как он не предназначен для работы со службами IIS.

В Visual Studio 2012 и более поздних версий LocalDB устанавливается по умолчанию с помощью Visual Studio. В Visual Studio 2010 и более ранних версиях SQL Server Express (без LocalDB) устанавливается по умолчанию с помощью Visual Studio; необходимо вручную установить его, если вы используете Visual Studio 2010.

В этом руководстве вы будете работать с LocalDB, чтобы базы данных могут храниться в *приложения\_данных* папке, что *.mdf* файл. Откройте корневой *Web.config* файл и добавьте новую строку подключения для `connectionStrings` коллекции, как показано в следующем примере. (Обязательно обновите *Web.config* файл в корневой папке проекта. Имеется также *Web.config* файл находится в *представления* вложенную папку, не нужно обновлять.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

По умолчанию, Entity Framework ищет строку подключения с именем, так же, как `DbContext` класс (`SchoolContext` для этого проекта). В строке подключения, вы добавили указан с именем базы данных LocalDB *ContosoUniversity.mdf* в *приложения\_данных* папки. Дополнительные сведения см. в разделе [строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Вам не нужен для указания строки подключения. Если вы не указали строку подключения, Entity Framework будет создан для вас; Тем не менее, база данных может не быть *приложения\_данных* папку приложения. Сведения о котором будет создана база данных, см. в разделе [Code First для новой базы данных](https://msdn.microsoft.com/data/jj193542).

`connectionStrings` Коллекция также имеет строку подключения с именем `DefaultConnection` — используется для базы данных членства. Не используется в базе данных членства в этом руководстве. Единственное различие между две строки подключения — это имя базы данных и значение атрибута name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Настройка и выполнение Code First Migration

При первом запуске для разработки приложений, данных изменении модели часто и каждый раз изменения модели, он получает не синхронизировано с базой данных. Вы можете настроить Entity Framework автоматически удалить и повторно создать базу данных каждый раз при изменении модели данных. Это не проблема на ранних этапах разработки, поскольку тестовые данные легко воссоздать, но после развертывания в рабочей среде обычно требуется обновить схему базы данных без удаления базы данных. Функция миграций позволяет Code First для обновления базы данных без удаления и повторного создания. На ранних этапах цикла разработки проекта может потребоваться использовать [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) drop, повторно создайте и повторно заполнить базу данных при каждом изменении модели. Один вы получите все готово для развертывания приложения, можно преобразовать в этот подход миграции. В этом руководстве будет использоваться только миграций. Дополнительные сведения см. в разделе [Code First Migrations](https://msdn.microsoft.com/data/jj591621) и [миграций серию](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Включение Code First Migrations

1. Из **средства** меню, щелкните **диспетчер пакетов библиотеки** и затем **консоль диспетчера пакетов**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. В `PM>` командной строке введите следующую команду:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![команды Enable-migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Эта команда создает *миграций* папки в проекте ContosoUniversity и он помещает в нее *Configuration.cs* файл, который можно изменять для настройки миграций.

    ![Папку migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration` Класс включает `Seed` метод, вызываемый при создании базы данных, и каждый раз при его обновлении после изменении модели данных.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Цель такого `Seed` метод — научить вас вставляемый тестовые данные в базу данных после Code First его создает или обновляет его.

### <a name="set-up-the-seed-method"></a>Настройка метода заполнения

[Начальное значение](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) метод выполняется, когда Code First Migrations создает базу данных, и каждый раз, он обновляет базу данных на последней миграции. Метод заполнения предназначена для вставки данных в таблицы перед приложения в первый раз обращается к базе данных.

В более ранних версиях Code First, до выпуска миграций было обычным `Seed` методы для вставки проверочных данных, так как при каждом изменении модели во время разработки базы данных должны были быть полностью удаляется и создается заново с нуля. С помощью Code First Migrations, тест, данные сохраняются после изменения базы данных, таким образом включая тестовых данных в [начальное значение](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) метод обычно не требуется. На самом деле, вы не хотите `Seed` метод для вставки данных теста при использовании миграции для развертывания базы данных в рабочей среде, так как `Seed` метод будет выполняться в рабочей среде. В этом случае требуется `Seed` метод для вставки в базу данных только данные, которые требуется вставить в рабочей среде. Например, может потребоваться базы данных, для включения названия фактическое отделов в `Department` таблицы, когда приложение становится доступным для рабочей среды.

В этом учебнике вы будете использовать миграции для развертывания, а `Seed` метод в любом случае Вставка тестовых данных для повышения его читаемости увидеть, как работает функциональных возможностей приложения без необходимости вручную вставить большие объемы данных.

1. Замените содержимое файла *Configuration.cs* файла следующий код, который загрузит тестовые данные в новую базу данных. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [Начальное значение](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) метод принимает объект контекста базы данных как входной параметр и код в метод использует этот объект для добавления новых сущностей в базу данных. Для каждого типа сущности, код создает коллекцию новых сущностей, добавляет их в соответствующий [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) свойства, а затем сохраняет изменения в базе данных. Нет необходимости вызывать [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) метод после каждой группы сущностей, как показано здесь, но это поможет вам найти источник проблемы, если возникает исключение, когда код записывает в базу данных.

    Некоторые операторы, которые вставляют данные используют [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) метод для выполнения операции «вставки-обновления». Так как `Seed` метод работает с каждой миграции, нельзя просто вставить данные, так как строки, вы пытаетесь добавить уже будут существует после первой миграции, который создает базу данных. Операция «вставки-обновления» позволяет избежать ошибок, произойдет при попытке вставить строку, которая уже существует, но он ***переопределяет*** любые изменения данных, внесенные во время тестирования приложения. Тестовыми данными в некоторых таблицах вы не хотите, чтобы происходить: в некоторых случаях при изменении данных во время тестирования необходимым вам изменениям после обновления базы данных. В этом случае необходимо выполнить операцию вставки условного: вставить строку, только в том случае, если он еще не существует. Метод заполнения использует оба подхода.

    Первый параметр, передаваемый [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) метод задает свойство, используемое для проверки, если строка уже существует. Для тестовых данных учащихся, вы предоставляете `LastName` свойство может использоваться для этой цели, так как каждой фамилии в списке является уникальным:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Предполагается, что последний имена являются уникальными. Если вручную добавить учащихся с повторяющимся именем последнего, вы получите следующее исключение при очередном выполнении миграции.

    Последовательность содержит более одного элемента

    Дополнительные сведения о `AddOrUpdate` метод, см. в разделе [Будьте внимательны с помощью метода AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) блога Джули Лерман.

    Код, который добавляет `Enrollment` сущности не используют `AddOrUpdate` метод. Он проверяет, если сущность уже существует и вставляет сущность, если он не существует. Такой подход позволяет сохранить изменения, внесенные для регистрации корпоративного уровня, при выполнении миграции. Код просматривает каждый член `Enrollment` [списка](https://msdn.microsoft.com/library/6sh2ey19.aspx) и если регистрации не найден в базе данных, он добавляет регистрации к базе данных. Первый раз, обновить базу данных, базы данных будут пусты, поэтому он добавляет каждой регистрации.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Сведения об отладке `Seed` метод и способ обработки избыточных данных, например, два учащихся с именем «Александр Carson», см. в разделе [заполнения и отладка Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) в блоге Рика Андерсона:.
2. Выполните построение проекта.

### <a name="create-and-execute-the-first-migration"></a>Создание и выполнение первой миграции

1. В окне консоли диспетчера пакетов введите следующие команды: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration` Команда добавляет в папку Migrations *[метки даты]\_InitialCreate.cs* файл, содержащий код, который создает базу данных. Первый параметр (`InitialCreate)` используется для файла имя и может быть любым; обычно выбрать слово или фразу, которая обобщает, что необходимо сделать после миграции. Например, можно назвать миграцию &quot;AddDepartmentTable&quot;.

    ![Папку migrations с первоначальной миграции](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up` Метод `InitialCreate` класс создает таблицы базы данных, которые соответствуют наборам сущностей модели данных, и `Down` метод удаляет их. Функция миграций вызывает метод `Up`, чтобы реализовать изменения модели данных для миграции. При вводе команды для отката обновления функция миграций вызывает метод `Down`. В следующем коде показано содержимое `InitialCreate` файла:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database` Выполняется команда `Up` метод для создания базы данных, а затем выполняется `Seed` метод для заполнения базы данных.

Базы данных SQL Server создан для вашей модели данных. Имя базы данных — *ContosoUniversity*и *.mdf* файл находится в проекте *приложения\_данных* папку потому что это вы указали в вашей Строка подключения.

Можно использовать либо **обозревателя серверов** или **обозреватель объектов SQL Server** (SSOX) для просмотра базы данных в Visual Studio. В этом руководстве вы используете **обозревателя серверов**. В Visual Studio Express 2012 для Web **обозревателя серверов** называется **обозреватель баз данных**.

1. Из **представление** меню, щелкните **обозревателя серверов**.
2. Нажмите кнопку **добавить подключение** значок.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. При появлении с **Выбор источника данных** диалоговое окно, нажмите кнопку **Microsoft SQL Server**, а затем нажмите кнопку **Продолжить**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. В **добавить подключение** диалогового окна введите **(localdb) \v11.0** для **имя сервера**. В разделе **выберите или введите имя базы данных**выберите **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Нажмите кнопку **ОК**.
6. Разверните **SchoolContext** и раскройте **таблиц**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Щелкните правой кнопкой мыши **учащихся** таблицы и нажмите кнопку **Показать таблицу данных** Чтобы просмотреть созданные столбцы и строки, которые были вставлены в таблицу.

    ![Таблица Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Создание контроллера учащихся и представлений

Следующим шагом является создание ASP.NET MVC контроллера и представлений в приложении, которая может работать с одной из этих таблиц.

1. Для создания `Student` контроллер, щелкните правой кнопкой мыши **контроллеров** папку в **обозревателе решений**выберите **добавить**и нажмите кнопку **контроллера** . В **Добавление контроллера** диалоговое окно, задайте следующие параметры и нажмите кнопку **добавить**: 

   - Имя контроллера: **StudentController**.
   - Шаблон: **контроллер MVC с действиями чтения и записи и представлениями, использующий Entity Framework**.
   - Класс модели: **учащегося (ContosoUniversity.Models)**. (Если вы не видите этот параметр в раскрывающемся списке, постройте проект и повторите попытку.)
   - Класс контекста данных: **SchoolContext (ContosoUniversity.Models)**.
   - Представления: **Razor (CSHTML)**. (По умолчанию).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio открывает *Controllers\StudentController.cs* файла. Видно, что класс переменной будет создана, создающий экземпляр объекта контекста базы данных:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index` Метод действия Получает список учащихся из *учащихся* сущности, установки по `Students` свойство экземпляра контекста базы данных:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml* представление отображает этот список в таблице:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Нажмите клавиши CTRL+F5, чтобы запустить проект.

     Нажмите кнопку **учащихся** tab, чтобы просмотреть тестовые данные, `Seed` добавленные методом.

     ![Страница индекса учащихся](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Соглашения

Объем кода, нужно было написать в порядке для платформы Entity Framework иметь возможность создать для вас всей базы данных из-за использования минимальна *соглашения*, или предположения, сделанные в Entity Framework. Некоторые из них упомянутые ранее:

- Во множественном числе форм имен классов сущностей используются в качестве имен таблиц.
- В качестве имен столбцов используются имена свойств сущностей.
- Свойства сущности, которые называются `ID` или *classname* `ID` распознаются как свойства первичного ключа.

Вы уже видели, что могут быть переопределены соглашения (например, вы указали что имена таблиц не должны быть имена во множественном числе), и вы узнаете о условные обозначения и их в Переопределите [Создание более сложной модели данных](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) учебника Далее в этой серии. Дополнительные сведения см. в разделе [первый соглашения о коде](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Сводка

Теперь вы создали простое приложение, которое использует Entity Framework и SQL Server Express для хранения и отображения данных. В следующем руководстве вы узнаете, как выполнять основные CRUD (Создание, чтение, обновление и удаление) операции. Вы можете оставить отзыв в нижней части этой страницы. Сообщите нам, как вам понравилось этой части руководства, и как можно улучшить его.

Ссылки на другие ресурсы Entity Framework можно найти в [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Вперед](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
