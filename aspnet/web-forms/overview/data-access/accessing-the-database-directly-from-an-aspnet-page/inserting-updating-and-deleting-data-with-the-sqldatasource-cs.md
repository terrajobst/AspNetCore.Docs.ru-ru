---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Вставка, обновление и удаление данных с помощью элемента управления SqlDataSource (C#) | Документация Майкрософт
author: rick-anderson
description: В предыдущих учебных курсах мы показали, как элемент управления ObjectDataSource, разрешенное для вставки, обновления и удаления данных. Элемент управления SqlDataSource поддерживает t...
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: d2958cb9cb0dd5ed89988a969663022a920a3dca
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812851"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Вставка, обновление и удаление данных с помощью элемента управления SqlDataSource (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) или [скачать PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> В предыдущих учебных курсах мы показали, как элемент управления ObjectDataSource, разрешенное для вставки, обновления и удаления данных. Элемента управления SqlDataSource поддерживает те же операции, но этот подход отличается учебнике демонстрируется настройка элемента управления SqlDataSource для вставки, обновления и удаления данных.


## <a name="introduction"></a>Вступление

Как уже говорилось в [Обзор для вставки, обновления и удаления](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), элемент управления GridView предоставляет встроенные обновление и удаление возможностей, хотя в элементы управления DetailsView и FormView включают в себя Вставка поддержку вместе с редактирование и удаление функциональные возможности. Эти возможности изменения данных можно подключать к элементу управления источником данных без написания кода, нужно записать. [Обзор для вставки, обновления и удаления](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) при просмотре с помощью элемента управления ObjectDataSource для облегчения вставки, обновления и удаления с элементами управления GridView, DetailsView и FormView. Кроме того элемента управления SqlDataSource может использоваться вместо элемента управления ObjectDataSource.

Помните, что для поддержки вставки, обновления и удаления, с помощью ObjectDataSource мы необходимые для указания методов уровня объекта для вызова для выполнения вставки, обновления или удаления действия. С помощью элемента управления SqlDataSource, необходимо предоставить `INSERT`, `UPDATE`, и `DELETE` SQL инструкции (или хранимые процедуры), для выполнения. Как мы увидим в этом руководстве, эти инструкции могут быть созданы вручную или автоматически создается с помощью мастера настройки источника данных элемента управления SqlDataSource s.

> [!NOTE]
> С момента мы ve описанию Вставка, редактирование и удаление возможностей элемента управления GridView, DetailsView и FormView управляет, этот учебник посвящен настройке элемента управления SqlDataSource для поддержки этих операций. Если вы хотите освежить по реализации этих функций в GridView, DetailsView и FormView, возврат к руководствам, редактирование, вставка и удаление данных, начиная с [Обзор для вставки, обновления и удаления](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Шаг 1: Указание`INSERT`,`UPDATE`, и`DELETE`инструкций

Как мы видели в двух предыдущих курсах, для получения данных из элемента управления SqlDataSource, нам нужно хранить задать два свойства:

1. `ConnectionString`, которое указывает, что базы данных для отправки запроса, и
2. `SelectCommand`, определяющий специальный оператор SQL или имя хранимой процедуры необходимо выполнить, чтобы вернуть результаты.

Для `SelectCommand` значения с параметрами, параметр значения задаются с помощью элемента управления SqlDataSource s `SelectParameters` коллекции и могут включать жестко запрограммированные значения источника значений общих параметров (поля строки запроса, переменные сеансов, значения элементов управления Web и т. д), или могут быть назначены программными методами. Если элемента управления SqlDataSource Управление s `Select()` метод вызывается из данных веб-элемента управления либо программно, либо автоматически установить подключение к базе данных, значения параметров назначаются в запрос и команда является создании База данных. Затем результаты возвращаются как DataSet или DataReader, в зависимости от значения элемента управления s `DataSourceMode` свойство.

При выборе данных элемента управления SqlDataSource может использоваться для вставки, обновления и удаления данных, указав `INSERT`, `UPDATE`, и `DELETE` инструкций SQL в таким же образом. Просто присваиваете `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства `INSERT`, `UPDATE`, и `DELETE` инструкций SQL для выполнения. Если операторы имеют параметры (как и всегда наиболее), включать их в `InsertParameters`, `UpdateParameters`, и `DeleteParameters` коллекций.

Один раз `InsertCommand`, `UpdateCommand`, или `DeleteCommand` задано значение, параметр Разрешить вставку, разрешить редактирование или разрешить удаление, в соответствующих данных смарт-теге элемента управления s Web станет доступным. Чтобы проиллюстрировать это, позволяющие s рассмотрим в качестве примера из `Querying.aspx` страницы, мы создали в [запрос данных с помощью элемента управления SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) учебник и расширить его для включения удалить возможности.

Сначала откройте `InsertUpdateDelete.aspx` и `Querying.aspx` страниц из `SqlDataSource` папки. Из конструктора, `Querying.aspx` выберите SqlDataSource и GridView из первого примера ( `ProductsDataSource` и `GridView1` элементы управления). После выбора двух элементов управления, перейдите к меню "Правка" и выберите команду Копировать (или просто нажать клавиши Ctrl + C). Перейдите в конструктор `InsertUpdateDelete.aspx` и вставьте в элементах управления. После перемещения двух элементов управления для `InsertUpdateDelete.aspx`, протестировать страницу в браузере. Вы должны увидеть значения `ProductID`, `ProductName`, и `UnitPrice` столбцы для всех записей в `Products` таблицы базы данных.


[![Все продукты в списке, упорядоченные по значению ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Рис. 1**: все продукты в списке, упорядоченные по `ProductID` ([Просмотр полноразмерного изображения](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Добавление элемента управления SqlDataSource s`DeleteCommand`и`DeleteParameters`свойства

На этом этапе у нас есть SqlDataSource, который просто возвращает все записи из `Products` таблицы и GridView, отображающий эти данные. Наша цель — расширить этот пример, чтобы разрешить пользователям удалять продукты с помощью GridView. Для этого нам нужно указать значения для элемента управления SqlDataSource s `DeleteCommand` и `DeleteParameters` свойства, а затем настройте GridView для поддержки удаления.

`DeleteCommand` И `DeleteParameters` свойства могут быть указаны в следующих случаях:

- Через декларативный синтаксис
- Из окна свойств в конструкторе
- Указать пользовательские инструкции SQL или хранимой процедуры экране мастера настройки источников данных
- С помощью «Дополнительно» укажите столбцы из таблицы экран просмотра в мастере настройки источника данных, который фактически автоматически создаст `DELETE` SQL и параметров инструкции коллекции, используемой в `DeleteCommand` и `DeleteParameters` Свойства

Мы изучим способ автоматически включать `DELETE` оператора, созданного на шаге 2. А теперь позвольте s используйте окно свойств в конструкторе, несмотря на то, что мастер настройки источника данных или параметр декларативный синтаксис будет работать точно так же.

Из конструктора в `InsertUpdateDelete.aspx`, щелкните `ProductsDataSource` SqlDataSource и затем откройте окно свойств (из меню «Вид» выберите окно "Свойства", или просто нажмите клавишу F4). Выберите свойство DeleteQuery, чтобы открыть набор многоточие.


![Выберите свойство DeleteQuery из окна свойств](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Рис. 2**: выберите свойство DeleteQuery из окна свойств


> [!NOTE]
> T SqlDataSource имеют свойство DeleteQuery. Вместо этого DeleteQuery представляет собой сочетание `DeleteCommand` и `DeleteParameters` свойства и перечисляется, только в окне «Свойства» при просмотре окна в конструкторе. Если вам нужны в окне свойств в представлении источника, вы найдете `DeleteCommand` свойство вместо этого.


Щелкните многоточие в свойстве DeleteQuery для открытия диалогового окна Редактор команд и параметров поле (см. рис. 3). В этом диалоговом окне можно указать `DELETE` инструкции SQL и укажите параметры. Введите следующий запрос в `DELETE` команда textbox (либо вручную или с помощью построителя запросов, если вы предпочитаете):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Затем нажмите кнопку Обновить параметры, чтобы добавить `@ProductID` параметр к списку параметров ниже.


[![Выберите свойство DeleteQuery из окна свойств](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Рис. 3**: выберите свойство DeleteQuery из окна свойств ([Просмотр полноразмерного изображения](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Сделать *не* предоставить значение для этого параметра (оставить ее параметр источник в None). Когда мы добавляем поддержки удаления к элементу GridView, GridView будет автоматически предоставлять значение этого параметра, используя значение его `DataKeys` коллекции, для которого была нажата для удаления строки.

> [!NOTE]
> Имя параметра, используемое в `DELETE` запроса *необходимо* совпадать со значением имя `DataKeyNames` значение в GridView, DetailsView и FormView. То есть параметр в `DELETE` инструкции намеренно называется `@ProductID` (вместо, например, `@ID`), так как является именем первичного ключевого столбца в таблице Products (и, следовательно, как значение DataKeyNames в GridView) `ProductID`.


Если имя параметра и `DataKeyNames` значения совпадают, GridView не удается назначить автоматически параметр значение из `DataKeys` коллекции.

После ввода команды относящиеся в диалоговом окне Редактор команд и параметров нажмите кнопку ОК, а затем перейдите к представлению источника для изучения итоговый декларативной разметки:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Обратите внимание, добавление `DeleteCommand` свойства, а также `<DeleteParameters>` раздел и параметр объекта с именем `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Настройка GridView для удаления

С помощью `DeleteCommand` добавить свойство, смарт-тега GridView s теперь содержит параметр Разрешить удаление. Продолжим и установите этот флажок. Как уже говорилось в [Обзор для вставки, обновления и удаления](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), это приводит к GridView добавить поле CommandField с его `ShowDeleteButton` свойство значение `true`. Рис. 4 показано при просмотре через обозреватель включается кнопку «Удалить». Проверьте эту страницу, удалив некоторые продукты.


[![Каждая строка GridView теперь включает кнопку «Удалить»](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Рис. 4**: каждая строка GridView теперь включает кнопки Delete ([Просмотр полноразмерного изображения](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


При нажатии кнопки "Удалить", происходит обратная передача, назначает GridView `ProductID` параметр значением из `DataKeys` коллекции значение для строки, кнопки "Удалить" была нажата и вызывает SqlDataSource s `Delete()` метод. Элемента управления SqlDataSource затем подключается к базе данных и выполняет `DELETE` инструкции. GridView затем выполняет повторную привязку для элемента управления SqlDataSource, получение и отображение текущего набора продуктов (который больше не включает только удаленные записи).

> [!NOTE]
> Так как использует GridView его `DataKeys` коллекцию для заполнения параметров SqlDataSource, он s жизненно важных, GridView s `DataKeyNames` свойства указать столбцы, которые составляют первичный ключ и что SqlDataSource s `SelectCommand` возвращает Эти столбцы. Кроме того он s, важно, что имя параметра в SqlDataSource s `DeleteCommand` присваивается `@ProductID`. Если `DataKeyNames` свойство не задано или не имеет параметра `@ProductsID`, нажав кнопку «Удалить» вызовет обратную передачу, но реализованных t фактически удалять любые записи.


Рис. 5 графически показано это взаимодействие. Вернуться к [Проверка события, связанные с вставки, обновления и удаления](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) учебник по более подробный рассказ о найти цепочку событий, связанных с Вставка, обновление и удаление из данных веб-элемента управления.


![Нажатие кнопки удаления в GridView вызывает метод s Delete() элемента управления SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Рис. 5**: нажатие кнопки удаления в GridView вызывает SqlDataSource s `Delete()` метод


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Шаг 2: Автоматическое создание`INSERT`,`UPDATE`, и`DELETE`инструкций

Как проверить, шаг 1 `INSERT`, `UPDATE`, и `DELETE` инструкций SQL можно указать с помощью окна свойств или s декларативный синтаксис элемента управления. Тем не менее этот подход требует, что мы вручную записать инструкций SQL вручную, который может быть монотонную и подверженной ошибкам. К счастью, мастер настройки источников данных предоставляет параметр, чтобы `INSERT`, `UPDATE`, и `DELETE` инструкций, автоматически создается при использовании укажите столбцы из таблицы экран просмотра.

Позволяют изучить эту возможность автоматического создания s. Добавьте в конструктор в элементе управления DetailsView `InsertUpdateDelete.aspx` и задайте его `ID` свойства `ManageProducts`. Затем выберите смарт-теге DetailsView s создать новый источник данных и создание элемента управления SqlDataSource с именем `ManageProductsDataSource`.


[![Создание нового элемента управления SqlDataSource с именем ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Рис. 6**: Создание нового элемента управления SqlDataSource с именем `ManageProductsDataSource` ([Просмотр полноразмерного изображения](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


С помощью мастера настройки источника данных, необязательно использовать `NORTHWINDConnectionString` подключения строку и нажмите кнопку Далее. От настройки на экране инструкции Select, оставьте укажите столбцы из таблицы или представления переключатель выбран и выбрать `Products` таблицу из раскрывающегося списка. Выберите `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` столбцы из списка флажок.


[![Используя таблицы Products, возвратить ProductID, ProductName, UnitPrice и неподдерживаемые столбцы](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Рис. 7**: с помощью `Products` таблицы, возвращаемые `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` столбцы ([Просмотр полноразмерного изображения](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


Для автоматического создания `INSERT`, `UPDATE`, и `DELETE` операторов на основании выбранной таблицы и столбцы, нажмите кнопку "Дополнительно" и проверьте формирования `INSERT`, `UPDATE`, и `DELETE` флажок инструкций.


![Проверка инструкций флажок Создать INSERT, UPDATE и DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Рис. 8**: Проверьте формирования `INSERT`, `UPDATE`, и `DELETE` инструкций флажок


Создание `INSERT`, `UPDATE`, и `DELETE` инструкций флажок будет только можно отметить флажком Если выбранную таблицу имеет первичный ключ и первичный ключевой столбец (или столбцы) включены в список возвращаемых столбцов. Флажок использования оптимистичного параллелизма, который становится доступным для выбора после формирования `INSERT`, `UPDATE`, и `DELETE` инструкций флажок, будут дополнять `WHERE` предложений в итоговом `UPDATE` и `DELETE` инструкции, чтобы предоставить управление оптимистичным параллелизмом. Пока оставьте этот флажок снят флажок; в следующем учебном курсе мы рассмотрим оптимистического параллелизма с помощью элемента управления SqlDataSource.

После проверки формирования `INSERT`, `UPDATE`, и `DELETE` флажок инструкций, нажмите кнопку ОК, чтобы вернуться на экран Настройка инструкции Select, а затем нажмите кнопку Далее и завершите работу, чтобы завершить работу мастера настройки источника данных. После завершения работы мастера, Visual Studio добавит поля BoundField, кроме к элементу управления DetailsView для `ProductID`, `ProductName`, и `UnitPrice` столбцы и по полю CheckBoxField для `Discontinued` столбца. Смарт-теге DetailsView s установите флажок Enable Paging таким образом, чтобы пользователь, посетив эту страницу можно пошагово выполнить продукты. Также очистить DetailsView s `Width` и `Height` свойства.

Обратите внимание на то, что смарт-тег имеет Разрешить вставку и разрешить редактирование Разрешить удаление параметров. Это обусловлено элемента управления SqlDataSource содержит значения для его `InsertCommand`, `UpdateCommand`, и `DeleteCommand`, как показано в следующем декларативном синтаксисе:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Обратите внимание на то, как элемент управления SqlDataSource имел значения, автоматически устанавливается для его `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства. Набор столбцов, на которые ссылается `InsertCommand` и `UpdateCommand` свойства зависят от числа содержащихся в `SELECT` инструкции. То есть вместо того *каждые* столбец продуктов в `InsertCommand` и `UpdateCommand`, существуют только те столбцы, указанные в `SelectCommand` (меньше `ProductID`, которой опущен, поскольку его s [ `IDENTITY` столбец](http://www.sqlteam.com/item.asp?ItemID=102), значение которого не может изменяться при редактировании и автоматически назначенной при вставке). Кроме того, для каждого параметра в `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства, соответствующие параметры в `InsertParameters`, `UpdateParameters`, и `DeleteParameters` коллекций.

Чтобы включить возможности изменения данных s DetailsView, проверьте Разрешить вставку, разрешить изменение и разрешить удаление параметров в его смарт-тега. Это поле добавляет CommandField с его `ShowInsertButton`, `ShowEditButton`, и `ShowDeleteButton` значение свойства `true`.

Посетите страницу в браузере и запишите изменения, удаления и новые кнопки, включенных в DetailsView. Нажав кнопку "Изменить" включает DetailsView в режим редактирования, который отображает каждый BoundField которого `ReadOnly` свойству `false` (по умолчанию) как TextBox и CheckBoxField как флажок.


[![DetailsView s по умолчанию интерфейс редактирования](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Рис. 9**: s DetailsView по умолчанию интерфейс правки ([Просмотр полноразмерного изображения](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


Аналогичным образом можно удалить текущий выбранный продукт или добавления нового продукта в систему. Так как `InsertCommand` оператор работает только с `ProductName`, `UnitPrice`, и `Discontinued` столбцы, либо имеют другие столбцы `NULL` или значения по умолчанию, присваивается базой данных после инструкции insert. Так же, как с помощью ObjectDataSource, если `InsertCommand` отсутствует любой таблицы базы данных, столбцы, где t позволяют `NULL` s и Дон t имеет значения по умолчанию, в SQL возникает ошибка при попытке выполнить `INSERT` инструкции.

> [!NOTE]
> S DetailsView, вставка и редактирование интерфейсы не хватает каких-либо настройки или проверки. Добавление элементов управления проверки или настроить интерфейсы, необходимо преобразовать полей BoundField в поля TemplateField. Ссылаться на [Добавление проверяющих элементов управления для редактирования и вставки интерфейсы](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) и [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) учебники Дополнительные сведения.


Кроме того, следует помнить, что для обновления и удаления, DetailsView использует текущий продукт s `DataKey` значение, которое присутствует, только если `DataKeyNames` свойство настроено. Если изменение или удаление не действовать, убедитесь, что `DataKeyNames` свойству.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Ограничения автоматического создания инструкций SQL

С момента формирования `INSERT`, `UPDATE`, и `DELETE` инструкций параметр доступен только в случае, если выбор столбцов из таблицы, для более сложными запросами, вам придется написать собственный `INSERT`, `UPDATE`, и `DELETE` инструкции, как это делалось в шаге 1. Обычно SQL `SELECT` инструкции используют `JOIN` s, чтобы вернуть данные из одной или нескольких таблиц подстановки для отображения (например возвращению `Categories` таблицы s `CategoryName` когда отображение информации о продукте). В то же время мы может потребоваться разрешить пользователю изменение, обновления или вставки данных в таблицу core (`Products`, в данном случае).

Хотя `INSERT`, `UPDATE`, и `DELETE` операторы могут быть введены вручную, рассмотрим следующий совет для экономии времени. Изначально установки элемента управления SqlDataSource, таким образом, чтобы он извлекает данные только из `Products` таблицы. Используйте мастер настройки источника данных s укажите столбцы из таблицы или представления экрана таким образом, вы можете автоматически создавать `INSERT`, `UPDATE`, и `DELETE` инструкций. После завершения работы мастера, выберите для настройки SelectQuery из окна свойств (или, в качестве альтернативы, вернитесь в мастер настройки источника данных, но указать пользовательские инструкции SQL используйте или параметр хранимых процедур). Затем обновите `SELECT` инструкции для включения `JOIN` синтаксис. Этот метод предоставляет преимущества позволяют экономить время, автоматически создаваемых инструкций SQL и позволяет более функциональный `SELECT` инструкции.

Другое ограничение автоматического создания `INSERT`, `UPDATE`, и `DELETE` инструкций является то, что столбцы в `INSERT` и `UPDATE` инструкции основаны на столбцы, возвращаемые `SELECT` инструкции. Нам нужно обновлять или вставлять поля больше или меньше, тем не менее. Например, в примере из шага 2, мы можем получить иметь `UnitPrice` BoundField быть доступны только для чтения. В этом случае он t не может отображаться в `UpdateCommand`. Или можно задать значение поля, которое не встречается в GridView. Например, при добавлении новых записей требуется `QuantityPerUnit` значение TODO.

Если требуются такие настройки, необходимо сделать их вручную, либо через окно свойств, указать пользовательские инструкции SQL или параметр хранимых процедур в мастере, или с помощью декларативного синтаксиса.

> [!NOTE]
> При Добавление параметров, у которых нет соответствующих полей в данных веб-элемент управления, помните, что эти значения параметров будет необходимо назначить значения какие-либо действия. Эти значения могут быть: жестко непосредственно в `InsertCommand` или `UpdateCommand`; могут поступать из какого-либо предварительно определенные источника (строки запроса, состояние сеанса, веб-элементов управления на странице и т. д.); так и программными средствами, как мы видели в предыдущем учебном курсе.


## <a name="summary"></a>Сводка

В порядке для данных веб-элементов управления для использования их встроенные возможности вставки, редактирование и удаление возможностей элемент управления источником данных, которые они привязываются к должна обеспечивать такие функциональные возможности. Для элемента управления SqlDataSource, это означает, что `INSERT`, `UPDATE`, и `DELETE` инструкций SQL должны быть назначены `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства. Эти свойства и соответствующие параметры коллекции, можно добавить вручную или автоматически формировать с помощью мастера настройки источника данных. В этом учебнике мы рассмотрели оба способа.

Мы рассмотрели использование оптимистичного параллелизма с помощью ObjectDataSource в [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) руководства. Элемента управления SqlDataSource также поддерживает оптимистичный параллелизм. Как указано в шаге 2, при автоматическом создании `INSERT`, `UPDATE`, и `DELETE` инструкций, мастер предлагает вариант использования оптимистичного параллелизма. Как мы увидим в следующем учебном курсе, с помощью оптимистичного параллелизма с помощью элемента управления SqlDataSource изменяет `WHERE` предложения в `UPDATE` и `DELETE` инструкции, чтобы убедиться, что значения для других столбцов писали сценарии t, измененный с момента последнего данных отображаются на странице.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [Вперед](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
