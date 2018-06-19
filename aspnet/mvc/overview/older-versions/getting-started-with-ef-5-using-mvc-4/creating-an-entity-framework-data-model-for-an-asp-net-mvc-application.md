---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Создание модели данных Entity Framework для приложения ASP.NET MVC (часть 1 из 10) | Документы Microsoft
author: tdykstra
description: Для Visual Studio 2013, Entity Framework 6 и MVC 5 доступна более новая версия этого учебника ряда. De приложения web образец университета Contoso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a963f26b408f2a54bd9cd3e852bc1e368f86c41f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877935"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Создание модели данных Entity Framework для приложения ASP.NET MVC (часть 1 из 10)
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Объект [более новой версии этого учебника ряда](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) доступно для Visual Studio 2013, Entity Framework 6 и MVC 5.
> 
> 
> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 4, с помощью Entity Framework 5 и Visual Studio 2012. В этом примере приложения реализуется веб-сайт вымышленного университета Contoso. На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей. Этот учебник ряд объясняется, как создать пример приложения Contoso университета. Вы можете [загрузка завершенного приложения](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Существует три способа, можно работать с данными в Entity Framework: *Database First*, *Model First*, и *Code First*. Этот учебник предназначен для Code First. Сведения о различиях между этими рабочими процессами и рекомендации о том, как выбирать наиболее подходящий для вашего сценария см. в разделе [процессов разработки Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Образец приложения основана на [ASP.NET MVC](../../../index.md). Если вы предпочитаете работать с моделью веб-форм ASP.NET, см. раздел [привязки модели и веб-форм](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) учебные курсы и [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Версии программного обеспечения
> 
> | **В этом учебнике показано** | **Также работает с** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio Express 2012 для Web. Это автоматически устанавливается с Windows Azure SDK, если нет Visual STUDIO 2012 или VS 2012 Express для Web. Visual Studio 2013 должна работать, но учебника не были проверены с ним, и некоторые пункты выбирает в меню и диалоговые окна отличаются. [Windows Azure SDK версии Visual STUDIO 2013](https://go.microsoft.com/fwlink/p/?linkid=323510) является обязательным для развертывания Windows Azure. |
> | .NET 4.5 | Большинство функций показано будет работать в .NET 4, но некоторые не будет. Например для поддержки перечисления в EF требуется .NET 4.5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Если пропустить шаги развертывания Windows Azure SDK не требуется. При выпуске новой версии пакета SDK, ссылка будет установить новую версию. В этом случае может потребоваться изменить некоторые инструкции для нового пользовательского интерфейса и функции. |
> 
> ## <a name="questions"></a>Вопросы
> 
> Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework и LINQ to Entities форум](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), или [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Благодарности
> 
> См. в учебнике последней в серии для [подтверждений и заметки о VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Исходной версии учебника
> 
> Исходной версии учебника доступен в [EF 4.1 и MVC 3 электронную](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Веб-приложение Contoso университета

В рамках этих учебников вы будете создавать приложение, которое представляет собой простой веб-сайт университета.

Пользователи приложения могут просматривать и обновлять сведения об учащихся, курсах и преподавателях. Будет создано несколько экранов.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Стиль пользовательского интерфейса этого сайта практически полностью основан на встроенных шаблонах, поскольку это позволяет сосредоточиться на изучении и использовании возможностей платформы Entity Framework.

## <a name="prerequisites"></a>Предварительные требования

Направления, а также снимков экрана в этом учебнике предполагается, что вы используете [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) или [Visual Studio 2012 Express для Web](https://go.microsoft.com/fwlink/?LinkID=275131)с последние обновления и пакет Azure SDK для .NET, начиная с июля, установлен 2013. Чтобы узнать все это с помощью следующей ссылки:

[Azure SDK для .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Если у вас установлено приложение Visual Studio, приведенную выше ссылку установит недостающие компоненты. Если у вас нет Visual Studio, по ссылке установит Visual Studio 2012 Express для Web. Можно использовать Visual Studio 2013, но некоторые необходимые процедуры и экраны будут отличаться.

## <a name="create-an-mvc-web-application"></a>Создание веб-приложения MVC

Откройте Visual Studio и создайте новый проект C# с именем «ContosoUniversity» с помощью **веб-приложение ASP.NET MVC 4** шаблона. Убедитесь, что целевая **.NET Framework 4.5** (вы будете использовать [ `enum` свойства](https://msdn.microsoft.com/data/hh859576.aspx), что требует .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

В **нового проекта ASP.NET MVC 4** диалогового окна выберите **веб-приложение** шаблона.

Оставить **Razor** системы выбранных представлений и оставить **Создание проекта модульного теста** флажок снят.

Нажмите кнопку **ОК**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Настройка стиля узла

Выполните незначительную настройку меню, макета и домашней страницы сайта.

Откройте *представления\общие\\_Layout.cshtml*и замените содержимое файла следующим кодом. Изменения выделены.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Этот код вносит следующие изменения:

- Заменяет экземпляры шаблона «My ASP.NET MVC Application» и «ваша эмблема здесь» «Contoso университета».
- Добавляет несколько ссылок действий, которые будут использоваться позже в этом учебнике.

В *Views\Home\Index.cshtml*, замените содержимое файла следующим кодом, чтобы исключить абзацев шаблона о ASP.NET и MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

В *Controllers\HomeController.cs*, измените значение `ViewBag.Message` в `Index` методом действия «Вас приветствует университета Contoso!», как показано в следующем примере:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Нажмите клавиши CTRL + F5 для запуска сайта. Можно просмотреть на домашней странице с главным меню.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Создание модели данных

Теперь необходимо создать классы сущностей для приложения университета Contoso. Начните с тремя следующими сущностями:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Между сущностями `Student` и `Enrollment`, а также между сущностями `Course` и `Enrollment` существует отношение "один ко многим". Другими словами, учащийся может быть зарегистрирован в любом количестве курсов, а в отдельном курсе может быть зарегистрировано любое количество учащихся.

В следующих разделах создаются классы для каждой из этих сущностей.

> [!NOTE]
> При попытке скомпилировать проект до завершения создания всех этих классов сущностей, то возникнут ошибки компилятора.


### <a name="the-student-entity"></a>Сущность Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

В *моделей* папке создайте *Student.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Свойство `StudentID` будет использоваться в качестве столбца первичного ключа в таблице базы данных, соответствующей этому классу. По умолчанию платформа Entity Framework интерпретирует свойство, которое называется `ID` или *classname* `ID` как первичный ключ.

`Enrollments` Свойство *свойство навигации*. Свойства навигации содержат другие сущности, связанные с этой сущностью. В этом случае `Enrollments` свойство `Student` сущность будет содержать все `Enrollment` сущностей, которые относятся к, `Student` сущности. Другими словами Если данной `Student` строк в базе данных есть две связанные `Enrollment` строк (значение строки, содержащие этого студента первичного ключа в их `StudentID` столбец внешнего ключа), которая `Student` сущности `Enrollments` свойство навигации будет содержать этих двух `Enrollment` сущностей.

Свойства навигации, обычно определяются как `virtual` так, чтобы такие как воспользоваться преимуществами некоторых возможностей Entity Framework *отложенную загрузку*. (Отложенная загрузка будут описаны далее в [чтение связанных данных](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) учебника далее в этой серии.

Если свойство навигации может содержать несколько сущностей (как в отношениях "многие ко многим" или "один ко многим"), оно должно иметь тип списка, допускающий добавление, удаление и обновление записей, такой как `ICollection`.

### <a name="the-enrollment-entity"></a>Сущность регистрации

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

В папке *Models* создайте файл *Enrollment.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Свойство уровень [перечисления](https://msdn.microsoft.com/data/hh859576.aspx). Знак вопроса после `Grade` объявление типа указывает, что `Grade` свойство [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx). Степень, которую имеет значение null отличается от нуля оценку — значение null означает оценку не известен или еще не назначена.

Свойство `StudentID` представляет собой внешний ключ. Ему соответствует свойство навигации `Student`. Сущность `Enrollment` связана с одной сущностью `Student`, поэтому это свойство может содержать одну сущность `Student` (в отличие от представленного ранее свойства навигации `Student.Enrollments`, которое может содержать несколько сущностей `Enrollment`).

Свойство `CourseID` представляет собой внешний ключ. Ему соответствует свойство навигации `Course`. Сущность `Enrollment` связана с одной сущностью `Course`.

### <a name="the-course-entity"></a>Сущность курса

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

В *моделей* папке создайте *Course.cs*, заменив существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

Свойство `Enrollments` является свойством навигации. Сущность `Course` может быть связана с любым числом сущностей `Enrollment`.

Допустим, что больше о [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([параметр DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Нет)] атрибута в следующем уроке. Фактически, этот атрибут позволяет ввести первичный ключ для курса, а не использовать базу данных, чтобы создать его.

## <a name="create-the-database-context"></a>Создание контекста базы данных

Основной класс, который координирует функции в заданной модели данных Entity Framework является *контекст базы данных* класса. Создание этого класса путем наследования от [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) класса. В коде указываются сущности, которые включаются в модель данных. Также вы можете настроить реакцию платформы Entity Framework на некоторые события. В этом проекте соответствующий класс называется `SchoolContext`.

Создайте папку с именем *DAL* (для доступа к данным). В этой папке создайте новый файл класса с именем *SchoolContext.cs*и замените существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Этот код создает [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) свойство для каждого набора сущностей. В терминологии Entity Framework *набора сущностей* обычно соответствует таблице базы данных и *сущности* соответствует строке таблицы.

`modelBuilder.Conventions.Remove` Инструкции в [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) метод запрещает имена таблиц имена во множественном числе. Если это не сделать, созданные таблицы будет называться `Students`, `Courses`, и `Enrollments`. Вместо этого имена таблиц будут `Student`, `Course`, и `Enrollment`. В среде разработчиков нет единого мнения о том, следует ли использовать имена таблиц во множественном числе. В этом учебнике используется существительные в единственном числе, но важно то, что вы можете выбрать любой формы, вы предпочитаете, включая или исключая эту строку кода.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) — это облегченная версия SQL Server Express Database Engine, запускаемая по запросу и запускается в пользовательском режиме. LocalDB выполняется в режиме выполнения специальных SQL Server Express, позволяет работать с базами данных как *.mdf* файлов. Как правило, хранятся файлы базы данных LocalDB в *приложения\_данные* папку веб-проекта. Функции пользовательского экземпляра в SQL Server Express позволяет также работать с *.mdf* файлов, но функции пользовательского экземпляра рекомендуется; таким образом, рекомендуется использовать LocalDB для работы с *.mdf* файлов.

Обычно SQL Server Express не используется для рабочей веб-приложений. LocalDB в частности не рекомендуется для использования в рабочей среде веб-приложению, так как он не предназначен для работы со службами IIS.

В Visual Studio 2012 и более поздних версиях LocalDB устанавливается по умолчанию вместе с Visual Studio. В Visual Studio 2010 и более ранних версиях SQL Server Express (без LocalDB) устанавливается по умолчанию вместе с Visual Studio; необходимо вручную установить его, если вы используете Visual Studio 2010.

В этом учебнике вы будете работать с LocalDB, чтобы база данных может храниться в *приложения\_данные* папке, что *.mdf* файла. Откройте корневой *Web.config* и добавьте новую строку подключения для файла `connectionStrings` коллекции, как показано в следующем примере. (Убедитесь, что при обновлении *Web.config* файл в корневой папке проекта. Имеется также *Web.config* файл находится в *представления* вложенной папки, которая не требует обновления.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

По умолчанию, Entity Framework ищет строку подключения с именем `DbContext` класса (`SchoolContext` для этого проекта). В строке подключения, вы добавили указан с именем базы данных LocalDB *ContosoUniversity.mdf* в *приложения\_данные* папки. Дополнительные сведения см. в разделе [строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Не требуется фактически укажите строку подключения. Если не указать строку подключения, Entity Framework создаст ее автоматически; Однако база данных может не быть *приложения\_данные* папку приложения. Сведения о котором будет создана база данных см. в разделе [Code First в новой базе данных](https://msdn.microsoft.com/data/jj193542).

`connectionStrings` Коллекция также содержит строку подключения с именем `DefaultConnection` которого используется для базы данных членства. Вы не используете базы данных членства в этом учебнике. Единственное различие между двух строки подключения является имя базы данных и значение атрибута name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Настройка и выполнение первой миграции кода

При первом запуске для разработки приложений данные изменений в модели часто и каждый раз изменения модели, он получает синхронизации с базой данных. Можно настроить для автоматического удаления и повторного создания базы данных каждый раз при изменении модели данных Entity Framework. Это не проблема раннем этапе разработки, поскольку легко создать данные теста, но после развертывания в рабочей среде обычно требуется обновить схему базы данных без удаления базы данных. Функция миграции позволяет Code First обновить базу данных без удаления и повторного создания. В начале цикла разработки нового проекта может потребоваться использовать [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) drop, создайте заново и повторно инициализировать базу данных при каждом запуске изменения модели. Один вы получаете все готово для развертывания приложения, можно преобразовать в подход миграции. В этом учебнике вы используете только миграции. Дополнительные сведения см. в разделе [Code First Migrations](https://msdn.microsoft.com/data/jj591621) и [миграций серию](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Включить Code First Migrations

1. Из **средства** меню, нажмите кнопку **диспетчер пакетов библиотеки** и затем **консоль диспетчера пакетов**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. В `PM>` строке введите следующую команду:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![команду Enable-migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Эта команда создает *миграций* папки в проект ContosoUniversity и помещает в этой папке *Configuration.cs* файл, который можно изменять для настройки миграции.

    ![Миграция папки](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration` Класс включает `Seed` метод, вызываемый при создании базы данных, и каждый раз при обновлении после изменения модели данных.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Цель этого `Seed` проще позволяют вставить тестовые данные в базу данных после Code First его создает или обновляет его.

### <a name="set-up-the-seed-method"></a>Настройка Seed-метод

[Начальное значение](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) метод выполняется при миграции Code First создает базу данных, и каждый раз, он обновляет базу данных на последней миграции. Seed-метод предназначен для вставки данных в таблицах, прежде чем приложение в первый раз обращается к базе данных.

В более ранних версиях Code First, до выпуска миграции было часто `Seed` методы для вставки проверочных данных, так как при каждом изменении модели во время разработки базы данных полностью удалить и повторно создать с нуля. Code First Migrations, тестирования, данные сохраняются после изменения базы данных, поэтому включив тестовых данных в [начальное значение](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) метод обычно не требуется. На самом деле вы не хотите `Seed` для вставки тестовых данных, если вы будете использовать миграции для развертывания базы данных в рабочей среде, так как `Seed` метод будет выполняться в рабочей среде. В этом случае требуется `Seed` для вставки в базу данных только данные, которые нужно вставить в рабочей среде. Например, может потребоваться базы данных, для включения отдел фактические имена в `Department` таблицы, когда приложение становится доступной в рабочей среде.

В этом учебнике вы будете использовать миграции для развертывания, а `Seed` метод все равно вставит тестовых данных позволит лучше понять, как работает функциональных возможностей приложения без необходимости вставлять большой объем данных вручную.

1. Замените содержимое *Configuration.cs* файл следующий код, который будет загружать данные теста в базе данных. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [Начальное значение](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) метод принимает объекта контекста базы данных в качестве входного параметра и кода в методе использует этот объект для добавления новых сущностей в базу данных. Для каждого типа сущности, код создает коллекцию новых сущностей, добавляет их в соответствующую [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) свойства, а затем сохраняет изменения в базе данных. Нет необходимости вызывать [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) метод после каждой группы сущностей, как показано здесь, но это поможет найти источник проблемы, если исключение возникает, когда код производит запись в базу данных.

    Некоторые операторы, которые вставляют данные с помощью [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) метод для выполнения операции «вставки-обновления». Поскольку `Seed` метод выполняется с каждой миграции, просто нельзя вставить данные, так как вы пытаетесь добавить строки уже существует после первой миграции, который создает базу данных. Операция «вставки-обновления» позволяет избежать ошибок, произойдет при попытке вставить строку, которая уже существует, но он ***переопределяет*** любые изменения данных, внесенные во время тестирования приложения. С тестовыми данными в некоторых таблицах не имеет смысла, происходит: в некоторых случаях при изменении данных во время тестирования требуется изменения после обновления базы данных. В этом случае необходимо выполнить операцию вставки условного: вставьте строку только в том случае, если он еще не существует. Seed-метод использует оба подхода.

    Первый параметр, передаваемый [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) метод задает свойство, используемое для проверки, если строки уже существует. Для тестовых данных студентов, которому предоставляется `LastName` свойство может использоваться для этой цели, так как каждый последнее имя в списке является уникальным:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Данный код предполагает, что последний имена являются уникальными. Если вручную добавить студента с повторяющимся именем последней, вы получите следующее исключение при последующем выполнении миграции.

    Последовательность содержит более одного элемента

    Дополнительные сведения о `AddOrUpdate` метода, в разделе [Будьте внимательны с EF 4.3 AddOrUpdate метод](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) в блоге Джули Лерман.

    Код, который добавляет `Enrollment` сущностей не использует `AddOrUpdate` метод. Проверяется, если сущность уже существует и вставляет сущность, если он не существует. Этот подход сохранит изменения, внесенные уровень регистрации при выполнении миграции. Код выполняет цикл через каждый член `Enrollment` [список](https://msdn.microsoft.com/library/6sh2ey19.aspx) и если регистрации не найден в базе данных, он добавляет регистрацию в базу данных. При первом обновлении базы данных, база данных будет пустым, он добавляет каждый регистрации.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Сведения об отладке `Seed` метод и как обрабатывать избыточных данных, например двух учащихся с именем «Александр Carson», см. [заполнения и отладка Entity Framework (EF) БД](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) в блоге Рика Андерсона.
2. Выполните построение проекта.

### <a name="create-and-execute-the-first-migration"></a>Создание и выполнение первой миграции

1. В окне консоли диспетчера пакетов введите следующие команды: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration` Команда добавляет папку Migrations *[метки даты]\_InitialCreate.cs* файл, содержащий код, который создает базу данных. Первый параметр (`InitialCreate)` используется для файла имя и может принимать любые необходимые; обычно выбрать слово или фразу, в которой говорится, что выполняется в процессе переноса. Например, можно назвать последующей миграции &quot;AddDepartmentTable&quot;.

    ![Папка миграции с первоначальной миграции](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up` Метод `InitialCreate` класс создает таблицы базы данных, которые соответствуют наборов сущностей модели данных, и `Down` метод удаляет их. Функция миграций вызывает метод `Up`, чтобы реализовать изменения модели данных для миграции. При вводе команды для отката обновления функция миграций вызывает метод `Down`. В следующем коде показано содержимое `InitialCreate` файла:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database` Выполняется команда `Up` метод для создания базы данных, а затем выполняется `Seed` метод для заполнения базы данных.

База данных SQL Server теперь был создан для вашей модели данных. Имя базы данных — *ContosoUniversity*и *.mdf* файл находится в вашем проекте *приложения\_данных* папки так как указанный в вашей Строка подключения.

Можно использовать любой **обозревателя серверов** или **обозреватель объектов SQL Server** (SSOX) для просмотра базы данных в Visual Studio. В этом учебнике будет использоваться **обозревателя серверов**. В Visual Studio Express 2012 для Web **обозревателя серверов** называется **обозревателя базы данных**.

1. Из **представление** меню, нажмите кнопку **обозревателя серверов**.
2. Нажмите кнопку **добавить подключение** значок.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. При появлении соответствующего запроса с **Выбор источника данных** диалоговое окно, нажмите кнопку **Microsoft SQL Server**, а затем нажмите кнопку **Продолжить**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. В **добавить подключение** диалогового окна введите **(localdb) \v11.0** для **имя сервера**. В разделе **выберите или введите имя базы данных**выберите **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Нажмите кнопку **ОК**.
6. Разверните **SchoolContext** и разверните **таблиц**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Щелкните правой кнопкой мыши **студента** и нажмите кнопку **Показать таблицу данных** столбцы, которые были созданы и строк, которые были вставлены в таблицу.

    ![Таблица учащихся](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Создание контроллера студентов и представлений

Следующим шагом является создание ASP.NET MVC контроллер и представления в приложении, которая может работать с одной из этих таблиц.

1. Для создания `Student` контроллера, щелкните правой кнопкой мыши **контроллеров** папки в **обозревателе решений**выберите **добавить**и нажмите кнопку **контроллера** . В **добавления контроллера** диалогового окна, задайте следующие параметры и нажмите кнопку **добавить**: 

   - Имя контроллера: **StudentController**.
   - Шаблон: **контроллер MVC с действиями чтения и записи и представлениями, использующий Entity Framework**.
   - Класс модели: **студента (ContosoUniversity.Models)**. (Если вы не видите этот параметр в раскрывающемся списке, постройте проект и повторите попытку.)
   - Класс контекста данных: **SchoolContext (ContosoUniversity.Models)**.
   - Представления: **Razor (CSHTML)**. (По умолчанию).

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio открывает *Controllers\StudentController.cs* файла. Вы увидите, что переменная класса будет создана, создает экземпляр объекта контекста базы данных:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index` Метод действия Получает список студентов *учащихся* посредством чтения набора сущностей `Students` свойства экземпляра контекста базы данных:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml* представление отображает этот список в таблице:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Нажмите клавиши CTRL+F5, чтобы запустить проект.

     Нажмите кнопку **учащихся** вкладку, чтобы увидеть данные теста, `Seed` метод вставки.

     ![Страница индекса студента](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Соглашения

Из-за использования объем кода, вы должны были написать в порядке иметь возможность создавать всей базы данных автоматически Entity Framework сводится к минимуму *соглашения*, или предположения, что позволяет платформе Entity Framework. Некоторые из них уже было отмечено:

- Pluralized формы имен классов сущностей, которые используются как имена таблиц.
- В качестве имен столбцов используются имена свойств сущностей.
- Свойства сущности, которые именуются `ID` или *classname* `ID` распознаются как свойства основного ключа.

Вы уже видели, что может быть переопределен соглашения (например, вы указали, имена таблиц не должны быть имена во множественном числе), и вы узнаете о условные обозначения и переопределить их в [создания более сложной модели данных](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) учебника Далее в этой серии. Дополнительные сведения см. в разделе [соглашения о написании кода для первого](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Сводка

Теперь вы создали простого приложения, использующего платформу Entity Framework и экспресс-выпуска SQL Server для хранения и отображения данных. В этом руководстве вы узнаете, как выполнять основные CRUD (Создание, чтение, обновление и удаление) операции. Можно оставить отзывы в нижней части этой страницы. Сообщите нам, как вам понравилось этой части учебника и как можно улучшить его.

Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Вперед](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
