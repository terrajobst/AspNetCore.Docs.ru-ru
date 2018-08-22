---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Добавление дополнительных столбцов DataTable (Visual Basic) | Документация Майкрософт
author: rick-anderson
description: При использовании мастера TableAdapter, чтобы создать типизированный набор DataSet, соответствующем объекте DataTable содержит столбцы, возвращаемые запросом к основной базе данных. Но здесь...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 75b5a1e1d6beb00079d754601860d0c25bc8a23e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829489"
---
<a name="adding-additional-datatable-columns-vb"></a>Добавление дополнительных столбцов DataTable (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) или [скачать PDF](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> При использовании мастера TableAdapter, чтобы создать типизированный набор DataSet, соответствующем объекте DataTable содержит столбцы, возвращаемые запросом к основной базе данных. Но бывают случаи, когда объект DataTable нужно включать дополнительные столбцы. В этом руководстве мы Узнайте, почему рекомендуется использовать хранимые процедуры когда нам требуется дополнительных столбцов DataTable.


## <a name="introduction"></a>Вступление

При добавлении адаптера таблицы TableAdapter к типизированному набору DataSet, соответствующая схема DataTable s определяется s основного запроса адаптера таблицы. Например, если основной запрос возвращает поля данных *объект*, *B*, и *C*, таблицы данных будет иметь три соответствующих столбцов с именем *объект*, *B*, и *C*. Помимо своего основного запроса адаптера таблицы могут включать дополнительные запросы, возвращающие, возможно, набор данных, созданный на основе некоторых параметров. Например, в дополнение к `ProductsTableAdapter` s основной запрос, возвращающий информацию обо всех продуктах, он также содержит методы, такие как `GetProductsByCategoryID(categoryID)` и `GetProductByProductID(productID)`, при котором возвращаются сведения конкретного продукта на основе предоставленного параметра.

Модель предоставления схемы таблицы данных s отражают основного запроса адаптера таблицы s работает также в том случае, если все методы TableAdapter s возвращает тот же или меньшим числом полей данных больше, чем указано в основном запросе. Если метод TableAdapter должен возвращать дополнительных полей данных, мы должны раскройте s схемы таблицы данных соответствующим образом. В [Master/Detail, с использованием маркированного списка основных записей с элементом управления DataList сведения](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) руководстве, мы добавили способ `CategoriesTableAdapter` которые вернули `CategoryID`, `CategoryName`, и `Description` полей данных, определенных в основной запрос плюс `NumberOfProducts`, дополнительные поля, сообщаемые количества продуктов, связанных с каждой категорией. Вручную, мы добавили новый столбец для `CategoriesDataTable` чтобы записать `NumberOfProducts` значение из этого нового метода поля данных.

Как уже говорилось в [загрузка файлов](../working-with-binary-files/uploading-files-vb.md) учебника, отлично Будьте осторожны с помощью адаптеров таблиц TableAdapter с помощью нерегламентированных инструкций SQL и имеет методы, поля которых данные не соответствуют основного запроса. Если повторно запускается мастер настройки адаптера таблицы, он обновит все методы TableAdapter s таким образом, чтобы их список полей данных соответствует основного запроса. Следовательно любые методы со списками изменяемых столбцов будет вернуться к основной запрос s список столбцов и не возвращает ожидаемые данные. Эта проблема не возникает при использовании хранимых процедур.

В этом руководстве мы рассмотрим способы расширения схемы s DataTable для включения дополнительных столбцов. Из-за хрупкости адаптера таблицы при использовании специализированные инструкции SQL в этом руководстве мы будем использовать хранимые процедуры. Ссылаться на [создание новых хранимых процедур для адаптеров таблиц TableAdapter типизированного набора DataSet s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) и [существующих хранимых процедур для адаптеров таблиц TableAdapter типизированного набора DataSet s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) учебники, Дополнительные сведения о Настройка использования TableAdapter для использования хранимых процедур.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Шаг 1: Добавление`PriceQuartile`столбец`ProductsDataTable`

В *создание новых хранимых процедур для адаптеров таблиц TableAdapter типизированного набора DataSet s* руководстве мы создали типизированный набор DataSet с именем `NorthwindWithSprocs`. Этот набор данных содержит двух таблиц DataTable: `ProductsDataTable` и `EmployeesDataTable`. `ProductsTableAdapter` Имеет следующие три метода:

- `GetProducts` — основной запрос, возвращающий все записи из `Products` таблицы
- `GetProductsByCategoryID(categoryID)` — Возвращает все продукты с указанным *categoryID*.
- `GetProductByProductID(productID)` — Возвращает конкретного продукта с указанным *productID*.

Основной запрос и два дополнительных метода возвращают одинаковый набор полей данных, а именно: все столбцы из `Products` таблицы. Существуют не коррелированные вложенные запросы или `JOIN` s, извлечение связанных данных из `Categories` или `Suppliers` таблиц. Таким образом `ProductsDataTable` которой соответствует столбец для каждого поля в `Products` таблицы.

Для этого руководства, позволяющие s Добавление метода для `ProductsTableAdapter` с именем `GetProductsWithPriceQuartile` , возвращающий все продукты. Помимо полей данных продукта стандартный `GetProductsWithPriceQuartile` также будет содержать `PriceQuartile` поля данных, которое указывает, в какой квартиль попадают цена продукта s. Например, будет иметь эти продукты, цены указаны в самых дорогих 25% `PriceQuartile` значение 1, а те, цены, попадают в последние 25% будет иметь значение 4. Прежде чем думать о создании хранимой процедуры для возврата этих сведений, тем не менее, необходимо сначала обновить `ProductsDataTable` для включения столбца, содержащего `PriceQuartile` результаты при `GetProductsWithPriceQuartile` используется метод.

Откройте `NorthwindWithSprocs` набора данных и щелкните правой кнопкой мыши `ProductsDataTable`. Выберите команду "Добавить" в контекстном меню, а затем выберите столбец.


[![Добавить новый столбец для ProductsDataTable](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Рис. 1**: добавьте новый столбец для `ProductsDataTable` ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image3.png))


Это будет добавлен новый столбец к таблице DataTable с именем Column1 типа `System.String`. Нам необходимо изменить это имя столбца s PriceQuartile и его тип на `System.Int32` так, как он будет использоваться для хранения числа от 1 до 4. Выберите столбец, добавленные в `ProductsDataTable` и в окне «Свойства» задайте `Name` свойства PriceQuartile и `DataType` свойства `System.Int32`.


[![Задайте имя нового столбца-s и свойств типа данных](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**Рис. 2**: задайте новый столбец s `Name` и `DataType` свойства ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image6.png))


Как показано на рис. 2, существуют дополнительные свойства, которые могут быть установлены, например ли значения в столбце должны быть уникальными, если столбец является столбцом с автоматическим приращением, ли база данных `NULL` значения допускаются и т. д. Оставьте эти значения, значения по умолчанию.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Шаг 2: Создание`GetProductsWithPriceQuartile`метод

Теперь, когда `ProductsDataTable` был обновлен для включения `PriceQuartile` столбца, мы готовы к созданию `GetProductsWithPriceQuartile` метод. Запустить, щелкнув правой кнопкой мыши в TableAdapter и выбрав добавить запрос в контекстном меню. Откроется мастер настройки запроса TableAdapter, который сначала запрашивает относительно хотим ли мы использовать специализированные инструкции SQL или новой или существующей хранимой процедуры. Поскольку мы кое t, но имеется хранимая процедура, возвращающая данные квартиль цена, позвольте s разрешить TableAdapter для создания этой хранимой процедуры для нас. Выберите параметр создания новой хранимой процедуры и нажмите кнопку Далее.


[![Дать указание мастеру TableAdapter для создания хранимой процедуры для нас](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Рис. 3**: дать указание мастеру TableAdapter для создания хранимой процедуры для США ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image9.png))


В последующих экране, показанном на рис. 4 мастер спрашивает нас, какой тип запроса для добавления. Так как `GetProductsWithPriceQuartile` метод будет возвращать все столбцы и записи `Products` таблицы, выберите SELECT, который возвращает строки и нажмите кнопку Далее.


[![Наш запрос будет инструкцию SELECT, возвращает несколько строк](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Рис. 4**: наш запрос будет `SELECT` оператор, возвращает несколько строк ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image12.png))


Далее мы будет предложено ввести `SELECT` запроса. В мастере, введите следующий запрос:


[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

Приведенный выше запрос использует новый SQL Server 2005 s [ `NTILE` функция](https://msdn.microsoft.com/library/ms175126.aspx) разбиение результатов на четыре группы, где определяются группы `UnitPrice` значения сортируются в нисходящем порядке.

К сожалению, конструктор запросов не знает, как выполнить синтаксический анализ `OVER` ключевое слово и отобразит ошибку при синтаксическом анализе приведенный выше запрос. Таким образом введите приведенный выше запрос непосредственно в текстовое поле в мастере без использования построителя запросов.

> [!NOTE]
> Дополнительные сведения о функции NTILE и SQL Server 2005 s других функциях ранжирования см. в разделе [возвращение ранжированные результаты с помощью Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) и [разделе Ранжирующие функции](https://msdn.microsoft.com/library/ms189798.aspx) из [SQL Документации по Server 2005](https://msdn.microsoft.com/library/ms189798.aspx).


После ввода `SELECT` запроса и нажмите кнопку Далее, мастер запрашивает нам предоставить имя хранимой процедуры, она будет создана. Присвойте имя новой хранимой процедуры `Products_SelectWithPriceQuartile` и нажмите кнопку Далее.


[![Имя хранимой процедуры Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Рис. 5**: имя хранимой процедуры `Products_SelectWithPriceQuartile` ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image15.png))


Наконец мы предложено назовите методы адаптера таблицы. Оставить оба заполнения таблицы данных и вернуться в DataTable флажков с новым именем методы `FillWithPriceQuartile` и `GetProductsWithPriceQuartile`.


[![Методы TableAdapter s имя и нажмите кнопку Готово.](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Рис. 6**: имя s методы адаптера таблицы и нажмите "Готово" ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image18.png))


С помощью `SELECT` указан запрос и хранимой процедуры и методы адаптера таблицы с именем, щелкните "Готово", чтобы завершить работу мастера. На этом этапе может появиться предупреждение или две из мастера, о том, что `OVER` конструкция или оператор SQL не поддерживается. Эти предупреждения можно игнорировать.

По завершении работы мастера TableAdapter должен включать `FillWithPriceQuartile` и `GetProductsWithPriceQuartile` методы и базы данных должен содержать хранимую процедуру с именем `Products_SelectWithPriceQuartile`. Отвлекитесь и убедитесь, что TableAdapter действительно содержит этот новый метод и что хранимая процедура правильно добавлен к базе данных. При проверке базы данных, если вы не видите хранимой процедуры try щелкните правой кнопкой мыши папку хранимые процедуры и выполнить операцию.


![Убедитесь, что в адаптер таблицы был добавлен новый метод](adding-additional-datatable-columns-vb/_static/image19.png)

**Рис. 7**: Убедитесь, что в адаптер таблицы был добавлен новый метод


[![Убедитесь, что база данных содержит Products_SelectWithPriceQuartile хранимой процедуры](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Рис. 8**: Убедитесь, что база данных содержит `Products_SelectWithPriceQuartile` хранимой процедуры ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image22.png))


> [!NOTE]
> Одним из преимуществ использования хранимых процедур вместо специализированные инструкции SQL — это что повторный запуск мастера настройки адаптера таблицы не будет изменять списки столбцов хранимых процедур. Проверьте, щелкнув правой кнопкой мыши в TableAdapter, выбрав в контекстном меню для запуска мастера настройку и затем нажмите кнопку Finish, чтобы завершить его. Теперь перейдите к базе данных и представление `Products_SelectWithPriceQuartile` хранимой процедуры. Обратите внимание на то, что его список столбцов не был изменен. Мы использовались специализированные инструкции SQL, повторно запустив мастер настройки TableAdapter будет существовавшему этот запрос s список столбцов для сопоставления со списком столбцов основного запроса, тем самым убирая инструкцию NTILE из запроса, используемого `GetProductsWithPriceQuartile` метод.


При s уровня доступа к данным `GetProductsWithPriceQuartile` вызова метода, выполняет TableAdapter `Products_SelectWithPriceQuartile` хранимой процедуры и добавляет строку к `ProductsDataTable` для каждой возвращаемой записи. Поля данных, возвращаемых хранимой процедурой, сопоставляются `ProductsDataTable` s столбцов. Так как `PriceQuartile` поля данных, возвращаемых хранимой процедурой, его значение будет назначено `ProductsDataTable` s `PriceQuartile` столбца.

Для этих методов TableAdapter, которого запросы не возвращают `PriceQuartile` поля данных, `PriceQuartile` значение столбца s равно значению, заданному в его `DefaultValue` свойство. Как показано на рис. 2, это значение присваивается `DBNull`, значение по умолчанию. Если вы предпочитаете другое значение, просто задайте `DefaultValue` свойство соответствующим образом. Просто убедитесь, что `DefaultValue` значение является допустимым учетом столбца s `DataType` (т. е. `System.Int32` для `PriceQuartile` столбца).

На этом этапе мы выполнили необходимые действия для добавления дополнительного столбца в таблицу данных. Чтобы убедиться, что этот дополнительный столбец работает должным образом, позволяют создавать страницы ASP.NET, которая отображает каждый s название продукта, цена и цена квартиль s. Перед этим, однако сначала необходимо обновить уровня бизнес-логики, чтобы включить метод, который вызывает DAL s `GetProductsWithPriceQuartile` метод. Мы затем обновить BLL на шаге 3 и затем созданию страницы ASP.NET на шаге 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Шаг 3: Расширения уровня бизнес-логики

Прежде чем мы будет использовать новый `GetProductsWithPriceQuartile` метод от слоя представления, мы должны сначала добавить соответствующий метод BLL. Откройте `ProductsBLLWithSprocs` и добавьте следующий код:


[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Как другие методы получения данных, которые в `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` метод просто вызывает DAL соответствующий s `GetProductsWithPriceQuartile` метод и возвращает его результаты.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Шаг 4: Отображение информации о квартиль цены на веб-странице ASP.NET

С добавлением BLL завершения мы будет готов создать страницу ASP.NET, показывающий квартиль цена для каждого продукта. Откройте `AddingColumns.aspx` странице в `AdvancedDAL` папки и перетащите элемент управления GridView с панели элементов в конструктор, установив его `ID` свойства `Products`. Смарт-теге GridView s, привязать его к элементу управления ObjectDataSource с именем `ProductsDataSource`. Настройка ObjectDataSource на использование `ProductsBLLWithSprocs` класс s `GetProductsWithPriceQuartile` метод. Так как это будет только для чтения сетка, установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет).


[![Настройка ObjectDataSource на использование класса ProductsBLLWithSprocs](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Рис. 9**: Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класс ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image25.png))


[![Получить сведения о продукте из метода GetProductsWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Рис. 10**: получить сведения о продукте из `GetProductsWithPriceQuartile` метод ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image28.png))


После завершения работы мастера настройки источников данных Visual Studio автоматически добавит BoundField или CheckBoxField GridView для каждого поля данных, возвращаемых методом. Одно из этих полей данных является `PriceQuartile`, который является столбцом, добавленным к `ProductsDataTable` на шаге 1.

Измените поля GridView s, удаляя все, кроме `ProductName`, `UnitPrice`, и `PriceQuartile` полей BoundField. Настройка `UnitPrice` BoundField для форматирования значения как денежной единицы и `UnitPrice` и `PriceQuartile` поля BoundFields — и center — по правому, соответственно. Наконец, обновите оставшихся полей BoundFields `HeaderText` свойства продукта, цены и цены квартиль, соответственно. Кроме того установите флажок Включить сортировку в смарт-теге GridView s.

После внесения этих изменений GridView и ObjectDataSource s декларативная разметка должна выглядеть следующим образом:


[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

Рис. 11 показана эта страница при посещении через браузер. Обратите внимание на то, что изначально продукты упорядочены по цене, их в порядке убывания с каждой назначен соответствующий продукт `PriceQuartile` значение. Само собой эти данные можно отсортировать по другим критериям со значением столбца квартиль цены, по-прежнему отражения ранжирования продукта s отношении цена (см. рис. 12).


[![Продукты сортируются по их цены](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Рис. 11**: продукты упорядочены по их цены ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image31.png))


[![Продукты упорядочены по именам](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Рис. 12**: продукты упорядочены по их именам ([Просмотр полноразмерного изображения](adding-additional-datatable-columns-vb/_static/image34.png))


> [!NOTE]
> Несколько строк кода, мы может дополнить GridView, чтобы он цветных строк продукта на основе их `PriceQuartile` значение. Мы может этих продуктов в первый квартиль светло-зеленый, во второй квартиль Светло-желтый цвет и так далее. Я призываю вас ознакомиться немного, чтобы добавить эту функцию. Если вы хотите освежить на форматирование элемента управления GridView, обратитесь к [форматирование на основе по данным](../custom-formatting/custom-formatting-based-upon-data-vb.md) руководства.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Альтернативный подход — создание другого адаптера таблицы

Как мы видели в этом руководстве описано, когда добавление методов к TableAdapter, возвращает поля данных, кроме тех, написаны с основного запроса, мы можем добавить соответствующие столбцы к таблице DataTable. Такой подход, тем не менее, хорошо работает только в том случае, если существует несколько методов в TableAdapter, которые возвращают различные поля данных и поля альтернативный данных не изменяются слишком многое из основного запроса.

Вместо добавления столбцов к таблице DataTable, вместо этого можно добавить другой адаптера таблицы к набору данных, который содержит методы из первого TableAdapter, которые возвращают различные поля данных. Для этого руководства вместо добавления `PriceQuartile` столбец `ProductsDataTable` (где он будет использоваться только службой `GetProductsWithPriceQuartile` метод), можно было добавить дополнительные адаптера таблицы к набору данных с именем `ProductsWithPriceQuartileTableAdapter` , используемый `Products_SelectWithPriceQuartile` хранятся процедура, в качестве его основного запроса. Использовать страницы ASP.NET, которые необходимы для получения сведения о продукте с квартиль цена `ProductsWithPriceQuartileTableAdapter`, а те, которые не удалось продолжить использование `ProductsTableAdapter`.

Добавление нового адаптера таблицы, DataTables остаются untarnished и их столбцов точно отражают поля данных, возвращаемые методами s их адаптера таблицы. Однако дополнительные адаптеры таблицы может стать причиной повторяющихся задач и функций. Например, если эти страницы ASP.NET, который отображен `PriceQuartile` столбца также требуется для предоставления вставки, обновления и удаления поддержки, `ProductsWithPriceQuartileTableAdapter` необходимо иметь его `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства должным образом настроить. Хотя эти свойства будут отражены `ProductsTableAdapter` s, эта конфигурация предоставляет дополнительное действие. Кроме того, теперь доступны два способа обновления, удаления или добавления продукта в базу данных - через `ProductsTableAdapter` и `ProductsWithPriceQuartileTableAdapter` классы.

В этом руководстве загружаемый файл содержит `ProductsWithPriceQuartileTableAdapter` в класс `NorthwindWithSprocs` набора данных, иллюстрирующая этот альтернативный подход.

## <a name="summary"></a>Сводка

В большинстве случаев все методы в адаптере таблицы возвращает тот же набор полей данных, но бывают случаи, когда конкретный метод или два может потребоваться возвратить дополнительное поле. Например, в [Master/Detail, с использованием маркированного списка основных записей с элементом управления DataList сведения](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) руководстве, мы добавили способ `CategoriesTableAdapter` , кроме основного запроса s полей данных, возвращаемых `NumberOfProducts` поле, указывается число продуктов, связанных с каждой категории. В этом учебнике мы рассмотрели Добавление метода в `ProductsTableAdapter` которые вернули `PriceQuartile` поля в дополнение к основным запросом s полей данных. Для сбора дополнительных данных поля возвращаемые методами s TableAdapter, что нам нужно добавить соответствующие столбцы к таблице DataTable.

Если планируется вручную добавления столбцов к таблице DataTable, рекомендуется использование хранимых процедур в TableAdapter. Если адаптер таблицы использует инструкции SQL ad-hoc, любое время настройки адаптера таблицы запускается мастер, все методы, поля данных, возвращенной основным запросом восстановить списки полей данных. Эта проблема не распространяется на хранимые процедуры, поэтому они являются рекомендуемым и использовались в этом руководстве.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Рэнди Шмидт, Джеки Goor, Leigh Екатерина и (Hilton giesenow), стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](updating-the-tableadapter-to-use-joins-vb.md)
> [Вперед](working-with-computed-columns-vb.md)
