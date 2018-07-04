---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 3 | Документация Майкрософт
author: tdykstra
description: Пример веб-приложение университета Contoso демонстрирует создание приложений веб-форм ASP.NET, с помощью Entity Framework. Пример приложения является...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 51964f86e66e21d63105f1043641c0096d1d41b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391151"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Начало работы с Entity Framework 4.0 Database First и ASP.NET 4 Web Forms — часть 3
====================
по [том Дайкстра](https://github.com/tdykstra)

> Пример веб-приложение университета Contoso демонстрирует создание приложения веб-форм ASP.NET, используя Entity Framework 4.0 и Visual Studio 2010. Сведения об этой серии руководств см. в разделе [в первом учебнике серии](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Фильтрация, упорядочение и группирование данных

В предыдущем учебном курсе мы использовали `EntityDataSource` элемента управления для отображения и редактирования данных. В этом руководстве вы фильтрации, заказа и группирования данных. После этого путем задания свойств `EntityDataSource` элемента управления, синтаксис отличается от других элементов управления источниками данных. Как вы увидите, тем не менее, можно использовать `QueryExtender` элемента управления, чтобы свести к минимуму эти различия.

Вам предстоит изменить *Students.aspx* страницы для фильтрации для студентов, сортировка по имени и поиск по имени. Вы также измените *Courses.aspx* страницы для отображения курсов для выбранного отдела и найдите курсы по имени. Наконец, вы добавите статистики учащихся к *About.aspx* страницы.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>С помощью EntityDataSource «Where» свойства для фильтрации данных

Откройте *Students.aspx* страницу, созданную в предыдущем учебном курсе. В настоящее время `GridView` элемента управления на странице отображаются все имена из `People` набора сущностей. Тем не менее, вы хотите показать только учащихся, который можно найти, выбрав `Person` сущностей, которые имеют дат зачисления отличное от null.

Переключиться в режим **разработки** просматривать и выбирать `EntityDataSource` элемента управления. В **свойства** окне `Where` свойства `it.EnrollmentDate is not null`.

[!["Image01"](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

Использовать в синтаксис `Where` свойство `EntityDataSource` элемент управления является Entity SQL. Язык Entity SQL аналогична Transact-SQL, но он настраивается для использования с сущностями, а не объекты базы данных. В выражении `it.EnrollmentDate is not null`, слово `it` представляет ссылку на сущность, возвращенных запросом. Таким образом `it.EnrollmentDate` ссылается на `EnrollmentDate` свойство `Person` сущности, `EntityDataSource` управления возвращает.

Откройте страницу. В список учащихся теперь содержит только учащихся. (Нет ни одной строки, где отображается дата регистрации не установлена.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>С помощью свойства «OrderBy» EntityDataSource для упорядочения данных

Требуется, этот список отсортирован по фамилиям, при первом отображении. С *Students.aspx* страницы, все еще открыт в **разработки** представление и с `EntityDataSource` по-прежнему выбран в элемент управления **свойства** задайте  **OrderBy** свойства `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Откройте страницу. В список учащихся теперь находится в порядке по фамилии.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>С помощью параметра управления установить свойство «Where»

Как с помощью других источников данных, можно передать значения параметров для `Where` свойство. На *Courses.aspx* странице, что вы создали в части 2 этого руководства, этот метод можно использовать для отображения курсов, которые связаны с отделу, который пользователь выбирает из раскрывающегося списка.

Откройте *Courses.aspx* и переключиться в режим **разработки** представления. Добавьте второй `EntityDataSource` к странице и назовите его `CoursesEntityDataSource`. Подключите его к `SchoolEntities` модели и выберите `Courses` как **EntitySetName** значение.

В **свойства** окно, нажмите кнопку с многоточием в **где** «свойство». (Убедитесь, что `CoursesEntityDataSource` сохраняя выбранным элемент управления перед использованием **свойства** окна.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

**Редактор выражений** диалоговое окно. В этом диалоговом окне выберите **Where создавалось автоматически выражения на основе предоставленного параметров**, а затем нажмите кнопку **добавить параметр**. Имя параметра `DepartmentID`выберите **управления** как **источник параметра** значение и выберите **DepartmentsDropDownList** как **ControlID**  значение.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Нажмите кнопку **Показать дополнительные свойства**и в **свойства** окно **редактор выражений** » диалогового окна «Изменение `Type` свойства `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Когда все будет готово, нажмите кнопку **ОК**.

Выберите в раскрывающемся списке, добавьте `GridView` на страницу элемент управления и назовите его `CoursesGridView`. Подключите его к `CoursesEntityDataSource` источника данных элемента управления, нажмите кнопку **обновить схему**, нажмите кнопку **Правка столбцов**и удалите `DepartmentID` столбца. `GridView` Разметка элемента управления аналогичный приведенному ниже.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Когда пользователь изменяет отдела, выбранного в раскрывающемся списке, необходим список связанных курсов для изменения автоматически. Для этого выберите стрелку раскрывающегося списка и в **свойства** задайте `AutoPostBack` свойства `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Теперь, когда вы закончите, с помощью конструктора, переключитесь в **источника** просмотра и замените `ConnectionString` и `DefaultContainer` называют свойства `CoursesEntityDataSource` управления `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` атрибута. Когда все будет готово, разметку для элемента управления будет выглядеть как в следующем примере.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Откройте страницу и используйте стрелку раскрывающегося списка для выбора различных отделов. Только курсы, предлагаемые выбранного отдела отображаются в `GridView` элемента управления.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>С помощью свойства «GroupBy» EntityDataSource для группирования данных

Предположим, что университета Contoso хочет поместить некоторую статистику текст учащегося на страницу About по его. В частности он хочет показать декомпозиция количество студентов по дате, когда они зарегистрированы.

Откройте *About.aspx*и в **источника** Просмотр, замените содержимое существующего файла `BodyContent` управления с «Статистики учащихся текст» между `h2` теги:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

После заголовка, добавить `EntityDataSource` управления и назовите его `StudentStatisticsEntityDataSource`. Подключите его к `SchoolEntities`выберите `People` набор сущностей и оставить **выберите** поле в мастере без изменений. Задайте следующие свойства **свойства** окна:

- Чтобы отфильтровать только для учащихся, установить `Where` свойства `it.EnrollmentDate is not null`.
- Чтобы сгруппировать результаты по дате зачисления, задайте `GroupBy` свойства `it.EnrollmentDate`.
- Чтобы выбрать дату регистрации и число учащихся, присвойте `Select` свойства `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Чтобы упорядочить результаты по дате зачисления, задайте `OrderBy` свойства `it.EnrollmentDate`.

В **источника** Просмотр, замените `ConnectionString` и `DefaultContainer` имя свойства с `ContextTypeName` свойство. `EntityDataSource` Разметка элемента управления, теперь, аналогичный приведенному ниже.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

Синтаксис `Select`, `GroupBy`, и `Where` свойства напоминает Transact-SQL, за исключением `it` ключевое слово, которое указывает текущую сущность.

Добавьте следующую разметку для создания `GridView` управления для отображения данных.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Откройте страницу для просмотра полного перечня, в которой отображается число учащихся по дате зачисления.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>С помощью элемента управления для фильтрации и сортировки

`QueryExtender` Элемент управления предоставляет способ указания фильтрации и сортировки в разметке. Синтаксис не зависит от системы управления базами данных (СУБД), который вы используете. Это также обычно зависит от платформы Entity Framework, за исключением, что синтаксис, используемый для свойств навигации является уникальным на платформу Entity Framework.

В этой части руководства вы будете использовать `QueryExtender` элемент управления для фильтрации и порядок данных и одно из полей предложение order by будет свойством навигации.

(Если вы предпочитаете использовать код вместо разметки для расширения запросы, которые автоматически создаются путем `EntityDataSource` элемента управления, это можно сделать обработки `QueryCreated` событий. Это как `QueryExtender` расширяет `EntityDataSource` также контролировать запросы.)

Откройте *Courses.aspx* странице и ниже разметку, добавленный ранее, вставьте следующую разметку, чтобы создать заголовок, текстовое поле для ввода строки поиска, кнопку поиска и `EntityDataSource` элемента управления, привязанного к `Courses` набора сущностей.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Обратите внимание, что `EntityDataSource` элемента управления `Include` свойству `Department`. В базе данных `Course` таблица не содержит название отдела; он содержит `DepartmentID` внешний ключевой столбец. Если запросы к базе данных напрямую, чтобы получить названия отдела вместе с данными курс, вам будет нужно присоединить `Course` и `Department` таблицы. Установив `Include` свойства `Department`, вы укажите, что платформа Entity Framework необходимо выполнить работу по началу связанный `Department` сущности, когда он получает `Course` сущности. `Department` Сущность сохраняется в `Department` свойство навигации `Course` сущности. (По умолчанию `SchoolEntities` класс, который был создан конструктором моделей данных получающего связанные данные, при необходимости, а элемент управления источником данных был присоединен к этому классу, поэтому установка `Include` свойство не требуется. Тем не менее, его установка повышает производительность страницы, так как в противном случае Entity Framework сделает отдельные вызовы к базе данных для получения данных для `Course` сущностей и для соответствующего `Department` сущностей.)

После `EntityDataSource` элемента управления, вы только что создали, вставьте следующую разметку для создания `QueryExtender` элемента управления, привязанного к этому `EntityDataSource` элемента управления.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

`SearchExpression` Элемент указывает, что вы хотите выбрать курсы, названия которых соответствуют значению, введенному в текстовом поле. Только по мере столько вводятся в текстовом поле символов будут сравниваться, так как `SearchType` указывает свойство `StartsWith`.

`OrderByExpression` Элемент указывает, что результирующий набор будет упорядочен по название курса в название отдела. Обратите внимание на то, как указывается название отдела: `Department.Name`. Так как связь между `Course` сущности и `Department` сущности взаимно-однозначное, `Department` свойство навигации содержит `Department` сущности. (Если бы это было отношение один ко многим, свойство будет содержать коллекция.) Чтобы получить названия отдела, необходимо указать `Name` свойство `Department` сущности.

Наконец, добавьте `GridView` управления для отображения списка курсов:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

Первый столбец имеет поле шаблона, которое отображает название отдела. Указывает выражение привязки данных `Department.Name`, так же, как видно из `QueryExtender` элемента управления.

Откройте страницу. Исходное отображение показывает список курсов, все в порядке по отделам, а затем по название курса.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Введите «m» и нажмите кнопку **поиска** Чтобы просмотреть все курсы, названия которых начинаются с буквы «m» (поиск является не с учетом регистра).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Использование оператора «Как» для фильтрации данных

Можно достичь эффекта, подобного действию `QueryExtender` элемента управления `StartsWith`, `Contains`, и `EndsWith` поиск типов с помощью `Like` оператор в `EntityDataSource` элемента управления `Where` свойство. В этой части руководства, вы увидите, как использовать `Like` оператора для поиска учащегося по имени.

Откройте *Students.aspx* в **источника** представления. После `GridView` управления, добавьте следующую разметку:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Эта разметка похоже на то, что вы уже видели ранее за исключением `Where` значение свойства. Во второй части `Where` выражение определяет поиск подстроки (`LIKE %FirstMidName% or LIKE %LastName%`), выполняет поиск первого и последнего имена для вводятся в текстовом поле.

Откройте страницу. Изначально отображаются все учащиеся, так как значение по умолчанию для `StudentName` равен «%».

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Введите букву «g» в текстовом поле и нажмите кнопку **поиска**. Появится список учащихся, которые имеют «g» в либо имени или фамилии.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Вы теперь отображаются, обновлены, отфильтрованы, упорядоченные и сгруппированные данные из отдельных таблиц. В следующем руководстве вы начнете работать со связанными данными (сценарии «основной-подробности»).

> [!div class="step-by-step"]
> [Назад](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Вперед](the-entity-framework-and-aspnet-getting-started-part-4.md)
