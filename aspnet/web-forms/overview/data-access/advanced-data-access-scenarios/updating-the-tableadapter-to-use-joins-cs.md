---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Обновление адаптера таблицы для использования JOIN (C#) | Документация Майкрософт
author: rick-anderson
description: При работе с базой данных, довольно часто запрашивать данные, которые распределены по нескольким таблицам. Для получения данных из двух различных таблиц можно использовать любой...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ff63f1dc7c01f2be66dd99e1e00e2c2bb70058d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391005"
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>Обновление адаптера таблицы для использования JOIN (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) или [скачать PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> При работе с базой данных, довольно часто запрашивать данные, которые распределены по нескольким таблицам. Для получения данных из двух различных таблиц можно использовать операцию СОЕДИНЕНИЯ или коррелированные вложенные запросы. В этом руководстве мы сравнение коррелированные вложенные запросы и синтаксис JOIN перед тем как рассмотреть способ создания, включающий ОБЪЕДИНЕНИЕ в его основного запроса адаптера таблицы.


## <a name="introduction"></a>Вступление

С реляционными базами данных данные, которые мы заинтересованы в работе с часто распределены по нескольким таблицам. Например если отображение информации о продукте мы скорее всего требуется перечислить каждой соответствующей категории s продукта и имен поставщиков s. `Products` Таблица имеет `CategoryID` и `SupplierID` значения, но фактическое имя, категорию и поставщика находятся в `Categories` и `Suppliers` таблицы, соответственно.

Чтобы получить данные из другой, связанные таблицы, мы может либо использовать *коррелированных вложенных запросах* или `JOIN` *s*. Коррелированные вложенные запросы — это вложенный `SELECT` запрос, который ссылается на столбцы во внешнем запросе. Например, в [создание уровня доступа к данным](../introduction/creating-a-data-access-layer-cs.md) руководстве мы использовали двух коррелированных запросов осуществляется в `ProductsTableAdapter` s основного запроса для возвращения имен категорию и поставщика для каждого продукта. Объект `JOIN` SQL конструкция, которая объединяет связанные строки из двух различных таблиц. Мы использовали `JOIN` в [запрос данных с помощью элемента управления SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) руководству, чтобы отобразить сведения о категории наряду с каждого продукта.

Причина, мы abstained с помощью `JOIN` s с помощью адаптеров таблиц — из-за ограничений в мастере адаптера таблицы s автоматически создает соответствующий `INSERT`, `UPDATE`, и `DELETE` инструкций. В частности при наличии основного запроса адаптера таблицы s `JOIN` s, TableAdapter не может автоматически создавать специализированные инструкции SQL или хранимые процедуры для его `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства.

В этом руководстве мы кратко сравнивает и контрастности коррелированных вложенных запросах и `JOIN` s перед изучением Создание TableAdapter, который включает в себя `JOIN` s в его основной запрос.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Сравнивает и противопоставляет коррелированных вложенных запросах и`JOIN` s

Помните, что `ProductsTableAdapter` создана в первом учебнике `Northwind` набор данных использует коррелированные вложенные запросы, чтобы вернуть каждый s соответствующий категорию и поставщика название продукта. `ProductsTableAdapter` s основного запроса приведен ниже.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Двух коррелированных запросов - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` и `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -являются `SELECT` запросы, которые возвращают одно значение каждого продукта в качестве дополнительного столбца во внешнем `SELECT` списке столбцов инструкции s.

Кроме того `JOIN` может использоваться для получения имени поставщика и категории каждого продукта s. Следующий запрос возвращает такие же выходные данные, что и выше, но использует `JOIN` s вместо вложенных запросов:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

Объект `JOIN` объединяет записи из одной таблицы с записями из другой таблицы, на основе определенных критериев. В запросе выше, к примеру `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` сообщает SQL Server для слияния каждой записи продукта с категорией записи, которой `CategoryID` значение соответствует продукт s `CategoryID` значение. Результаты слияния позволяет нам работать с в соответствующие поля категории для каждого продукта (такие как `CategoryName`).

> [!NOTE]
> `JOIN` s обычно используются при запросе данных из реляционных баз данных. Если вы не знакомы с `JOIN` синтаксис или нужно немного освежить по его использованию, d рекомендую [SQL Join руководстве](http://www.w3schools.com/sql/sql_join.asp) в [W3 Schools](http://www.w3schools.com/). Также следует ознакомиться, [ `JOIN` основы](https://msdn.microsoft.com/library/ms191517.aspx) и [запросах](https://msdn.microsoft.com/library/ms189575.aspx) разделы [документации по SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Так как `JOIN` s коррелированные вложенные запросы могут совместно использовать и извлекать данные из других таблиц, многие разработчики, остаются головой (Joe weinman) и хотите узнать, какие подходы использовать. Все SQL гуру я ve говорили, что примерно то же самое, что он t важны с точки как SQL Server будет создавать планы выполнения примерно идентичны. Их рекомендации будет используйте технику, вам и вашей группе наиболее удобен. Он заслуживает отметить, что после imparting этому совету экспертам немедленно express свои предпочтения `JOIN` s через коррелированные вложенные запросы.

При создании слой доступа к данным, использующего типизированные наборы DataSet, инструменты работают лучше при использовании вложенных запросов. В частности, мастер TableAdapter s не автоматически создаст соответствующий `INSERT`, `UPDATE`, и `DELETE` инструкции, если основной запрос содержит любые `JOIN` s, но будет автоматически создавать эти инструкции, если корреляции вложенные запросы используются.

Чтобы изучить этот недостаток, создание временных типизированный набор DataSet в `~/App_Code/DAL` папку. При работе с мастером настройки адаптера таблицы решили использовать специализированные инструкции SQL и введите следующую `SELECT` запроса (см. рис. 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Введите основной запрос, который содержит соединения](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Рис. 1**: Введите запрос Main, который содержит `JOIN` s ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


По умолчанию, автоматически создаст TableAdapter `INSERT`, `UPDATE`, и `DELETE` инструкций на основе основного запроса. Если нажать кнопку «Дополнительно», вы увидите, что эта функция включена. Несмотря на этот параметр TableAdapter будет невозможно создать `INSERT`, `UPDATE`, и `DELETE` инструкции так, как основной запрос содержит `JOIN`.


![Введите основной запрос, который содержит соединения](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Рис. 2**: введите основной запрос, который содержит `JOIN` s


Нажмите кнопку Готово, чтобы завершить работу мастера. На этом этапе ваш набор данных s конструктор будет включать одним адаптером TableAdapter с объект DataTable со столбцами для каждого из полей, возвращаемых в `SELECT` списка столбца запроса s. Сюда входят `CategoryName` и `SupplierName`, как показано на рис. 3.


![Объект DataTable со столбцом для каждого поля, возвращаемые в списке столбцов](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Рис. 3**: объект DataTable со столбцом для каждого поля, возвращаемых в списке столбцов


Хотя объект DataTable есть соответствующие столбцы, TableAdapter отсутствуют значения для его `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства. Чтобы проверить это, щелкните TableAdapter в конструкторе и перейдите в окно свойств. Там вы увидите, что `InsertCommand`, `UpdateCommand`, и `DeleteCommand` задано значение (None).


[![InsertCommand, UpdateCommand и DeleteCommand свойства задаются (нет)](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Рис. 4**: `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойств (нет) ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


Чтобы обойти этот недостаток, мы можно вручную указать инструкций SQL и параметры для `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойств с помощью окна «Свойства». Может в качестве альтернативы мы начнем с настройки адаптера таблицы s основного запроса к *не* , содержащие любые `JOIN` s. Таким образом, `INSERT`, `UPDATE`, и `DELETE` инструкции, чтобы автоматически создавать для нас. После завершения работы мастера, мы может затем вручную обновите TableAdapter s `SelectCommand` из окна свойств, чтобы он включал `JOIN` синтаксис.

Хотя этот подход работает, это очень значительно усложняло работу при использовании ad-hoc-запросов SQL, так как в основном запросе s TableAdapter в любое время, повторно настроить в мастере, автоматически созданные `INSERT`, `UPDATE`, и `DELETE` инструкций создаются повторно. Это означает, что все мы впоследствии присвоить пользовательские настройки будут потеряны, если мы щелкнули правой кнопкой мыши в TableAdapter, выбрали Настройка из контекстного меню и завершения работы мастера снова.

Хрупкости s TableAdapter, который автоматически генерируемым `INSERT`, `UPDATE`, и `DELETE` инструкций является, к счастью, ограничен специализированные инструкции SQL. Если TableAdapter использует хранимые процедуры, можно настроить `SelectCommand`, `InsertCommand`, `UpdateCommand`, или `DeleteCommand` хранимые процедуры и повторно запустите мастер настройки адаптера таблицы без необходимости бояться, что хранимые процедуры будут изменить.

Следующие несколько шагов, мы создадим адаптер таблицы, изначально использует основной запрос, который пропускает любые `JOIN` s таким образом, чтобы соответствующий вставки, обновления и удаления хранимых процедур будут создаваться автоматически. Затем мы обновим `SelectCommand` таким образом, использующий `JOIN` , возвращает дополнительные столбцы из связанных таблиц. Наконец мы создадим соответствующий класс уровня бизнес-логики и демонстрируют использование TableAdapter в веб-страницу ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Шаг 1: Создание с помощью упрощенного основного запроса адаптера таблицы

В этом руководстве мы добавим TableAdapter и DataTable со строгой типизацией для `Employees` в таблицу `NorthwindWithSprocs` набора данных. `Employees` Таблица содержит `ReportsTo` поле, которое указано `EmployeeID` диспетчера сотрудника s. Например, сотрудник, Петров Anne имеет `ReportTo` значение 5, которое равно `EmployeeID` из Стивен Иванов. Следовательно Anne сообщает Стивен, ее Менеджер. А также отчеты каждому сотруднику s `ReportsTo` значение, мы может также потребоваться извлечения имени записью их менеджера. Это можно сделать с помощью `JOIN`. Однако применение `JOIN` при первоначальном создании TableAdapter исключает возможность мастера автоматического создания соответствующих вставки, обновления и удаления возможности. Таким образом, мы начнем с создания адаптера таблицы, основного запроса не содержит `JOIN` s. Затем, на шаге 2, мы обновим основного запроса хранимые процедуры, чтобы получить имя диспетчера s через `JOIN`.

Сначала откройте `NorthwindWithSprocs` набора данных в `~/App_Code/DAL` папку. Щелкните правой кнопкой мыши в конструкторе и выберите Добавить параметр в контекстном меню выбрать пункт меню адаптера таблицы. Это приведет к запуску мастера настройки адаптера таблицы. Как показано на рис. 5, имеют мастер создал новые хранимые процедуры и нажмите кнопку Далее. О создании новых хранимых процедур с помощью мастера TableAdapter s, обратитесь к [создание новых хранимых процедур для адаптеров таблиц TableAdapter типизированного набора DataSet s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) руководства.


[![Выберите создать новые хранимые процедуры параметр](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Рис. 5**: Укажите, создать новые хранимые процедуры параметр ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Используйте следующую команду `SELECT` инструкции для основного запроса адаптера таблицы s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Так как этот запрос не содержит `JOIN` s, адаптера таблицы мастер автоматически создаст хранимых процедур с соответствующим `INSERT`, `UPDATE`, и `DELETE` инструкции, а также хранимой процедуры для выполнения основной запрос.

Следующий шаг позволит нам имя процедуры, которые хранятся адаптера таблицы. Используйте имена `Employees_Select`, `Employees_Insert`, `Employees_Update`, и `Employees_Delete`, как показано на рис. 6.


[![Имя процедуры, которые хранятся адаптера таблицы](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Рис. 6**: имя адаптера таблицы хранимых процедур s ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


Последним шагом запрашивает имя s методы адаптера таблицы. Используйте `Fill` и `GetEmployees` как имена методов. Также не забудьте оставить флажок создавать методы для отправки обновлений непосредственно в базу данных (GenerateDBDirectMethods) флажок установленным.


[![Имя свойства Fill методы TableAdapter s и GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Рис. 7**: имя TableAdapter s методы `Fill` и `GetEmployees` ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


После завершения работы мастера, Отвлекитесь и проверьте хранимые процедуры в базе данных. Вы увидите четыре новых: `Employees_Select`, `Employees_Insert`, `Employees_Update`, и `Employees_Delete`. Затем проверьте `EmployeesDataTable` и `EmployeesTableAdapter` только что создали. Объект DataTable содержит столбец для каждого поля, возвращенной основным запросом. Щелкните TableAdapter и перейдите в окно свойств. Там вы увидите, что `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства настроены правильно для вызова соответствующих хранимых процедур.


[![TableAdapter включает вставки, обновления и удаления возможности](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Рис. 8**: TableAdapter включает Insert, Update и удалить возможности ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


С помощью инструкции insert, update и delete хранимые процедуры, автоматически созданные и `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства правильно настроены, можно приступить к настройке `SelectCommand` s хранимой процедуры для возврата дополнительных сведения о диспетчере каждого сотрудника s. В частности, необходимо обновить `Employees_Select` хранимой процедуры для использования `JOIN` и возврата manager s `FirstName` и `LastName` значения. После обновления хранимой процедуры, необходимо обновить объект DataTable, теперь она содержит следующие дополнительные столбцы. Это будет рассмотрено, эти две задачи на шагах 2 и 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Шаг 2: Настройка хранимой процедуры для включения`JOIN`

Start, перейдя в проводник по серверам, углубляться в папке Stored Procedures s базы данных Northwind и открыв `Employees_Select` хранимой процедуры. Если вы не видите эту хранимую процедуру, хранимые процедуры папку правой кнопкой мыши и выберите "Обновить". Обновить хранимую процедуру, чтобы он использовал `LEFT JOIN` для возврата manager s сначала имени и фамилии:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

После обновления `SELECT` инструкции, сохранить эти изменения, перейдите в меню "файл" и выберите Save `Employees_Select`. В качестве альтернативы можно щелкните значок "Сохранить" на панели инструментов или нажмите сочетание клавиш Ctrl + S. После сохранения изменений, щелкните правой кнопкой мыши `Employees_Select` хранимой процедуры в обозревателе серверов и выберите команду выполнить. Это выполните хранимую процедуру и показать его результаты в окне вывода (см. рис. 9).


[![Результаты хранимой процедуры отображаются в окне вывода](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Рис. 9**: результатов в хранимые процедуры отображаются в окне вывода ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Шаг 3: Обновление s столбцов таблицы данных

На этом этапе `Employees_Select` хранимая процедура возвращает `ManagerFirstName` и `ManagerLastName` значения, но `EmployeesDataTable` отсутствует этих столбцов. Эти отсутствующие столбцы можно добавить к таблице DataTable в одном из двух способов:

- **Вручную** — щелкните правой кнопкой мыши в таблице в конструкторе наборов данных и из меню "Добавить", выберите столбец. Можно затем назовите этот столбец и задайте свойства, соответствующим образом.
- **Автоматически** -мастера настройки адаптера таблицы будет обновлять столбцы s DataTable в соответствии с полям, возвращенным `SelectCommand` хранимой процедуры. Если вы используете специализированные инструкции SQL, мастер также удалит `InsertCommand`, `UpdateCommand`, и `DeleteCommand` свойства с момента `SelectCommand` теперь содержит `JOIN`. Но при использовании хранимых процедур, эти свойства команды остаются без изменений.

Ручное добавление столбцов таблицы данных мы изучили в предыдущих учебных курсах, включая [Master/Detail, с использованием маркированного списка основных записей с элементом управления DataList сведения](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) и [загрузка файлов](../working-with-binary-files/uploading-files-cs.md), и мы Рассмотрим этот процесс более подробно в снова в нашем следующем учебном курсе. Для этого руководства но позвольте s использовать автоматической настройке с помощью мастера настройки адаптера таблицы.

Запустите, щелкнув правой кнопкой мыши `EmployeesTableAdapter` и выбрав пункт "Настройка" в контекстном меню. Откроется мастер настройки адаптера таблицы, который перечисляет хранимые процедуры, используемые для выбора, вставки, обновления и удаления, а также их возвращаемые значения и параметры (если таковые имеются). Рис. 10 показан этот мастер. Здесь можно увидеть, что `Employees_Select` хранимая процедура возвращает теперь `ManagerFirstName` и `ManagerLastName` поля.


[![В мастере показан список обновленных столбцов для Employees_Select хранимой процедуры](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Рис. 10**: В мастере отображается список столбцов обновлены для `Employees_Select` хранимой процедуры ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Следуйте указаниям мастера, нажмите кнопку Finish. При возвращении в конструктор DataSet, `EmployeesDataTable` включает два дополнительных столбца: `ManagerFirstName` и `ManagerLastName`.


[![EmployeesDataTable содержит два новых столбца](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Рис. 11**: `EmployeesDataTable` содержит два новых столбца ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Чтобы проиллюстрировать, обновленный `Employees_Select` хранимая процедура по сути и что вставки, обновления и удаления возможностей адаптера таблицы по-прежнему работают, давайте s Создание веб-страницы, который позволяет пользователю просматривать и удалять сотрудников. Прежде чем мы создания такой страницы, тем не менее, необходимо сначала создать новый класс в уровне бизнес-логики для работы с сотрудниками из `NorthwindWithSprocs` набора данных. На шаге 4 мы создадим `EmployeesBLLWithSprocs` класса. На шаге 5 мы будем использовать этот класс со страницы ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Шаг 4: Реализация уровня бизнес-логики

Создайте новый файл класса в `~/App_Code/BLL` папку с именем `EmployeesBLLWithSprocs.cs`. Этот класс имитирует существующий семантику `EmployeesBLL` класса, только этот новый один обеспечивает меньшее число методов и использует `NorthwindWithSprocs` набора данных (вместо `Northwind` набора данных). Добавьте следующий код в класс `EmployeesBLLWithSprocs`.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

`EmployeesBLLWithSprocs` Класс s `Adapter` свойство возвращает экземпляр `NorthwindWithSprocs` s набора данных `EmployeesTableAdapter`. Это значение используется класс s `GetEmployees` и `DeleteEmployee` методы. `GetEmployees` Вызовы методов `EmployeesTableAdapter` соответствующий s `GetEmployees` метод, который вызывает `Employees_Select` хранимой процедуры и заполняет его результаты в `EmployeeDataTable`. `DeleteEmployee` Аналогичным образом вызывает метод `EmployeesTableAdapter` s `Delete` метод, который вызывает `Employees_Delete` хранимой процедуры.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Шаг 5: Работа с данными на уровне представления данных

С помощью `EmployeesBLLWithSprocs` класс полный, мы будет готов для работы с данными о сотрудниках через страницы ASP.NET. Откройте `JOINs.aspx` странице в `AdvancedDAL` папки и перетащите элемент управления GridView с панели элементов в конструктор, установив его `ID` свойства `Employees`. Затем из смарт-тега GridView s, привязка сетки для нового элемента управления ObjectDataSource с именем `EmployeesDataSource`.

Настройка ObjectDataSource на использование `EmployeesBLLWithSprocs` класса и из вкладки SELECT и DELETE, убедитесь, что `GetEmployees` и `DeleteEmployee` методы выбираются из раскрывающихся списков. Нажмите кнопку Готово, чтобы завершить настройку s ObjectDataSource.


[![Настройка ObjectDataSource на использование класса EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Рис. 12**: Настройка ObjectDataSource для использования `EmployeesBLLWithSprocs` класс ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![У использование ObjectDataSource GetEmployees и методы DeleteEmployee](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Рис. 13**: для использования ObjectDataSource `GetEmployees` и `DeleteEmployee` методы ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio добавит поле BoundField GridView для каждого из `EmployeesDataTable` s столбцов. Удалить все эти поля BoundField, кроме `Title`, `LastName`, `FirstName`, `ManagerFirstName`, и `ManagerLastName` и переименуйте `HeaderText` свойства для последних четырех полей BoundFields Фамилия, имя, имя диспетчера s, и Диспетчер s Last Name, соответственно.

Чтобы разрешить пользователям удалять сотрудников на этой странице, нам нужно сделать две вещи. Во-первых указать для предоставления Удаление возможностей, выбрав параметр Разрешить удаление смарт-теге GridView. Во-вторых, измените элемент управления ObjectDataSource s `OldValuesParameterFormatString` из значения установлено с помощью мастера ObjectDataSource (`original_{0}`) к значению по умолчанию (`{0}`). После внесения этих изменений, GridView и ObjectDataSource s декларативной разметке должен выглядеть следующим образом:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Протестировать страницу, посетив его через браузер. Как показано на рис. 14, будут отображаться на странице каждого сотрудника и его s имя диспетчера (если они есть).


[![СОЕДИНЕНИЕ в Employees_Select хранимая процедура возвращает имя диспетчера s](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Рис. 14**: `JOIN` в `Employees_Select` хранимая процедура возвращает диспетчер s имя ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


При нажатии кнопки Delete запускается удаление рабочего процесса, который завершающийся выполнения `Employees_Delete` хранимой процедуры. Тем не менее попытка `DELETE` инструкции в хранимой процедуре завершается неудачно из-за нарушения ограничения внешнего ключа (см. рис. 15). В частности, каждый сотрудник имеет одну или несколько записей `Orders` таблицы, вызывая delete переход на другой.


[![Удаление сотрудника с соответствующими результатами заказов в нарушение ограничения внешнего ключа](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Рис. 15**: удаления сотрудника с соответствующими результатами заказов в нарушение ограничения внешнего ключа ([Просмотр полноразмерного изображения](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Чтобы разрешить сотрудника удалена, вы может:

- Ограничение внешнего ключа для каскадное удаление, обновление
- Вручную удалите записи из `Orders` таблицы для сотрудников, которые вы хотите удалить, или
- Обновление `Employees_Delete` хранимую процедуру, чтобы сначала удалить связанные записи из `Orders` таблицы перед удалением `Employees` записи. Мы описывала этот вариант в [существующих хранимых процедур для адаптеров таблиц TableAdapter типизированного набора DataSet s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) руководства.

Оставить этот в качестве упражнения для чтения.

## <a name="summary"></a>Сводка

При работе с реляционными базами данных, обычно для запросов, чтобы извлечь данные из нескольких связанных таблиц. Зависимые вложенные запросы и `JOIN` предоставляет два различных способа для доступа к данным из связанных таблиц в запросе. В предыдущих учебных курсах, мы внесли чаще всего использовать коррелированных вложенных запросов, поскольку TableAdapter не может автоматически создавать `INSERT`, `UPDATE`, и `DELETE` инструкций для запросов с использованием `JOIN` s. Хотя эти значения можно указать вручную, при использовании инструкции SQL ad-hoc все настройки будут перезаписаны при завершении выполнения мастера настройки адаптера таблицы.

К счастью адаптеры таблиц, созданных с помощью хранимых процедур не приводит же хрупкости, как были созданы с помощью нерегламентированных инструкций SQL. Таким образом, можно создать адаптер таблицы использует которого основного запроса `JOIN` при использовании хранимых процедур. В этом руководстве мы рассмотрели создание таких адаптера таблицы. Мы начали с помощью `JOIN`-меньше `SELECT` запрос для основного запроса адаптера таблицы s, чтобы соответствующий вставки, обновления и удаления хранимых процедур будет автоматически создана. С помощью адаптера таблицы s начальная настройка завершена, дополнена необходимостью `SelectCommand` хранимой процедуры для использования `JOIN` и повторного запуска мастера настройки адаптера таблицы для обновления `EmployeesDataTable` s столбцов.

Повторный запуск мастера настройки адаптера таблицы автоматически обновляется `EmployeesDataTable` столбцы для отображения поля данных, возвращенных `Employees_Select` хранимой процедуры. Кроме того удалось добавлена эти столбцы вручную к таблице DataTable. Мы рассмотрим вручную Добавление столбцов к таблице DataTable в следующем учебном курсе.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Хилтон Geisenow, Дэвид Suru и Терезой Мерфи, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Вперед](adding-additional-datatable-columns-cs.md)
