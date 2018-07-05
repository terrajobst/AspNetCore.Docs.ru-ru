---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Использование существующих хранимых процедур для адаптеров таблиц TableAdapter типизированного DataSet (VB) | Документация Майкрософт
author: rick-anderson
description: В предыдущем учебном курсе мы показали, как использовать мастер TableAdapter для создания новых хранимых процедур. В этом руководстве мы Узнайте, как один адаптер таблицы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: a478b52387a6bbf726872978a0ad4a83c22c9911
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382900"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Использование существующих хранимых процедур для адаптеров таблиц TableAdapter типизированного DataSet (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) или [скачать PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> В предыдущем учебном курсе мы показали, как использовать мастер TableAdapter для создания новых хранимых процедур. В этом руководстве мы Узнайте, как один и тот же мастер TableAdapter можно работать с хранимым процедурам. Мы также узнайте, как вручную добавить новые хранимые процедуры в базе данных.


## <a name="introduction"></a>Вступление

В [предыдущем учебном курсе](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) мы видели, как s типизированный набор DataSet адаптеры таблицы могут быть настроены для использования хранимых процедур для доступа к данных, а не ad-hoc инструкций SQL. В частности мы увидели, как мастер TableAdapter, автоматически создал эти хранимые процедуры. При переносе устаревших приложений в ASP.NET 2.0 или при создании на веб-сайте ASP.NET 2.0 вокруг существующей модели анализа данных, скорее всего, что база данных уже содержит хранимые процедуры, которые нужны. Кроме того можно создать хранимые процедуры, вручную или с помощью какого-либо средства, кроме мастеру TableAdapter, который автоматически создает хранимые процедуры.

В этом руководстве мы рассмотрим способы настройки адаптера таблицы, чтобы использовать существующие хранимые процедуры. Так как базы данных Northwind есть только небольшой набор встроенных хранимых процедур, мы также рассмотрим шаги, которые необходимы, чтобы вручную добавить новые хранимые процедуры в базе данных с помощью среды Visual Studio. Позвольте s приступить к работе!

> [!NOTE]
> В [перенос изменений базы данных в транзакции](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) руководстве, мы добавили методы адаптера таблицы для поддержки транзакций (`BeginTransaction`, `CommitTransaction`, и так далее). Кроме того транзакции могут управляться целиком в пределах хранимой процедуры, которая не требует модификации кода уровня доступа к данным. В этом руководстве мы рассмотрим команды T-SQL, используемые для выполнения инструкций s хранимой процедуры в области транзакции.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Шаг 1: Добавление хранимых процедур в базе данных "Борей"

Visual Studio упрощает добавление новых хранимых процедур в базе данных. S позволяют добавить новую хранимую процедуру в базу данных Northwind, который возвращает все столбцы из `Products` таблицы для тех, которые имеют определенный `CategoryID` значение. В окне обозревателя серверов разверните базу данных "Борей" для отображения ее папок - диаграмм баз данных, таблиц, представлений и т. д. Как мы видели в предыдущем учебном курсе, папке Stored Procedures содержит базы данных s существующие хранимые процедуры. Чтобы добавить новую хранимую процедуру, щелкните правой кнопкой мыши папку хранимые процедуры и выберите в контекстном меню параметр Добавить новую хранимую процедуру.


