---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Реализация в приложении ASP.NET MVC (9 из 10) репозитория и шаблонов Unit of Work | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, с помощью Entity Framework 5 Code First и Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 0ca67c67b763d003e5f395ddd4bac0c5ec28f1f1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366373"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Реализация репозитория и шаблонов Unit of Work в приложении ASP.NET MVC (9 из 10)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Скачать завершенный проект](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Пример веб-приложение университета Contoso демонстрирует создание приложения ASP.NET MVC 4, используя Entity Framework 5 Code First и Visual Studio 2012. Сведения о серии руководств см. в [первом руководстве серии](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Серии руководств можно начать с самого начала или [Загрузите начальный проект для этой главы](building-the-ef5-mvc4-chapter-downloads.md) и начните здесь.
> 
> > [!NOTE] 
> > 
> > Если вы столкнулись с проблемами, не удается устранить, [скачать завершенного глава](building-the-ef5-mvc4-chapter-downloads.md) и попробуйте воспроизвести проблему. Обычно можно найти решение проблемы, сравнивая код, чтобы полный код. Некоторые распространенные ошибки и способы их устранения, см. в разделе [ошибки и способы их устранения.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


В предыдущем руководстве вы использовали наследования для уменьшения избыточный код в `Student` и `Instructor` классов сущностей. В этом руководстве вы увидите некоторые способы использования репозиторий и блок рабочих шаблонов для операций CRUD. Как и в предыдущем учебном курсе это одним вам предстоит изменить, как работает ваш код со страницы вы уже создан, а не создавать новые страницы.

## <a name="the-repository-and-unit-of-work-patterns"></a>Репозиторий и блок рабочих шаблонов

Репозиторий и блок рабочих шаблонов предназначены для создания уровня абстракции между уровня доступа к данным и бизнес-логики приложения. Реализация таких шаблонов позволяет изолировать приложение от изменений в хранилище данных и упрощает автоматическое модульное тестирование или разработку на основе тестирования.

В этом руководстве вы реализуете класс репозитория для каждого типа сущности. Для `Student` тип сущности, мы создадим интерфейс репозитория и класс репозитория. При создании экземпляра репозитория в контроллере, будет использовать интерфейс, чтобы контроллер принимает ссылку на любой объект, реализующий интерфейс репозитория. Если контроллер работает на веб-сервер, он получает это репозиторий, который работает с Entity Framework. Когда контроллер работает в области класса модульных тестов, он получает это репозиторий, который работает с данными, хранящимися в виде, можно легко управлять для тестирования, такие как коллекции в памяти.

Далее в этом руководстве, вы будете использовать несколько репозиториев и рабочий класс для единиц `Course` и `Department` типами сущностей в `Course` контроллера. Класс рабочих единиц координирует работу нескольких репозиториях, создав класс контекста отдельной базы данных совместно используется всеми из них. Если вы хотите иметь возможность выполнять автоматическое тестирование модулей, можно будет создать и использовать интерфейсы для этих классов в так же, как для `Student` репозитория. Тем не менее чтобы упростить пример, будет создавать и использовать эти классы без интерфейсов.

Ниже показан один способ представить связи между контроллером и классы контекста, по сравнению с не используется вообще репозитория или шаблон единицы работы.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

В этой серии руководств, не создаются модульные тесты. Введение в TDD, с помощью приложения MVC, который использует шаблон репозитория, см. в разделе [Пошаговое руководство: использование TDD с ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Дополнительные сведения о шаблон репозитория см. следующие ресурсы:

- [Шаблон репозитория](https://msdn.microsoft.com/library/ff649690.aspx) на сайте MSDN.
- [Использование шаблонов репозитория и единица работы с Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) в блоге группы Entity Framework.
- [Agile Entity Framework 4 репозитория](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) серию публикаций в блоге Джули Лерман.
- [Создание учетной записи в приложении HTML5/jQuery быстро](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) в блоге Дэна Вахлина.

> [!NOTE]
> Существует много способов реализации репозиторий и блок рабочих шаблонов. Можно использовать классы репозитория с или без класс рабочих единиц. Вы можете реализовать один репозиторий для всех типов сущностей или для каждого типа. Если вы реализуете одной для каждого типа, можно использовать отдельные классы, универсального базового класса и производных классов или абстрактного базового класса и производных классов. Можно включать бизнес-логики в репозиторий или ограничить его логики доступа к данным. Можно также создать уровень абстракции в класс контекста базы данных с помощью [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) существует интерфейсов вместо [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) типы для наборов сущностей. Подход к реализации уровня абстракции в этом учебнике — один из вариантов, о котором стоит подумать, не рекомендацию для всех сценариях и средах.


## <a name="creating-the-student-repository-class"></a>Создание класса Student репозитория

В *DAL* папке создайте файл класса с именем *IStudentRepository.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Этот код объявляет типичный набор методов CRUD, включая два метода чтения — одна из которых возвращает все `Student` сущностей, а тот, который находит один `Student` сущности по идентификатору.

В *DAL* папке создайте файл класса с именем *StudentRepository.cs* файла. Замените существующий код следующим кодом, который реализует `IStudentRepository` интерфейса:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Контекст базы данных определяется в переменной класса и конструктор ожидает, что вызывающий объект передавать экземпляр контекста:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Можно было создать новый контекст в репозитории, но затем при использовании нескольких репозиториев в одном контроллере, каждый попала бы с отдельным контекстом. Позже вы используете несколько репозиториев в `Course` контроллера и вы увидите, как класс рабочих единиц убедитесь, что все репозитории использовать тот же контекст.

Реализует хранилище [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) и удаляет контекст базы данных, как было показано ранее в контроллер, и его CRUD-методы выполнения вызовов контекст базы данных таким же образом, обсуждавшейся ранее.

## <a name="change-the-student-controller-to-use-the-repository"></a>Изменение контроллера учащегося использование репозитория

В *StudentController.cs*, замените код класса следующим кодом. Изменения выделены.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Контроллер теперь объявляется переменная класса для объекта, реализующего `IStudentRepository` интерфейса, а не класс контекста:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

По умолчанию конструктор создает новый экземпляр контекста, и дополнительный конструктор позволяет вызывающей стороне передать в экземпляр контекста.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Если вы использовали *внедрения зависимостей*, или внедрения Зависимостей, конструктор по умолчанию не требуется, так как DI бы убедитесь, что объект правильный репозиторий всегда будет предоставляться.)

В CRUD-методы а не в контексте теперь называется репозитория:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

И `Dispose` метод теперь удаляет хранилище, а не в контексте:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Запустите сайт и нажмите кнопку **учащихся** вкладки.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Страница выглядит и работает так же, так же, как изменить код, чтобы использовать хранилище и на других страницах учащегося также работают одинаково. Тем не менее, есть важное различие в способе `Index` метод контроллера выполняет фильтрацию и упорядочивание. Исходная версия этого метода содержит следующий код:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Обновленный `Index` метод содержит следующий код:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Только выделенный код был изменен.

В исходной версии кода `students` типизируется как `IQueryable` объекта. Запрос не отправляется в базу данных, пока оно преобразуется в коллекции, такие как с помощью метода `ToList`, которой не происходит, пока в представление Index обращается к модели учащегося. `Where` Метод в приведенный выше исходный код становится `WHERE` предложение в SQL-запрос, который отправляется в базу данных. В свою очередь, это означает, что в базе данных, возвращаются только выбранные сущности. Тем не менее, в результате изменения `context.Students` для `studentRepository.GetStudents()`, `students` переменной после этой инструкции `IEnumerable` коллекцию, включающую всех учащихся в базе данных. В результате применения `Where` метод одинаков, но теперь работа выполняется в памяти на веб-сервере, а не базы данных. Для запросов, которые возвращают большие объемы данных это может быть неэффективным.

> [!TIP]
> 
> **IQueryable vs. Интерфейс IEnumerable**
> 
> После реализации хранилище как показано ниже, даже если введено в **поиска** поле запросов, отправленных на SQL Server возвращает все строки учащегося, так как в нем не указаны условия поиска:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Этот запрос возвращает все данные об учащихся, так как хранилище выполнила запрос, не зная о критериях поиска. Процесс сортировки, применяя условия поиска и выбора подмножества данных для разбиения на страницы (в этом случае отображается только 3 строки), создается в памяти более поздние версии `ToPagedList` вызывается метод `IEnumerable` коллекции.
> 
> В предыдущей версии кода (прежде чем вы реализовали хранилище), запрос не отправляется в базу данных до после применения условия поиска, когда `ToPagedList` вызывается для `IQueryable` объекта.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> При вызове ToPagedList на `IQueryable` объектов, запросов, отправленных на SQL Server указывает строка поиска и в результате возвращаются только те строки, удовлетворяющие условиям поиска и фильтрации необходимо выполнить в памяти.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (В следующем учебнике рассматриваются анализ запросов, отправленных на SQL Server.)


Ниже показано, как реализовать методы репозитории, которые позволяют указать, что это действия должны выполняться в базе данных.

Теперь вы создали уровень абстракции между контроллером и контекст базы данных Entity Framework. Если вы предполагаете выполнять автоматическое модульное тестирование с помощью этого приложения, можно создать класс альтернативным репозиторий в проект модульного теста, который реализует `IStudentRepository` *.* Вместо вызова контекста для чтения и записи данных, этот класс для фиктивного репозитория может управлять коллекциями в памяти для тестирования функции контроллера.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Реализация универсального репозитория и класс рабочих единиц

Создание класса репозиторий для каждого типа сущности может привести к массу избыточный код, и она может вызвать частичное обновление. Например предположим, что вам нужно обновить два разных типа сущностей в рамках одной транзакции. Если каждый использует экземпляр контекста отдельную базу данных, один может быть выполнена успешно, и другой может завершиться ошибкой. Одним из способов минимизировать избыточный код является использование универсального репозитория, а один из способов, чтобы убедиться, что все репозитории использовать один и тот же контекст базы данных (и тем самым координировать все обновления) является использование класс рабочих единиц.

В этом разделе руководства вы создадите `GenericRepository` класс и `UnitOfWork` класса и использовать их в `Course` контроллера для доступа как к `Department` и `Course` наборов сущностей. Как упоминалось ранее, чтобы не усложнять этой части руководства, вы не создаете интерфейсов для этих классов. Но если вы предполагаете использовать их для упрощения TDD, вы бы обычно их реализации с интерфейсами так же, как `Student` репозитория.

### <a name="create-a-generic-repository"></a>Создание универсального репозитория

В *DAL* папке создайте *GenericRepository.cs* и замените существующий код следующим кодом:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Переменные класса объявляются для контекста базы данных, а также для набора сущностей, создается в репозитории:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Конструктор принимает экземпляр контекста базы данных и инициализирует переменную набора сущностей:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get` Метод использует лямбда-выражения, позволяющие вызывающему коду указать условие фильтра и столбец для сортировки результатов по, и строковый параметр позволяет вызывающему объекту указать разделенный запятыми список свойств навигации для Безотложная загрузка:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Код `Expression<Func<TEntity, bool>> filter` означает, что вызывающий объект будет предоставить лямбда-выражения на основе `TEntity` типа и это выражение возвращает значение типа Boolean. Например, если создается в репозитории `Student` тип сущности может указать код в вызывающем методе `student => student.LastName == "Smith` &quot; для `filter` параметра.

Код `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` также означает, что вызывающий объект предоставляет лямбда-выражение. Но в этом случае входные данные для выражения является `IQueryable` для объекта `TEntity` типа. Выражение вернет упорядоченный версию, `IQueryable` объекта. Например, если создается в репозитории `Student` тип сущности может указать код в вызывающем методе `q => q.OrderBy(s => s.LastName)` для `orderBy` параметра.

Код в `Get` метод создает `IQueryable` объекта, а затем применяет критерий фильтра, если таковой имеется:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Затем он применяется выражения безотложной загрузки после синтаксического анализа в списке с разделителями запятыми:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Наконец, он применяется `orderBy` выражения, если таковой имеется и возвращает результаты; в противном случае он возвращает результаты из неупорядоченный запрос:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

При вызове `Get` метод, можно выполнить фильтрацию и сортировку на `IEnumerable` коллекции, возвращаемой методом вместо указания параметров для этих функций. Но сортировка и фильтрация рабочих затем выполняются в памяти на веб-сервере. С помощью следующих параметров, убедитесь, что работа выполняется в базе данных, а не веб-сервера. Альтернативой является создание производных классов для определенной сущности типов и добавьте специализированный `Get` методы, такие как `GetStudentsInNameOrder` или `GetStudentsByName`. Тем не менее в сложном приложении это может привести много таких производных классов и специализированные методы, которые могут быть намного больше усилий для обслуживания.

Код в `GetByID`, `Insert`, и `Update` методы аналогичны показанным в репозитории не являющегося универсальным. (Не предоставляя параметром Безотложная загрузка в `GetByID` подписи, так как не безотложную загрузку с `Find` метод.)

Две перегрузки предоставляются для `Delete` метод:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Один из этих позволяет передавать идентификатор сущности для удаления, и одна принимает экземпляр сущности. Как видно из [обработки параллелизма](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) учебника, вы обработка параллельности позаботиться о `Delete` метод, который принимает экземпляр сущности, который содержит исходное значение свойства отслеживания.

Этот универсальный репозиторий будет обрабатывать обычным требованиям CRUD. Когда сущности определенного типа имеет особые требования, такие как более сложные фильтрация или упорядочение, можно создать производный класс, имеющий дополнительные методы для этого типа.

## <a name="creating-the-unit-of-work-class"></a>Создание класс рабочих единиц

Класс рабочих единиц служит цели: чтобы убедитесь, что при использовании нескольких репозиториев, они совместно используют контекст отдельной базы данных. Таким образом, при завершении единицы работы можно вызвать `SaveChanges` метода для этого экземпляра контекста и быть уверенным, что все связанные изменения будет согласовываться. Все, что нужен класс `Save` методов и свойств для каждого репозитория. Каждое свойство репозиторий возвращает экземпляр репозитория, который был создан экземпляр, используя один и тот же экземпляр контекста базы данных как другие экземпляры репозитория.

В *DAL* папке создайте файл класса с именем *UnitOfWork.cs* и замените код шаблона следующим кодом:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Код создает класс переменные для каждого репозитория и контекст базы данных. Для `context` переменной, создается новый контекст:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Каждое свойство репозитория проверяет, существует ли уже репозитория. В противном случае он создает хранилище, передав экземпляр контекста. В результате все репозитории совместно используют один и тот же экземпляр контекста.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save` Вызовы методов `SaveChanges` на контекст базы данных.

Любой класс, который создает экземпляр контекста базы данных в переменной класса, такие как `UnitOfWork` класс реализует `IDisposable` и удаляет контекст.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Изменение контроллера курс, чтобы использовать класс UnitOfWork и репозиториев

Замените код, присутствующих в *CourseController.cs* следующим кодом:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Этот код добавляет переменную класса для `UnitOfWork` класса. (Если вы здесь используете интерфейсов, вы не инициализировать переменную здесь; вместо этого будет реализовать шаблон или двух конструкторов, как это делалось `Student` репозитория.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

В оставшейся части класса, все ссылки на контекст базы данных будут заменены ссылки на соответствующий репозиторий, используя `UnitOfWork` свойства для доступа к репозиторию. `Dispose` Метод уничтожает `UnitOfWork` экземпляра.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Запустите сайт и нажмите кнопку **курсы** вкладки.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Страница выглядит и работает так же, как раньше изменения, а также других страниц курса работать так же.

## <a name="summary"></a>Сводка

Теперь вы реализовали шаблонов unit of work и репозиторий. Метод в качестве параметров универсального репозитория, вы использовали лямбда-выражения. Дополнительные сведения об использовании этих выражений с `IQueryable` объекта, см. в разделе [IQueryable(T) интерфейс (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) в библиотеке MSDN. В следующем руководстве вы узнаете, как обрабатывать некоторые сложные сценарии.

Ссылки на другие ресурсы Entity Framework можно найти в [схема содержимого для доступа к данным ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Назад](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Вперед](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
