---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Добавление дополнительных DataTable столбцов (VB) | Документы Microsoft
author: rick-anderson
description: При создании типизированного набора данных с помощью мастера TableAdapter, соответствующий объект DataTable содержит столбцы, возвращенные запросом к основной базе данных. Но есть...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: b51089057ad1e14901cb09589534d6e575261c3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879053"
---
<a name="adding-additional-datatable-columns-vb"></a>Добавление дополнительных DataTable столбцов (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) или [скачать PDF](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> При создании типизированного набора данных с помощью мастера TableAdapter, соответствующий объект DataTable содержит столбцы, возвращенные запросом к основной базе данных. Однако бывают ситуации, когда необходимо DataTable для включения дополнительных столбцов. В этом учебнике показано, почему рекомендуется использовать хранимые процедуры в случае необходимости дополнительных столбцов таблицы данных.


## <a name="introduction"></a>Вступление

При добавлении адаптера таблицы к типизированным набором данных, соответствующей схеме s DataTable определяется s основного запроса адаптера таблицы. Например, если основной запрос возвращает данные поля *A*, *B*, и *C*, таблицы данных будет иметь три соответствующего столбца с именем *A*, *B*, и *C*. В дополнение к его основной запрос адаптера таблицы могут включать дополнительные запросы, которые возвращают, возможно, подмножества данных, на основе некоторых параметра. Например, в дополнение к `ProductsTableAdapter` s основной запрос, который возвращает сведения обо всех приложениях, он также содержит методы, такие как `GetProductsByCategoryID(categoryID)` и `GetProductByProductID(productID)`, при котором возвращаются сведения определенного продукта на основе предоставленного параметра.

Модель предоставления схеме s DataTable отражают основного запроса адаптера таблицы s работает также в том случае, если все методы TableAdapter s возвращает тот же или меньшее число полей данных, чем указано в основном запросе. Если метод адаптера таблицы должен возвращать дополнительных полей данных, мы должны разверните схема s DataTable соответствующим образом. В [главный/Detail, с помощью маркированного списка из главного записей с DataList сведения](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) учебника мы добавили метод `CategoriesTableAdapter` , которые вернули `CategoryID`, `CategoryName`, и `Description` поля данных, заданные в основной запрос плюс `NumberOfProducts`, дополнительное поле данных, количество продуктов, связанные с каждой категории в отчет. Мы вручную добавить новый столбец в `CategoriesDataTable` для захвата `NumberOfProducts` значение из этого метода новые поля данных.

Как было сказано в [передачи файлов](../working-with-binary-files/uploading-files-vb.md) учебника, отлично необходимо соблюдать осторожность с адаптеры таблиц TableAdapter, использование нерегламентированных инструкций SQL и имеют методы, поля данных не соответствуют основного запроса. Если повторно запускается мастер настройки адаптера таблицы, обновите все методы адаптера таблицы s, чтобы их список полей данных соответствует основного запроса. Следовательно любые методы со списками изменяемых столбцов вернуться к основной запрос s список столбцов и не возвращает ожидавшиеся данные. Эта проблема не возникает при использовании хранимых процедур.

В этом учебнике мы рассмотрим способы расширения схемы s DataTable для включения дополнительных столбцов. Из-за хрупкость адаптера таблицы, при использовании нерегламентированных инструкций SQL в этом учебнике мы будем использовать хранимые процедуры. Ссылаться на [создание новых хранимых процедур для типизированного набора данных s адаптеры таблиц TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) и [существующих хранимых процедур для типизированного набора данных s адаптеры таблиц TableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) учебники Дополнительные сведения о Настройка адаптера таблицы с помощью хранимых процедур.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Шаг 1: Добавление`PriceQuartile`столбца`ProductsDataTable`

В *создание новых хранимых процедур для типизированного набора данных s адаптеры таблиц TableAdapter* учебника мы создали типизированный набор данных, с именем `NorthwindWithSprocs`. Этот набор данных в настоящее время содержит два DataTables: `ProductsDataTable` и `EmployeesDataTable`. `ProductsTableAdapter` Имеет следующих трех способов:

- `GetProducts` -основной запрос, который возвращает все записи из `Products` таблицы
- `GetProductsByCategoryID(categoryID)` — Возвращает все продукты с указанным *categoryID*.
- `GetProductByProductID(productID)` — Возвращает определенный продукт с указанным *productID*.

Основной запрос и два дополнительных метода возвращают один и тот же набор полей данных, а именно все столбцы из `Products` таблицы. Существуют не коррелированные вложенные запросы или `JOIN` s извлечение связанных данных из `Categories` или `Suppliers` таблицы. Таким образом `ProductsDataTable` имеет соответствующего столбца для каждого поля в `Products` таблицы.

В этом учебнике позволяют s добавьте метод `ProductsTableAdapter` с именем `GetProductsWithPriceQuartile` , возвращающий все продукты. Помимо поля данных стандартные продукта `GetProductsWithPriceQuartile` также входят `PriceQuartile` поля данных, которое указывает, в какой квартиль попадают цена продукта s. Например, будет иметь те продукты, цены находятся в наиболее ресурсоемких 25% `PriceQuartile` значение 1, а те, цены, попадают в последние 25% будет иметь значение 4. Прежде чем мы беспокоиться о создании хранимой процедуры для возврата этих сведений, тем не менее, сначала необходимо обновить `ProductsDataTable` для включения столбца, содержащего `PriceQuartile` результатов при `GetProductsWithPriceQuartile` используется метод.

Откройте `NorthwindWithSprocs` набора данных и щелкните правой кнопкой мыши `ProductsDataTable`. Выберите команду "Добавить" в контекстном меню, а затем выбрать столбец.


[![Добавить новый столбец ProductsDataTable](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Рис. 1**: добавить новый столбец в `ProductsDataTable` ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image3.png))


Это будет добавлен новый столбец в таблицу DataTable с именем Column1 типа `System.String`. Необходимо обновить это имя столбца s PriceQuartile и его тип `System.Int32` , так как он будет использоваться для хранения числа от 1 до 4. Выберите столбец, добавленные в `ProductsDataTable` и в окне свойств задайте `Name` свойства PriceQuartile и `DataType` свойства `System.Int32`.


[![Задайте новое имя столбца s и свойства типа данных](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**На рисунке 2**: задайте новый столбец s `Name` и `DataType` свойства ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image6.png))


Как показано на рисунке 2, существуют дополнительные свойства, которые могут быть установлены, например ли значения в столбце должны быть уникальными, если столбец является столбцом с автоматическим приращением, ли базы данных `NULL` значения допускаются и т. д. Оставьте эти значения, значения по умолчанию.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Шаг 2: Создание`GetProductsWithPriceQuartile`метод

Теперь, когда `ProductsDataTable` был обновлен для включения `PriceQuartile` столбца, все готово для создания `GetProductsWithPriceQuartile` метод. Запустить, щелкнув и выбрав команду Добавить запрос в контекстном меню. Откроется мастер настройки запроса адаптера таблицы, который сначала запрашивает нам о том, является ли мы хотим использовать специализированные инструкции SQL или в новую или существующую хранимую процедуру. Поскольку мы не хотите t, но имеется хранимая процедура, возвращающая данные квартиль цены, позволяет разрешить адаптера таблицы для создания этой хранимой процедуры для нас s. Выберите параметр Создать новые хранимые процедуры и нажмите кнопку Далее.


[![Указать мастера TableAdapter, чтобы создать хранимую процедуру для нас](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Рис. 3**: поручить адаптера таблицы мастер для создания хранимой процедуры для нас ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image9.png))


В последующих экране, показанном на рис. 4 мастер задает нам какой тип запроса для добавления. Поскольку `GetProductsWithPriceQuartile` метод будет возвращать все столбцы и записи `Products` таблицы, выберите SELECT, возвращающая строки и нажмите кнопку Далее.


[![Наш запрос будет инструкция SELECT, возвращает несколько строк](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Рис. 4**: наш запрос будет `SELECT` инструкции, возвращает несколько строк ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image12.png))


Далее мы запрашивается `SELECT` запроса. В мастере введите следующий запрос:


[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

Приведенный выше запрос использует новый SQL Server 2005 s [ `NTILE` функция](https://msdn.microsoft.com/library/ms175126.aspx) для разделения результаты на четыре группы, где определяются группы `UnitPrice` значений, отсортированных в порядке убывания.

К сожалению, конструктор запросов не знает, как выполнить синтаксический анализ `OVER` ключевое слово и выводится сообщение об ошибке при анализе приведенный выше запрос. Таким образом введите приведенный выше запрос непосредственно в текстовое поле в мастере без использования построителя запросов.

> [!NOTE]
> Дополнительные сведения о s NTILE и SQL Server 2005 см. другие функции ранжирования [возврат ранжированные результаты с Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) и [раздел Ранжирующие функции](https://msdn.microsoft.com/library/ms189798.aspx) из [SQL 2005 электронной документации по Server](https://msdn.microsoft.com/library/ms189798.aspx).


После ввода `SELECT` запроса и нажмите кнопку Далее, мастер появляется запрос на указание имени для хранимой процедуры, она будет создана. Имя новой хранимой процедуры `Products_SelectWithPriceQuartile` и нажмите кнопку Далее.


[![Имя хранимой процедуры Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Рис. 5**: имя хранимой процедуры `Products_SelectWithPriceQuartile` ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image15.png))


Наконец мы предлагается имя методы адаптера таблицы. Оставьте оба заполнения таблицы данных и возвращать проверяется флажки DataTable и имя методы `FillWithPriceQuartile` и `GetProductsWithPriceQuartile`.


[![Методы TableAdapter s имя и нажмите кнопку Готово.](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Рис. 6**: имя s методы адаптера таблицы и нажмите "Готово" ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image18.png))


С `SELECT` указан запрос и хранимой процедуры и методы адаптера таблицы с именем, щелкните "Готово" Чтобы завершить работу мастера. На этом этапе может появиться предупреждение или два из мастера, о том, что `OVER` конструкция или оператор SQL не поддерживается. Эти предупреждения можно игнорировать.

По завершении работы мастера TableAdapter должен включать `FillWithPriceQuartile` и `GetProductsWithPriceQuartile` методы и базы данных должно включать хранимую процедуру с именем `Products_SelectWithPriceQuartile`. Теперь пора убедитесь, что TableAdapter на самом деле содержит этот новый метод и правильно добавлен хранимой процедуры в базе данных. При проверке базы данных, если хранимая процедура try правой кнопкой мыши папку хранимые процедуры и выбрав обновления не отображаются.


![Убедитесь, что в адаптер таблицы добавлен новый метод](adding-additional-datatable-columns-vb/_static/image19.png)

**Рис. 7**: Убедитесь, что в адаптер таблицы добавлен новый метод


[![Убедитесь, что база данных содержит Products_SelectWithPriceQuartile хранимой процедуры](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Рис. 8**: Убедитесь, что база данных содержит `Products_SelectWithPriceQuartile` хранимой процедуры ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image22.png))


> [!NOTE]
> Одним из преимуществ использования хранимых процедур вместо нерегламентированных инструкций SQL является, повторный запуск мастера настройки адаптера таблицы не будет изменять список столбцов для хранимых процедур. Проверьте, щелкнув правой кнопкой мыши в TableAdapter, вариант в контекстном меню для запуска мастера настройки и нажмите кнопку Готово, чтобы завершить его. Теперь перейдите к базе данных, просматривать `Products_SelectWithPriceQuartile` хранимой процедуры. Обратите внимание, что его список столбцов не был изменен. Мы использовались нерегламентированных инструкций SQL, повторный запуск мастера настройки адаптера таблицы будет возвращаются этот запрос s список столбцов, чтобы соответствовать списку столбцов основного запроса, тем самым убирая инструкцию NTILE из запросов, используемых `GetProductsWithPriceQuartile` метод.


При s уровня доступа к данным `GetProductsWithPriceQuartile` вызывается метод, адаптера таблицы выполняет `Products_SelectWithPriceQuartile` хранимой процедуры и добавляет строку в `ProductsDataTable` для каждой возвращаемой записи. Поля данных, возвращаемых хранимой процедурой сопоставляются с `ProductsDataTable` s столбцов. Так как `PriceQuartile` поля данных, возвращаемых из хранимой процедуры, его значение будет назначено `ProductsDataTable` s `PriceQuartile` столбца.

Для этих методов адаптера таблицы, чьи запросы не возвращают `PriceQuartile` поля данных `PriceQuartile` значение столбца s — значения, указанного в его `DefaultValue` свойство. Как показано на рисунке 2, это значение равно `DBNull`, значение по умолчанию. Если вы хотите использовать другое значение, просто установите `DefaultValue` свойство соответствующим образом. Просто убедитесь, что `DefaultValue` значение допустимо, если s `DataType` (т. е. `System.Int32` для `PriceQuartile` столбца).

На этом этапе мы выполнили необходимые действия для добавления дополнительного столбца к таблице данных. Чтобы убедиться, что работает этот дополнительный столбец, позволяют создавать страницы ASP.NET, которая отображает каждого s название продукта, цены и цены квартиль s. Перед этим, сначала необходимо обновить уровень бизнес-логики, чтобы включить метод, который вызывает DAL s `GetProductsWithPriceQuartile` метод. Мы затем обновить МЕТОДА на шаге 3 и затем создайте страницу ASP.NET в шаге 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Шаг 3: Изменить уровень бизнес-логики

Прежде чем мы используем новый `GetProductsWithPriceQuartile` метод от уровня представления, мы должны сначала добавьте соответствующий метод для МЕТОДА. Откройте `ProductsBLLWithSprocs` и добавьте следующий код:


[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Как и другие методы получения данных в `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` метод просто вызывает DAL соответствующий s `GetProductsWithPriceQuartile` метод и возвращает его результаты.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Шаг 4: Отображение информации квартиль цены на веб-странице ASP.NET

С добавлением МЕТОДА завершения мы готов для создания страницы ASP.NET, которая показывает квартиль цены для каждого продукта. Откройте `AddingColumns.aspx` страницы в `AdvancedDAL` папки и перетащите элемент управления GridView с панели элементов в конструктор параметр его `ID` свойства `Products`. В смарт-теге s GridView, привязать его к новой ObjectDataSource с именем `ProductsDataSource`. Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класса s `GetProductsWithPriceQuartile` метод. Поскольку это будет только для чтения сетка, установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет).


[![Настройка ObjectDataSource с помощью класса ProductsBLLWithSprocs](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Рис. 9**: Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класса ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image25.png))


[![Получить сведения о продукте из метода GetProductsWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Рис. 10**: получить сведения о продукте из `GetProductsWithPriceQuartile` метод ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image28.png))


После завершения работы мастера настройки источника данных Visual Studio автоматически добавит BoundField или CheckBoxField GridView для каждого поля данных, возвращаемых методом. Одно из этих полей данных является `PriceQuartile`, мы добавили в столбец `ProductsDataTable` в шаге 1.

Изменить поля s GridView, удаляя все, кроме `ProductName`, `UnitPrice`, и `PriceQuartile` стояли. Настройка `UnitPrice` BoundField и отформатировать его значения в качестве валюты `UnitPrice` и `PriceQuartile` стояли вправо — и центру, соответственно. Наконец, обновление оставшихся стояли `HeaderText` свойства с продуктом, цена и цена квартиль, соответственно. Кроме того установите флажок Включить сортировку в смарт-теге s GridView.

После этих изменений GridView и ObjectDataSource s декларативная разметка должна выглядеть следующим образом:


[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

Рис. 11 показана эта страница при посещении через браузер. Обратите внимание, что изначально продукты упорядочены по цене, их в порядке убывания с каждого продукта, назначаются соответствующие `PriceQuartile` значение. Конечно эти данные могут быть отсортированы по другим критериям со значением столбца квартиль цены по-прежнему отражения ранжирования продукта s по отношению к цене (см. рис. 12).


[![Продукты в порядке их цены](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Рис. 11**: продукты упорядочены по их цены ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image31.png))


[![Продукты упорядочены по именам](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Рис. 12**: продукты упорядочены по их именам ([Просмотр полноразмерное изображение](adding-additional-datatable-columns-vb/_static/image34.png))


> [!NOTE]
> Несколько строк кода, мы может дополнять GridView, чтобы его цвет продукта строк на основе их `PriceQuartile` значение. Мы может этих продуктов в первый квартиль светло-зеленый цвет, указанные в второй квартиль Светло-желтый цвет и так далее. Я рекомендую занять некоторое время, чтобы добавить эту функциональность. Если вам требуется форматирование элемента управления GridView в памяти, обратитесь к [форматирование на основе по данным](../custom-formatting/custom-formatting-based-upon-data-vb.md) учебника.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Альтернативный подход — создание другого адаптера таблицы

Как было показано в этом учебнике, когда Добавление метода в TableAdapter, возвращающий полей данных, отличного от указано с основным запросом, можно добавить соответствующие столбцы в таблицу DataTable. Такой подход, однако является только в том случае, если существует небольшое количество методов в адаптер таблицы, которые возвращают различные поля данных и поля альтернативный данных не меняются слишком сильно из основного запроса.

Вместо добавления столбцов в таблицу DataTable, вместо этого можно добавить другой адаптер таблицы к набору данных, который содержит методы из первого адаптера таблицы, которые возвращают различные поля данных. В этом учебнике, а не добавлять `PriceQuartile` столбец `ProductsDataTable` (где он используется только `GetProductsWithPriceQuartile` метод), удалось добавлена дополнительных адаптера таблицы к набору данных с именем `ProductsWithPriceQuartileTableAdapter` , используемый `Products_SelectWithPriceQuartile` хранимых процедура, как его основным запросом. Использовать страниц ASP.NET, которые необходимы для получения сведения о продукте с квартиль цены `ProductsWithPriceQuartileTableAdapter`, а те, которые не удается продолжить использование `ProductsTableAdapter`.

Добавление нового адаптера таблицы, DataTables остаются untarnished и их столбцы точно отражают поля данных, возвращаемые методами s их адаптера таблицы. Тем не менее дополнительные адаптеры таблицы могут появиться повторяющиеся задачи и функции. Например, если эти страницы ASP.NET, которая отображается `PriceQuartile` столбца также требуется для предоставления вставки, обновления и удаления поддержки, `ProductsWithPriceQuartileTableAdapter` необходимо иметь его `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства должным образом настроить. Во время этих свойств может отражают `ProductsTableAdapter` s, эта конфигурация предоставляет дополнительный этап. Кроме того, существуют теперь два способа обновления, удаления или добавления продукта к базе данных — через `ProductsTableAdapter` и `ProductsWithPriceQuartileTableAdapter` классы.

В этом учебнике загружаемый файл содержит `ProductsWithPriceQuartileTableAdapter` в класс `NorthwindWithSprocs` набора данных, иллюстрирующий этот альтернативный подход.

## <a name="summary"></a>Сводка

В большинстве случаев все методы в адаптере таблицы возвращает тот же набор полей данных, но бывают случаи, когда конкретный метод или два может потребоваться вернуть дополнительное поле. Например, в [главный/Detail, с помощью маркированного списка из главного записей с DataList сведения](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) учебника мы добавили метод `CategoriesTableAdapter` , в дополнение к основным запросом s поля данных, возвращаемых `NumberOfProducts` полю, указывается число продукты, связанные с каждой категории. В этом учебнике мы рассмотрели Добавление метода в `ProductsTableAdapter` , которые вернули `PriceQuartile` поля в дополнение к основным запросом s поля данных. Для сбора дополнительных данных полей, возвращенных s методы адаптера таблицы, необходимо добавить соответствующие столбцы в таблицу DataTable.

Если планируется вручную добавлять столбцы в таблицу DataTable, рекомендуется использование хранимых процедур адаптера таблицы. Если адаптер таблицы использует инструкции SQL ad-hoc, любое время настройки адаптера таблицы запускается мастер, все методы, поля данных, возвращенной основным запросом восстановить списки полей данных. Эта проблема не распространяется на хранимые процедуры, поэтому их рекомендуется использовать и использовались в этом учебнике.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Рэнди Шмидт, Jacky Goor, Екатерина Leigh и – Хилтон Гизнау. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](updating-the-tableadapter-to-use-joins-vb.md)
> [Вперед](working-with-computed-columns-vb.md)
