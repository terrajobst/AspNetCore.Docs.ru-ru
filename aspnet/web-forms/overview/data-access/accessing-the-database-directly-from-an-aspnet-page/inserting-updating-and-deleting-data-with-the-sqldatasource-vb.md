---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: Вставка, обновление и удаление данных в SqlDataSource (VB) | Документы Microsoft
author: rick-anderson
description: В предыдущих уроках мы узнали, как элемент управления ObjectDataSource разрешен для вставки, обновления и удаления данных. Поддерживает элемент управления SqlDataSource t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 92d195c3e1e349cd82e0625cf9a6c5a82644b5db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>Вставка, обновление и удаление данных в SqlDataSource (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe) или [скачать PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> В предыдущих уроках мы узнали, как элемент управления ObjectDataSource разрешен для вставки, обновления и удаления данных. Элемент управления SqlDataSource поддерживает те же операции, но этот подход отличается и этого учебника показано, как настроить SqlDataSource для вставки, обновления и удаления данных.


## <a name="introduction"></a>Вступление

Как было сказано в [Обзор для вставки, обновления и удаления](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), элемент управления GridView предоставляет встроенные обновление и удаление возможности, пока существуют элементы управления DetailsView и FormView Вставка поддерживают вместе с Изменение и удаление функциональные возможности. Эти возможности модификации данных можно включить непосредственно в элемент управления источником данных без строки кода, которые требуется записать. [Обзор для вставки, обновления и удаления](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) просматривать при помощи ObjectDataSource для облегчения вставки, обновления и удаления с элементами управления GridView, DetailsView и FormView. Кроме того можно использовать SqlDataSource вместо ObjectDataSource.

Помните, что для поддержки вставки, обновления и удаления, с ObjectDataSource мы требуется указание методы уровня объекта для вызова для выполнения вставки, обновления или удаления действия. В SqlDataSource, требуется предоставить `INSERT`, `UPDATE`, и `DELETE` SQL инструкции (или хранимые процедуры), для выполнения. Как будет показано в этом учебнике, эти инструкции могут быть созданы вручную или автоматически создается с помощью мастера настройки источника данных SqlDataSource s.

> [!NOTE]
> Поскольку мы хранять описанные вставки, редактирования и удаления возможности GridView, DetailsView и управляет FormView, этот учебник основное внимание уделяется настройке элемента управления SqlDataSource для поддержки этих операций. Если вам нужно подтянуть свои навыки в реализации этих возможностей в GridView, DetailsView и FormView, возврат к учебникам, редактирование, вставка и удаление данных, начиная с [Обзор для вставки, обновления и удаления](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Шаг 1: Указание`INSERT`,`UPDATE`, и`DELETE`инструкций

Как мы хранять встречается за последние два учебника для получения данных из элемента управления SqlDataSource, нам нужно задать два свойства:

1. `ConnectionString`, которое указывает, что базы данных для отправки запроса, и
2. `SelectCommand`, определяющий нерегламентированные инструкции SQL или имя хранимой процедуры необходимо выполнить, чтобы вернуть результаты.

Для `SelectCommand` значения с параметрами, параметр заданы значения с помощью SqlDataSource s `SelectParameters` коллекции и может включать жестко закодированные значения исходного значения общих параметров (поля строки запроса, переменные сеанса значения элементов управления Web и т. д), так и программными средствами. Если SqlDataSource Управление s `Select()` метод вызывается из данных веб-элемента управления программными средствами или автоматически установить подключение к базе данных, назначенных значения параметров запроса и команда является создании База данных. Результаты затем возвращаются в виде набора данных или DataReader, в зависимости от значения элемента управления s `DataSourceMode` свойство.

Вместе с выбора данных, можно использовать элемент управления SqlDataSource для вставки, обновления и удаления данных, указав `INSERT`, `UPDATE`, и `DELETE` инструкции SQL в таким же образом. Просто назначить `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства `INSERT`, `UPDATE`, и `DELETE` инструкции SQL для выполнения. Если операторы имеют параметры (как и наиболее всегда), включите их в `InsertParameters`, `UpdateParameters`, и `DeleteParameters` коллекции.

Один раз `InsertCommand`, `UpdateCommand`, или `DeleteCommand` значение указано, параметр Разрешить вставку, разрешить изменение или удаление включить соответствующие данные смарт-тег элемента управления s Web скоро станет доступен. Чтобы проиллюстрировать это, рассмотрим позволяют s в качестве примера из `Querying.aspx` страницы, мы создали [запрос данных с помощью элемента управления SqlDataSource](querying-data-with-the-sqldatasource-control-vb.md) учебник и расширить для включения удалить возможности.

Сначала откройте `InsertUpdateDelete.aspx` и `Querying.aspx` страниц из `SqlDataSource` папки. Из конструктора, `Querying.aspx` выберите SqlDataSource и GridView из первого примера ( `ProductsDataSource` и `GridView1` элементы управления). После выбора двух элементов управления, перейдите в меню "Правка" и выберите команду Копировать (или просто нажмите клавиши Ctrl + C). Теперь перейдите в конструктор `InsertUpdateDelete.aspx` и вставьте в элементах управления. После перемещения двух элементов управления для `InsertUpdateDelete.aspx`, проверить страницы в браузере. Вы увидите значения `ProductID`, `ProductName`, и `UnitPrice` столбцы для всех записей в `Products` таблицы базы данных.


[![Все продукты в списке, упорядоченные по значению ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**Рис. 1**: всех продуктов в списке, упорядоченные по `ProductID` ([Просмотр полноразмерное изображение](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Добавление SqlDataSource s`DeleteCommand`и`DeleteParameters`свойства

На этом этапе у нас есть SqlDataSource, который просто возвращает все записи из `Products` таблицы и GridView, отображающий эти данные. Наша цель — Чтобы расширить этот пример, чтобы разрешить пользователю удалять продуктов через GridView. Для этого необходимо указать значения для элемента управления SqlDataSource s `DeleteCommand` и `DeleteParameters` свойства, а затем настройте GridView для поддержки удаления.

`DeleteCommand` И `DeleteParameters` свойства могут быть указаны в следующих случаях:

- Посредством декларативного синтаксиса
- В окне свойств в конструкторе
- Указать пользовательские инструкции SQL или хранимой процедуры экрана в мастере настройки источника данных
- Через «Дополнительно» укажите столбцы из таблицы экран просмотра в мастере настройки источника данных, который фактически автоматически создать `DELETE` SQL и параметров инструкции коллекцию, используемую в `DeleteCommand` и `DeleteParameters` Свойства

Мы рассмотрим, как можно автоматически получать `DELETE` инструкции, созданный на шаге 2. Теперь позволяют s используйте окно свойств в конструкторе, несмотря на то, что мастер настройки источника данных или параметр декларативный синтаксис будет работать точно так же.

В конструкторе в `InsertUpdateDelete.aspx`, щелкните на `ProductsDataSource` SqlDataSource и затем откройте окно свойств (из меню «Вид» выберите «свойства», или просто нажать клавишу F4). Выберите свойство DeleteQuery, чтобы открыть набор многоточие.


![Выберите свойство DeleteQuery из окна «Свойства»](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**На рисунке 2**: выберите свойство DeleteQuery из окна «Свойства»


> [!NOTE]
> T SqlDataSource имеют свойство DeleteQuery. Вместо этого DeleteQuery представляет собой сочетание `DeleteCommand` и `DeleteParameters` свойства и только перечисленные в окне «Свойства» при просмотре окна в конструкторе. Если вам нужны в окне «Свойства» в представлении источника, вы найдете `DeleteCommand` свойство вместо него.


Щелкните многоточие в свойстве DeleteQuery, чтобы открыть диалоговое окно Редактор команд и параметров поле (см. рис. 3). В этом окне можно указать `DELETE` инструкции SQL и укажите параметры. Введите следующий запрос в `DELETE` текстовое поле команду (либо вручную или с помощью построителя запросов, если вы предпочитаете):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

Затем нажмите кнопку Обновить параметры для добавления `@ProductID` параметра к списку ниже параметров.


[![Выберите свойство DeleteQuery из окна «Свойства»](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**Рис. 3**: выберите свойство DeleteQuery из окна «Свойства» ([Просмотр полноразмерное изображение](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))


Сделать *не* предоставить значение для этого параметра (оставьте его параметр источник в None). После удаления поддержки добавить GridView, GridView автоматически предоставит значение этого параметра, используя значение его `DataKeys` коллекции, для которого была нажата для удаления строки.

> [!NOTE]
> Имя параметра, используемое в `DELETE` запроса *должен* совпадает имя `DataKeyNames` значение в GridView, DetailsView и FormView. То есть параметр в `DELETE` инструкции намеренно называется `@ProductID` (вместо, скажем, `@ID`), так как имя столбца первичного ключа в таблице Products (и, таким образом, как значение DataKeyNames в GridView) `ProductID`.


Если имя параметра и `DataKeyNames` значения совпадают, GridView не автоматического назначения параметра значение из `DataKeys` коллекции.

После ввода команды удаления связанных в диалоговом окне Редактор команд и параметров нажмите кнопку ОК, а затем перейдите в представление источника для просмотра результирующего декларативная разметка:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

Обратите внимание, добавление `DeleteCommand` свойство а также `<DeleteParameters>` раздел и объект параметра с именем `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Настройка GridView для удаления

С `DeleteCommand` свойство добавлено смарт-тег s GridView теперь содержит параметр Разрешить удаление. Продолжим и установите этот флажок. Как было сказано в [Обзор для вставки, обновления и удаления](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), в результате GridView добавить CommandField с его `ShowDeleteButton` свойство `True`. Как при просмотре через браузер рис. 4 показано входит кнопку «Удалить». Проверьте эту страницу, удалив некоторые продукты.


[![Каждая строка GridView теперь включает кнопку «Удалить»](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**Рис. 4**: каждой строки GridView теперь отображается кнопка удаления ([Просмотр полноразмерное изображение](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))


После нажатия кнопки "Удалить" обратной передаче, назначает GridView `ProductID` значение параметра из `DataKeys` коллекции значение для строки была нажата, кнопка "Удалить" и вызывает SqlDataSource s `Delete()` метод. Элемент управления SqlDataSource подключается к базе данных и затем выполняет `DELETE` инструкции. Затем повторно GridView привязывает SqlDataSource, получение и отображение текущего набора продуктов (содержащий больше не просто удалить запись).

> [!NOTE]
> Поскольку GridView использует его `DataKeys` коллекцию для заполнения параметров SqlDataSource его s важных, GridView s `DataKeyNames` свойству присвоить значение столбцы, которые составляют первичный ключ и что SqlDataSource s `SelectCommand` возвращает Эти столбцы. Кроме того он s, важно, что имя параметра в SqlDataSource s `DeleteCommand` равно `@ProductID`. Если `DataKeyNames` свойство не задано или не имеет параметра `@ProductsID`, нажав кнопку «Удалить» приведет к обратной передачи, но реализованных t фактически удалить все записи.


Рис. 5 показан это взаимодействие графически. Обращаться к [проверки события, связанные с вставки, обновления и удаления](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) учебника более подробное рассмотрение в цепочку событий, связанных с вставки, обновления и удаления из данных веб-элемента управления.


![Нажав кнопку «Удалить» в GridView вызывает метод Delete() s SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**Рис. 5**: нажмите кнопку «Удалить» в GridView вызывает SqlDataSource s `Delete()` метод


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Шаг 2: Автоматическое создание`INSERT`,`UPDATE`, и`DELETE`инструкций

Как проверить, шаг 1 `INSERT`, `UPDATE`, и `DELETE` инструкции SQL можно указать с помощью окна «Свойства» или s декларативный синтаксис элемента управления. Однако этот подход требует, что мы вручную записать инструкции SQL вручную, который может быть монотонных и вероятность ошибок. К счастью, мастер настройки источника данных предоставляет возможность иметь `INSERT`, `UPDATE`, и `DELETE` инструкций, автоматически создается при использовании укажите столбцы из таблицы Просмотр экрана.

Позволяют просматривать этот параметр автоматического создания s. Добавить в конструктор в DetailsView `InsertUpdateDelete.aspx` и задайте его `ID` свойства `ManageProducts`. Затем в DetailsView s смарт-теге, выберите новый источник данных и создать SqlDataSource с именем `ManageProductsDataSource`.


[![Создать новый SqlDataSource с именем ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**Рис. 6**: создать новый именованный SqlDataSource `ManageProductsDataSource` ([Просмотр полноразмерное изображение](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))


С помощью мастера настройки источника данных, выбрать использование `NORTHWINDConnectionString` подключения строку и нажмите кнопку Далее. От настройки на экране инструкции Select, оставьте укажите столбцы из таблицы или представления переключателя выбран и выбрать `Products` таблицу из раскрывающегося списка. Выберите `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` столбцы из списка флажок.


[![С помощью таблицы Products, возвращают ProductID, ProductName, UnitPrice и неподдерживаемые столбцы](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**Рис. 7**: с помощью `Products` таблицы, возвращаемые `ProductID`, `ProductName`, `UnitPrice`, и `Discontinued` столбцы ([Просмотр полноразмерное изображение](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))


Для автоматического создания `INSERT`, `UPDATE`, и `DELETE` операторов на основании выбранной таблицы и столбцы, нажмите кнопку "Дополнительно" и проверьте формирования `INSERT`, `UPDATE`, и `DELETE` флажок инструкций.


![Проверка инструкций флажок Создать инструкции INSERT, UPDATE и DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**Рис. 8**: Проверьте формирования `INSERT`, `UPDATE`, и `DELETE` инструкций флажок


Создание `INSERT`, `UPDATE`, и `DELETE` флажок инструкций будет только проверяемым Если выбранную таблицу первичного ключа и столбец первичного ключа (или столбцы) включены в список возвращаемых столбцов. Флажок использования оптимистичного параллелизма, который становится доступным для выбора после формирования `INSERT`, `UPDATE`, и `DELETE` флажок инструкций будет проверен, будет дополнить `WHERE` предложений в результате `UPDATE` и `DELETE` инструкций, обеспечивая возможность управления оптимистичным параллелизмом. Пока оставьте этот флажок снят; в следующем уроке мы изучим оптимистичный параллелизм с помощью элемента управления SqlDataSource.

После проверки формирования `INSERT`, `UPDATE`, и `DELETE` флажки, инструкции, нажмите кнопку ОК, чтобы вернуться на экран Настройка инструкции Select, а затем нажмите кнопку Далее и затем завершите работу мастера настройки источника данных. После завершения работы мастера, Visual Studio добавит стояли DetailsView для `ProductID`, `ProductName`, и `UnitPrice` столбцы и CheckBoxField для `Discontinued` столбца. В смарт-теге s DetailsView установите флажок Включить разбиение на страницы, чтобы пользователь посещении этой страницы можно пошагово проходить продукты. Также очистить DetailsView s `Width` и `Height` свойства.

Обратите внимание, что смарт-тег Разрешить вставку, разрешить изменение и разрешить удаление варианты. Это обусловлено SqlDataSource содержит значения для его `InsertCommand`, `UpdateCommand`, и `DeleteCommand`, как показывает следующий синтаксис:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

Обратите внимание на то, как элемент управления SqlDataSource имел значения, автоматически устанавливается для его `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства. Набор столбцов, на которые ссылается `InsertCommand` и `UpdateCommand` свойства основаны на значениях в `SELECT` инструкции. Т. е вместо того, чтобы *каждый* столбца продуктов в `InsertCommand` и `UpdateCommand`, существуют только те столбцы, указанные в `SelectCommand` (меньше `ProductID`, который пропущен, так как его s [ `IDENTITY` столбца](http://www.sqlteam.com/item.asp?ItemID=102), значение которого не может изменяться при редактировании и который автоматически назначается при вставке). Кроме того, для каждого параметра в `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства, соответствующие параметры в `InsertParameters`, `UpdateParameters`, и `DeleteParameters` коллекции.

Чтобы включить функции изменения данных в DetailsView s, проверьте Разрешить вставку, разрешить изменение и разрешить удаление параметров в смарт-тег. Это добавляет CommandField с его `ShowInsertButton`, `ShowEditButton`, и `ShowDeleteButton` значение свойства `True`.

Перейдите на страницу в браузере и запишите изменения, удаления и новые кнопки, включенных в DetailsView. Нажав кнопку «Изменить» превращает DetailsView в режим редактирования, который отображает каждый BoundField которого `ReadOnly` свойству `False` (по умолчанию) как текстовое поле и CheckBoxField как флажок.


[![DetailsView s по умолчанию интерфейс редактирования](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**Рис. 9**: s DetailsView интерфейс редактирования по умолчанию ([Просмотр полноразмерное изображение](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))


Аналогичным образом можно удалить выбранных продуктов или добавить новый продукт в системе. Поскольку `InsertCommand` инструкции работает только с `ProductName`, `UnitPrice`, и `Discontinued` столбцы, либо имеют другие столбцы `NULL` или значения по умолчанию, присвоенный базы данных после инструкции insert. Так же, как с ObjectDataSource, если `InsertCommand` отсутствует любой базы данных таблицы, столбцы, где t позволяют `NULL` s и пункт не имеют значения по умолчанию, возникнет ошибка SQL при попытке выполнения `INSERT` инструкции.

> [!NOTE]
> DetailsView s, вставка и редактирование интерфейсы не хватает каких-либо настройки или проверки. Добавление элементов управления проверки или настроить интерфейсы, необходимо преобразовать стояли TemplateFields. Ссылаться на [Добавление проверяющих элементов управления для редактирования и вставка интерфейсов](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) и [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) учебники для получения дополнительной информации.


Кроме того, имейте в виду, что для обновления и удаления, DetailsView использует текущего продукта s `DataKey` значение, которое присутствует, только если `DataKeyNames` свойство настроено. Если изменение или удаление не действуют, убедитесь, что `DataKeyNames` свойству.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Ограничения автоматического создания инструкции SQL

С момента формирования `INSERT`, `UPDATE`, и `DELETE` инструкции параметр доступен только в случае, когда комплектации столбцы из таблицы, для более сложных запросов, вы должны написать собственный `INSERT`, `UPDATE`, и `DELETE` операторы, как мы делали на шаге 1. Как правило, SQL `SELECT` инструкции используют `JOIN` s, чтобы вернуть данные из одной или нескольких таблиц подстановки для отображения (например возвращения `Categories` таблицу s `CategoryName` когда Отображение сведений о продукте). В то же время, которые можно разрешить пользователю изменение, обновления или вставки данных в таблицу core (`Products`в данном случае).

Хотя `INSERT`, `UPDATE`, и `DELETE` операторы могут быть введены вручную, рассмотрим следующий совет для экономии времени. Изначально настроить SqlDataSource, чтобы он снова запрашивает данные только из `Products` таблицы. Используйте мастер настройки источника данных s укажите столбцы из таблицы или представления экрана, чтобы могли автоматически создавать `INSERT`, `UPDATE`, и `DELETE` инструкции. После завершения работы мастера, выберите для настройки SelectQuery из окна «Свойства» (или, кроме того, вернитесь в мастер настройки источника данных, но указать пользовательские инструкции SQL используйте или параметр хранимых процедур). Затем обновите `SELECT` инструкции для включения `JOIN` синтаксиса. Такой подход имеет преимущества экономии времени автоматически создаваемых инструкций SQL и обеспечивает более специализированного `SELECT` инструкции.

Другое ограничение автоматического создания `INSERT`, `UPDATE`, и `DELETE` инструкций является то, что столбцы в `INSERT` и `UPDATE` инструкции основаны на столбцы, возвращенные `SELECT` инструкции. Нам нужно обновить или вставить большее или меньшее количество полей, однако. Например, в примере из шага 2, может быть нужно иметь `UnitPrice` BoundField доступно только для чтения. В этом случае она не старее отображаемой в `UpdateCommand`. Или нужно задать значение поля таблицы, которая не отображается в GridView. Например, при добавлении новой записи мы может потребоваться `QuantityPerUnit` значение TODO.

Если такие изменения не требуются, необходимо сделать их вручную, либо через окно свойств указать пользовательские инструкции SQL или параметр хранимых процедур в мастере или с помощью декларативного синтаксиса.

> [!NOTE]
> При добавлении параметров, у которых нет соответствующего поля данных веб-элемента управления, помнить, что значения этих параметров необходимо присваивать значения некоторым способом. Эти значения могут быть: жестко непосредственно в `InsertCommand` или `UpdateCommand`; могут поступать из предварительно определенного источника (строки запроса, состояние сеанса, веб-элементы управления на странице и т. д); так и программно, как было показано в предыдущем учебнике.


## <a name="summary"></a>Сводка

В порядке для данных, веб-элементы управления для использования их встроенные возможности вставки, редактирование и удаление возможности элемента управления источником данных, которые они привязаны к должно предоставлять такие функциональные возможности. Для SqlDataSource, это означает, что `INSERT`, `UPDATE`, и `DELETE` инструкции SQL должен быть назначен `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства. Эти свойства и коллекции соответствующих параметров можно добавлять вручную или автоматически формировать с помощью мастера настройки источника данных. В этом учебнике мы рассмотрели обоих методов.

Мы рассмотрели, опираясь на оптимистичный параллелизм с ObjectDataSource в [реализации оптимистичного параллелизма](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) учебника. Элемент управления SqlDataSource также поддерживает оптимистичный параллелизм. Как указано в шаге 2, при автоматическом создании `INSERT`, `UPDATE`, и `DELETE` инструкций, мастер предлагает вариант использования оптимистичного параллелизма. Как будет показано в следующем уроке, опираясь на оптимистичный параллелизм с SqlDataSource изменяет `WHERE` предложения в `UPDATE` и `DELETE` инструкции, чтобы убедиться, что значения для других столбцов, не удалось t изменены с момента последнего данных отображаются на странице.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](using-parameterized-queries-with-the-sqldatasource-vb.md)
> [Вперед](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
