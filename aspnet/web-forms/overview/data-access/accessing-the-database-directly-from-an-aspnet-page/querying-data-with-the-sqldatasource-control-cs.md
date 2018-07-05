---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Запрос данных с помощью элемента управления SqlDataSource (C#) | Документация Майкрософт
author: rick-anderson
description: В предшествующих учебных курсах мы использовали элемент управления ObjectDataSource для полностью отделить слой представления от слоя доступа к данным. Начиная с этого учебного...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 0bde7d76db2da71b3f12fc9ead2baa4b2028206d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369813"
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>Запрос данных с помощью элемента управления SqlDataSource (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) или [скачать PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> В предшествующих учебных курсах мы использовали элемент управления ObjectDataSource для полностью отделить слой представления от слоя доступа к данным. Начиная с этим руководством, мы Узнайте, как можно использовать элемент управления SqlDataSource для простых приложений, которые не требуют таких строгое разделение представления и доступа к данным.


## <a name="introduction"></a>Вступление

Все учебные мы ve проверяется в данный момент используется многоуровневая архитектура, состоящий из представления, бизнес-логику и слои доступа к данным. Уровень доступа к данным (DAL) изготовлен в первом руководстве ([создание уровня доступа к данным](../introduction/creating-a-data-access-layer-cs.md)) и уровня бизнес-логики во втором ([Создание слой бизнес-логики](../introduction/creating-a-business-logic-layer-cs.md)). Начиная с [отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) было показано, как использовать ASP.NET 2.0 s элемента управления ObjectDataSource декларативно взаимодействовать с архитектурой от слоя представления.

Хотя всех руководств, имеющихся в данный момент использовали архитектуры для работы с данными, его можно также получить доступ к, вставки, обновления и удаления базы данных непосредственно из страницы ASP.NET, обходя архитектуры. Таким образом помещает запросы к определенной базе данных и бизнес-логику непосредственно в веб-страницы. Для достаточно больших или сложных приложений проектировании, реализации и использовании многоуровневые архитектуры жизненно важна для успеха, возможность обновления и удобство поддержки приложения. Разработка надежной архитектуры, тем не менее, может быть излишним при создании приложений крайне простой и одноразовые.

ASP.NET 2.0 предоставляет пять встроенных данных элементами управления источниками [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), и [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). Элемента управления SqlDataSource можно использовать для доступа и изменения данных непосредственно из реляционной базы данных, включая Microsoft SQL Server, Microsoft Access, Oracle, MySQL и другим пользователям. В этом учебнике и трех последующих мы рассмотрим, как работать с помощью элемента управления SqlDataSource, изучение, как выполнять запросы и фильтрации данных базы данных, а также способы использования элемента управления SqlDataSource для вставки, обновления и удаления данных.


![ASP.NET 2.0 включает пять встроенных источников данных](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Рис. 1**: ASP.NET 2.0 включает пять встроенных источников данных


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Сравнение ObjectDataSource и SqlDataSource

По существу элемент управления ObjectDataSource и SqlDataSource элементы управления являются просто учетных записей-посредников к данным. Как уже говорилось в [отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) руководстве элемент управления ObjectDataSource имеет свойства, указывающие тип объекта, который предоставляет данные и методы для вызова для выбора, вставки, обновления и удаления данных из базового типа объекта. После настройки свойств s ObjectDataSource можно привязкой к данным веб-элемента управления, например GridView, DetailsView или DataList, к элементу управления, с помощью FTPS ObjectDataSource `Select()`, `Insert()`, `Delete()`, и `Update()` методы взаимодействие с базовой архитектурой.

Элемента управления SqlDataSource предоставляет те же функциональные возможности, но работает с реляционной базой данных, а не библиотеку объектов. С помощью элемента управления SqlDataSource необходимо указать строку подключения базы данных и SQL-запросов ad-hoc, или хранимые процедуры, которые необходимо выполнить, чтобы вставлять, обновлять, удалять и получения данных. SqlDataSource s `Select()`, `Insert()`, `Update()`, и `Delete()` методов, при вызове подключиться к указанной базе данных и выдают соотвтетствующий запрос SQL. Как показано на рисунке, эти методы выполняют тяжелую рутинную работу соединения с базой данных, выполнение запроса и возвращения результатов.


![Элемента управления SqlDataSource служит в качестве прокси-сервера в базу данных](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Рис. 2**: элемента управления SqlDataSource служит в качестве прокси-сервера в базу данных


> [!NOTE]
> В этом руководстве мы сосредоточимся на извлечение данных из базы данных. В [Вставка, обновление и удаление данных с помощью элемента управления SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) учебника, будет рассмотрена настройка элемента управления SqlDataSource для поддержки вставки, обновления и удаления.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource и элементы управления AccessDataSource

Помимо элемента управления SqlDataSource ASP.NET 2.0 также включает элемент управления AccessDataSource. Этих двух элементов управления привести многие разработчики знакомы с ASP.NET 2.0, следует полагать, что элемент управления AccessDataSource предназначен для работы исключительно с Microsoft Access с помощью элемента управления SqlDataSource, разработанные для работы исключительно с Microsoft SQL Server. Хотя AccessDataSource предназначен специально для работы с Microsoft Access, элемента управления SqlDataSource работает с *любой* реляционной базы данных, который можно получить с помощью .NET. Сюда входят любые OleDb или ODBC-совместимую хранилищ данных, таких как Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL и PostgreSQL, среди многих других.

Единственная разница между элементами управления AccessDataSource и SqlDataSource является, как указывается подключение к базе данных. Элемент управления AccessDataSource должен просто путь к файлу базы данных Access. Элемента управления SqlDataSource, с другой стороны, требует полной строки соединения.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Шаг 1: Создание элемента управления SqlDataSource веб-страниц

Прежде чем приступать, показывающих, как работать непосредственно с базам данных с помощью элемента управления SqlDataSource, позвольте s уделим несколько минут созданию страниц ASP.NET в нашем проекте веб-сайта, что нам понадобится для изучения этого учебника и следующие три. Начните с добавления новой папки с именем `SqlDataSource`. Добавьте следующие страницы ASP.NET в этой папке, не забывая связывать каждую с `Site.master` главной страницы:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Добавление страниц ASP.NET для элемента управления SqlDataSource руководств](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Рис. 3**: Добавление страниц ASP.NET для элемента управления SqlDataSource руководств


Как и в других папках, `Default.aspx` в `SqlDataSource` папку перечислит учебные курсы в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Поэтому добавьте данный пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений на странице s режиме конструктора.


[![Добавление элемента управления Sectionleveltutoriallisting.ascx к странице Default.aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Рис. 4**: добавление `SectionLevelTutorialListing.ascx` для пользовательского элемента управления `Default.aspx` ([Просмотр полноразмерного изображения](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))


Наконец, добавьте эти четыре страницы как записи для `Web.sitemap` файла. В частности, добавьте следующую разметку после Добавление настраиваемых кнопок для элементов управления DataList и Repeater `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

После обновления `Web.sitemap`, Отвлекитесь и просмотрите учебный веб-узел в обозревателе. В меню слева теперь есть элементы для редактирования, вставки и удаления учебных курсов.


![Карта узла теперь включают записи для элемента управления SqlDataSource учебники](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Рис. 5**: карта узла теперь включают записи для элемента управления SqlDataSource учебники


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Шаг 2: Добавление и настройка элемента управления SqlDataSource

Сначала откройте `Querying.aspx` странице в `SqlDataSource` папку и переключиться в режим конструктора. Перетащите элемент управления SqlDataSource из области элементов в конструктор и задайте его `ID` для `ProductsDataSource`. С помощью элемента управления ObjectDataSource, SqlDataSource не создает все выводимые данные и таким образом отображается как серый квадрат в области конструктора. Настройка элемента управления SqlDataSource, щелкните ссылку Настройка источника данных в смарт-теге элемента управления SqlDataSource s.


![Щелкните ссылка Configure Data Source в смарт-теге элемента управления SqlDataSource s](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Рис. 6**: щелкните ссылка Configure Data Source в смарт-теге элемента управления SqlDataSource s


Откроется мастер настройки источника данных элемента управления SqlDataSource s элемента управления. Хотя мастер s отличаются от элемента управления ObjectDataSource s, конечная цель — так же, для предоставления сведений о том, как для извлечения, вставки, обновления и удаления данных с помощью источника данных. Для элемента управления SqlDataSource это влечет за собой указание основной базы данных для использования и предоставления нерегламентированных инструкций SQL или хранимых процедур.

Первым шагом мастер запрашивает для базы данных. Выберите в раскрывающемся списке включает эти базы данных в s приложения web `App_Data` папки и запросы, которые были добавлены к узлу подключения к данным в обозревателе серверов. Так как мы уже добавили строку подключения для `NORTHWIND.MDF` базы данных в `App_Data` папку, чтобы наш проект s `Web.config` файл, выберите в раскрывающемся списке включает ссылку на эту строку подключения, `NORTHWINDConnectionString`. Выберите этот элемент в раскрывающемся списке и нажмите кнопку Далее.


![Выберите в раскрывающемся списке NORTHWINDConnectionString](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Рис. 7**: выберите `NORTHWINDConnectionString` из раскрывающегося списка


После выбора базы данных, мастер попросит указать запрос для возврата данных. Можно либо указать столбцы из таблицы или представления, чтобы вернуть или ввести собственную инструкцию SQL или указать хранимую процедуру. Можно переключаться между этот выбор через определенные пользовательские инструкции SQL или хранимой процедуры и указать столбцы из таблицы или просмотр переключателей.

> [!NOTE]
> Для этого первого примера позволяют использовать укажите столбцы из таблицы или представления параметр s. Мы вернуться в мастер далее в этом руководстве и изучите указать собственную инструкцию SQL или параметр хранимых процедур.


Рис. 8 показан настройки на экране инструкции Select, при выборе укажите столбцы из таблицы или представления переключателя. Выберите в раскрывающемся списке содержит набор таблиц и представлений в базе данных "Борей" с выбранной таблицы или представления s столбцов, отображаемых в списке флажок внизу. В этом примере возврата let s `ProductID`, `ProductName`, и `UnitPrice` столбцы из `Products` таблицы. Как показано на рис. 8, когда эти выбора Мастер покажет результирующая инструкция SQL `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Возврат данных из таблицы Products](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Рис. 8**: вернуть данные от `Products` таблицы


После настройки мастера, чтобы вернуть `ProductID`, `ProductName`, и `UnitPrice` столбцы из `Products` таблицы, нажмите кнопку "Далее". Данное окно предоставляет возможность проверить результаты запроса, настроенный на предыдущем шаге. Нажав кнопку Проверить запрос выполняет настроенного `SELECT` оператор и отображает результаты в сетке.


![Щелкните кнопку тестирования запроса, чтобы просмотреть запрос SELECT](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Рис. 9**: нажмите кнопку тестирования запрос на проверку вашего `SELECT` запроса


Завершите работу мастера, нажмите кнопку "Готово".

Как и с помощью ObjectDataSource, SqlDataSource мастер s просто присваивает значения свойства элемента управления s, а именно [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) и [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) свойства. После завершения работы мастера, ваш элемент управления SqlDataSource s декларативная разметка должен выглядеть следующим образом:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

`ConnectionString` Свойстве содержатся сведения о том, как соединиться с базой данных. Это свойство можно назначить полный, жестко значение строки подключения или может указывать на строку подключения в `Web.config`. Чтобы сослаться на значение строки подключения в файле Web.config, используйте синтаксис `<%$ expressionPrefix:expressionValue %>`. Как правило *expressionPrefix* является ConnectionStrings и *expressionValue* имя строки подключения в `Web.config` [ `<connectionStrings>` разделе](https://msdn.microsoft.com/library/bf7sd233.aspx). Тем не менее, можно использовать синтаксис ссылка `<appSettings>` элементы или содержимое из файлов ресурсов. См. в разделе [Общие сведения о выражениях ASP.NET](https://msdn.microsoft.com/library/d5bd1tad.aspx) Дополнительные сведения об этом синтаксисе.

`SelectCommand` Свойство определяет специальный оператор SQL или хранимая процедура, выполняющаяся для возврата данных.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Шаг 3: Добавление веб-элемент данных и привязки для элемента управления SqlDataSource

После настройки элемента управления SqlDataSource, его можно привязать к данным веб-элемента управления GridView или DetailsView. Для этого руководства позволяют отобразить данные в элементе управления GridView s. Из панели элементов перетащите элемент управления GridView на страницу и привяжите его к `ProductsDataSource` SqlDataSource, выбрав источник данных из раскрывающегося списка в смарт-теге GridView s.


[![Добавьте элемент управления GridView и привяжите его к элементу управления SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Рис. 10**: Добавление элемента управления GridView и привяжите его к элементу управления SqlDataSource ([Просмотр полноразмерного изображения](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))


После выбора элемента управления SqlDataSource из раскрывающегося списка в смарт-теге GridView s вам продуктом Visual Studio автоматически добавит BoundField или CheckBoxField GridView для каждого из столбцов, возвращаемых элементом управления источником данных. Так как элемента управления SqlDataSource возвращает три базы данных столбцы `ProductID`, `ProductName`, и `UnitPrice` в GridView есть три поля.

Отвлекитесь и настройка s GridView трех полей BoundField. Изменение `ProductName` поле s `HeaderText` свойство, чтобы название продукта и `UnitPrice` поле s цене. Также отформатировать `UnitPrice` как денежная единица. После внесения этих изменений, декларативная разметка s GridView должен выглядеть следующим образом:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Посетите эту страницу через обозреватель. Как показано на рис. 11, GridView перечисляет каждый продукт s `ProductID`, `ProductName`, и `UnitPrice` значения.


[![Элемент GridView отображает по каждого продукта s ProductID, ProductName и UnitPrice значения](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Рис. 11**: s GridView отображает каждый продукт `ProductID`, `ProductName`, и `UnitPrice` значения ([Просмотр полноразмерного изображения](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))


При просмотре GridView вызывает его элемент управления источником данных s `Select()` метод. Когда мы использовали элемент управления ObjectDataSource, это называется `ProductsBLL` класс s `GetProducts()` метод. С помощью элемента управления SqlDataSource, однако `Select()` метод подключается к указанной базе данных и проблемы `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, в этом примере). Элемента управления SqlDataSource возвращает свои результаты, которые GridView затем перечисляется, создавая строку в GridView для каждой записи базы данных.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Функции управления веб встроенных данных и элемента управления SqlDataSource

В целом, возможностей, имеющиеся данные веб-элементов управления, разбиение по страницам, сортировки, редактирования, удаления, вставки и т. д. относятся к веб-управления данными и не зависящие от элемента управления источником данных используется. То есть GridView может использовать встроенного разбиение по страницам, сортировку, редактирование и удаление ли он привязан к элементу ObjectDataSource и SqlDataSource. Однако определенные данные веб-средства управления чувствительны к используется элемент управления источником данных или элемента управления s конфигурацию источника данных.

Например, в [эффективного разбиения на страницы через больших объемов данных](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) учебнике мы рассмотрели, как это сделать, по умолчанию логики разбиения на страницы для данных Web управляет наивно возвращает *все* записей из базового источник данных и затем отображает соответствующее подмножество записей индекса текущей страницы и количество записей, отображаемых на одной странице. Эта модель является крайне неэффективно, если разбиение по страницам достаточно больших наборах результатов. К счастью элемент управления ObjectDataSource можно настроить для поддержки пользовательского разбиения по страницам, который возвращает только точные подмножества записей для отображения. Элемента управления SqlDataSource, тем не менее, не имеет свойства для реализации пользовательского разбиения по страницам.

С помощью элемента управления SqlDataSource возникает другой тонкость с разбиение по страницам и сортировка. По умолчанию данные, возвращенные из элемента управления SqlDataSource можно выгружать или сортируются GridView. Чтобы продемонстрировать это, проверьте параметры включить разбиение по страницам и включить сортировку GridView s смарт-тега в `Querying.aspx` и убедитесь, что все работает должным образом.

Сортировка и разбиение на страницы работает, так как элемента управления SqlDataSource извлекает данные базы данных в слабо типизированный набор DataSet. Возможно установить общее число записей, возвращаемых запросом важный аспект для реализации подкачки из набора данных. Кроме того можно сортировать результаты s набора данных через DataView. Эти возможности автоматически используются элемента управления SqlDataSource, когда запросы GridView, разбитых на страницы и отсортированные данные.

Можно настроить элемента управления SqlDataSource для возврата объекта DataReader вместо набора данных, изменив его [ `DataSourceMode` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) из `DataSet` (по умолчанию) для `DataReader`. С помощью объекта DataReader может предпочтительный в ситуациях при передаче результатов s SqlDataSource для существующего кода, который ожидает объекта DataReader. Кроме того так как объекты DataReader объектов значительно проще, чем наборы данных, они обеспечивают более высокую производительность. При внесении этого изменения, но веб-управления данными не можно сортировать и страницы, так как элемента управления SqlDataSource невозможно выяснить, сколько записей возвращаемых запросом, а также назначения DataReader предлагают любые методы сортировка возвращаемых данных.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Шаг 4: Использование инструкции настраиваемый SQL или хранимой процедуры

При настройке элемента управления SqlDataSource, запрос, который используется для возвращения данных можно указать в одном из двух подходов, как пользовательские инструкции SQL или хранимой процедуры или как столбцы из существующей таблицы или представления. На шаге 2, мы рассмотрели Выбор столбцов из `Products` таблицы. Разрешить использовать пользовательскую инструкцию SQL s.

Добавление другого элемента управления GridView для `Querying.aspx` странице и создать новый источник данных из раскрывающегося списка в смарт-теге. Затем указать, что данные будут извлекаться из базы данных это создаст новый элемент управления SqlDataSource. Назовите элемент управления `ProductsWithCategoryInfoDataSource`.


![Создание нового элемента управления SqlDataSource с именем ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Рис. 12**: Создание нового элемента управления SqlDataSource с именем `ProductsWithCategoryInfoDataSource`


Следующий экран появляется запрос на укажите базу данных. Как и обратно на рис. 7, выберите `NORTHWINDConnectionString` из раскрывающегося списка и нажмите кнопку Далее. В настройки на экране инструкции Select выберите Specify собственную инструкцию SQL или хранимой процедуры переключатель и нажмите кнопку Далее. Откроется экран определение пользовательских инструкций или хранимых процедур, что имеются вкладки с меткой SELECT, UPDATE, INSERT и DELETE. На каждой вкладке можно ввести собственную инструкцию SQL в текстовое поле или выбрать хранимую процедуру из раскрывающегося списка. В этом руководстве мы рассмотрим ввод пользовательские инструкции SQL; Следующее руководство включает в себя пример использования хранимой процедуры.


![Введите инструкцию SQL, пользовательских или выбрать хранимую процедуру](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Рис. 13**: Введите инструкцию SQL, пользовательских или выбрать хранимую процедуру


Пользовательские инструкции SQL могут быть вручную введены в текстовом поле или может быть создан в графическом виде, нажав кнопку построителя запросов. Построитель запросов или текстового поля, используйте следующий запрос для возврата `ProductID` и `ProductName` поля из `Products` таблицу с помощью `JOIN` извлекаемого продукта s `CategoryName` из `Categories` таблицы:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![Вы можете графически создать запрос, используя построитель запросов](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Рис. 14**: вы можете графически создать запрос с помощью построителя запросов


После указания запроса, нажмите кнопку "Далее", чтобы перейти к экрану Проверка запроса ". Нажмите кнопку Finish в мастере создания элемента управления SqlDataSource.

По завершении работы мастера GridView будет иметь три поля BoundField, кроме добавлен на нее отображение `ProductID`, `ProductName`, и `CategoryName` столбцы, возвращенные из запроса и приводит к следующей декларативной разметке:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![Все КОДЫ продуктов s, имя категории, имя и связанный показывает GridView](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Рис. 15**: идентификатор GridView показывает каждого продукта, имя и связанное имя категории ([Просмотр полноразмерного изображения](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))


## <a name="summary"></a>Сводка

В этом учебнике мы рассмотрели для запроса и отображения данных с помощью элемента управления SqlDataSource. Как элемент управления ObjectDataSource SqlDataSource служит в качестве прокси-сервер, предоставляя декларативный подход для доступа к данным. Укажите свойства для подключения к базе данных и SQL `SELECT` запросом для выполнения; они могут быть указаны в окне «Свойства» или с помощью мастера настройки источника данных.

`SELECT` Примеры запросов, изученные в этом руководстве возвращаются все записи из указанного запроса. Тем не менее, можно включить элемент управления SqlDataSource `WHERE` предложение с параметрами, значения которых назначаются программным способом или автоматически извлекаются из указанного источника. Как создать и использовать параметризованные запросы в следующем учебном курсе будет рассмотрен!

Счастливого вам программирования!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, обсуждавшимся в этом руководстве см. в следующих ресурсах:

- [Доступ к реляционным базам данных](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Обзор элемента управления SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Основам безопасности ASP.NET: Элемент управления SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Файл Web.config `<connectionStrings>` элемент](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Ссылка строки подключения базы данных](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Сьюзан Connery Leigh Екатерина и Дэвид Suru, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Вперед](using-parameterized-queries-with-the-sqldatasource-cs.md)
