---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: "Реализация репозитория и единицы шаблонов рабочих элементов в приложении ASP.NET MVC (9, 10) | Документы Microsoft"
author: tdykstra
description: "Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 02b1de31b9513247facc92bc6b72247865d176f9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Реализация репозитория и единицы шаблонов рабочих элементов в приложении ASP.NET MVC (9, 10)
====================
По [Tom Dykstra](https://github.com/tdykstra)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso университета примера веб-приложения демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio 2012. Сведения о учебника серии см [в первом учебнике ряда](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Учебник рядов можно запустить с самого начала или [загрузить начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если возникли проблемы, не удается устранить, [загрузить завершенного главе](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы путем сравнения код для завершения кода. Некоторые распространенные ошибки и способы их устранения см. в разделе [ошибок и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем учебнике используется наследование для уменьшения избыточный код в `Student` и `Instructor` классы сущностей. В этом учебнике вы увидите некоторые способы использования репозитория и единицы шаблонов рабочих элементов для операций CRUD. Как показано в предыдущем учебнике это одним вы измените как код работает со страницами вы уже создан, а не создавать новые страницы.

## <a name="the-repository-and-unit-of-work-patterns"></a>Репозиторий и единицы шаблонов рабочих элементов

Репозиторий и единицы шаблонов рабочих элементов предназначены для создания уровень абстракции между уровня доступа к данным и бизнес-логики приложения. Реализация этих шаблонов для изоляции приложения от изменений в хранилище данных и может упростить автоматических модульного тестирования или разработки через тестирование (TDD).

В этом учебнике необходимо реализовать класс репозитория для каждого типа сущности. Для `Student` типа сущности, вы создадите интерфейс репозитория и класс репозитория. При создании экземпляра репозитория в контроллере, используется интерфейс, чтобы контроллер принимает ссылку на любой объект, реализующий интерфейс репозитория. Если контроллер работает в веб-сервере, он получает репозитории, которая работает с платформой Entity Framework. Если контроллер работает в группе класс модульного теста, он получает репозитории, которая работает с данными, хранящимися в виде, которую можно легко изменять для тестирования, такие как коллекции в памяти.

Далее в этом учебнике вы используете несколько репозиториев и единицы работы класса для `Course` и `Department` типы сущностей в `Course` контроллера. Класс рабочих единиц координирует работу нескольких репозиториев, создав класс контекста одной базы данных совместно используется всеми из них. Если вы хотите иметь возможность выполнять автоматическое тестирование модулей, будет создавать и использовать интерфейсы для этих классов таким же образом, как это было сделано для `Student` репозитория. Тем не менее для упрощения работы с учебником предстоит создать и использовать эти классы без интерфейсов.

Ниже показан один из способов представить связи между контроллером и классы контекста, по сравнению с не используется вообще репозитория или единицей работы шаблон.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

В этой серии учебника не будет создавать модульные тесты. Введение TDD с приложением MVC, который использует шаблон репозитория см. в разделе [Пошаговое руководство: использование TDD с ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Дополнительные сведения о шаблон репозитория см. следующие ресурсы:

- [Шаблон репозитория](https://msdn.microsoft.com/library/ff649690.aspx) на сайте MSDN.
- [С помощью шаблонов репозитория и единица работы с платформой Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) блоге группы разработчиков платформы Entity Framework.
- [Agile Entity Framework 4 репозитория](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) последовательность записей в блоге Джули Лерман.
- [Создание учетной записи в приложении HTML5/jQuery быстро](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) в блоге Дэна Wahlin.

> [!NOTE]
> Существует много способов реализации репозитория и единицы шаблонов рабочих элементов. Можно использовать классы репозитория с или без класс рабочих единиц. Вы можете реализовать один репозиторий для всех типов сущностей, или для каждого типа. Если вы реализуете для каждого типа, можно использовать отдельные классы, универсального базового класса и производные классы или абстрактный базовый класс и производные классы. Может содержать бизнес-логику в репозитории или ограничить его логики доступа к данным. Можно также создавать уровень абстракции в класс контекста базы данных с помощью [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) существует интерфейсы вместо [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) типы параметров сущности. Подход к реализации уровня абстракции, показанными в данном руководстве является одной из возможностей необходимо учитывать рекомендации не для всех сценариях и средах.


## <a name="creating-the-student-repository-class"></a>Создание класса Student репозитория

В *DAL* папки, создайте файл класса с именем *IStudentRepository.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Этот код объявляет типичный набор методов CRUD, включая двух методов чтения — один, который возвращает все `Student` сущности и которая находит один `Student` сущности по идентификатору.

В *DAL* папки, создайте файл класса с именем *StudentRepository.cs* файла. Замените существующий код следующим кодом, который реализует `IStudentRepository` интерфейс:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Контекст базы данных определяется в переменной класса и конструктор ожидает, что вызывающий объект передавать экземпляр контекста:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Можно было создать новый контекст в репозитории, но затем при использовании нескольких репозиториев в один контроллер каждого ограничится отдельном контексте. Позже вы воспользуетесь нескольких репозиториев в `Course` контроллера и вы увидите, как класс рабочих единиц убедитесь, что все репозитории использовать тот же контекст.

Реализует репозитории [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) и удаляет контекст базы данных, как было показано ранее в контроллере, и его методы CRUD выполнять вызовы контекст базы данных таким же образом, вы уже видели раньше.

## <a name="change-the-student-controller-to-use-the-repository"></a>Изменение контроллера студента для использования в репозиторий

В *StudentController.cs*, замените кода класса следующим кодом. Изменения выделены.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Контроллер теперь объявляет переменную для объекта, реализующего класс `IStudentRepository` интерфейс вместо класса контекста:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

По умолчанию конструктор создает новый экземпляр контекста, и необязательный конструктор позволяет вызывающему объекту передать экземпляр контекста.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(При использовании *внедрения зависимостей*, или DI, конструктор по умолчанию не требуется, так как программное обеспечение DI благодаря объекта правильный репозиторий предоставлен всегда будет.)

В методах CRUD репозитория теперь называется вместо контекста:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

И `Dispose` теперь освобождает репозитории, а не в контексте:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Запустите сайт и нажмите кнопку **учащихся** вкладки.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Страница выглядит и работает так же, как и до изменения кода, чтобы использовать хранилище и другие страницы студента также работать так же. Однако имеется существенное различие в способе `Index` метод контроллера выполняет фильтрацию и упорядочивание. Исходную версию этого метода содержит следующий код:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Обновленный `Index` метод содержит следующий код:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Только выделенный код был изменен.

В исходной версии кода `students` типизируется как `IQueryable` объект. Запрос не отправлены в базу данных, пока не преобразуется в коллекции, такие как с помощью метода `ToList`, которой не возникает, пока представление Index обращается к модели студента. `Where` Метод в приведенный выше код становится `WHERE` предложение в запросе SQL, который отправляется в базу данных. В свою очередь, это означает, что в базе данных возвращаются только выбранные сущности. Тем не менее, в результате изменения `context.Students` для `studentRepository.GetStudents()`, `students` переменной после этой инструкции `IEnumerable` коллекцию, включающую всех студентов в базе данных. В результате применения `Where` метод такой же, но теперь работу в памяти на веб-сервере, а не базы данных. Для запросов, которые возвращают большие объемы данных это может быть неэффективным.

> [!TIP] 
> 
> **IQueryable vs. Интерфейс IEnumerable**
> 
> После реализации репозитория как показано здесь, даже если вы вводите что-то в **поиска** поле запросов, отправленных на SQL Server возвращает все строки студента, так как они не включают условия поиска:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Этот запрос возвращает все данные студента, так как репозиторий выполнения запроса, не зная о критериях поиска. Процесс сортировки, применяя условия поиска и выбора подмножества данных для подкачки (в этом случае отображаются только 3 строки) выполняется в памяти более поздние версии `ToPagedList` метод будет вызван на `IEnumerable` коллекции.
> 
> В предыдущей версии кода (до реализован репозитория), запрос не отправляется в базу данных до после применения условия поиска, когда `ToPagedList` будет вызван на `IQueryable` объекта.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> При вызове ToPagedList на `IQueryable` запросов, отправленных на SQL Server определяет строку поиска и в результате возвращаются только те строки, которые соответствуют критериям поиска и фильтрации необходимо сделать в памяти.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Этом руководстве объясняется, как для проверки запросов, отправляемых на SQL Server.)


Ниже показан способ реализации репозитория методы, которые позволяют указать, что этот действия должны выполняться в базе данных.

Теперь вы создали уровень абстракции между контроллером и контекст базы данных Entity Framework. Если предполагается выполнять автоматические модульное тестирование с помощью этого приложения, можно создать в проекте модульного теста, который реализует класс альтернативных репозитория `IStudentRepository` *.* Вместо вызова контекста для чтения и записи данных, этот класс макета репозитория возможность управлять коллекции в памяти для функции контроллера тестирования.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Реализация универсального репозитория и класс рабочих единиц

Создание класса репозитория для каждого типа сущности может привести к большой объем избыточных кода и это может привести к частичных обновлений. Например предположим, что необходимо обновить два разных типа сущностей в рамках одной транзакции. Если каждый использует экземпляр контекста отдельной базы данных, один может завершиться успешно и другой может привести к ошибке. Один из способов минимизировать избыточный код является использование универсального репозитория и один из способов убедиться, что все репозитории использовать один и тот же контекст базы данных (и таким образом обновлялись все) является использование класс рабочих единиц.

В этом разделе учебника вы создадите `GenericRepository` класса и `UnitOfWork` класса и использовать их в `Course` контроллера для доступа к `Department` и `Course` наборов сущностей. Как упоминалось ранее, чтобы не усложнять этой части учебника вы не создаете интерфейсы для этих классов. Но если предполагается использовать их для упрощения TDD, следует обычно реализовать их с интерфейсами так же, как `Student` репозитория.

### <a name="create-a-generic-repository"></a>Создание универсального репозитория

В *DAL* папке создайте *GenericRepository.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Переменные класса объявляются контекст базы данных и набора сущностей, хранилище создается в результате:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Конструктор принимает экземпляр контекста базы данных и инициализирует переменную набора сущностей:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get` Метод используются лямбда-выражения, чтобы вызывающий код, чтобы указать условие фильтра и столбца для упорядочивания результатов по и строковый параметр позволяет вызывающему объекту обеспечения упреждающую разделенный запятыми список свойств навигации:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Код `Expression<Func<TEntity, bool>> filter` означает вызывающий объект будет предоставлять лямбда-выражение на основе `TEntity` типа и это выражение возвращает значение типа Boolean. Например, если хранилище создается в результате `Student` тип сущности может указать код в вызывающем методе `student => student.LastName == "Smith` &quot; для `filter` параметра.

Код `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` также означает вызывающий объект будет предоставлять лямбда-выражение. Но в этом случае входные данные для выражения является `IQueryable` для объекта `TEntity` типа. Выражение вернет упорядоченный версии, `IQueryable` объекта. Например, если хранилище создается в результате `Student` тип сущности может указать код в вызывающем методе `q => q.OrderBy(s => s.LastName)` для `orderBy` параметра.

Код в `Get` создает метод `IQueryable` объекта, а затем применяет критерий фильтра, если он существует:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Затем он применяется выражения eager загрузки после синтаксического анализа список с разделителями-запятыми:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Наконец, оно применяется `orderBy` выражения, если таковой имеется и возвращает результаты; в противном случае он возвращает результаты из неупорядоченный запрос:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

При вызове `Get` метод, можно выполнить фильтрацию и сортировку для `IEnumerable` коллекция, возвращаемая этим методом, вместо указания параметров для этих функций. Однако сортировка и фильтрация рабочих затем выполняются в памяти на веб-сервере. С помощью этих параметров, обеспечить работу базы данных, а не веб-сервер. Альтернативой является создание производных классов для конкретной сущности типов и добавьте специализированный `Get` методы, такие как `GetStudentsInNameOrder` или `GetStudentsByName`. Однако в сложных приложениях это может привести к большому числу таких производные классы и специализированные методы, которые могут быть несколько действий для обслуживания.

Код в `GetByID`, `Insert`, и `Update` методов аналогична видно в репозитории, не являющимися универсальными. (Не предоставляя параметром упреждающую в `GetByID` подписи, поскольку не удается выполнить упреждающую с `Find` метод.)

Две перегрузки предоставляются для `Delete` метод:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Одно из этих позволяет передать идентификатор сущности для удаления, и одна принимает экземпляр сущности. Как видно из [обработки параллелизма](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) учебника требуется обработка вы параллельности `Delete` метод, который принимает экземпляр сущности, который содержит исходное значение свойства отслеживания.

Этот универсальный репозиторий будет обрабатывать обычные требования CRUD. Если определенного типа сущности содержит специальные требования, такие как более сложные фильтрации или упорядочения, можно создать производный класс, имеющий дополнительные методы для этого типа.

## <a name="creating-the-unit-of-work-class"></a>Создание класс рабочих единиц

Класс рабочих единиц служит одной цели: чтобы убедиться, что при использовании нескольких репозиториев, они совместно используют контекст одной базы данных. Таким образом, при завершении единицу работы, можно вызвать `SaveChanges` метода для этого экземпляра контекста и быть уверенным в том, все связанные изменения координироваться. Все, что нужен класс `Save` методов и свойств для каждого репозитория. Каждое свойство репозиторий возвращает экземпляр репозитория, который был создан экземпляр, используя один и тот же экземпляр контекста базы данных как в других экземплярах репозитория.

В *DAL* папки, создайте файл класса с именем *UnitOfWork.cs* и замените код шаблона с помощью следующего кода:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Код создает класс переменные для каждого репозитория и контекст базы данных. Для `context` переменной, создается новый контекст:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Каждое свойство репозитория проверяет, существует ли уже репозитория. В противном случае он создает хранилище, передав экземпляр контекста. В результате все репозитории совместно использовать один и тот же экземпляр контекста.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save` Вызовы метода `SaveChanges` в контексте базы данных.

Как и любой класс, который создает экземпляр контекста базы данных в переменной класса `UnitOfWork` класс реализует `IDisposable` и удаляет контекст.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Изменение контроллера для использования репозиториев и класс UnitOfWork курса

Замените код, которые отсутствуют в *CourseController.cs* следующим кодом:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Этот код добавляет переменную класса для `UnitOfWork` класса. (Если вы использовали интерфейсы здесь, не инициализировать переменную здесь; вместо этого следует реализовать шаблон из двух конструкторов, как это делалось `Student` репозитория.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

В остальной части класса, все ссылки на контекст базы данных заменяются ссылки на соответствующие репозитория с помощью `UnitOfWork` свойства для доступа к репозиторию. `Dispose` Метод уничтожает `UnitOfWork` экземпляра.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Запустите сайт и нажмите кнопку **курсы** вкладки.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Страница выглядит и работает так же, как раньше изменения и другие страницы курса также работать так же.

## <a name="summary"></a>Сводка

Теперь реализована модульных шаблонов рабочих элементов и репозитория. Как параметры метода в репозитории универсального используется лямбда-выражения. Дополнительные сведения о том, как использовать эти выражения с `IQueryable` см. в разделе [IQueryable(T) интерфейса (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) в библиотеке MSDN. В следующем учебнике вы узнаете, как для обработки некоторых сложных сценариях.

Ссылки на другие ресурсы Entity Framework можно найти в [Карта содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Назад](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Вперед](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
