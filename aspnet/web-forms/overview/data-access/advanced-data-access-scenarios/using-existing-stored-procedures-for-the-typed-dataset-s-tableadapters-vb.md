---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Использование существующих хранимых процедур для адаптеров таблиц типизированного набора данных (Visual Basic) | Документы Microsoft
author: rick-anderson
description: В предыдущем учебнике мы узнали, как создать новые хранимые процедуры с помощью мастера TableAdapter. В этом учебнике рассказано, как один адаптер таблицы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: ac319b67c9215c5dde8e7507076ed45a1f7825c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877376"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Использование существующих хранимых процедур для адаптеров таблиц типизированного набора данных (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) или [скачать PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> В предыдущем учебнике мы узнали, как создать новые хранимые процедуры с помощью мастера TableAdapter. В этом учебнике рассказано принципов работы одного мастера TableAdapter с существующие хранимые процедуры. Мы также рассказывается об вручную добавить новые хранимые процедуры в базе данных.


## <a name="introduction"></a>Вступление

В [предыдущего учебника](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) мы узнали, как s типизированного набора данных адаптеры таблицы могут быть настроены для использования хранимых процедур для доступа данных, а не нерегламентированных инструкций SQL. В частности мы рассмотрели, как с помощью мастера TableAdapter, который автоматически создавать эти хранимые процедуры. При переносе прежних версий приложений на ASP.NET 2.0 или при создании веб-сайта ASP.NET 2.0 вокруг существующей модели данных, скорее всего, база данных уже содержит хранимые процедуры, которые необходимы. Кроме того можно создать хранимые процедуры, вручную или с помощью некоторых средство, отличное от Мастер TableAdapter, который автоматически создает хранимые процедуры.

В этом учебнике мы рассмотрим способы настройки адаптера таблицы, чтобы использовать существующие хранимые процедуры. Поскольку в базе данных Northwind только небольшой набор встроенных хранимых процедур, мы также рассмотрим шаги, необходимо вручную добавить новые хранимые процедуры в базе данных с помощью среды Visual Studio. S позволяют начать работу!

> [!NOTE]
> В [упаковки изменения базы данных в рамках транзакции](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) учебника мы добавили методы адаптера таблицы для поддержки транзакций (`BeginTransaction`, `CommitTransaction`и так далее). Кроме того можно управлять транзакциями целиком в пределах хранимой процедуры, которая не требует модификации кода уровня доступа к данным. В этом учебнике мы изучим, команды T-SQL, используемые для выполнения инструкций s хранимой процедуры в области транзакции.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Шаг 1: Добавление хранимых процедур в базе данных Northwind

Visual Studio позволяет легко добавлять новые хранимые процедуры в базе данных. Let s добавить новую хранимую процедуру в базе данных Northwind, возвращаются все столбцы из `Products` таблицу для тех, которые имеют определенный `CategoryID` значение. В окне обозревателя серверов для отображения папки - диаграммы базы данных, таблицы, представления и т. д - разверните узел базы данных Northwind. Как упоминалось в предыдущем учебнике папку хранимые процедуры содержит базы данных s существующие хранимые процедуры. Чтобы добавить новую хранимую процедуру, щелкните правой кнопкой мыши папку хранимые процедуры и выберите в контекстном меню параметр Добавить новую хранимую процедуру.


