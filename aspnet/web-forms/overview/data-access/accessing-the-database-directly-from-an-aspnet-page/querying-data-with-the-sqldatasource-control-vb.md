---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: "Запрос данных с помощью элемента управления SqlDataSource (VB) | Документы Microsoft"
author: rick-anderson
description: "В учебниках по выше мы использовали управления ObjectDataSource для полностью отделить уровень представления из уровня доступа к данным. Начиная с этого учебного..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a3832bd9847ec8e789b71d13b30a673c8779f4ac
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>Запрос данных с помощью элемента управления SqlDataSource (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) или [скачать PDF](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> В учебниках по выше мы использовали управления ObjectDataSource для полностью отделить уровень представления из уровня доступа к данным. Начиная с этого учебника, рассказано, как можно использовать элемент управления SqlDataSource для простых приложений, которые не требуют таких строгое разделение представления и доступа к данным.


## <a name="introduction"></a>Вступление

Все части руководства мы проверяются в данный момент открывалось многоуровневой архитектуры, состоящий из презентации, бизнес-логики и уровней доступа к данным. Уровень доступа к данным (DAL) изготовлен в первом руководстве ([Создание слой доступа к данным](../introduction/creating-a-data-access-layer-vb.md)) и уровня бизнес-логики в течение секунды ([Создание слой бизнес-логики](../introduction/creating-a-business-logic-layer-vb.md)). Начиная с [отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) учебника, мы узнали, как с помощью элемента управления ASP.NET 2.0 s новый ObjectDataSource декларативно интерфейс с архитектурой от уровня представления.

Во время работы с данными всех руководств в данный момент с помощью архитектуры, также существует возможность доступа к, вставки, обновления и удаления базы данных непосредственно из страницы ASP.NET, минуя архитектуры. Таким образом запросы к определенной базе данных и бизнес-логики помещаются непосредственно в веб-странице. Для достаточно больших или сложных приложений разработке, реализации и использовании многоуровневая архитектура жизненно важна для успеха, возможность обновления и удобства поддержки приложения. Разработка надежной архитектуры, тем не менее, может быть излишним при создании крайне простой, одноразовые приложений.

ASP.NET 2.0 предоставляет элементы управления источником данных пять встроенных [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), и [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource можно использовать для доступа и изменения данных непосредственно из реляционной базы данных, включая Microsoft SQL Server, Microsoft Access, Oracle, MySQL и другим пользователям. В этом учебнике и далее три мы изучим, как работать с помощью элемента управления SqlDataSource, изучение способ запроса и фильтра базы данных, а также как использовать SqlDataSource для вставки, обновления и удаления данных.


![ASP.NET 2.0 включает пять встроенных источников данных](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Рис. 1**: ASP.NET 2.0 включает пять встроенных источников данных


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Сравнение ObjectDataSource и SqlDataSource

По существу элементы управления ObjectDataSource и SqlDataSource являются просто учетных записей-посредников к данным. Как было сказано в [отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) учебнике ObjectDataSource имеет свойства, которые указывают тип объекта, который предоставляет данные и методы для вызова неуправляемого кода, чтобы выбрать, вставка, обновление и удаление данных из базового типа объекта. После настройки свойств s ObjectDataSource данных веб-элемента управления, такие как GridView, DetailsView или DataList могут быть привязаны к элемента управления, используя ObjectDataSource s `Select()`, `Insert()`, `Delete()`, и `Update()` методы взаимодействие с базовой архитектуры.

SqlDataSource предоставляет те же функциональные возможности, но работает с реляционной базой данных, а не библиотеки объектов. В SqlDataSource необходимо указать строку подключения базы данных и нерегламентированные запросы SQL, или хранимых процедур для выполнения для вставки, обновления, удаления и получения данных. SqlDataSource s `Select()`, `Insert()`, `Update()`, и `Delete()` методов, при вызове подключиться к указанной базе данных и создать соответствующий SQL-запрос. Как показано на рисунке, эти методы выполняют grunt соединения с базой данных, выполнение запроса и возвращает результаты.


![SqlDataSource служит в качестве прокси-сервера в базу данных](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**На рисунке 2**: SqlDataSource служит в качестве прокси-сервера в базу данных


> [!NOTE]
> В этом учебнике мы сосредоточимся на получении данных из базы данных. В [Вставка, обновление и удалении данных с помощью элемента управления SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) учебника будет рассмотрена настройка SqlDataSource для поддержки вставки, обновления и удаления.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource и элементы управления AccessDataSource

Помимо управления SqlDataSource ASP.NET 2.0 также включает элемент управления AccessDataSource. Этих двух элементов управления привести многие разработчики знакомы с ASP.NET 2.0, следует полагать, что элемент управления AccessDataSource предназначен для работы только с Microsoft Access с помощью элемента управления SqlDataSource предназначены для работы только с Microsoft SQL Server. Хотя AccessDataSource предназначен специально для работы с Microsoft Access, элемент управления SqlDataSource работает с *любой* реляционная база данных, можно получить с помощью .NET. Сюда входят любые OleDb или ODBC-совместимую хранилищам данных, например Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL и PostgreSQL, многие другие.

Единственным различие между элементами управления AccessDataSource и SqlDataSource — как задается соединение с базой данных. Для элемента управления AccessDataSource необходим только путь к файлу базы данных Access. SqlDataSource, с другой стороны, требуется полная строка подключения.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Шаг 1: Создание SqlDataSource веб-страниц

Прежде чем начать изучение работа непосредственно с базы данных с помощью элемента управления SqlDataSource, позволяют сначала занять некоторое время для создания страниц ASP.NET в данном проекте веб-сайта, он понадобится для этого учебника и трех последующих s. Начните с добавления в новую папку с именем `SqlDataSource`. Добавьте следующие страницы ASP.NET в этой папке, убедитесь, что связать каждую страницу с `Site.master` главной страницы:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Добавление страниц ASP.NET для руководств SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Рис. 3**: Добавление страницы ASP.NET для руководств SqlDataSource


Как и в других папках, `Default.aspx` в `SqlDataSource` папки будут перечислены учебники в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Таким образом, добавьте этот пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений на странице s представление конструктора.


[![Добавление Default.aspx SectionLevelTutorialListing.ascx пользовательского элемента управления](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Рис. 4**: добавление `SectionLevelTutorialListing.ascx` пользовательский элемент управления `Default.aspx` ([Просмотр полноразмерное изображение](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


И, наконец, добавьте эти четыре страницы как операции для `Web.sitemap` файла. В частности, добавьте следующую разметку после добавления кнопок настраиваемый DataList и повторителя `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

После обновления `Web.sitemap`, занять некоторое время для просмотра учебников веб-сайт с помощью браузера. В левом меню теперь включает элементы для редактирования, вставки и удаления учебники.


![Карта сайта теперь включает записи для использования учебников SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Рис. 5**: карта сайта теперь включает записи для использования учебников SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Шаг 2: Добавление и настройка элемента управления SqlDataSource

Сначала откройте `Querying.aspx` страницы в `SqlDataSource` папки и переключитесь в представление конструктора. Перетащите элемент управления SqlDataSource из области элементов в область конструктора и задайте его `ID` для `ProductsDataSource`. Как и в элемент управления ObjectDataSource SqlDataSource не создает готовый для просмотра выходных данных и таким образом отображается как серый прямоугольник в рабочей области конструирования. Чтобы настроить SqlDataSource, щелкните ссылку настроить источник данных смарт-теге SqlDataSource s.


![Выберите команду настроить связь с источником данных в смарт-теге s SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Рис. 6**: щелкните настроить связь с источником данных в смарт-теге s SqlDataSource


Появится мастер настройки источника данных s SqlDataSource элементов управления. Во время действия мастера s, отличаются от элемента управления ObjectDataSource s, конечная цель — то же самое для предоставления сведений о том, как получить, вставки, обновления и удаления данных с помощью источника данных. SqlDataSource это включает указание основной базы данных для использования и предоставления нерегламентированных инструкций SQL или хранимых процедур.

Сначала мастер запрашивает базу данных. Раскрывающегося списка включает эти базы данных в s приложения web `App_Data` папки и те, которые были добавлены в узел подключения данных в обозревателе серверов. Так как мы хранять уже добавлена строка подключения для `NORTHWIND.MDF` базы данных в `App_Data` папки в свой проект s `Web.config` файл в раскрывающемся списке, включает ссылки на этой строки подключения `NORTHWINDConnectionString`. Выберите этот элемент из раскрывающегося списка и нажмите кнопку Далее.


![Выберите из раскрывающегося списка NORTHWINDConnectionString](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Рис. 7**: выберите `NORTHWINDConnectionString` из раскрывающегося списка


После выбора базы данных, то мастер предложит для запроса к данным. Можно либо указать столбцы из таблицы или представления для возврата или можно ввести собственную инструкцию SQL или указать хранимую процедуру. Можно переключаться между этот выбор через определенные пользовательские инструкции SQL или хранимой процедуры и указать столбцы из таблицы или представления переключателей.

> [!NOTE]
> В этом первом примере позволяют использовать укажите столбцы из таблицы или представления параметр s. Мы вернуться в мастер далее в этом учебнике и исследовать указать пользовательские инструкции SQL или параметр хранимых процедур.


На рисунке 8 показана настройка экрана инструкции Select, при выборе укажите столбцы из таблицы или представления переключателя. Список раскрывающийся список содержит набор таблиц и представлений в базе данных Northwind с выбранной таблицы или представления s столбцов, отображаемых в списке флажок внизу. В этом примере возврата позволяют s `ProductID`, `ProductName`, и `UnitPrice` столбцы из `Products` таблицы. Как показано на рис. 8, после внесения выбранные мастер показывает результирующая инструкция SQL `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Возврат данных из таблицы Products](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Рис. 8**: вернуть данные от `Products` таблицы


После настройки мастера, чтобы вернуть `ProductID`, `ProductName`, и `UnitPrice` столбцы из `Products` таблицы, нажмите кнопку "Далее". Данное окно предоставляет возможность проверки результатов запроса, настройки из предыдущего шага. Нажав кнопку Проверить запрос выполняет настроенного `SELECT` оператор и отображает результаты в сетке.


![Нажмите кнопку запроса проверить просмотреть запрос SELECT](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Рис. 9**: нажмите кнопку запроса теста для проверки вашей `SELECT` запроса


Чтобы завершить работу мастера, нажмите кнопку "Готово".

Как и с ObjectDataSource мастер s SqlDataSource просто присваивает значения свойства элемента управления s, а именно [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) и [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) свойства. По завершении работы мастера в SqlDataSource управления s должна выглядеть следующим образом:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

`ConnectionString` Свойстве содержатся сведения о том, как подключиться к базе данных. Это свойство можно назначить значение строки подключения, полным, жестко или может указывать на строку подключения в `Web.config`. Для ссылки на значение строки подключения в файле Web.config, используйте синтаксис `<%$ expressionPrefix:expressionValue %>`. Как правило *expressionPrefix* — ConnectionStrings и *expressionValue* имя строки подключения в `Web.config` [ `<connectionStrings>` раздел](https://msdn.microsoft.com/library/bf7sd233.aspx). Тем не менее, можно использовать синтаксис для ссылки `<appSettings>` элементы или содержимое из файлов ресурсов. В разделе [Общие сведения о выражениях ASP.NET](https://msdn.microsoft.com/library/d5bd1tad.aspx) для получения дополнительных сведений об этом синтаксисе.

`SelectCommand` Свойство указывает нерегламентированные инструкции SQL или хранимую процедуру для выполнения для получения данных.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Шаг 3: Добавление веб-элемент управления данными и ее привязки к SqlDataSource

После настройки SqlDataSource его можно привязать к данным веб-элемента управления GridView и DetailsView. В этом учебнике позволяют отобразить данные в элементе управления GridView s. Из панели элементов перетащите элемент управления GridView на страницу и привяжите его к `ProductsDataSource` SqlDataSource, выбрав источник данных из раскрывающегося списка в смарт-теге s GridView.


[![Добавление элемента управления GridView и привяжите его к элементу управления SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Рис. 10**: Добавление элемента управления GridView и привяжите его к элементу управления SqlDataSource ([Просмотр полноразмерное изображение](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


После выбора элемента управления SqlDataSource можно удалить из раскрывающегося списка в смарт-теге s GridView Visual Studio автоматически добавит BoundField или CheckBoxField GridView для каждого из столбцов, возвращаемых элементом управления источника данных. Так как SqlDataSource возвращает три базы данных столбцы `ProductID`, `ProductName`, и `UnitPrice` существуют три поля в GridView.

Занять некоторое время для настройки s GridView трех стояли. Изменение `ProductName` поля s `HeaderText` свойство названия продукта и `UnitPrice` поля s цене. Также отформатировать `UnitPrice` поле как денежное значение. После внесения этих изменений, к GridView s должна выглядеть следующим образом:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Посетите эту страницу через браузер. Как показано на рис. 11, GridView, выводит список всех продуктов s `ProductID`, `ProductName`, и `UnitPrice` значения.


[![GridView выведет на экран каждого продукта s ProductID, ProductName и UnitPrice значения](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Рис. 11**: s GridView отображает каждый продукт `ProductID`, `ProductName`, и `UnitPrice` значения ([Просмотр полноразмерное изображение](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


При просмотре GridView вызывает его источника данных s `Select()` метод. Когда мы использовали управления ObjectDataSource, это называется `ProductsBLL` класса s `GetProducts()` метод. В SqlDataSource, однако `Select()` метод устанавливает подключение к указанной базе данных и проблемы `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, в этом примере). SqlDataSource возвращает свои результаты, которые GridView затем перечисляется, создания строк в GridView для каждой записи базы данных.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Возможности управления Web встроенных данных и управления SqlDataSource

В общем, функции, присущие данных, веб-элементы управления, разбиение по страницам, сортировка, редактирование, удаление, вставка и т. д относящиеся к веб-управления данными и не зависят от элемента управления источником данных используется. То есть GridView можно воспользоваться встроенного разбивка на страницы, сортировку, редактирование и удаление ли он привязан к элементу ObjectDataSource или SqlDataSource. Тем не менее определенные данные веб-средства управления чувствительны к элементу управления источником данных используются или для управления s конфигурации источника данных.

Например, в [эффективно подкачки через большие объемы данных](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) учебника мы рассмотрели, как, по умолчанию логика разбиения на страницы данных Web управляет просто возвращает *всех* записей из базовой источник данных и затем отображает соответствующего подмножества записей индекса текущей страницы и число записей, отображаемых на каждой странице. Эта модель является крайне неэффективно при постраничного просмотра достаточно больших результирующих наборов. К счастью элемент управления ObjectDataSource можно настроить для поддержки пользовательское разбиение по страницам, который возвращает только точные подмножество записей для отображения. Тем не менее, элемент управления SqlDataSource отсутствуют свойства для реализации пользовательского разбиения на страницы.

С SqlDataSource возникает другой тонкости с разбиение по страницам и сортировка. По умолчанию данные, возвращенные из SqlDataSource можно выгружать или сортируются GridView. Чтобы продемонстрировать это, проверьте параметры включить разбиение на страницы и включить сортировку в GridView s смарт-тега в `Querying.aspx` и убедитесь, что это работает ожидаемым образом.

Сортировка и разбиение по страницам работает, поскольку SqlDataSource извлекает базы данных в слабо типизированный набор данных. Возможно установить общее количество записей, возвращаемых запросом важный аспект для реализации подкачки из набора данных. Кроме того можно сортировать результаты s набора данных через объект DataView. Эти возможности автоматически используются SqlDataSource при запросы GridView, разбитых на страницы или отсортированные данные.

SqlDataSource могут быть настроены для возвращения DataReader вместо набора данных, изменив его [ `DataSourceMode` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) из `DataSet` (по умолчанию) для `DataReader`. С помощью объекта DataReader может предпочтительный в ситуациях, при передаче результатов s SqlDataSource существующего кода, который ожидает объекта DataReader. Кроме того поскольку объекты DataReader объектов значительно проще, чем наборы данных, они обеспечивают более высокую производительность. Если внести это изменение, веб-управления данными ни можно сортировать и страницы, поскольку SqlDataSource невозможно выяснить, сколько записей возвращаемых запросом, а также DataReader предоставляют все методы для сортировки возвращаемых данных.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Шаг 4: Использование инструкции пользовательские SQL или хранимой процедуры

При настройке элемента управления SqlDataSource, запрос, используемый для возврата данных можно указать одним из двух способов, как пользовательские инструкции SQL или хранимой процедуры или как столбцы из существующей таблицы или представления. На шаге 2 мы рассмотрели Выбор столбцов из `Products` таблицы. Разрешить использовать пользовательскую инструкцию SQL s.

Добавление другого элемента управления GridView к `Querying.aspx` странице и выберите Создание нового источника данных из раскрывающегося списка в смарт-тег. Затем указать, что данные будут извлекаться из базы данных это создаст новый элемент управления SqlDataSource. Имя элемента управления `ProductsWithCategoryInfoDataSource`.


![Создание нового элемента управления SqlDataSource с именем ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Рис. 12**: Создание нового элемента управления SqlDataSource с именем`ProductsWithCategoryInfoDataSource`


Следующий экран появляется запрос на укажите базу данных. Как это делалось обратно на рис. 7 выберите `NORTHWINDConnectionString` из раскрывающегося списка и нажмите кнопку Далее. В настройки на экране инструкции Select Выбор указать пользовательские инструкции SQL или хранимой процедуры "переключатель" и нажмите кнопку Далее. Появится экран Определите специальные операторы или хранимые процедуры, которая содержит вкладки с меткой SELECT, UPDATE, INSERT и DELETE. На каждой из вкладок можно ввести собственную инструкцию SQL в текстовое поле или выбрать хранимую процедуру из раскрывающегося списка. В этом учебнике мы рассмотрим ввод пользовательские инструкции SQL; Далее учебник содержит пример использования хранимой процедуры.


![Введите инструкцию SQL, пользовательские или выбора хранимой процедуры](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Рис. 13**: Введите инструкцию SQL, пользовательские или выбора хранимой процедуры


Пользовательские инструкции SQL, можно вручную ввести в текстовое поле или могут создаваться в графическом виде, нажав кнопку построителя запросов. Из построителя запросов или в текстовое поле, используйте следующий запрос для возврата `ProductID` и `ProductName` поля из `Products` таблицу с помощью `JOIN` для получения продукта s `CategoryName` из `Categories` таблицы:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![Можно графически создать запрос с помощью построителя запросов](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Рис. 14**: вы можете создать графически запроса с помощью построителя запросов


После указания запроса, нажмите кнопку Далее, чтобы перейти к экрану проверить запрос. Нажмите кнопку Готово, чтобы завершить работу мастера SqlDataSource.

По завершении работы мастера GridView будет иметь три стояли, добавить к нему отображение `ProductID`, `ProductName`, и `CategoryName` столбцов, возвращенных запросом и приведет к следующей декларативная разметка:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![Каждый идентификатор продукта s, имя категории, имя и связанный показывает GridView](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Рис. 15**: s идентификатор GridView показывает каждого продукта, имя и связанное имя категории ([Просмотр полноразмерное изображение](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>Сводка

В этом учебнике мы узнали, как запрос и отображение данных с помощью элемента управления SqlDataSource. Как элемент управления ObjectDataSource SqlDataSource служит в качестве прокси-сервера, предоставляя декларативный подход, доступ к данным. Укажите свойства для подключения к базе данных и SQL `SELECT` запросом для выполнения; их можно задать в окне «Свойства» или с помощью мастера настройки источника данных.

`SELECT` Примеры запросов, мы рассмотрели в этом учебнике возвращены все записи из указанного запроса. Тем не менее, можно включить элемент управления SqlDataSource `WHERE` предложение с параметрами, значения которых были назначены программными средствами, или автоматически извлекаются из указанного источника. Мы рассмотрим, как создать и использовать параметризованные запросы в следующем уроке!

Программирование довольны!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Доступ к реляционным базам данных](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Обзор элемента управления SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Примеры использования ASP.NET учебники: Элемент управления SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Файл Web.config `<connectionStrings>` элемент](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Ссылка строки подключения базы данных](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Connery Сьюзан, Екатерина Leigh и Дэвид Suru. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
[Вперед](using-parameterized-queries-with-the-sqldatasource-vb.md)
