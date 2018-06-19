---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4, веб-формы - часть 3 | Документы Microsoft
author: tdykstra
description: Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, использующий Entity Framework. Это образец приложения...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 654f3556af5d05ec186e1811421966bbaffd2e21
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889258"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Приступая к работе с базой данных Entity Framework 4.0 сначала и ASP.NET 4 Web Forms — часть 3
====================
по [Tom Dykstra](https://github.com/tdykstra)

> Contoso университета примера веб-приложения показано, как для создания приложений веб-форм ASP.NET, с помощью Entity Framework 4.0 и Visual Studio 2010. Сведения о учебника серии см [в первом учебнике ряда](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Фильтрацию, упорядочение и группирование данных

В предыдущем учебнике используется `EntityDataSource` управления для отображения и изменения данных. В этом учебнике вы фильтрации, упорядочения и группирования данных. Если это сделать, задав свойства `EntityDataSource` элемента управления, синтаксис отличается от других элементов управления источниками данных. Как вы увидите, тем не менее, можно использовать `QueryExtender` элемента управления, чтобы свести к минимуму эти различия.

Мы изменим *Students.aspx* страницы для фильтрации для учащихся, сортировка по имени и поиск по имени. Вы также измените *Courses.aspx* страницы для отображения курсы для выбранного подразделения и поиска курсов по имени. Наконец, мы добавим студента статистики для *About.aspx* страницы.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>С помощью элемента управления EntityDataSource «Where» свойства для фильтрации данных

Откройте *Students.aspx* страницу, созданную в предыдущем учебнике. В настоящее время настроен, `GridView` управления на странице отображаются все имена из `People` набора сущностей. Однако нужно показать учащихся, который можно найти, выбрав `Person` сущностей, которые имеют дат регистрации, отличных от null.

Переключитесь в **разработки** просматривать и выбирать `EntityDataSource` элемента управления. В **свойства** задайте `Where` свойства `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Синтаксис, который используется в `Where` свойство `EntityDataSource` элемент управления является Entity SQL. Язык Entity SQL является напоминает Transact-SQL, но он настраивается для использования с сущностями, а не объекты базы данных. В выражении `it.EnrollmentDate is not null`, слово `it` представляет ссылку на сущность, возвращенных запросом. Таким образом `it.EnrollmentDate` ссылается на `EnrollmentDate` свойство `Person` сущности, `EntityDataSource` возвращает управление.

Запустите страницу. Список студентов теперь содержит только учащихся. (Нет ни одной строки, где отсутствует дата регистрации.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>С помощью свойства «OrderBy» EntityDataSource для упорядочения данных

Также можно этот список в порядке имя при первом отображении. С *Students.aspx* страницы, все еще открыт в **разработки** представление и с `EntityDataSource` управления по-прежнему выбран в **свойства** задайте  **OrderBy** свойства `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Запустите страницу. Список студентов теперь находится в порядке по фамилии.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>С помощью параметра управления необходимо задать свойство «Где»

С помощью других элементов управления источниками данных при передаче значения параметров, `Where` свойство. На *Courses.aspx* страницы, созданные в части 2 этого руководства, этот метод можно использовать для отображения курсов, которые связаны с отдела, которые пользователь выбирает из раскрывающегося списка.

Откройте *Courses.aspx* и переключитесь в **разработки** представления. Добавьте второй `EntityDataSource` на страницу элемент управления и назовите его `CoursesEntityDataSource`. Подключите ее к `SchoolEntities` модели и выберите `Courses` как **EntitySetName** значение.

В **свойства** окно, нажмите кнопку с многоточием в **где** поле свойства. (Убедитесь, что `CoursesEntityDataSource` управления выделен перед использованием **свойства** окна.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**Редактор выражений** диалоговое окно. В этом диалоговом окне выберите **автоматически создавать предложение Where выражения на основе указанных параметров**, а затем нажмите кнопку **добавить параметр**. Имя параметра `DepartmentID`выберите **управления** как **источник параметра** значение и выберите **DepartmentsDropDownList** как **ControlID**  значение.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Нажмите кнопку **Показать дополнительные свойства**и в **свойства** окно **редактор выражений** диалоговом изменение `Type` свойства `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Когда закончите, нажмите кнопку **ОК**.

Под раскрывающемся списке, добавьте `GridView` на страницу элемент управления и назовите его `CoursesGridView`. Подключите ее к `CoursesEntityDataSource` данных система управления версиями, нажмите кнопку **обновить схему**, нажмите кнопку **Правка столбцов**и удалите `DepartmentID` столбца. `GridView` Разметку элемента управления приведенной в следующем примере.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

При изменении пользователем выбранного подразделения, в раскрывающемся списке, требуется список связанных курсов обновляться автоматически. Для этого выберите в раскрывающемся списке и в **свойства** задайте `AutoPostBack` свойства `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Теперь, когда закончите, с помощью конструктора, переключитесь в **источника** просмотра и замените `ConnectionString` и `DefaultContainer` имя свойства `CoursesEntityDataSource` управления `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` атрибута. Когда закончите, разметки для элемента управления будет выглядеть как в следующем примере.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Запустите страницу и использовать раскрывающегося списка для выбора различных отделов. Только у курсов, предлагаемые выбранного подразделения, отображаются в `GridView` элемента управления.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>С помощью свойства «GroupBy» EntityDataSource для группирования данных

Предположим, что университета Contoso хочет размещение некоторую статистику студента текст на его о странице. В частности ему нужно показать декомпозиция количество студентов по дате, когда они зарегистрированы.

Откройте *About.aspx*и в **источника** Просмотр, заменить существующее содержимое `BodyContent` было управлять с помощью «Статистика студента текст» между `h2` теги:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

После заголовка, добавьте `EntityDataSource` управления и назовите его `StudentStatisticsEntityDataSource`. Подключите ее к `SchoolEntities`выберите `People` набор сущностей и оставить **выберите** поле в мастере без изменений. Задайте следующие свойства **свойства** окна:

- Чтобы отфильтровать только для учащихся, установить `Where` свойства `it.EnrollmentDate is not null`.
- Чтобы сгруппировать результаты по дате регистрации, задайте `GroupBy` свойства `it.EnrollmentDate`.
- Чтобы выбрать дату регистрации и число учеников, установите `Select` свойства `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Чтобы упорядочить результаты по дате регистрации, задайте `OrderBy` свойства `it.EnrollmentDate`.

В **источника** просмотра, замените `ConnectionString` и `DefaultContainer` в именах свойств с `ContextTypeName` свойство. `EntityDataSource` Разметку элемента управления теперь приведенной в следующем примере.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Синтаксис `Select`, `GroupBy`, и `Where` свойства напоминает Transact-SQL, за исключением `it` ключевое слово, которое указывает текущей сущности.

Добавьте следующую разметку для создания `GridView` элемента управления для отображения данных.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Запустите страницу, чтобы просмотреть список, отображающий количество студентов по дате регистрации.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>С помощью элемента управления QueryExtender для фильтрации и сортировки

`QueryExtender` Элемент управления предоставляет способ указания фильтрации и сортировки в разметке. Синтаксис зависит от система управления базами данных (СУБД), который вы используете. Это обычно зависит от платформы Entity Framework, за тем исключением, что синтаксис, используемый для свойств навигации является уникальным на платформу Entity Framework.

В этой части руководства мы используем `QueryExtender` управления фильтра и упорядочения данных и одно из полей order-by будет свойством навигации.

(Если вы предпочитаете использовать кода вместо разметки для расширения запросы, которые автоматически генерируются службами `EntityDataSource` управления, это можно сделать обработки `QueryCreated` событий. Это как `QueryExtender` расширяет `EntityDataSource` также управлять запросов.)

Откройте *Courses.aspx* страницы и под разметкой, был добавлен ранее, вставьте следующую разметку для создания заголовка, текстовое поле для ввода строки поиска, кнопка поиска и `EntityDataSource` элемента управления, привязанного к `Courses` набора сущностей.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Обратите внимание, что `EntityDataSource` элемента управления `Include` свойству `Department`. В базе данных `Course` таблица не содержит название отдела; он содержит `DepartmentID` столбец внешнего ключа. Если запросы к базе данных напрямую, чтобы получить название отдела, а также данные о курсе, будет необходимо соединить `Course` и `Department` таблицы. Установив `Include` свойства `Department`, укажите, что Entity Framework должна выполняют получение связанной `Department` сущности, когда он получает `Course` сущности. `Department` Сущности сохраняется в `Department` свойство навигации `Course` сущности. (По умолчанию `SchoolEntities` класс, который был создан конструктором моделей данных извлекает данные при необходимости, а элемент управления источником данных был присоединен к этому классу, поэтому при задании `Include` свойства не требуется. Тем не менее, значения этого свойства позволяет повысить производительность страницы, так как в противном случае Entity Framework сделает отдельные вызовы к базе данных для получения данных для `Course` сущностей и для соответствующего `Department` сущности.)

После `EntityDataSource` управления только что создали, вставьте следующую разметку для создания `QueryExtender` управления, привязанного к этому `EntityDataSource` элемента управления.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression` Элемент указывает, что вы хотите выбрать курсов, названия которых соответствуют значению, введенному в текстовом поле. Только число символов в текстовом поле вводятся будут сравниваться, так как `SearchType` указывает свойство `StartsWith`.

`OrderByExpression` Элемент указывает, что результирующий набор будет упорядочен по курса заголовок в название отдела. Обратите внимание на то, как указывается название отдела: `Department.Name`. Так как связь между `Course` сущности и `Department` сущность является один к одному, `Department` свойство навигации содержит `Department` сущности. (Если бы это был один ко многим, свойство будет содержать коллекцию.) Чтобы получить имя подразделения, необходимо указать `Name` свойство `Department` сущности.

Наконец, добавьте `GridView` элемента управления для отображения списка курсов:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

Первый столбец имеет шаблона поле, отображающее название отдела. Указывает выражение привязки данных `Department.Name`так же, как видно из `QueryExtender` элемента управления.

Запустите страницу. Список всех курсов содержатся первоначального отображения в порядке по отделам, а затем по названию курса.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Введите «m» и нажмите кнопку **поиска** для просмотра всех курсов, названия которых начинаются с буквы «m» (поиск не является учитывается регистр).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>С помощью оператора «Мне нравится» для фильтрации данных

Можно добиться эффекта, подобного `QueryExtender` элемента управления `StartsWith`, `Contains`, и `EndsWith` поиска типов с помощью `Like` оператор в `EntityDataSource` элемента управления `Where` свойство. В этой части руководства вы увидите, как использовать `Like` оператор для поиска студент по имени.

Откройте *Students.aspx* в **источника** представления. После `GridView` управления, добавьте следующую разметку:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Эта разметка похоже на то, что вы уже видели ранее за исключением `Where` значение свойства. Во второй части `Where` выражение определяет поиск подстроки (`LIKE %FirstMidName% or LIKE %LastName%`), выполняет поиск первого и последнего имена для вводятся в текстовом поле.

Запустите страницу. Изначально отображаются студентов, так как значение по умолчанию для `StudentName` параметр является «%».

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Введите букву «g» в текстовом поле и нажмите кнопку **поиска**. Появится список студентов, «g» имеют либо имя или Фамилия.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Вы теперь отображается, обновить, фильтрации, упорядоченные и сгруппированные данные из отдельных таблиц. В следующем уроке будет начать работу со связанными данными (основной-подробности).

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-4.md)