[![Щелкните правой кнопкой мыши папку хранимые процедуры и добавить новую хранимую процедуру](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Рис. 1**: щелкните правой кнопкой мыши папку хранимые процедуры и добавить новую хранимую процедуру ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Как показано на рис. 1, при выборе параметра добавить новую хранимую процедуру открывает окно скрипта в Visual Studio рамкой скрипт SQL, необходимый для создания хранимой процедуры. Это наш задания конкретизировать этот скрипт или выполнить ее, после чего хранимую процедуру будут добавлены в базу данных.

Введите следующий сценарий:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Этот скрипт во время выполнения добавит новую хранимую процедуру в базе данных Northwind с именем `Products_SelectByCategoryID`. Эта хранимая процедура принимает один входной параметр (`@CategoryID`, типа `int`) и возвращает все поля для этих продуктов с соответствующим `CategoryID` значение.

Чтобы выполнить этот `CREATE PROCEDURE` скрипта и добавления хранимой процедуры в базу данных, щелкните значок «сохранить» на панели инструментов или нажмите сочетание клавиш Ctrl + S. После этого обновление папки хранимых процедур, отображаются только что созданный хранимой процедуры. Кроме того, сценарий в окне изменится тонкости из `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` для `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Добавляет новую хранимую процедуру в базе данных, пока `ALTER PROCEDURE` обновляет существующую. С момента запуска скрипта было изменено на `ALTER PROCEDURE`, изменение хранимых процедур входные параметры или инструкций SQL и щелкнув значок сохранения этих изменений обновить хранимую процедуру.

На рисунке 2 представлен Visual Studio после `Products_SelectByCategoryID` хранимая процедура была сохранена.


[![Хранимая процедура Products_SelectByCategoryID был добавлен в базу данных](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**На рисунке 2**: хранимая процедура `Products_SelectByCategoryID` был добавлен в базу данных ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Шаг 2: Настройка адаптера таблицы для использования существующей хранимой процедуры

Теперь, когда `Products_SelectByCategoryID` хранимую процедуру будет добавлен в базу данных, можно настроить нашей уровня доступа к данным, чтобы использовать эту хранимую процедуру при вызове одного из его методов. В частности, мы добавим `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` метод `ProductsTableAdapter` в `NorthwindWithSprocs` типизированного набора данных, который вызывает `Products_SelectByCategoryID` хранимой процедуры, мы только что создали.

Сначала откройте `NorthwindWithSprocs` набора данных. Щелкните правой кнопкой мыши `ProductsTableAdapter` и выберите команду Добавить запрос, чтобы запустить мастер настройки запроса адаптера таблицы. В [предыдущего учебника](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) мы решено имеют создать новую хранимую процедуру для нас адаптера таблицы. В этом учебнике, тем не менее, мы хотим Подключите новый метод адаптера таблицы для существующего `Products_SelectByCategoryID` хранимой процедуры. Таким образом выберите параметр использовать существующие хранимые процедуры из первого пошагового мастера s и нажмите кнопку Далее.


[![Выберите использование существующей хранимой процедуры, параметр](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Рис. 3**: выберите, использовать существующие хранимой процедуры параметр ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


Откроется следующий экран предоставляет раскрывающегося списка заполняется базы данных s хранимых процедур. При выборе хранимой процедуры перечислены свои входные параметры в левой части и полей данных, возвращаемых (если таковые имеются) в правой части. Выберите `Products_SelectByCategoryID` хранимую процедуру из списка и нажмите кнопку Далее.


[![Выбрать Products_SelectByCategoryID хранимой процедуры](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Рис. 4**: выбрать `Products_SelectByCategoryID` хранимой процедуры ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


Следующий экран спрашивает нас, какие данные возвращается хранимой процедурой и нашей ответа определяет тип, возвращаемый методом s адаптера таблицы. Например, если указать, что табличные данные возвращаются, метод возвращает `ProductsDataTable` экземпляр, заполненный записей, возвращаемых хранимой процедурой. Напротив, если мы указываем, что эта хранимая процедура возвращает одно значение TableAdapter вернет `Object` , присваивается значение в первом столбце первой записи, возвращаемых хранимой процедурой.

Поскольку `Products_SelectByCategoryID` хранимая процедура возвращает все продукты, принадлежащие к определенной категории, выберите первый ответ - табличные данные — и нажмите кнопку Далее.


[![Указывает, что хранимая процедура возвращает табличные данные](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Рис. 5**: Указывает, что хранимая процедура возвращает табличных данных ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Осталось указывают, что метод шаблонов, используемых следуют имена для этих методов. Оставьте оба заполнения DataTable и возвращаемых параметров DataTable этот флажок установлен, но его можно переименовать методы `FillByCategoryID` и `GetProductsByCategoryID`. Нажмите кнопку Далее, чтобы просмотреть сводку задач, которые мастер выполнит. Если все выглядит правильно, нажмите кнопку "Готово".


[![Имя методы FillByCategoryID и GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Рис. 6**: имя методы `FillByCategoryID` и `GetProductsByCategoryID` ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> Методы TableAdapter, который мы только что создали, `FillByCategoryID` и `GetProductsByCategoryID`, ожидается, что входной параметр типа `Integer`. Это значение входного параметра передается в хранимую процедуру через его `@CategoryID` параметра. При изменении `Products_SelectByCategory` параметры хранимой процедуры s, необходимо также обновить параметры для этих методов адаптера таблицы. Как было сказано в [с предыдущим учебником](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), это можно сделать одним из двух способов: путем вручную добавления или удаления параметров из коллекции parameters или путем повторного запуска мастера TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Шаг 3: Добавление`GetProductsByCategoryID(categoryID)`метода для МЕТОДА

С `GetProductsByCategoryID` DAL метод завершения, следующим шагом является предоставляют доступ к этому методу в бизнес-логики. Откройте `ProductsBLLWithSprocs` и добавьте следующий метод:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Этот уровень бизнес-ЛОГИКИ метод просто возвращает `ProductsDataTable` , возвращенные `ProductsTableAdapter` s `GetProductsByCategoryID` метод. `DataObjectMethodAttribute` Атрибут содержит метаданные, используемые с помощью мастера настройки источника данных ObjectDataSource s. В частности этот метод будет отображаться в раскрывающемся списке s ВЫБЕРИТЕ вкладку.

## <a name="step-4-displaying-products-by-category"></a>Шаг 4: Отображение продукты по категориям

Чтобы проверить только что добавленном `Products_SelectByCategoryID` хранимой процедуры и соответствующих методов DAL и уровень бизнес-ЛОГИКИ, позволяют создавать страницы ASP.NET, которая содержит DropDownList и GridView s. DropDownList список всех категорий в базе данных, пока GridView отобразит продуктов, принадлежащих выбранной категории.

> [!NOTE]
> Мы интерфейсы иерархического хранять создан с помощью элементами управления DropDownList в предыдущих учебниках. Более подробное рассмотрение реализации такого отчета главного и подчиненного представлений см. в разделе [иерархического фильтрации с DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) учебника.


Откройте `ExistingSprocs.aspx` страницы в `AdvancedDAL` папки и перетащите DropDownList из области элементов в конструктор. Набор DropDownList s `ID` свойства `Categories` и его `AutoPostBack` свойства `True`. Затем из его смарт-тег привязки DropDownList новый ObjectDataSource с именем `CategoriesDataSource`. Настройте элемент управления ObjectDataSource таким образом, чтобы он извлекает данные из `CategoriesBLL` класса s `GetCategories` метод. Установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет).


[![Получить данные из метода GetCategories класса CategoriesBLL s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Рис. 7**: получение данных из `CategoriesBLL` класса s `GetCategories` метод ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Рис. 8**: задать раскрывающихся списков в обновления, вставки и удаления вкладок (нет) ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


После завершения работы мастера ObjectDataSource настроить DropDownList для отображения `CategoryName` поля данных и использовать `CategoryID` как `Value` для каждого `ListItem`.

На этом этапе DropDownList и ObjectDataSource s декларативная разметка должен быть аналогичен следующему:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Затем перетащите элемент управления GridView в конструктор, поместив ее под DropDownList. Набор GridView s `ID` для `ProductsByCategory` и его смарт-теге, привязать его к новой ObjectDataSource с именем `ProductsByCategoryDataSource`. Настройка `ProductsByCategoryDataSource` ObjectDataSource для использования `ProductsBLLWithSprocs` класса, его получить его данные с помощью `GetProductsByCategoryID(categoryID)` метод. Поскольку это GridView будет использоваться только для отображения данных, раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет) и нажмите кнопку Далее.


[![Настройка ObjectDataSource с помощью класса ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Рис. 9**: Настройка ObjectDataSource для использования `ProductsBLLWithSprocs` класса ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Получить данные из метода GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Рис. 10**: получение данных из `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


Метода, выбранного на вкладке "SELECT" ожидает параметр, поэтому на последнем этапе мастер запрашивает источник параметра s. Задайте параметр исходного раскрывающегося списка для элемента управления и выберите `Categories` элемента управления из раскрывающегося списка ControlID. Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Использовать в качестве источника categoryID параметр DropDownList категорий](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Рис. 11**: использование `Categories` DropDownList как источник `categoryID` параметра ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


После завершения работы мастера ObjectDataSource, Visual Studio добавит стояли и CheckBoxField для каждого из полей данных продукта. Вы можете настроить эти поля, по своему усмотрению.

Перейдите на страницу с помощью браузера. При посещении страницы выбрана категория напитков и соответствующих продуктов, перечисленных в сетке. Изменение раскрывающегося списка альтернативных категории, как на рис. 12 показан, вызывает обратную передачу и перезагружает сетки с продуктами новой выбранной категории.


[![Отображаются продукты в категории «создать»](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Рис. 12**: продукты в категории «создать» отображаются ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Шаг 5: Упаковки инструкций s хранимой процедуры в пределах транзакции

В [упаковки изменения базы данных в рамках транзакции](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) учебника мы обсуждали методы выполнения ряд инструкций изменения базы данных в области транзакции. Напомним, что изменения, выполненные в рамках транзакции, либо все завершиться успешно "или" Ошибка все, что гарантирует атомарность. Следующие методы для использования транзакций.

- Используя классы в `System.Transactions` пространства имен
- Наличие доступа к данным использования классов ADO.NET, например `SqlTransaction`, и
- Добавление команд транзакции T-SQL непосредственно в хранимой процедуре

*Упаковки изменения базы данных в рамках транзакции* учебнике используются классы ADO.NET в DAL. В оставшейся части этого учебника рассматривается управление транзакцию с помощью команды T-SQL из хранимой процедуры.

Три ключевых команды SQL для ручной запуск, фиксации и отката транзакции `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, и `ROLLBACK TRANSACTION`соответственно. Как подход ADO.NET, и при использовании транзакций из хранимой процедуры, нам нужно применить следующий шаблон:

1. Указывают на начало транзакции.
2. Выполните инструкции SQL, которые составляют транзакции.
3. Если имеется ошибка в любой из инструкций из шага 2, производится откат транзакции.
4. Если все инструкции шага 2 завершается без ошибок, зафиксируйте транзакцию.

Этот шаблон может быть реализован в синтаксис T-SQL, используя следующий шаблон:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Начинается шаблон, определив `TRY...CATCH` блокировать конструкцию, новые для SQL Server 2005. Как и с `Try...Catch` блоков в Visual Basic, SQL `TRY...CATCH` блок выполняет операторы в `TRY` блока. Если любая инструкция вызывает ошибку, управление немедленно передается `CATCH` блока.

Если ошибок нет, выполнение инструкций SQL, состав транзакции, `COMMIT TRANSACTION` оператор фиксирует изменения и завершает транзакцию. Если, однако одна из инструкций приводит к ошибке `ROLLBACK TRANSACTION` в `CATCH` блок возвращает базу данных в состояние до начала транзакции. Хранимая процедура вызывает ошибки с помощью [команда RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), чего `SqlException` вызываемого в приложении.

> [!NOTE]
> Поскольку `TRY...CATCH` блок является новой возможностью в SQL Server 2005, выше шаблона не будет работать, если вы используете более старых версий Microsoft SQL Server. Если вы не используете SQL Server 2005, обратитесь к [управление транзакции в хранимых процедур SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) для шаблона, который будет работать с другими версиями SQL Server.


Позволяет просмотреть конкретный пример s. Существует ограничение внешнего ключа между `Categories` и `Products` таблиц, это означает, что каждый `CategoryID` в `Products` необходимо сопоставить таблицу `CategoryID` значение в `Categories` таблицы. Любое действие, которое нарушает это ограничение, например попытка удалить категорию, которая имеет связанные продукты, приводит к нарушению ограничения внешнего ключа. Чтобы проверить это, вернитесь в примере обновление и удаление существующих двоичных данных, при работе с двоичными данными раздела (`~/BinaryData/UpdatingAndDeleting.aspx`). На этой странице перечислены каждой категории в системе, а также кнопки изменения и удаления (см. рис. 13), но при попытке удалить категорию, которая имеет связанные продукты - например Напитки - операция удаления завершается ошибкой из-за нарушения ограничения внешнего ключа (см. рис. 14).


[![Каждая категория будет отображена в GridView, изменить и удалить кнопки](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Рис. 13**: каждый категория будет отображена в элементе управления GridView, изменить и удалить кнопок ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Не удается удалить категории с существующие продукты](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Рис. 14**: не удается удалить категории с существующие продукты ([Просмотр полноразмерное изображение](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Представьте себе, что нам нужно разрешить категории удаляются независимо от того, является ли они имеют связанные продукты. Удалить категорию с продуктами, предположим, нам необходимо также удалить свои существующие продукты (хотя можно было бы просто присвоить свои продукты `CategoryID` значения `NULL`). Эта функция может быть реализован через правила cascade ограничения внешнего ключа. Кроме того, можно было бы создать хранимую процедуру, которая принимает `@CategoryID` входного параметра и при вызове явным образом удаляются и все связанные продукты, а затем указанной категории.

Первая попытка хранимая процедура может выглядеть следующим образом:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Хотя это определенно приведет к удалению соответствующих продуктов и категорий, она не делает в рамках транзакции. Представьте, что на некоторых других ограничение внешнего ключа `Categories` , может запретить удаление конкретной `@CategoryID` значение. Проблема заключается в том, что в этом случае все эти продукты будут удалены перед предпринимается попытка удалить категорию. Конечным результатом становится, для такой категории Эта хранимая процедура будет удалить все свои продукты пока оставались категорию, так как он все еще связаны другие записи в другой таблице.

Если хранимая процедура были в оболочку в пределах транзакции, однако удаление для `Products` таблицы был бы выполнен откат в случае не удалось удалить `Categories`. Следующий скрипт хранимая процедура использует транзакцию для обеспечения атомарности между двумя `DELETE` инструкции:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Теперь пора добавить `Categories_Delete` хранимой процедуры в базе данных Northwind. Вернуться к шагу 1 инструкции по добавлению хранимых процедур в базе данных.

## <a name="step-6-updating-thecategoriestableadapter"></a>Шаг 6: Обновление`CategoriesTableAdapter`

Хотя мы добавлены хранять `Categories_Delete` хранимой процедуры в базе данных, DAL в настоящее время настроен для использования нерегламентированных инструкций SQL для выполнения удаления. Необходимо обновить `CategoriesTableAdapter` и настройте его для использования `Categories_Delete` вместо хранимой процедуры.

> [!NOTE]
> Ранее в этом учебнике мы работали с `NorthwindWithSprocs` набора данных. Однако этот набор данных содержит только одну сущность `ProductsDataTable`, и нам необходимо работать с категориями. Таким образом, в течение оставшейся части этого учебника, когда я говорю о ссылке на m уровень данных Access I `Northwind` набора данных, сначала мы создали в [Создание слой доступа к данным](../introduction/creating-a-data-access-layer-vb.md) учебника.


Выберите набор данных "Борей", откройте `CategoriesTableAdapter`и перейдите в окно «Свойства». Списки окна свойств `InsertCommand`, `UpdateCommand`, `DeleteCommand`, и `SelectCommand` используемого адаптера таблицы, а также его имя и сведения о соединении. Разверните `DeleteCommand` свойство, чтобы просмотреть сведения о нем. Как показано на рис. 15, `DeleteCommand` s `ComamndType` задано значение текста, который указывает, что в сообщение текст `CommandText` свойство как нерегламентированные SQL-запроса.


![Выберите CategoriesTableAdapter в конструкторе, чтобы посмотреть его свойства в окне «Свойства»](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Рис. 15**: выберите `CategoriesTableAdapter` в конструкторе, чтобы посмотреть его свойства в окне «Свойства»


Чтобы изменить эти параметры, выберите текст (DeleteCommand) в окне «Свойства» и выберите из раскрывающегося списка (создать). Это приведет к очистке параметры для `CommandText`, `CommandType`, и `Parameters` свойства. Затем задайте `CommandType` свойства `StoredProcedure` , а затем введите имя хранимой процедуры для `CommandText` (`dbo.Categories_Delete`). Если вы убедитесь, что в следующем порядке: сначала введите свойства `CommandType` и затем `CommandText` -Visual Studio автоматически заполняет коллекцию параметров. Если не ввести эти свойства в указанном порядке, необходимо вручную добавить параметры через редактор коллекции параметров. В любом случае его s, разумным шагом будет щелкнуть многоточие в свойстве параметры обеспечить копирование редактор коллекции параметров убедитесь, что были внесены изменения параметров правильный параметр (см. рис. 16). Если вы не видите все параметры в диалоговом окне, добавьте `@CategoryID` параметр вручную (необходимо добавить `@RETURN_VALUE` параметр).


![Убедитесь, что правильно заданы параметры параметры](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**На рисунке 16**: Убедитесь, что правильно заданы параметры параметры


После обновления DAL удаление категории будут автоматически удалены все соответствующие продукты и делать это в рамках транзакции. Чтобы проверить это, вернуться на страницу обновление и удаление существующих двоичных данных и нажмите кнопку «Удалить» для одной из категорий. С одним щелчком мыши категории и все его связанные продукты будут удалены.

> [!NOTE]
> Перед тестированием `Categories_Delete` хранимая процедура, которая приведет к удалению число продуктов вместе с выбранной категории, он может быть полезным сделать резервную копию базы данных. Если вы используете `NORTHWND.MDF` базы данных в `App_Data`, просто закройте Visual Studio и скопируйте файлы MDF и LDF в `App_Data` другую указанную папку. После тестирования функциональные возможности, можно восстановить базу данных, закрыв Visual Studio и замены текущей MDF и LDF файлы в `App_Data` с резервными копиями.


## <a name="summary"></a>Сводка

Хотя мастер TableAdapter s будет автоматически создаваться хранимые процедуры для нас, бывают ситуации, когда мы может такие хранимые процедуры, созданные или уже должны создавать их вручную или с помощью других средств вместо. Для поддержки таких сценариев, адаптер можно настроить для указания существующей хранимой процедуры. В этом учебнике мы рассмотрели вручную добавить хранимые процедуры в базе данных с помощью среды Visual Studio и способ передачи s методы адаптера таблицы для этих хранимых процедур. Также мы рассмотрели, команды T-SQL и скриптов шаблон используется для начала, фиксации и отката транзакции из хранимой процедуры.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Хилтон Geisenow, S ren Алексей Lauritsen и Мерфи Тереза д. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Вперед](updating-the-tableadapter-to-use-joins-vb.md)
