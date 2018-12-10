---
title: 'Razor Pages с Entity Framework Core в ASP.NET Core: учебник 1 из 8'
author: rick-anderson
description: Сведения о том, как создать приложение Razor Pages с помощью Entity Framework Core
ms.author: riande
ms.custom: seodec18
ms.date: 11/22/2018
uid: data/ef-rp/intro
ms.openlocfilehash: 34c7238b689993245e033625dcd0e728b7c45163
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121704"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Razor Pages с Entity Framework Core в ASP.NET Core: учебник 1 из 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

На примере учебного веб-приложения "Университет Contoso" демонстрируется процесс создания веб-приложений Razor Pages ASP.NET Core с помощью Entity Framework (EF) Core.

В этом примере приложения реализуется веб-сайт вымышленного университета Contoso. На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей. Это первое руководство из серии, в котором описывается создание примера приложения для университета Contoso.

[Скачайте или ознакомьтесь с готовым приложением.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Указания по скачиванию](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Предварительные требования

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

Знакомство с [Razor Pages](xref:razor-pages/index). Новые программисты должны изучить статью [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), прежде чем приступать к этой серии.

## <a name="troubleshooting"></a>Устранение неполадок

Если вы столкнулись с проблемами, для их решения можно попробовать сравнить свой код с кодом [готового проекта](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Чтобы получить помощь, вы можете опубликовать вопрос на веб-сайте [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) в разделе, посвященном [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) или [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-contoso-university-web-app"></a>Веб-приложение университета Contoso

Приложение, создаваемое в этих руководствах, является простым веб-сайтом университета.

Пользователи приложения могут просматривать и обновлять сведения об учащихся, курсах и преподавателях. Здесь приведено несколько экранов, создаваемых в руководстве.

![Страница указателя учащихся](intro/_static/students-index.png)

![Страница редактирования учащихся](intro/_static/student-edit.png)

Стиль пользовательского интерфейса для этого сайта близок к обеспечиваемому встроенными шаблонами. Это руководство посвящено EF Core с Razor Pages, а не пользовательскому интерфейсу.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>Создание веб-приложения Razor Pages ContosoUniversity

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** Visual Studio откройте **Создать** > **Проект**.
* Создайте новое веб-приложение ASP.NET Core. Назовите проект **ContosoUniversity**. Очень важно, чтобы проект имел имя *ContosoUniversity*, чтобы совпадали пространства имен при копировании и вставке кода.
* Выберите в раскрывающемся списке **ASP.NET Core 2.1**, а затем **Веб-приложение**.

Изображения для приведенных выше шагов см. в разделе [Создание веб-приложения Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app).
Запустите приложение.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a>Настройка стиля сайта

Выполните небольшую настройку меню, макета и домашней страницы сайта. Внесите следующие изменения в файл *Pages/Shared/_Layout.cshtml*:

* Замените все вхождения "ContosoUniversity" на "Contoso University". Таких элементов будет три.

* Добавьте пункты меню **Students** (Учащиеся), **Courses** (Курсы), **Instructors** (Преподаватели) и **Departments** (Кафедры). Удалите пункт меню **Contact** (Контакты).

Изменения выделены. (Вся разметка *не* отображается.)

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

Замените содержимое файла *Pages/Index.cshtml* следующим кодом, который заменяет текст о ASP.NET и MVC описанием этого приложения:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Создание модели данных

Создайте классы сущностей для приложения университета Contoso. Начните со следующих трех сущностей:

![Схема модели данных "курс-регистрация-учащийся"](intro/_static/data-model-diagram.png)

Между сущностями `Student` и `Enrollment` действует связь один ко многим. Между сущностями `Course` и `Enrollment` действует связь один ко многим. Студент может записаться на любое число курсов. На курс может быть зачислено любое количество учащихся.

В следующих разделах создается класс для каждой из этих сущностей.

### <a name="the-student-entity"></a>Сущность Student

![Схема сущности Student](intro/_static/student-entity.png)

Создайте папку *Models*. В папке *Models* создайте файл класса с именем *Student.cs*, содержащий следующий код:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

Свойство `ID` используется в качестве столбца первичного ключа в таблице базы данных, соответствующей этому классу. По умолчанию платформа EF Core интерпретирует в качестве первичного ключа свойство `ID` или `classnameID`. В `classnameID` `classname` — это имя класса. Альтернативным автоматически распознаваемым первичным ключом является `StudentID` в предыдущем примере.

Свойство `Enrollments` является [свойством навигации](/ef/core/modeling/relationships). Свойства навигации ссылаются на другие сущности, связанные с этой сущностью. В этом случае свойство `Enrollments` сущности `Student entity` содержит все сущности `Enrollment`, которые связаны с этой сущностью `Student`. Например, если строка "Student" (Учащийся) в базе данных имеет две связанные строки "Enrollment" (зачисление), свойство навигации `Enrollments` содержит две этих сущности `Enrollment`. Связанная строка `Enrollment` содержит значение первичного ключа для этого учащегося в столбце `StudentID`. Предположим, например, что учащийся с идентификатором 1 имеет две строки в таблице `Enrollment`. Таблица `Enrollment` содержит две строки с `StudentID` = 1. `StudentID` является внешним ключом в таблице `Enrollment`, указывающим учащегося в таблице `Student`.

Если свойство навигации может содержать несколько сущностей, оно должно иметь тип списка, такой как `ICollection<T>`. Можно указать `ICollection<T>` либо тип, такой как `List<T>` или `HashSet<T>`. Если используется `ICollection<T>`, платформа EF Core по умолчанию создает коллекцию `HashSet<T>`. Свойства навигации, содержащие несколько сущностей, поступают по связям многие ко многим и один ко многим.

### <a name="the-enrollment-entity"></a>Сущность Enrollment

![Схема сущности Enrollment](intro/_static/enrollment-entity.png)

В папке *Models* создайте файл *Enrollment.cs*, содержащий следующий код:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

Свойство `EnrollmentID` является первичным ключом. Эта сущность использует шаблон `classnameID` вместо `ID`, как сущность `Student`. Обычно разработчики выбирают один шаблон и используют его в рамках всей модели данных. В одном из следующих руководств показано, за счет чего использование идентификатора без имени класса позволяет упростить реализацию наследования в модели данных.

Свойство `Grade` имеет тип `enum`. Знак вопроса после объявления типа `Grade` указывает, что свойство `Grade` допускает значение null. Оценка со значением null отличается от нулевой оценки тем, что при таком значении оценка еще не известна или не назначена.

Свойство `StudentID` представляет собой внешний ключ. Ему соответствует свойство навигации `Student`. Сущность `Enrollment` связана с одной сущностью `Student`, поэтому свойство содержит отдельную сущность `Student`. Сущность `Student` отличается от свойства навигации `Student.Enrollments`, которое содержит несколько сущностей `Enrollment`.

Свойство `CourseID` представляет собой внешний ключ. Ему соответствует свойство навигации `Course`. Сущность `Enrollment` связана с одной сущностью `Course`.

EF Core воспринимает свойство как внешний ключ, если он имеет имя `<navigation property name><primary key property name>`. Например, `StudentID` для свойства навигации `Student`, так как сущность `Student` имеет первичный ключ `ID`. Свойства внешнего ключа также могут называться `<primary key property name>`. Например, `CourseID`, так как сущность `Course` имеет первичный ключ `CourseID`.

### <a name="the-course-entity"></a>Сущность Course

![Схема сущности Course](intro/_static/course-entity.png)

В папке *Models* создайте файл *Course.cs*, содержащий следующий код:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

Свойство `Enrollments` является свойством навигации. Сущность `Course` может быть связана с любым числом сущностей `Enrollment`.

Атрибут `DatabaseGenerated` позволяет приложению указать первичный ключ, а не использовать созданный базой данных.

## <a name="scaffold-the-student-model"></a>Формирование шаблона для модели Student

В этом разделе формируется шаблон для модели Student. То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели Student.

* Выполните построение проекта.
* Создайте папку *Pages/Students*.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В **обозревателе решений** щелкните правой кнопкой мыши папку *Pages/Students* и выберите **Добавить** > **Создать шаблонный элемент**.
* В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **ДОБАВИТЬ**.

Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)**:

* В раскрывающемся списке **Класс модели** выберите **Student (ContosoUniversity.Models)**.
* В строке **Класс контекста данных** щелкните значок плюса (**+**) и измените автоматически присвоенное имя на **ContosoUniversity.Models.SchoolContext**.
* В раскрывающемся списке **Класс контекста данных** выберите **ContosoUniversity.Models.SchoolContext**
* Нажмите **Добавить**.

![Диалоговое окно CRUD](intro/_static/s1.png)

Если на предыдущем шаге у вас возникли проблемы, см. раздел [Создание модели фильма](xref:tutorials/razor-pages/model#scaffold-the-movie-model).

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните следующую команду, чтобы сформировать шаблон для модели Student.

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

В процессе формирования шаблонов были созданы и изменены следующие файлы:

### <a name="files-created"></a>Создаваемые файлы

* *Pages/Students* Create, Delete, Details, Edit, Index.
* *Data/SchoolContext.cs*.

### <a name="file-updates"></a>Обновления файла

* *Startup.cs*. Изменения в этом файле подробно описываются в следующем разделе.
* *appsettings.json*. Добавляется строка подключения, используемая для подключения к локальной базе данных.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Проверка контекста, зарегистрированного с помощью внедрения зависимостей

ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection). С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения. Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора. Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.

Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.

Проверьте метод `ConfigureServices` в файле *Startup.cs*. Средством формирования шаблонов была добавлена выделенная строка:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions). При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.

## <a name="update-main"></a>Обновление метода Main

В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее:

* Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.
* Вызовите [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).
* Высвободите контекст после завершения работы метода `EnsureCreated`.

В следующем примере кода показан обновленный файл *Program.cs*.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` позволяет проверить существование базы данных для контекста. Если контекст существует, никаких действий не предпринимается. Если контекст не существует, создаются база данных и вся ее схема. `EnsureCreated` не использует миграции для создания базы данных. Созданную с помощью `EnsureCreated` базу данных впоследствии нельзя обновить, используя миграции.

`EnsureCreated` вызывается при запуске приложения, что обеспечивает следующий рабочий процесс:

* Удалите базу данных.
* Измените схему базы данных (например, добавьте поле `EmailAddress`).
* Запустите приложение.
* `EnsureCreated` создает базу данных со столбцом `EmailAddress`.

`EnsureCreated` удобно использовать на ранних стадиях разработки, когда схема часто меняется. Далее в этом учебнике база данных удаляется и используются миграции.

### <a name="test-the-app"></a>Тестирование приложения

Запустите приложение и примите политику использования файлов cookie. Это приложение не хранит персональные данные. Сведения о политике использования файлов cookie можно прочитать в разделе [Поддержка общего регламента по защите данных ЕС (GDPR)](xref:security/gdpr).

* Щелкните ссылку **Students** и выберите **Создать**.
* Протестируйте ссылки Edit, Details и Delete.

## <a name="examine-the-schoolcontext-db-context"></a>Проверка контекста базы данных SchoolContext

Контекст базы данных — это класс main, который координирует функциональные возможности EF Core для заданной модели данных. Контекст данных получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Контекст данных указывает сущности, которые включаются в модель данных. В этом проекте соответствующий класс называется `SchoolContext`.

Обновите файл *SchoolContext.cs*, добавив в него следующий код:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

Выделенный код создает свойство [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для каждого набора сущностей. В терминологии EF Core:

* Набор сущностей обычно соответствует таблице базы данных.
* Сущность соответствует строке в таблице.

`DbSet<Enrollment>` и `DbSet<Course>` можно опустить. Платформа EF Core включает эти операторы неявно, так как сущность `Student` ссылается на сущность `Enrollment`, а `Enrollment` ссылается на сущность `Course`. В рамках данного руководства оставьте `DbSet<Enrollment>` и `DbSet<Course>` в `SchoolContext`.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

Строка подключения указывает базу данных [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки приложений и не ориентированная на использование в рабочей среде. LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны. По умолчанию LocalDB создает файлы базы данных *MDF* в каталоге `C:/Users/<user>`.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Добавление кода для инициализации базы данных с использованием тестовых данных

EF Core создает пустую базу данных. В этом разделе создается метод `Initialize`, предназначенный для заполнения тестовыми данными.

В папке *Data* создайте файл класса с именем *DbInitializer.cs* и добавьте следующий код:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Код проверяет наличие учащихся в базе данных. Если учащихся в базе данных нет, она заполняется тестовыми данными. Для повышения производительности тестовые данные загружаются массивами, а не коллекциями `List<T>`.

Метод `EnsureCreated` автоматически создает базу данных для контекста базы данных. Если такая база данных существует, `EnsureCreated` возвращает управление без изменения базы данных.

В файле *Program.cs* измените метод `Main`, чтобы реализовать вызов `Initialize`:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

Удалите все записи учащихся и перезапустите приложение. Если инициализация базы данных не была выполнена, добавьте точку останова в метод `Initialize` для диагностики проблем.

## <a name="view-the-db"></a>Просмотр базы данных

Откройте **обозреватель объектов SQL Server** (SSOX) из меню **Вид** в Visual Studio.
В SSOX щелкните **(localdb)\MSSQLLocalDB > Базы данных > ContosoUniversity1**.

Разверните узел **Таблицы**.

Щелкните правой кнопкой мыши таблицу **Student** (Учащийся) и выберите пункт **Просмотр данных**, чтобы просмотреть созданные столбцы и строки, вставленные в таблицу.

## <a name="asynchronous-code"></a>Асинхронный код

Асинхронное программирование по умолчанию применяется в ASP.NET Core и EF Core.

Веб-сервер имеет ограниченное число потоков, поэтому при высокой загрузке могут использоваться все доступные потоки. В таких случаях сервер не может обрабатывать новые запросы до тех пор, пока не будут высвобождены потоки. В синхронном коде многие потоки могут быть заняты, не выполняя при этом какие-либо операции и ожидая завершения ввода-вывода. В асинхронном коде в то время, когда процесс ожидает завершения ввода-вывода, его поток высвобождается и может использоваться сервером для обработки других запросов. Таким образом, асинхронный код позволяет более эффективно использовать ресурсы сервера, который может обрабатывать больше трафика без задержек.

Во время выполнения асинхронный код использует немного больше служебных ресурсов. Однако при низком объеме трафика этим можно пренебречь. Тем не менее в случае большого объема трафика это дает существенный выигрыш в производительности.

В следующем коде для асинхронного выполнения используются ключевое слово [async](/dotnet/csharp/language-reference/keywords/async), возвращаемое значение `Task<T>`, ключевое слово `await` и метод `ToListAsync`.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* Ключевое слово `async` указывает компилятору:
  * создавать обратные вызовы для частей тела метода;
  * автоматически создавать возвращаемый объект [Task](/dotnet/api/system.threading.tasks.task). Дополнительные сведения см. в разделе [Тип возвращаемых значений задач](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Неявный тип возвращаемого значения `Task` представляет текущую операцию.
* Ключевое слово `await` предписывает компилятору разделить метод на две части. Первая часть завершается операцией, которая запускается в асинхронном режиме. Вторая часть помещается в метод обратного вызова, который вызывается при завершении операции.
* `ToListAsync` является асинхронной версией метода расширения `ToList`.

При написании асинхронного кода, который использует EF Core, нужно учитывать некоторые моменты:

* Асинхронно выполняются только те операторы, в результате которых в базу данных отправляются запросы или команды. К ним относятся `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` и `SaveChangesAsync`. В их число не входят операторы, которые просто изменяют `IQueryable`, такие как `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* Контекст EF Core не является потокобезопасным, поэтому не следует пытаться выполнять несколько операций параллельно.
* Чтобы воспользоваться преимуществами повышенной производительности асинхронного кода, убедитесь, что пакеты библиотек (например, для разбиения на страницы) используют асинхронную модель, когда вызывают методы EF Core, которые направляют запросы в базу данных.

Дополнительные сведения об асинхронном программировании см. в разделах [Обзор асинхронной модели](/dotnet/standard/async) и [Асинхронное программирование с использованием ключевых слов Async и Await](/dotnet/csharp/programming-guide/concepts/async/).

Следующее руководство посвящено базовым операциям CRUD (создание, чтение, обновление, удаление).

::: moniker-end

> [!div class="step-by-step"]
> [Вперед](xref:data/ef-rp/crud)