[![Щелкните правой кнопкой мыши папку хранимых процедур и добавить новую хранимую процедуру](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Рис. 1**: щелкните правой кнопкой мыши папку хранимые процедуры и добавить новую хранимую процедуру ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Как показано на рис. 1, выбрав параметр Добавить новую хранимую процедуру открывает окно скрипта, в Visual Studio с контуром скрипт SQL, необходимые для создания хранимой процедуры. Он является нашей работой детализировать этот сценарий и выполните его, после чего хранимая процедура будет добавлена в базу данных.

Введите следующий скрипт:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Этот сценарий, при выполнении, добавит новую хранимую процедуру в базу данных Northwind с именем `Products_SelectByCategoryID`. Эта хранимая процедура принимает один входной параметр (`@CategoryID`, типа `int`) и возвращает все поля продуктов с соответствующим `CategoryID` значение.

Для выполнения этого `CREATE PROCEDURE` скрипта и добавление хранимой процедуры в базу данных, щелкните значок "Сохранить" на панели инструментов или нажмите сочетание клавиш Ctrl + S. После этого обновляет папку хранимых процедур, отображаются только что созданный хранимой процедуры. Кроме того, скрипт в окне изменится тонкость из `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` для `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Добавляет новую хранимую процедуру в базе данных, хотя `ALTER PROCEDURE` обновляет существующий. С момента начала скрипта изменилось на `ALTER PROCEDURE`, изменение хранимых процедур входные параметры или инструкций SQL и щелкнув значок "Сохранить" будет обновить хранимую процедуру с этими изменениями.

Рис. 2 показана система Visual Studio после `Products_SelectByCategoryID` хранимая процедура была сохранена.


[![Хранимая процедура Products_SelectByCategoryID была добавлена в базу данных](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Рис. 2**: хранимая процедура `Products_SelectByCategoryID` был добавлен в базу данных ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Шаг 2: Настройка использования TableAdapter для использования существующей хранимой процедуры

Теперь, когда `Products_SelectByCategoryID` хранимой процедуры был добавлен в базу данных, мы можем настроить слою доступа данных, чтобы использовать эту хранимую процедуру при вызове одного из его методов. В частности, мы добавим `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` метод `ProductsTableAdapter` в `NorthwindWithSprocs` типизированный набор DataSet, который вызывает `Products_SelectByCategoryID` хранимой процедуры, которые мы только что создали.

Сначала откройте `NorthwindWithSprocs` набора данных. Щелкните правой кнопкой мыши `ProductsTableAdapter` и выберите Добавить запрос, чтобы запустить мастер настройки запроса адаптера таблицы. В [предыдущем учебном курсе](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) мы выбрали, чтобы создать новую хранимую процедуру для нас TableAdapter. Для этого руководства, тем не менее, мы хотим привязать новый метод TableAdapter для существующего `Products_SelectByCategoryID` хранимой процедуры. Таким образом выберите использование существующей хранимой процедуры с первым шагом мастера s и нажмите кнопку Далее.


[![Выберите использование существующей хранимой процедуры, параметр](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Рис. 3**: выберите, использовать существующую хранимую процедуру параметр ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


Следующий экран предоставляет стрелку раскрывающегося списка, заполненные с базой данных s хранимых процедур. Выбрав хранимой процедуры список ее входных параметров в левой части и поля данных, возвращаемых (если таковые имеются) в правой части. Выберите `Products_SelectByCategoryID` хранимую процедуру из списка и нажмите кнопку Далее.


[![Выбрать Products_SelectByCategoryID хранимой процедуры](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Рис. 4**: выбрать `Products_SelectByCategoryID` хранимой процедуры ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


Далее спрашивает нас, какие данные возвращается хранимой процедурой и считать ответом здесь определяет тип, возвращаемый методом s TableAdapter. Например, если мы указываем, что табличные данные возвращаются, метод вернет `ProductsDataTable` экземпляр, заполненный записей, возвращаемых хранимой процедурой. Напротив, если мы указываем, что эта хранимая процедура возвращает одно значение TableAdapter вернет `Object` , присваивается значение в первом столбце первой записи, возвращаемые хранимой процедурой.

Так как `Products_SelectByCategoryID` хранимая процедура возвращает все продукты, которые принадлежат определенной категории, выберите первый ответ - табличные данные — и нажмите кнопку Далее.


[![Указывает, что хранимая процедура возвращает табличные данные](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Рис. 5**: Указывает, что хранимая процедура возвращает табличные данные ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Остается лишь для указания, что метод шаблоны для использования следуют имена для этих методов. Оставить оба свойства Fill DataTable и возвращают параметры DataTable флажок установлен, но его можно переименовать методы, которые `FillByCategoryID` и `GetProductsByCategoryID`. Нажмите кнопку Далее для просмотра сводки по задачи, которые выполнит мастер. Если все выглядит правильно, нажмите кнопку "Готово".


[![Имя FillByCategoryID методы и метода GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Рис. 6**: назовите методы `FillByCategoryID` и `GetProductsByCategoryID` ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> Методы TableAdapter, который мы только что создали, `FillByCategoryID` и `GetProductsByCategoryID`, необходим входной параметр типа `Integer`. Это значение входного параметра передается в хранимую процедуру с помощью его `@CategoryID` параметра. При изменении `Products_SelectByCategory` параметры хранимой процедуры s, необходимо также обновить параметры для этих методов TableAdapter. Как уже говорилось в [предыдущем учебном курсе](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), это можно сделать одним из двух способов: можно вручную добавлять или удалять параметры из коллекции parameters или путем повторного запуска мастер TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Шаг 3: Добавление`GetProductsByCategoryID(categoryID)`метода BLL

С помощью `GetProductsByCategoryID` метод DAL доделан, следующим шагом является предоставляют доступ к этому методу в уровне бизнес-логики. Откройте `ProductsBLLWithSprocs` и добавьте следующий метод:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Этот метод BLL просто возвращает `ProductsDataTable` возвращаемые `ProductsTableAdapter` s `GetProductsByCategoryID` метод. `DataObjectMethodAttribute` Атрибут содержит метаданные, используемые с помощью мастера настройки источника данных s ObjectDataSource. В частности этот метод будет отображаться в раскрывающемся списке ВЫБЕРИТЕ вкладку s.

## <a name="step-4-displaying-products-by-category"></a>Шаг 4: Отображение продуктов по категориям

Чтобы проверить только что добавленного `Products_SelectByCategoryID` хранимой процедуры и соответствующих методов DAL и BLL, позволяют создавать страницы ASP.NET, которая содержит DropDownList и GridView s. DropDownList будут перечислены все категории в базе данных, хотя GridView будет отображаться продуктов, принадлежащих выбранной категории.

> [!NOTE]
> Мы интерфейсы ve создан «основной/подробности» с помощью элементов управления DropDownList в предыдущих учебных курсах. Более углубленное рассмотрение реализации такого отчета «основной/подробности», см. в разделе ["основной/подробности" Фильтрация с помощью элемента управления DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) руководства.


Откройте `ExistingSprocs.aspx` странице в `AdvancedDAL` папки и перетащите DropDownList с панели инструментов в конструктор. Набор DropDownList s `ID` свойства `Categories` и его `AutoPostBack` свойства `True`. Затем из его смарт-тега привязки DropDownList новый ObjectDataSource, именуемый `CategoriesDataSource`. Настройте элемент управления ObjectDataSource, таким образом, чтобы он извлекает данные из `CategoriesBLL` класс s `GetCategories` метод. Установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет).


[![Получить данные из метода GetCategories класса CategoriesBLL s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Рис. 7**: получение данных из `CategoriesBLL` класс s `GetCategories` метод ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Рис. 8**: задать раскрывающиеся списки в обновления, вставки и удаления вкладок (нет) ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


После завершения работы мастера ObjectDataSource Настройка DropDownList для отображения `CategoryName` поля данных и использовать `CategoryID` как `Value` для каждого `ListItem`.

На этом этапе DropDownList и ObjectDataSource s декларативная разметка должен быть аналогичен следующему:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Затем перетащите элемент управления GridView в конструктор, поместив его ниже элемента управления DropDownList. Набор GridView s `ID` для `ProductsByCategory` и его смарт-теге, привязать его к элементу управления ObjectDataSource с именем `ProductsByCategoryDataSource`. Настройка `ProductsByCategoryDataSource` ObjectDataSource на использование `ProductsBLLWithSprocs` класса, его получить, его данные с помощью `GetProductsByCategoryID(categoryID)` метод. Так как этот GridView будет использоваться только для отображения данных, раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет) и нажмите кнопку Далее.


[![Настройка ObjectDataSource на использование класса ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Рис. 9**: Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класс ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Получить данные из метод GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Рис. 10**: получение данных из `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


Метод выбран на вкладке "ВЫБЕРИТЕ" ожидает параметр, чтобы последний шаг мастера запрашивает источник параметра s. Значение параметра исходного стрелку раскрывающегося списка элемента управления и выберите `Categories` элемента управления из раскрывающегося списка ControlID. Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Используйте категории DropDownList как источник параметра categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Рис. 11**: использование `Categories` DropDownList как источник `categoryID` параметра ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


После завершения работы мастера ObjectDataSource, Visual Studio добавит поля BoundFields и по полю CheckBoxField для каждого из полей данных продукта. Вы можете настроить эти поля, по своему усмотрению.

Посетите страницу через обозреватель. При посещении страницы выбранные категории «Напитки» и соответствующих продуктов, перечисленных в сетке. Изменение стрелку раскрывающегося списка на альтернативные категории, как на рис. 12 показано, вызывает обратную передачу и перезагружает сетки с продуктами для новой выбранной категории.


[![Отображаются продукты категории создания](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Рис. 12**: отображаются продукты категории создают ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Шаг 5: Упаковки s хранимой процедуры инструкций в пределах области транзакции

В [перенос изменений базы данных в транзакции](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) учебнике мы рассмотрели методы выполнения ряда инструкций изменения базы данных в области транзакции. Вспомним, что изменений, выполненных в рамках транзакции, либо все завершиться успешно или всех ошибок, что гарантировало неделимость. Методы для использования транзакций включают следующее:

- С помощью классов в `System.Transactions` пространства имен,
- Наличие доступа к данным используйте классы ADO.NET, например `SqlTransaction`, и
- Добавление команд транзакции T-SQL непосредственно в хранимой процедуре

*Перенос изменений базы данных в транзакции* руководстве используются классы ADO.NET в слое DAL. Оставшейся части этого руководства рассматривается управление транзакцию с помощью команды T-SQL из в хранимой процедуре.

Три ключевых команды SQL, запуск вручную, фиксация и откат транзакции, `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, и `ROLLBACK TRANSACTION`, соответственно. При использовании транзакций из в хранимой процедуре, нам нужно применить следующий шаблон, например при использовании ADO.NET:

1. Указывают на начало транзакции.
2. Выполнение инструкций SQL, которые составляют транзакцию.
3. Если возникает ошибка в любой из инструкций из шага 2, производится откат транзакции.
4. Если все инструкции из шага 2 завершается без ошибок, зафиксируйте транзакцию.

Этот шаблон может быть реализован в синтаксисе T-SQL, используя следующий шаблон:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Начинается шаблон, определив `TRY...CATCH` блокировать конструкцию, знакомы с SQL Server 2005. Как с помощью `Try...Catch` блоков в Visual Basic, SQL `TRY...CATCH` блок выполняет инструкции в `TRY` блока. Если любая инструкция вызывает ошибку, управление немедленно передается `CATCH` блока.

Если ошибок нет, выполнение инструкций SQL, состав транзакции, `COMMIT TRANSACTION` инструкция фиксирует изменения и завершает транзакцию. Если, однако одна из инструкций приводит к ошибке `ROLLBACK TRANSACTION` в `CATCH` блок возвращает базу данных в состояние до начала транзакции. Хранимая процедура вызывает ошибку с помощью [команду RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), чего `SqlException` вызываемого в приложении.

> [!NOTE]
> Так как `TRY...CATCH` блок является новой возможностью в SQL Server 2005, шаблон не будет работать, если вы используете более старых версиях Microsoft SQL Server. Если вы не используете SQL Server 2005, обратитесь к [управление транзакциями в хранимые процедуры SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) для шаблона, который будет работать с другими версиями SQL Server.


Позвольте s Рассмотрим конкретный пример. Существует ограничение внешнего ключа между `Categories` и `Products` таблиц, это означает, что каждый `CategoryID` в `Products` таблицы должно быть сопоставлено `CategoryID` значение в `Categories` таблицы. Любое действие, которое будет нарушать это ограничение, например, попытки удалить категорию, связанную продуктов, приводит к нарушению ограничения внешнего ключа. Чтобы проверить это, вернемся к примеру обновление и удаление существующих двоичных данных, при работе с разделе двоичные данные (`~/BinaryData/UpdatingAndDeleting.aspx`). На этой странице перечислены каждой категории в системе, а также кнопки изменения и удаления (см. рис. 13), но при попытке удалить категорию, связанную продуктов — таких как «Напитки» — операция удаления завершается ошибкой из-за нарушения ограничения внешнего ключа (см. рис. 14).


[![Каждая категория отображается в элементе управления GridView, изменить и удалить кнопки](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Рис. 13**: Каждая категория отображается в элементе управления GridView, изменить и удалить кнопок ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Не удается удалить категории с существующих продуктов](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Рис. 14**: не удается удалить категории с существующих продуктов ([Просмотр полноразмерного изображения](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Представьте себе, что нам нужно разрешить категории, чтобы удалить независимо от того, является ли они имеют связанные продукты. Удалить категорию с продуктами, предположим, нам необходимо также удалить свои существующие продукты (несмотря на то, что другой вариант, можно просто задать свои продукты для `CategoryID` значения `NULL`). Эта функция может быть реализован через правила cascade ограничения внешнего ключа. Кроме того, мы может создать хранимую процедуру, которая принимает `@CategoryID` входной параметр и, при вызове явным образом удаляются и все соответствующие продукты, а затем указанной категории.

Первая попытка хранимая процедура может выглядеть следующим образом:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Хотя это определенно будет удален, связанных с ними продуктов и категории, он не делает этого в рамках транзакции. Представьте, что на некоторые другие ограничения внешнего ключа `Categories` , будет запрещена областью удаление конкретной `@CategoryID` значение. Проблема в том, что в этом случае все эти продукты будут удалены перед предпринимается попытка удалить категорию. Конечным результатом является то, что для такой категории Эта хранимая процедура будет удалить все свои продукты хотя отображается как категории, так как он по-прежнему связанные записи в другой таблице.

Если хранимая процедура было заключены в пределах транзакции, тем не менее, удаление для `Products` таблицы был бы выполнен откат в случае не удалось удалить `Categories`. Следующий сценарий хранимая процедура использует транзакции для обеспечения атомарности между двумя `DELETE` инструкции:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Отвлекитесь и добавьте `Categories_Delete` хранимой процедуры в базе данных "Борей". Вернитесь к шагу 1 инструкции по добавлению хранимых процедур в базе данных.

## <a name="step-6-updating-thecategoriestableadapter"></a>Шаг 6: Обновление`CategoriesTableAdapter`

Хотя мы добавили `Categories_Delete` хранимой процедуры в базе данных, DAL в настоящий момент настроен на выполнение удаления с помощью инструкций SQL ad-hoc. Необходимо обновить `CategoriesTableAdapter` и настроить его для использования `Categories_Delete` вместо хранимой процедуры.

> [!NOTE]
> Ранее в этом учебнике мы работали `NorthwindWithSprocs` набора данных. Но, что набор данных содержит только одну сущность, `ProductsDataTable`, и нам нужно работать с категориями. Таким образом, для оставшейся части этого руководства, когда я рассказываю о ссылке на m уровень данных доступа I `Northwind` набора данных, тот, который мы впервые создали [создание уровня доступа к данным](../introduction/creating-a-data-access-layer-vb.md) руководства.


Откройте набор данных "Борей", выберите `CategoriesTableAdapter`и перейдите в окно свойств. Списки окно свойств `InsertCommand`, `UpdateCommand`, `DeleteCommand`, и `SelectCommand` используемые TableAdapter, а также его имя и сведения о соединении. Разверните `DeleteCommand` свойство, чтобы увидеть сведения о нем. Как показано на рис. 15, `DeleteCommand` s `ComamndType` задано значение текста, который запускает его для отправки текста `CommandText` свойства как ad-hoc SQL-запроса.


![Выберите CategoriesTableAdapter в конструкторе, чтобы просмотреть его свойства в окне «Свойства»](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Рис. 15**: выберите `CategoriesTableAdapter` в конструкторе, чтобы просмотреть его свойства в окне «Свойства»


Чтобы изменить эти параметры, выделите текст (DeleteCommand) в окне «Свойства» и выберите в раскрывающемся списке (создать). Это приведет к устранению параметров для `CommandText`, `CommandType`, и `Parameters` свойства. Затем задайте `CommandType` свойства `StoredProcedure` и введите имя хранимой процедуры для `CommandText` (`dbo.Categories_Delete`). Если вы не забудьте ввести свойства в следующем порядке: сначала `CommandType` и затем `CommandText` -Visual Studio автоматически заполнит коллекцию параметров. Если вы не вводите эти свойства в указанном порядке, необходимо вручную добавить параметры через редактор коллекции параметров. В любом случае он s, разумно щелкните эллипсы в свойстве параметров на отображение редактора коллекций параметров, чтобы убедиться, что были внесены изменения параметров правильный параметр (см. рис. 16). Если вы не видите все параметры в диалоговом окне, добавьте `@CategoryID` параметр вручную (необходимо добавить `@RETURN_VALUE` параметр).


![Убедитесь, что правильно заданы параметры параметры](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Рис. 16**: Убедитесь, что правильно заданы параметры параметры


После обновления DAL, удаление раздела будет автоматически удалять все соответствующие продукты и делать это в рамках транзакции. Чтобы проверить это, вернитесь на страницу обновление и удаление существующих двоичных данных и нажмите кнопку "Удалить" для одной из категорий. С одним щелчком мыши категории и все его связанные продукты будут удалены.

> [!NOTE]
> Перед тестированием `Categories_Delete` хранимая процедура, которая приведет к удалению ряд продуктов вместе с выбранной категории, было бы разумно сделать резервную копию базы данных. Если вы используете `NORTHWND.MDF` базы данных в `App_Data`, просто закройте Visual Studio и скопируйте файлы MDF и LDF в `App_Data` к другой папке. После тестирования функциональные возможности, вы можете восстановить базы данных, закрыть Visual Studio и замены текущей MDF и LDF файлы в `App_Data` резервными копиями.


## <a name="summary"></a>Сводка

Хотя мастер TableAdapter s будет автоматически создаваться хранимые процедуры для нас, бывают случаи, когда мы может уже есть такой хранимой процедуры, созданные или создать их вручную или с другими средствами вместо этого. Чтобы реализовать такие сценарии, адаптер можно настроить для указания существующей хранимой процедуры. В этом учебнике мы рассмотрели способы вручную добавлять хранимые процедуры в базе данных с помощью среды Visual Studio и связать эти хранимые процедуры методов класса TableAdapter s. Мы также рассмотрели команды T-SQL и шаблон скрипта, используемый для запуска, фиксация и откат транзакций из хранимой процедуры.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Хилтон Geisenow S ren Алексей Lauritsen и Терезой Мерфи, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Вперед](updating-the-tableadapter-to-use-joins-vb.md)
