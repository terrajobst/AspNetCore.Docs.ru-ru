---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "Приступая к работе с Entity Framework 6 Code First MVC 5 с помощью | Документы Microsoft"
author: tdykstra
description: "Доступна более новая версия этой серии учебник: Приступая к работе с ASP.NET Core и Entity Framework Core, используя Visual Studio 2015. Contoso Universi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84ca4bbaebe401d14233131bcaa027debf7ea0f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Начало работы с Entity Framework 6 Code First с помощью MVC 5
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) или [скачать PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Доступна более новая версия этого учебника рядов: [приступить к работе с ASP.NET Core и Entity Framework с помощью Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Contoso университета примера веб-приложения показано, как создавать приложения ASP.NET MVC 5 с помощью платформы Entity Framework 6 и Visual Studio 2013. В этом учебнике используется Code First рабочего процесса. Сведения о выборе между Code First, Database First и Model First см. в разделе [процессов разработки Entity Framework](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).
> 
> Образец приложения является веб-сайт для вымышленной компании Contoso университета. Он включает функции, такие как допуском студента, создание курса и инструктора назначения. Этот учебник ряд объясняется, как создать пример приложения Contoso университета. Вы можете [загрузка завершенного приложения](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Доступна версия Visual Basic, преобразовываются Майк Бринд: [MVC 5 с 6 EF в Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) на сайте Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (пакет NuGet EntityFramework 6.1.0)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (необязательно)
>   
> 
> Учебник также должна работать с [Visual Studio 2013 Express для Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) или Visual Studio 2012. [Windows Azure SDK версии Visual STUDIO 2012](https://go.microsoft.com/fwlink/p/?linkid=323511) является обязательным для развертывания Windows Azure с помощью Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Версии учебника
> 
> Для предыдущих версий этого учебника, см. [EF 4.1 и MVC 3 электронную](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) и [Приступая к работе с EF 5 с помощью MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Вопросы и комментарии
> 
> Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы. Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework и LINQ to Entities форум](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/), или [ StackOverflow.com](http://stackoverflow.com/).
> 
> Если возникли проблемы, которую не удается устранить, обычно можно найти решение проблемы путем сравнения код для завершенного проекта, который можно загрузить. Некоторые распространенные ошибки и способы их устранения см. в разделе [распространенные ошибки и решения или обходные пути для них.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Веб-приложение Contoso университета

Приложение, которое вы будете построения в этих учебниках является простой университета веб-сайта.

Пользователи могут просматривать и обновлять студента курса и инструктора сведения. Ниже представлены несколько экранов, которые будут созданы.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Изменить студента](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Стиль пользовательского интерфейса этого сайта была сохранена близко к создаваемой с помощью встроенных шаблонов учебника можно сосредоточиться на том, как использовать платформу Entity Framework.

## <a name="prerequisites"></a>Предварительные требования

В разделе **версий программного обеспечения** в верхней части страницы. Entity Framework 6 не является обязательной, так как установить пакет EF NuGet как части этого руководства.

## <a name="create-an-mvc-web-application"></a>Создание веб-приложения MVC

Откройте Visual Studio и создайте новый проект C# Web с именем «ContosoUniversity».

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

В **новый проект ASP.NET** диалогового окна выберите **MVC** шаблона.

Если **узел в облаке** флажок в **Microsoft Azure** раздел установлен, снимите его.

Нажмите кнопку **изменения проверки подлинности**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

В **изменить аутентификацию** выберите **без проверки подлинности**, а затем нажмите кнопку **ОК**. Этот учебник вы не будет предлагать пользователям войти в систему или ограничить доступ на основании входящего в систему.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

В диалоговом окне нового проекта ASP.NET выберите **ОК** для создания проекта.

## <a name="set-up-the-site-style"></a>Настройка стиля узла

Несколько простых изменений будет установлен сайт меню, макета и домашнюю страницу.

Откройте *представления\общие\\_Layout.cshtml*и внесите следующие изменения:

- Измените каждое вхождение «Мое приложение ASP.NET» и «Имя приложения» на «Contoso University».
- Добавление пунктов меню для учащихся, курсы, инструкторов и отделах и удалите запись контакта.

Изменения выделены.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

В *Views\Home\Index.cshtml*, замените содержимое файла следующим кодом, чтобы заменить текст о ASP.NET и MVC на текст об этом приложении:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Нажмите клавиши CTRL + F5 для запуска сайта. Можно просмотреть на домашней странице с главным меню.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Установка платформы Entity Framework 6

Из **средства** меню **диспетчера пакетов NuGet** и нажмите кнопку **консоль диспетчера пакетов**.

В **консоль диспетчера пакетов** окна введите следующую команду:

`Install-Package EntityFramework`

![EF установлен](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

На рисунке показана 6.0.0 устанавливается, но NuGet установит последнюю версию Entity Framework (исключая предварительных версий), начиная с самого последнего обновления для работы с учебником это 6.1.1.

Этот шаг является одним из несколько шагов, что этот учебник содержит можно сделать вручную, но которой можно было бы автоматически с помощью ASP.NET MVC функции формирования шаблонов. Вы выполняете их вручную, чтобы вы могли видеть шаги, необходимые для использования платформы Entity Framework. Формирование шаблонов будет использоваться позже для создания контроллера MVC и представлений. Альтернативой является let формирование шаблонов автоматически установить пакет EF NuGet, создайте класс контекста базы данных и создания строки подключения. Когда вы будете готовы сделать его таким образом, все, что нужно сделать — пропустить эти шаги и формировать контроллер MVC после создания классов сущностей.

## <a name="create-the-data-model"></a>Создание модели данных

Затем вы создадите классы сущностей для приложения Contoso университета. Начните с тремя следующими сущностями:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Имеется отношение "один ко многим" между `Student` и `Enrollment` сущностями, и имеется отношение "один ко многим" между `Course` и `Enrollment` сущности. Другими словами студент может быть зарегистрированы в любое количество курсы и курс может иметь любое количество студентов, зарегистрированных в нем.

В следующих разделах вы создадите класса для каждого из этих сущностей.

> [!NOTE]
> При попытке скомпилировать проект до завершения создания всех этих классов сущностей, то возникнут ошибки компилятора.


### <a name="the-student-entity"></a>Сущность Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

В *моделей* папки, создайте файл класса с именем *Student.cs* и замените код шаблона с помощью следующего кода:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` Свойство станет столбец первичного ключа таблицы базы данных, соответствующий этому классу. По умолчанию платформа Entity Framework интерпретирует свойство, которое называется `ID` или *classname* `ID` как первичный ключ.

`Enrollments` Свойство *свойство навигации*. Свойства навигации хранения других сущностей, которые относятся к этой сущности. В этом случае `Enrollments` свойство `Student` сущность будет содержать все `Enrollment` сущностей, которые относятся к, `Student` сущности. Другими словами Если данной `Student` строк в базе данных есть две связанные `Enrollment` строк (значение строки, содержащие этого студента первичного ключа в их `StudentID` столбец внешнего ключа), которая `Student` сущности `Enrollments` свойство навигации будет содержать этих двух `Enrollment` сущностей.

Свойства навигации, обычно определяются как `virtual` так, чтобы такие как воспользоваться преимуществами некоторых возможностей Entity Framework *отложенную загрузку*. (Отложенная загрузка будут описаны далее в [чтение связанных данных](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) учебника далее в этой серии.)

Если свойство навигации может содержать несколько сущностей (как отношения "многие ко многим" или "один ко многим"), его тип должен быть список, в котором записи могут быть добавлены, удаленные и обновления, такие как `ICollection`.

### <a name="the-enrollment-entity"></a>Сущность регистрации

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

В *моделей* папке создайте *«Enrollment.cs» в качестве* и замените существующий код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID` Свойство будет иметь первичный ключ, использует эту сущность *classname* `ID` шаблона вместо `ID` сам по себе, как вы видели в `Student` сущности. Обычно следует выбрать один шаблон и использовать его во всей модели данных. Здесь значение варианта иллюстрирует, которые можно использовать либо шаблон. В этом руководстве, вы увидите том, каким образом `ID` без `classname` упрощает реализацию наследования в модели данных.

`Grade` Свойство [перечисления](https://msdn.microsoft.com/en-us/data/hh859576.aspx). Знак вопроса после `Grade` объявление типа указывает, что `Grade` свойство [nullable](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx). Степень, которую имеет значение null отличается от нуля оценку — значение null означает оценку не известен или еще не назначена.

`StudentID` Свойство внешнего ключа, и соответствующее свойство навигации `Student`. `Enrollment` Сущности связан с одним `Student` сущности, поэтому свойство может содержать только один `Student` сущности (в отличие от `Student.Enrollments` свойство навигации вы видели ранее, который может содержать несколько `Enrollment` сущностей).

`CourseID` Свойство внешнего ключа, и соответствующее свойство навигации `Course`. `Enrollment` Сущности связан с одним `Course` сущности.

Entity Framework интерпретирует свойство как свойство внешнего ключа, если он называется  *&lt;имя свойства навигации&gt;&lt;имя свойство первичного ключа&gt;*  (например, `StudentID`для `Student` свойство навигации с момента `Student` первичного ключа сущности является `ID`). Свойства внешнего ключа можно также совпадать с именем просто  *&lt;имя свойство первичного ключа&gt;*  (например, `CourseID` с момента `Course` первичного ключа сущности является `CourseID`).

### <a name="the-course-entity"></a>Сущность курса

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

В *моделей* папки, создать *Course.cs*, заменив шаблон код следующим кодом:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` Свойство является свойством навигации. Объект `Course` сущности могут быть связаны с любым количеством `Enrollment` сущностей.

Допустим, что больше о [DatabaseGenerated](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) атрибут позднее в этом учебнике этой серии. По сути этот атрибут позволяет ввести первичный ключ для курса вместо формирования базы данных.

## <a name="create-the-database-context"></a>Создать контекст базы данных

Основной класс, который координирует функции в заданной модели данных Entity Framework является *контекст базы данных* класса. Создание этого класса путем наследования от [System.Data.Entity.DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) класса. В коде указывается, какие сущности включаются в модель данных. Можно также настроить некоторые параметры поведения Entity Framework. В этом проекте класс с именем `SchoolContext`.

Создание папки в проекте ContosoUniversity, щелкните правой кнопкой мыши проект в **обозревателе решений** и нажмите кнопку **добавить**, а затем нажмите кнопку **новую папку**. Имя новой папки *DAL* (для доступа к данным). В этой папке создайте новый файл класса с именем *SchoolContext.cs*и замените код шаблона с помощью следующего кода:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Указание наборов сущностей

Этот код создает [DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=VS.103).aspx) свойство для каждого набора сущностей. В терминологии Entity Framework *набора сущностей* обычно соответствует таблице базы данных и *сущности* соответствует строке таблицы.

> [!NOTE] 
> 
> Может опущен `DbSet<Enrollment>` и `DbSet<Course>` инструкций и он будет работать так же. Платформа Entity Framework будет включать их неявно поскольку `Student` ссылки на сущности `Enrollment` сущности и `Enrollment` ссылки на сущности `Course` сущности.


### <a name="specifying-the-connection-string"></a>Указание строки подключения

Имя строки подключения (который вы добавите в файл Web.config более поздней версии), переданный в конструктор.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Можно также передавать в строке подключения вместо имени он хранится в файле Web.config. Дополнительные сведения о параметрах для указания базы данных для использования см. в разделе [Entity Framework - подключений и модели](https://msdn.microsoft.com/en-us/data/jj592674).

Если не указать строку соединения или имя одного явным образом, Entity Framework предполагает, что имя строки соединения является совпадает с именем класса. Имя строки подключения по умолчанию в этом примере будет `SchoolContext`, таким же, как то, что было указано явно.

### <a name="specifying-singular-table-names"></a>Указание имен таблицы в единственном числе

`modelBuilder.Conventions.Remove` Инструкции в [OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) метод запрещает имена таблиц имена во множественном числе. Если это не сделать, созданные таблицы в базе данных будет назван `Students`, `Courses`, и `Enrollments`. Вместо этого имена таблиц будут `Student`, `Course`, и `Enrollment`. В среде разработчиков нет единого мнения о том, следует ли использовать имена таблиц во множественном числе. В этом учебнике используется существительные в единственном числе, но важно то, что вы можете выбрать любой формы, вы предпочитаете, включая или исключая эту строку кода.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Настройка EF для инициализации базы данных с тестовыми данными

Платформа Entity Framework можно автоматически создать (или удалить и повторно создать) базы данных автоматически при запуске приложения. Можно указать, что это следует делать каждый раз при запуске приложения, или только в том случае, если модель не синхронизирован с существующей базой данных. Можно также написать `Seed` метод, который платформа Entity Framework, автоматически вызывается после создания базы данных, чтобы заполнить его данными теста.

По умолчанию выполняется создание базы данных только в том случае, если она не существует (и создания исключения, если модель данных была изменена, и база данных уже существует). В этом разделе вы зададите, что базы данных нужно удалить и повторно создается при каждом изменении модели. Удаление базы данных приводит к потере всех данных. Это обычно является ОК во время разработки, так как `Seed` метод выполняется в том случае, когда в базе данных создается заново и повторно создают тестовые данные. Однако в рабочей среде обычно не нужно каждый раз, чтобы изменить схему базы данных, все данные потеряны. Позже вы увидите, как обрабатывать изменения модели с помощью Code First Migrations можно изменить схему базы данных вместо удаления и повторного создания базы данных.

В папке DAL, создайте новый файл класса с именем *SchoolInitializer.cs* и замените код шаблона с  
Следующий код, в результате чего база данных, создаваемых при необходимости и загружает тестовых данных в новую базу данных.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed` Метод принимает объекта контекста базы данных в качестве входного параметра и кода в методе используется  
Этот объект для добавления новых сущностей в базу данных. Для каждого типа сущности код создает коллекцию, новых  
 сущности, добавляет их в соответствующую `DbSet` свойства, а затем сохраняет изменения в базе данных. Нет  
необязательно вызывать `SaveChanges` метод после каждой группы сущностей, как показано здесь, но действия, которое помогает  
найти источник проблемы, если исключение возникает, когда код производит запись в базу данных.

Чтобы сообщить Entity Framework, чтобы использовать инициализатор класса, добавьте элемент для `entityFramework` элемента в приложении *Web.config* файл (в корневой папке проекта), как показано в следующем примере:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type` Указывает контекст полное имя класса и сборки, он находится, и `databaseinitializer type` указывает инициализатор класса и сборки, он находится в полное имя. (Если вы не хотите EF использовать инициализатор, можно задать атрибут на `context` элемент: `disableDatabaseInitialization="true"`.) Дополнительные сведения см. в разделе [Entity Framework — параметры файла конфигурации](https://msdn.microsoft.com/en-us/data/jj556606).

В качестве альтернативы для задания инициализатора в *Web.config* файл — это в коде, добавив `Database.SetInitializer` инструкции `Application_Start` метод в *Global.asax.cs* файла. Дополнительные сведения см. в разделе [инициализаторы основные сведения о базе данных в Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Приложение теперь настроен вверх, чтобы при доступе к базе данных в первый раз в во время работы  
приложения, Entity Framework сравнивает базы данных в модель (ваш `SchoolContext` и классы сущностей). Если есть различие, приложение удаляет и повторно создает базу данных.

> [!NOTE]
> При развертывании приложения на веб-сервере в рабочей среде, необходимо удалить или отключить код, который удаляет и повторно создает базу данных. Вы выполните, позднее в этом учебнике этой серии.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Настройка EF на использование базы данных SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) — это облегченная версия SQL Server Express Database Engine. Можно легко установить и настроить, запускаемая по запросу и запускается в пользовательском режиме. LocalDB выполняется в режиме выполнения специальных SQL Server Express, позволяет работать с базами данных как *.mdf* файлов. Файлы базы данных LocalDB можно поместить в *приложения\_данные* папку веб-проекта, если вы хотите иметь возможность копирования базы данных с проектом. Функции пользовательского экземпляра в SQL Server Express позволяет также работать с *.mdf* файлов, но функции пользовательского экземпляра рекомендуется; таким образом, рекомендуется использовать LocalDB для работы с *.mdf* файлов. В Visual Studio 2012 и более поздних версиях LocalDB устанавливается по умолчанию вместе с Visual Studio.

Обычно SQL Server Express не используется для рабочей веб-приложений. LocalDB в частности не рекомендуется для использования в рабочей среде веб-приложению, так как он не предназначен для работы со службами IIS.

В этом учебнике вы будете работать с LocalDB. Откройте приложение *Web.config* и добавьте `connectionStrings` предыдущий элемент `appSettings` элемента, как показано в следующем примере. (Убедитесь, что при обновлении *Web.config* файл в корневой папке проекта. Имеется также *Web.config* файл находится в *представления* вложенной папки, которая не требует обновления.)

При использовании Visual Studio 2015, замените «v11.0» в строке подключения «MSSQLLocalDB», как имя экземпляра SQL Server по умолчанию было изменено.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Строка подключения, вы добавили задает использование Entity Framework с именем базы данных LocalDB *ContosoUniversity1.mdf*. (Базы данных еще не существует; EF создаст ее.) Если требуется, чтобы база данных будет создан в вашей *приложения\_данные* папки, можно добавить `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` в строку подключения. Дополнительные сведения о строках соединения см. в разделе [строки подключения SQL Server для веб-приложений ASP.NET](https://msdn.microsoft.com/en-us/library/jj653752.aspx).

Не обязательно должны иметь строку соединения *Web.config* файла. Если не указать строку подключения, Entity Framework будет использовать значение по умолчанию на класс контекста основе один. Дополнительные сведения см. в разделе [Code First в новой базе данных](https://msdn.microsoft.com/en-us/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Создание контроллера студентов и представлений

Теперь вы создадите веб-страницы для отображения данных и автоматически запустит процесс запрос данных  
Создание базы данных. Сначала путем создания нового контроллера. Однако перед этим построение проекта, чтобы сделать доступными для формирования шаблонов контроллера MVC классы модели и контекста.

1. Щелкните правой кнопкой мыши **контроллеров** папки в **обозревателе решений**выберите **добавить**и нажмите кнопку **новый элемент формирования шаблонов**.
- В **Добавление формирования шаблонов** выберите **контроллер MVC 5 с представлениями, использующий Entity Framework**.

    ![Добавление формирования шаблонов](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
- В диалоговом окне Добавить контроллер, задайте следующие параметры и нажмите кнопку **добавить**:

    - Класс модели: **студента (ContosoUniversity.Models)**. (Если вы не видите этот параметр в раскрывающемся списке, постройте проект и повторите попытку.)
    - Класс контекста данных: **SchoolContext (ContosoUniversity.DAL)**.
    - Имя контроллера: **StudentController** (не StudentsController).
    - Оставьте значения по умолчанию для других полей.

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    При нажатии кнопки **добавить**, scaffolder создает файл StudentController.cs и ряд представлений (.cshtml-файлы), работающие с контроллером. В дальнейшем при создании проектов, использующие Entity Framework можно также воспользоваться преимуществами некоторые дополнительные функциональные возможности scaffolder: просто создать свой первый класс модели, не создать строку подключения и затем в **добавить контроллер** поле укажите новый класс контекста. Будет создан scaffolder вашей `DbContext` класс и подключение к строка, а также представления и контроллера.
- Visual Studio открывает *Controllers\StudentController.cs* файла. Вы увидите, что переменная класса будет создана, создает экземпляр объекта контекста базы данных:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    `Index` Метод действия Получает список студентов *учащихся* посредством чтения набора сущностей `Students` свойства экземпляра контекста базы данных:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    *Student\Index.cshtml* представление отображает этот список в таблице:

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
- Нажмите клавиши CTRL+F5, чтобы запустить проект. (Если появляется сообщение об ошибке «Не удается создать теневую копию», закройте браузер и повторите попытку.)

    Нажмите кнопку **учащихся** вкладку, чтобы увидеть данные теста, `Seed` метод вставки. В зависимости от того, как узких — окно браузера, вы увидите ссылку вкладку студента в верхнем адресной строке или вам придется нажмите кнопку в правом верхнем углу, чтобы увидеть эту ссылку.

    ![Кнопки меню](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    ![Страница индекса студента](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Просмотр базы данных

При запуске страницы студентов и приложение пытается получить доступ к базе данных, EF узнали, что база данных не была и поэтому он создан, затем он был запущен метод начальное значение, чтобы заполнить базу данных.

Можно использовать любой **обозревателя серверов** или **обозреватель объектов SQL Server** (SSOX) для просмотра базы данных в Visual Studio. В этом учебнике будет использоваться **обозревателя серверов**. (В Visual Studio Express версий более ранних, чем 2013 **обозревателя серверов** называется **обозревателя базы данных**.)

1. Закройте браузер.
2. В **обозревателя серверов**, разверните **подключения к данным**, разверните **School контекста (ContosoUniversity)**и разверните **таблиц** для просмотра таблицы в новой базе данных.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Щелкните правой кнопкой мыши **студента** и нажмите кнопку **Показать таблицу данных** столбцы, которые были созданы и строк, которые были вставлены в таблицу.

    ![Таблица учащихся](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Закрыть **обозревателя серверов** соединения.

*ContosoUniversity1.mdf* и *.ldf* файлы базы данных находятся в `C:\Users\<yourusername>` папки.

Так как вы используете `DropCreateDatabaseIfModelChanges` инициализатор, может теперь внести изменения в `Student` класса, снова запустите приложение и базы данных автоматически будет создан повторно для сопоставления внесенные изменения. Например, при добавлении `EmailAddress` свойства `Student` класса, снова запустить страницу студентов и затем найдите таблицу снова, вы увидите новый `EmailAddress` столбца.

## <a name="conventions"></a>Соглашения

Из-за использования объем кода, вы должны были написать в порядке иметь возможность создавать всей базы данных автоматически Entity Framework сводится к минимуму *соглашения*, или предположения, что позволяет платформе Entity Framework. Некоторые из них уже отмечалось, или были использованы без вашего сведений о них:

- Pluralized формы имен классов сущностей, которые используются как имена таблиц.
- Имена свойств сущности используются для имен столбцов.
- Свойства сущности, которые именуются `ID` или *classname* `ID` распознаются как свойства основного ключа.
- Свойство интерпретируется как свойство внешнего ключа, если он называется  *&lt;имя свойства навигации&gt;&lt;имя свойство первичного ключа&gt;*  (например, `StudentID` для `Student` свойство навигации с момента `Student` первичного ключа сущности является `ID`). Свойства внешнего ключа можно также совпадать с именем просто &lt;имя свойство первичного ключа&gt; (например, `EnrollmentID` с момента `Enrollment` первичного ключа сущности является `EnrollmentID`).

Вы знаете, что соглашения могут быть переопределены. Например, указывать имена таблиц не должны иметь имена во множественном числе, что вы увидите Далее как явным образом пометить свойство как свойство внешнего ключа. Дополнительные сведения о условные обозначения и переопределить их в [создания более сложной модели данных](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) учебника далее в этой серии. Дополнительные сведения о соглашениях см. в разделе [соглашения о написании кода для первого](https://msdn.microsoft.com/en-us/data/jj679962).

## <a name="summary"></a>Сводка

Теперь вы создали простого приложения, использующего платформу Entity Framework и SQL Server Express LocalDB для хранения и отображения данных. В этом руководстве вы узнаете, как выполнять основные CRUD (Создание, чтение, обновление и удаление) операции.

Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить. Можно также запросить новые разделы на [показать мне как с код](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Ссылки на другие ресурсы Entity Framework можно найти в [доступа к данным ASP.NET - рекомендуется использовать ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Вперед](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
