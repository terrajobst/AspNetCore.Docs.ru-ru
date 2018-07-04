---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Начало работы с Entity Framework 6 Code First с помощью MVC 5 | Документация Майкрософт
author: tdykstra
description: 'Доступна новая версия этой серии руководств: начало работы с ASP.NET Core и Entity Framework Core в Visual Studio 2015. Contoso Universi...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1b8d78954746cd6908f9ca9c2a51591f45fa01f7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403087"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Начало работы с Entity Framework 6 Code First с помощью MVC 5
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Доступна новая версия этой серии руководств: [приступить к работе с ASP.NET Core и Entity Framework Core в Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 5, используя Entity Framework 6 и Visual Studio 2013. В этом учебнике используется Code First рабочего процесса. Сведения о том, как выбрать Code First, Database First или Model First, см. в разделе [рабочие процессы разработки Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> В этом примере приложения реализуется веб-сайт вымышленного университета Contoso. На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей. В этой серии руководств описывается построение примера приложения университета Contoso. Вы можете [загрузить готовое приложение](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Доступна версия Visual Basic, преобразовываются Майк Бринд: [MVC 5 с EF 6 в Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) на сайте Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемые в этом руководстве
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (пакета NuGet EntityFramework 6.1.0)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (необязательно)
>   
> 
> Руководство также должны работать с [Visual Studio 2013 Express для Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) или Visual Studio 2012. [Windows Azure SDK версии Visual STUDIO 2012](https://go.microsoft.com/fwlink/p/?linkid=323511) необходим для развертывания Windows Azure с помощью Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Учебника по версии
> 
> Предыдущие версии этого учебника, см. в разделе [EF 4.1 и MVC 3 электронная книга](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) и [Приступая к работе с EF 5 с помощью MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте свои отзывы на том, как вам понравилось, и этот учебник и что можно улучшить в комментариях в нижней части страницы. Если у вас есть вопросы, которые не имеют отношения к руководству, их можно разместить [форум ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework и LINQ to Entities форум](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), или [ StackOverflow.com](http://stackoverflow.com/).
> 
> Если вы столкнулись с проблемами, которые не удается разрешить, обычно можно найти решение проблемы путем сравнения кода готового проекта, который можно загрузить. Некоторые распространенные ошибки и способы их устранения, см. в разделе [распространенные ошибки и решения или обходные пути для них.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Веб-приложение университета Contoso

В рамках этих учебников вы будете создавать приложение, которое представляет собой простой веб-сайт университета.

Пользователи приложения могут просматривать и обновлять сведения об учащихся, курсах и преподавателях. Будет создано несколько экранов.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Изменение параметров учащегося](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Стиль пользовательского интерфейса этого сайта практически полностью основан на встроенных шаблонах, поскольку это позволяет сосредоточиться на изучении и использовании возможностей платформы Entity Framework.

## <a name="prerequisites"></a>Предварительные требования

См. в разделе **версий программного обеспечения** в верхней части страницы. Entity Framework 6 не является требованием, так как вы установите пакет EF NuGet в рамках этого руководства.

## <a name="create-an-mvc-web-application"></a>Создать веб-приложение MVC

Откройте Visual Studio и создайте новый проект C# Web, с именем «ContosoUniversity».

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

В **новый проект ASP.NET** диалогового окна выберите **MVC** шаблона.

Если **разместить в облаке** флажок в **Microsoft Azure** разделе установлен, снимите его.

Нажмите кнопку **изменить параметры проверки подлинности**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

В **изменить способ проверки подлинности** выберите **без проверки подлинности**, а затем нажмите кнопку **ОК**. В этом руководстве вы не быть предлагать пользователям войти в систему или ограничение доступа в зависимости от того, кто вошел в систему.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Вернитесь в диалоговое окно "новый проект ASP.NET", щелкните **ОК** для создания проекта.

## <a name="set-up-the-site-style"></a>Настройка стиля сайта

Выполните незначительную настройку меню, макета и домашней страницы сайта.

Откройте *Views\Shared\\_Layout.cshtml*и внесите следующие изменения:

- Замените все вхождения «My ASP.NET Application» и «Имя приложения» на «Contoso University».
- Добавление пунктов меню для учащихся, курсов, преподавателей и отделов и удалите запись контакта.

Изменения выделены.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

В *Views\Home\Index.cshtml*, замените содержимое файла следующим кодом, который заменяет текст о ASP.NET и MVC об этом приложении:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Нажмите клавиши CTRL + F5 для запуска сайта. Появится домашняя страница с меню.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Установка Entity Framework 6

Из **средства** меню **диспетчер пакетов NuGet** и нажмите кнопку **консоль диспетчера пакетов**.

В **консоль диспетчера пакетов** окна введите следующую команду:

`Install-Package EntityFramework`

![EF установлен](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

На рисунке показано 6.0.0 устанавливается, но NuGet установит последнюю версию Entity Framework (за исключением предварительных версий), который на момент последнего изменения руководства является 6.1.1.

Этот шаг является одним из нескольких этапов, в этом руководстве содержится можно сделать вручную, что которого можно было бы сделать автоматически с помощью функции формирования шаблонов ASP.NET MVC. Вы выполняете их вручную, чтобы вы могли видеть шаги, необходимые для использования платформы Entity Framework. Вы воспользуетесь формирования шаблонов позже для создания контроллера и представлений MVC. Альтернативным вариантом является для формирования шаблонов автоматически установить пакет EF NuGet создать класс контекста базы данных и создания строки подключения. Когда вы будете готовы сделать это таким образом, что необходимо сделать всего лишь пропустить эти шаги и сформировать шаблон контроллера MVC после создания классов сущностей.

## <a name="create-the-data-model"></a>Создание модели данных

Теперь необходимо создать классы сущностей для приложения университета Contoso. Сначала вы возьмете следующих трех сущностей:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Между сущностями `Student` и `Enrollment`, а также между сущностями `Course` и `Enrollment` существует отношение "один ко многим". Другими словами, учащийся может быть зарегистрирован в любом количестве курсов, а в отдельном курсе может быть зарегистрировано любое количество учащихся.

В следующих разделах создаются классы для каждой из этих сущностей.

> [!NOTE]
> При попытке компиляции проекта до завершения создания всех этих классов сущностей, вы получите ошибки компилятора.


### <a name="the-student-entity"></a>Сущность Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

В *моделей* папке создайте файл класса с именем *Student.cs* и замените код шаблона следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

Свойство `ID` будет использоваться в качестве столбца первичного ключа в таблице базы данных, соответствующей этому классу. По умолчанию Entity Framework интерпретирует свойство с именем `ID` или *classname* `ID` как первичный ключ.

`Enrollments` Свойство *свойство навигации*. Свойства навигации содержат другие сущности, связанные с этой сущностью. В этом случае `Enrollments` свойство `Student` сущность будет содержать все `Enrollment` сущностей, которые связаны `Student` сущности. Другими словами если заданный `Student` строк в базе данных имеет два связанных `Enrollment` строк (значение строки, которые содержат первичный ключ этого учащегося в их `StudentID` внешний ключевой столбец), в котором `Student` сущности `Enrollments` свойство навигации будет содержать две этих `Enrollment` сущностей.

Свойства навигации, обычно определяются как `virtual` таким образом, чтобы они можно воспользоваться преимуществами определенные функциональные возможности Entity Framework, такие как *отложенная загрузка*. (Отложенная загрузка, будет рассматриваться далее в [чтение связанных данных](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) далее в этой серии руководств.)

Если свойство навигации может содержать несколько сущностей (как в отношениях "многие ко многим" или "один ко многим"), оно должно иметь тип списка, допускающий добавление, удаление и обновление записей, такой как `ICollection`.

### <a name="the-enrollment-entity"></a>Сущность Enrollment

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

В папке *Models* создайте файл *Enrollment.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Свойство будет иметь первичный ключ; эта сущность использует *classname* `ID` шаблонов вместо `ID` , как вы видели в `Student` сущности. Как правило, следует выбирать один шаблон, который будет использоваться в рамках всей модели данных. В этом случае демонстрируется возможность использования любого из шаблонов. В этом руководстве, вы увидите как с помощью `ID` без `classname` позволяет упростить реализацию наследования в модели данных.

`Grade` Свойство [перечисления](https://msdn.microsoft.com/data/hh859576.aspx). Знак вопроса после `Grade` объявление типа указывает, что `Grade` свойство [допускает значения NULL](https://msdn.microsoft.com/library/2cf62fcy.aspx). Корпоративного класса, который имеет значение null отличается от нулевой оценки тем — значение null означает, что это оценка не известна или еще не назначена.

Свойство `StudentID` представляет собой внешний ключ. Ему соответствует свойство навигации `Student`. Сущность `Enrollment` связана с одной сущностью `Student`, поэтому это свойство может содержать одну сущность `Student` (в отличие от представленного ранее свойства навигации `Student.Enrollments`, которое может содержать несколько сущностей `Enrollment`).

Свойство `CourseID` представляет собой внешний ключ. Ему соответствует свойство навигации `Course`. Сущность `Enrollment` связана с одной сущностью `Course`.

Платформа Entity Framework интерпретирует свойство как свойство внешнего ключа, если оно имеет имя *&lt;имя свойства навигации&gt;&lt;имя свойство первичного ключа&gt;* (например, `StudentID`для `Student` свойство навигации с момента `Student` — первичный ключ сущности `ID`). Свойства внешнего ключа также могут называться так же просто *&lt;имя свойство первичного ключа&gt;* (например, `CourseID` поскольку `Course` — первичный ключ сущности `CourseID`).

### <a name="the-course-entity"></a>Сущности Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

В *моделей* папке создайте *Course.cs*, заменив код шаблона следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Свойство `Enrollments` является свойством навигации. Сущность `Course` может быть связана с любым числом сущностей `Enrollment`.

Мы скажем: сведения о [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) атрибута в этом руководстве этой серии. Фактически, этот атрибут позволяет ввести первичный ключ для курса, а не использовать базу данных, чтобы создать его.

## <a name="create-the-database-context"></a>Создание контекста базы данных

Основным классом, который координирует функциональные возможности Entity Framework для заданной модели данных является *контекст базы данных* класса. Этот класс создается путем наследования от [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) класса. В коде указываются сущности, которые включаются в модель данных. Также вы можете настроить реакцию платформы Entity Framework на некоторые события. В этом проекте соответствующий класс называется `SchoolContext`.

Чтобы создать папку в проекте ContosoUniversity, щелкните правой кнопкой мыши проект в **обозревателе решений** и нажмите кнопку **добавить**, а затем нажмите кнопку **новую папку**. Назовите новую папку *DAL* (для уровня доступа к данным). В этой папке создайте новый файл класса с именем *SchoolContext.cs*и замените код шаблона следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Указание наборов сущностей

Этот код создает [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) свойство для каждого набора сущностей. В терминологии Entity Framework *набор сущностей* обычно соответствует таблице базы данных и *сущности* соответствует строке в таблице.

> [!NOTE] 
> 
> Можно было опустить `DbSet<Enrollment>` и `DbSet<Course>` инструкций и он будет работать так же. Платформа Entity Framework включает эти инструкции неявно, поскольку сущность `Student` ссылается на сущность `Enrollment`, а `Enrollment` ссылается на сущность `Course`.


### <a name="specifying-the-connection-string"></a>Указание строки подключения

Имя строки подключения (который вы добавите в файл Web.config более поздней версии), переданный в конструктор.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Можно также передавать в строке подключения вместо имени он хранится в файле Web.config. Дополнительные сведения о параметрах для указания базы данных для использования, см. в разделе [Entity Framework — подключений и модели](https://msdn.microsoft.com/data/jj592674).

Если явно не указать строку подключения или имя одного, Entity Framework предполагает, что имя строки подключения — это же имя класса. Имя строки подключения по умолчанию в этом примере будут `SchoolContext`, так же, как то, что при указании явным образом.

### <a name="specifying-singular-table-names"></a>Указание имен таблиц в единственном числе

`modelBuilder.Conventions.Remove` Инструкции в [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) метод предотвращает имена таблиц имена во множественном числе. Если вы не сделаете этого, созданные таблицы в базе данных будет назван `Students`, `Courses`, и `Enrollments`. Вместо этого будет имена таблиц `Student`, `Course`, и `Enrollment`. В среде разработчиков нет единого мнения о том, следует ли использовать имена таблиц во множественном числе. В этом руководстве используется существительные в единственном числе, но важно то, что вы можете выбрать любую форму, вы предпочитаете, включая или исключая эту строку кода.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Настройка EF для инициализации базы данных тестовыми данными

Entity Framework можно автоматически создать (или удалить и повторно создать) базы данных автоматически при запуске приложения. Можно указать, что это следует делать каждый раз при запуске приложения или только в том случае, если модель не синхронизирован с существующей базой данных. Можно также написать `Seed` метод, Entity Framework вызывает автоматически после создания базы данных заполняет ее тестовыми данными.

Поведение по умолчанию — создать базу данных только в том случае, если он не существует (и создания исключения, если модель данных была изменена, и база данных уже существует). В этом разделе вы указываете, что базы данных нужно удалить и повторно создается при каждом изменении модели. Удаление базы данных приводит к потере всех данных. Обычно это ОК во время разработки, так как `Seed` метод выполняется в том случае, если база данных создается заново и повторно создают тестовые данные. Однако в рабочей среде вы обычно не хотите потерять все данные каждый раз, когда необходимо изменить схему базы данных. Позже вы узнаете, как обрабатывать изменения модели с помощью Code First Migrations для изменения схемы базы данных вместо удаления и повторного создания базы данных.

В папке DAL, создайте новый файл класса с именем *SchoolInitializer.cs* и замените код шаблона с помощью  
Следующий код, который обеспечивает создана, когда требуется и загружает тестовые данные в новую базу данных.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed` Метод принимает объект контекста базы данных как входной параметр и код в метод использует этот объект для добавления новых сущностей в базу данных. Для каждого типа сущности, код создает коллекцию новых сущностей, добавляет их в соответствующий `DbSet` свойства, а затем сохраняет изменения в базе данных. Нет необходимости вызывать `SaveChanges` метод после каждой группы сущностей, как показано здесь, но это поможет вам найти источник проблемы, если возникает исключение, когда код записывает в базу данных.

Чтобы сообщить Entity Framework использовать инициализатор класса, добавьте элемент для `entityFramework` элемент в приложении *Web.config* файл (в корневой папке проекта), как показано в следующем примере:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type` Указывает контекст полное имя класса и сборки, он находится, и `databaseinitializer type` указывает полное имя инициализатор класса и сборки, он выполняется. (Если вы не хотите использовать инициализатор EF, можно задать атрибут на `context` элемент: `disableDatabaseInitialization="true"`.) Дополнительные сведения см. в разделе [Entity Framework — параметры файла конфигурации](https://msdn.microsoft.com/data/jj556606).

В качестве альтернативы с установкой инициализатор в *Web.config* файл — это в коде, добавив `Database.SetInitializer` инструкцию, чтобы `Application_Start` метод в *Global.asax.cs* файл. Дополнительные сведения см. в разделе [инициализаторы основные сведения о базе данных в Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Приложение теперь имеет значение вверх, чтобы при доступе к базе данных в первый раз на время работы  
приложения, Entity Framework сравнивает базы данных в модель (вашей `SchoolContext` и классов сущностей). Если есть различия, приложение удаляет и повторно создает базу данных.

> [!NOTE]
> При развертывании приложения на веб-сервере в рабочей среде, необходимо удалить или отключить код, который удаляет и повторно создает базу данных. Можно сделать в следующем руководстве этой серии.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Настройка EF для использования базы данных SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) — это облегченная версия SQL Server Express Database Engine. Ее легко установить и настроить запускается по запросу и работает в пользовательском режиме. LocalDB выполняется в специального режима выполнения SQL Server Express, которая позволяет работать с базами данных как *.mdf* файлов. Файлы базы данных LocalDB можно поместить в *приложения\_данных* папку веб-проекта, если вы хотите иметь возможность копирования базы данных с проектом. Функции пользовательского экземпляра в SQL Server Express также позволяет работать с *.mdf* файлы, но функции пользовательского экземпляра устарело; таким образом, рекомендуется использовать LocalDB для работы с *.mdf* файлов. В Visual Studio 2012 и более поздних версий LocalDB устанавливается по умолчанию с помощью Visual Studio.

Обычно SQL Server Express не используется для рабочих веб-приложений. LocalDB в частности не рекомендуется для использования в рабочей среде с веб-приложение, так как он не предназначен для работы со службами IIS.

В этом руководстве вы будете работать с LocalDB. Откройте приложение *Web.config* файл и добавьте `connectionStrings` элементом, предшествующим `appSettings` элемента, как показано в следующем примере. (Обязательно обновите *Web.config* файл в корневой папке проекта. Имеется также *Web.config* файл находится в *представления* вложенную папку, не нужно обновлять.)

Если вы используете Visual Studio 2015, замените «v11.0» в строке подключения «MSSQLLocalDB», так как имя экземпляра SQL Server по умолчанию было изменено.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

В строке подключения, вы добавили указано, что платформа Entity Framework будет использовать базу данных LocalDB с именем *ContosoUniversity1.mdf*. (Базы данных еще не существует; EF создаст ее.) Если база данных будет создан в вашей *приложения\_данных* папки, можно добавить `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` к строке подключения. Дополнительные сведения о строках подключения см. в разделе [строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

У вас фактически нет требуется строка подключения в *Web.config* файл. Если вы не указали строку подключения, Entity Framework будет использовать один на основании контекста класса по умолчанию. Дополнительные сведения см. в разделе [Code First для новой базы данных](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Создание контроллера учащихся и представлений

Теперь вы создадите веб-страницы для отображения данных, и автоматически активирует процесс запроса данных  
Создание базы данных. Вы начнете с создания нового контроллера. Но перед этим, построение проекта, чтобы сделать доступными для формирования шаблонов контроллера MVC классы модели и контексту.

1. Щелкните правой кнопкой мыши **контроллеров** папку в **обозревателе решений**выберите **добавить**и нажмите кнопку **создать шаблонный элемент**.
2. В **Добавление шаблона** выберите **контроллер MVC 5 с представлениями, использующий Entity Framework**.

     ![Добавление элемента формирования шаблонов](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. В диалоговом окне Добавить контроллер, задайте следующие параметры и нажмите кнопку **добавить**:

   - Класс модели: **учащегося (ContosoUniversity.Models)**. (Если вы не видите этот параметр в раскрывающемся списке, постройте проект и повторите попытку.)
   - Класс контекста данных: **SchoolContext (ContosoUniversity.DAL)**.
   - Имя контроллера: **StudentController** (не StudentsController).
   - Оставьте значения по умолчанию для других полей.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     При нажатии кнопки **добавить**, шаблон создает файл StudentController.cs и набор представлений (файлы .cshtml), которые работают с контроллером. В будущем при создании проектов, использующих Entity Framework можно также воспользоваться преимуществами некоторых функциональных возможностей шаблон: просто создать первый класс модели, не создать строку подключения, а затем в **Добавление контроллера** поле укажите новый класс контекста. Шаблон создаст вашей `DbContext` класс и подключение к строка, а также контроллеры и представления.
4. Visual Studio открывает *Controllers\StudentController.cs* файла. Видно, что класс переменной будет создана, создающий экземпляр объекта контекста базы данных:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index` Метод действия Получает список учащихся из *учащихся* сущности, установки по `Students` свойство экземпляра контекста базы данных:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml* представление отображает этот список в таблице:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Нажмите клавиши CTRL+F5, чтобы запустить проект. (Если появится сообщение об ошибке «Не удается создать теневую копию», закройте браузер и повторите попытку.)

     Нажмите кнопку **учащихся** tab, чтобы просмотреть тестовые данные, `Seed` добавленные методом. В зависимости от размеров от — окно браузера, вы увидите ссылку на вкладку учащегося в верхнем адреса или вам придется щелкните в правом верхнем углу, чтобы увидеть эту ссылку.

     ![Кнопки меню](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Страница индекса учащихся](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Просмотр базы данных

При запуске на страницу учащихся и приложение пытается получить доступ к базе данных, платформа EF определяет, что было ни одной базы данных и так он создан, его выполнили метод заполнения для заполнения базы данных с данными.

Можно использовать либо **обозревателя серверов** или **обозреватель объектов SQL Server** (SSOX) для просмотра базы данных в Visual Studio. В этом руководстве вы используете **обозревателя серверов**. (В Visual Studio Express версий более ранних, чем 2013, **обозревателя серверов** называется **обозреватель баз данных**.)

1. Закройте браузер.
2. В **обозревателя серверов**, разверните **подключения к данным**, разверните **School контекста (ContosoUniversity)** и затем разверните **таблиц** для см. в разделе таблицы в новой базе данных.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Щелкните правой кнопкой мыши **учащихся** таблицы и нажмите кнопку **Показать таблицу данных** Чтобы просмотреть созданные столбцы и строки, которые были вставлены в таблицу.

    ![Таблица Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Закрыть **обозревателя серверов** подключения.

*ContosoUniversity1.mdf* и *.ldf* файлы базы данных находятся в `C:\Users\<yourusername>` папку.

Так как вы используете `DropCreateDatabaseIfModelChanges` инициализатора, теперь вы можете внести изменения в `Student` класса, снова запустить приложение и базы данных автоматически будет создана повторно в соответствии с внесенными изменениями. Например, если вы добавите `EmailAddress` свойства `Student` класса, снова запустите на страницу учащихся и снова посмотрим на таблицы еще раз, вы увидите новый `EmailAddress` столбца.

## <a name="conventions"></a>Соглашения

Объем кода, нужно было написать в порядке для платформы Entity Framework иметь возможность создать для вас всей базы данных из-за использования минимальна *соглашения*, или предположения, сделанные в Entity Framework. Некоторые из них уже было отмечено, или был использован без вашего сведений о них:

- Во множественном числе форм имен классов сущностей используются в качестве имен таблиц.
- В качестве имен столбцов используются имена свойств сущностей.
- Свойства сущности, которые называются `ID` или *classname* `ID` распознаются как свойства первичного ключа.
- Свойство интерпретируется как свойство внешнего ключа, если оно имеет имя *&lt;имя свойства навигации&gt;&lt;имя свойство первичного ключа&gt;* (например, `StudentID` для `Student` свойство навигации с момента `Student` — первичный ключ сущности `ID`). Свойства внешнего ключа также могут называться так же просто &lt;имя свойство первичного ключа&gt; (например, `EnrollmentID` поскольку `Enrollment` — первичный ключ сущности `EnrollmentID`).

Вы видели, что соглашения могут быть переопределены. Например, вы указали, что имена таблиц не должны быть имена во множественном числе, и вы увидите Далее как явным образом пометить свойство как свойство внешнего ключа. Вы узнаете о условные обозначения и их в Переопределите [Создание более сложной модели данных](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) далее в этой серии руководств. Дополнительные сведения о правилах см. в разделе [первый соглашения о коде](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Сводка

Теперь вы создали простое приложение, которое использует Entity Framework и SQL Server Express LocalDB для хранения и отображения данных. В следующем руководстве вы узнаете, как выполнять основные CRUD (Создание, чтение, обновление и удаление) операции.

Оставьте свои отзывы на том, как вам понравилось, и этот учебник, и что можно улучшить. Можно также запросить новые темы на [показать мне как с помощью кода](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Вперед](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
