---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: "Создание поставщика карты сайта пользовательские базы данных (C#) | Документы Microsoft"
author: rick-anderson
description: "По умолчанию поставщик карты узла в ASP.NET 2.0 извлекает свои данные из статических XML-файла. Несмотря на то поставщика на основе XML подходит к большинству малый и средний размер..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: cc0de856cb1ae2cf8e1f18a29ae29a3b226c12ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="building-a-custom-database-driven-site-map-provider-c"></a>Создание поставщика карты сайта пользовательские базы данных (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) или [скачать PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> По умолчанию поставщик карты узла в ASP.NET 2.0 извлекает свои данные из статических XML-файла. Хотя поставщика на основе XML, подходит для многих небольших и средних веб-сайтов, больше веб-приложения требуют более динамической карты сайта. В данном руководстве, мы выполним сборку поставщика карты пользовательский узел, который извлекает данные из бизнес-логики, который в свою очередь извлекает данные из базы данных.


## <a name="introduction"></a>Вступление

ASP.NET 2.0 функция карты сайта s позволяет разработчику определить карты сайта приложения s web некоторых постоянных носителе, например в XML-файл. После определения данных карты сайта может осуществляться программными средствами с помощью [ `SiteMap` класса](https://msdn.microsoft.com/library/system.web.sitemap.aspx) в [ `System.Web` имен](https://msdn.microsoft.com/library/system.web.aspx) или через разнообразные навигации веб-элементы управления, такие как Элементы управления SiteMapPath, меню и TreeView. Система карты сайта использует [модель поставщика](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , чтобы реализации сериализации другой узел карты можно создавать и подключать веб-приложения. Поставщик карты сайта по умолчанию, который поставляется вместе с ASP.NET 2.0 сохраняется структура карты узла в XML-файл. В [главные страницы и переходов](../introduction/master-pages-and-site-navigation-cs.md) учебника мы создали в файл с именем `Web.sitemap` , содержащиеся в этой структуре и комплектации XML с каждый новый раздел учебника.

Поставщик карты XML-узел по умолчанию работает также в том случае, если структура карты сайта довольно статические, такие как для этих учебников. Во многих сценариях Однако, более динамической карты сайта требуется. Рассмотрите возможность карты сайта, приведенный на рисунке 1, где каждый category и product отображаются как разделы в структуре s веб-сайта. С этой картой сайта посещении веб-страницы, соответствующий корневой узел может список всех категорий, тогда как посещения веб-страницы определенной категории s будет выведен список s этой категории продуктов и просмотр веб-страницы конкретного продукта s будет подробно продукта s.


[![Категории и продукты состава структуры s карты сайта](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Рис. 1**: категорий и продуктов состава s карты узла структуры ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))


Во время этой структуры на основе категории и продукта может быть жестко закодирована в `Web.sitemap` -файл, файл будет необходимо обновить каждый раз, когда категории или продукта был добавлен, удален или переименован. Таким образом узел карты значительно упрощает поддержку при ее структуру был извлечен из базы данных или, в идеальном случае уровне бизнес-логики s архитектуры приложения. Таким образом, как были добавлены продукты и категории, переименован или удален, карта сайта будет автоматически обновляться для отражения этих изменений.

Так как карта сайта сериализации для ASP.NET 2.0 s строится поверх модель поставщика, можно создать собственную поставщика карты, извлекает данные из хранилища данных, например базы данных или архитектуры. В этом учебнике мы создадим пользовательский поставщик, который извлекает данные из МЕТОДА. S позволяют начать работу!

> [!NOTE]
> Поставщик карты пользовательского узла, созданной в этом учебнике тесно связана с s архитектуры и данные модели приложения. Джефф Просайз s [хранения карты сайта в SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) и [поставщик карты узла SQL можно хранить ожидал](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) статьи изучите обобщенный подход для хранения данных карты сайта в SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Шаг 1: Создание пользовательского узла поставщика карты веб-страниц

Прежде чем начать создание поставщика карты, позволяют s сначала добавить на страницах ASP.NET, которые потребуются для этого учебника. Начните с добавления в новую папку с именем `SiteMapProvider`. Добавьте следующие страницы ASP.NET в этой папке, убедитесь, что связать каждую страницу с `Site.master` главной страницы:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Кроме того, добавить `CustomProviders` вложенную папку для `App_Code` папки.


![Добавление страницы ASP.NET учебников связанные с поставщиком карты сайта](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**На рисунке 2**: Добавление страницы ASP.NET для сайта сопоставить учебные материалы, связанные с поставщиком


Так как существует только один учебник для этого раздела, мы не хотите необходимость t `Default.aspx` списка учебников секции s. Вместо этого `Default.aspx` категории отображаются в элементе управления GridView. Мы будем разбираться в шаге 2.

Затем обновите `Web.sitemap` включать ссылку на `Default.aspx` страницы. В частности, добавьте следующую разметку после кэширования `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

После обновления `Web.sitemap`, занять некоторое время для просмотра учебников веб-сайт с помощью браузера. В левом меню теперь включает элемент учебника поставщика карты единственный узел.


![Карта сайта теперь содержит запись для учебника поставщика карты сайта](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Рис. 3**: карта сайта теперь содержит запись для учебника поставщика карты сайта


Этот учебник s основной задачей которого является демонстрация создания поставщика карты и настройке веб-приложения для использования этого поставщика. В частности мы выполним сборку поставщика, который возвращает карты сайта, включающий корневой узел и узел для каждой категории и продукта, как показано на рисунке 1. Как правило каждый узел в карте узла может указать URL-адрес. Карта узла узел s корневой URL-адрес будет `~/SiteMapProvider/Default.aspx`, который отображает список всех категорий в базе данных. Каждый узел категории в карте узла будет иметь URL-адрес, указывающий на `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, который отображает список всех продуктов в указанном *categoryID*. Наконец, узел карты каждого продукта будет указывать на `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, который будет отображать данные конкретного продукта s.

Для запуска необходимо создать `Default.aspx`, `ProductsByCategory.aspx`, и `ProductDetails.aspx` страниц. Эти страницы выполняются в шагах 2, 3 и 4, соответственно. Потому надежность работы с этим учебником включен поставщиков карты веб-узла, а последние учебники рассмотрели создание такого рода многостраничную иерархического сообщает, мы будет спешке через шаги 2 – 4. Если необходимо создание главного и подчиненного представлений отчетов, которые охватывают несколько страниц в памяти, обращаться к [иерархического фильтрации по две страницы](../masterdetail/master-detail-filtering-across-two-pages-cs.md) учебника.

## <a name="step-2-displaying-a-list-of-categories"></a>Шаг 2: Отображение список категорий

Откройте `Default.aspx` страницы в `SiteMapProvider` папки и перетащите элемент управления GridView с панели элементов в конструктор параметр его `ID` для `Categories`. В смарт-теге s GridView, привязать его к новой ObjectDataSource с именем `CategoriesDataSource` и настроить его таким образом, чтобы он извлекает его данные с помощью `CategoriesBLL` класса s `GetCategories` метод. Поскольку это GridView просто отображает категории и не предоставляет возможности модификации данных, установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет).


[![Настройка ObjectDataSource для возврата категории с помощью метода GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Рис. 4**: Настройка ObjectDataSource возвращают категории с помощью `GetCategories` метод ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))


[![Установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Рис. 5**: задать раскрывающихся списков в обновления, вставки и удаления вкладок (нет) ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))


После завершения работы мастера настройки источника данных Visual Studio добавит BoundField для `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, и `BrochurePath`. Изменить GridView, чтобы он содержал только `CategoryName` и `Description` стояли и обновить `CategoryName` BoundField s `HeaderText` свойства категории.

Затем добавьте HyperLinkField и разместите его, так что он s поле слева. Задать `DataNavigateUrlFields` свойства `CategoryID` и `DataNavigateUrlFormatString` свойства `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Задать `Text` свойства для просмотра продуктов.


![Добавление категории GridView HyperLinkField](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Рис. 6**: Добавление HyperLinkField для `Categories` GridView


После создания ObjectDataSource и настройка полей s GridView, два элемента управления будет выглядеть следующим образом:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

На рисунке 7 показано `Default.aspx` при просмотре через браузер. Если щелкнуть имя категории s Просмотр продуктов ссылке вы перейдете к `ProductsByCategory.aspx?CategoryID=categoryID`, который будет создан на шаге 3.


[![Каждой категории — в списке вместе со ссылкой продуктов представления](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Рис. 7**: каждой категории — в списке вместе со ссылкой продуктов представление ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Шаг 3: Перечисление s выбранной категории продуктов

Откройте `ProductsByCategory.aspx` и добавьте GridView, присвоив ему имя `ProductsByCategory`. Из его смарт-тег привязки GridView новый ObjectDataSource с именем `ProductsByCategoryDataSource`. Настройка ObjectDataSource для использования `ProductsBLL` класса s `GetProductsByCategoryID(categoryID)` метод и набор в раскрывающемся списке перечислены (нет) на вкладках UPDATE, INSERT и DELETE.


[![Используйте метод GetProductsByCategoryID(categoryID) класса ProductsBLL s](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Рис. 8**: использование `ProductsBLL` класса s `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))


Последний шаг в мастере настройки источника данных запрашивает источник параметров для *categoryID*. Так как эта информация передается через поле querystring `CategoryID`, выберите из раскрывающегося списка строк запроса и введите в текстовом поле QueryStringField CategoryID, как показано на рис. 9. Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Поле Querystring CategoryID используется для categoryID параметра](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Рис. 9**: использование `CategoryID` поля строки запроса для *categoryID* параметра ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))


По завершении работы мастера Visual Studio добавит соответствующие стояли и CheckBoxField GridView для полей данных продукта. Удалите все, кроме `ProductName`, `UnitPrice`, и `SupplierName` стояли. Настройка этих трех стояли `HeaderText` свойства для чтения продукта, цену и поставщика, соответственно. Формат `UnitPrice` BoundField как денежное значение.

Затем добавьте HyperLinkField и переместить его в крайней левой позиции. Задайте его `Text` свойство, чтобы просмотреть подробные сведения, его `DataNavigateUrlFields` свойства `ProductID`и его `DataNavigateUrlFormatString` свойства `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Добавить HyperLinkField сведения представления, который указывает ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Рис. 10**: Добавление представления сведений, указывающий на HyperLinkField`ProductDetails.aspx`


После внесения этих настроек, GridView и ObjectDataSource s декларативная разметка должна выглядеть следующим образом:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Вернуться к просмотра `Default.aspx` через браузер и Просмотр продуктов щелкните ссылку для напитков. Это требуется для `ProductsByCategory.aspx?CategoryID=1`, отображаются имена, цены и поставщики продуктов в базе данных Northwind, который принадлежит к категории «Напитки» (см. рис. 11). Вы можете расширить эту страницу, чтобы включить ссылку для возврата на страницу списка категории пользователей (`Default.aspx`) и элемент управления DetailsView и FormView, который отображает имя выбранной категории s и описание.


[![Отображаются имена напитков, цены и поставщиками](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Рис. 11**: отображаются имена напитков, цены и поставщики ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Шаг 4: Отображение сведений о продукте s

Последней странице `ProductDetails.aspx`, отображение выбранных продуктов. Откройте `ProductDetails.aspx` и перетащите DetailsView из области элементов в конструктор. Набор DetailsView s `ID` свойства `ProductInfo` и очистить его `Height` и `Width` значения свойств. Из его смарт-тег привязки DetailsView новый ObjectDataSource с именем `ProductDataSource`, Настройка ObjectDataSource для извлечения данных из `ProductsBLL` класса s `GetProductByProductID(productID)` метод. Как и в предыдущих веб-страниц, созданных в шагах 2 и 3, установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет).


[![Настройка ObjectDataSource можно использовать метод GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Рис. 12**: Настройка ObjectDataSource для использования `GetProductByProductID(productID)` метод ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))


Последний шаг мастера настройки источника данных запрашивает источник *productID* параметра. Так как эти данные идет через поле querystring `ProductID`, значение строки запроса и текстовое поле QueryStringField на ProductID раскрывающегося списка. Наконец нажмите кнопку "Готово", чтобы завершить работу мастера.


[![Настройка параметров для извлечения значения из поля строки запроса ProductID productID](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Рис. 13**: Настройка *productID* параметр, чтобы получить значение из `ProductID` поля строки запроса ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))


После завершения работы мастера настройки источника данных Visual Studio создаст соответствующий стояли и CheckBoxField в DetailsView для полей данных продукта. Удалить `ProductID`, `SupplierID`, и `CategoryID` стояли и настройте остальные поля по своему усмотрению. После небольшое число конфигураций внешнего вида my DetailsView и ObjectDataSource s декларативная разметка выглядело следующим образом:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Чтобы протестировать эту страницу, вернуться к `Default.aspx` и щелкните Просмотреть продукты для категории «Напитки». Из списка программ напитков, щелкните ссылку Просмотр сведений для чай. Это требуется для `ProductDetails.aspx?ProductID=1`, который демонстрирует s чай сведения (см. рис. 14).


[![Отображается чай s поставщика, категорию, цены и другие сведения](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Рис. 14**: отображается чай s поставщика, категорию, цены и другие сведения ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Шаг 5: Основные сведения о внутренней работы поставщика карты веб-узла

Карта сайта представлена в памяти сервера s web коллекцию `SiteMapNode` экземпляров, которые формируют иерархию. Должно быть ровно один корневой, все узлы некорневой должен иметь ровно один родительский узел и все узлы могут иметь произвольное число дочерних элементов. Каждый `SiteMapNode` объект представляет раздел в структуре веб-сайт s; эти разделы часто имеют соответствующие веб-страницы. Следовательно [ `SiteMapNode` класса](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) имеет свойства, такие как `Title`, `Url`, и `Description`, которые предоставляют сведения для раздела `SiteMapNode` представляет. Имеется также `Key` свойство, которое уникально определяет каждый `SiteMapNode` в иерархии, а также свойства, используемые для установления этой иерархии `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, и т. д.

Рис. 15 показана структура карты сайта общие рис. 1, но имеет более подробно в виде эскиза детали реализации.


[![Каждый SiteMapNode имеет свойства, такие как заголовок, URL-адрес, ключ и т. Д](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Рис. 15**: каждый `SiteMapNode` имеет свойства как `Title`, `Url`, `Key`и так далее ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))


Карта сайта доступен с помощью [ `SiteMap` класса](https://msdn.microsoft.com/library/system.web.sitemap.aspx) в [ `System.Web` пространства имен](https://msdn.microsoft.com/library/system.web.aspx). Этот класс s `RootNode` свойство возвращает корневой узел карты s `SiteMapNode` экземпляра; `CurrentNode` возвращает `SiteMapNode` которого `Url` соответствует URL-адрес текущей запрошенной страницы. Этот класс используется внутренне ASP.NET 2.0 s навигации веб-элементами управления.

Когда `SiteMap` доступа к свойствам класса s, его необходимо сериализовать структуре карты сайта с некоторых постоянных носителя в память. Однако логики сериализации карты сайта несложно запрограммированы `SiteMap` класса. Вместо этого во время выполнения `SiteMap` класс определяет, какой узел карты *поставщика* должно использоваться для сериализации. По умолчанию [ `XmlSiteMapProvider` класса](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) используется, который читает структуре карты s сайта из файла правильный формат XML. Тем не менее с небольшим работы можно создать собственную поставщика карты.

Все поставщики карты сайта должен быть производным от [ `SiteMapProvider` класса](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), которая содержит основные методы и свойства, необходимые для сайта, поставщиков карты, но исключает многие детали реализации. Второй класс [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), расширяет `SiteMapProvider` класс и содержит более надежной реализации необходимую функциональность. На внутреннем уровне `StaticSiteMapProvider` хранит `SiteMapNode` экземпляров узла сопоставления в `Hashtable` и предоставляет методы, например `AddNode(child, parent)`, `RemoveNode(siteMapNode),` и `Clear()` , добавлять и удалять `SiteMapNode` s ко внутреннему `Hashtable`. Класс `XmlSiteMapProvider` является производным от `StaticSiteMapProvider`.

При создании поставщика карты, расширяет `StaticSiteMapProvider`, существует два абстрактные методы, которые должны быть переопределены: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) и [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, как и предполагает его имя, отвечает за загрузку структуре карты сайта из постоянного хранилища и создав его в памяти. `GetRootNodeCore`Возвращает корневой узел карты веб-узла.

Перед на веб-приложение может использовать поставщик карты узла, он должен быть зарегистрирован в конфигурации приложения s. По умолчанию `XmlSiteMapProvider` класс регистрируется с помощью имени `AspNetXmlSiteMapProvider`. Для регистрации поставщиков карты дополнительного сайта, добавьте следующую разметку, чтобы `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

*Имя* значение присваивается понятное имя поставщика при *тип* указывает полное имя поставщика карты веб-узла. Мы изучим конкретные значения для *имя* и *тип* значения в шаге 7, когда мы хранять создаст нашей поставщика карты.

Класс поставщика карты сайта создается впервые, осуществляется из `SiteMap` класса и остается в памяти в течение времени существования веб-приложения. Так как существует только один экземпляр поставщика карты сайта, который может вызываться из нескольких параллельных посетителей веб-узла, крайне важно, что методы поставщик s быть *поточно-*.

Для производительности и масштабируемости он s, важно, что мы кэша в памяти узла представляют структуру и возвращают это в кэше структуры, вместо того, чтобы создать его заново при каждом `BuildSiteMap` вызывается метод. `BuildSiteMap`может вызываться несколько раз для каждого запроса страницы для пользователя, в зависимости от элементов управления навигацией используется на странице и глубину структуре карты сайта. В любом случае, если не следует кэшировать структуре карты сайта в `BuildSiteMap` то при каждом вызове нам требуется повторно получить сведения о продукте и категории архитектуре (что могло бы привести в запросе к базе данных). Как указывалось в предыдущих учебниках кэширования кэшированных данных может стать устаревшим. Для решения этой мы используем времени - или кэше на основе зависимость кэша SQL.

> [!NOTE]
> Поставщик карты узла может при необходимости переопределить [ `Initialize` метод](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize`вызывается, когда поставщик карты узла сначала создается и передается все настраиваемые атрибуты, назначенные поставщика в `Web.config` в `<add>` элемент, например: `<add name="name" type="type" customAttribute="value" />`. Оно полезно в том случае, если вы хотите разрешить разработчику указать различные параметры поставщика карты сайта без изменения кода s поставщика. Например, если мы бы чтение данных категорий и продуктов непосредственно из базы данных в отличие от через архитектуру, мы d скорее всего нужно позволяют разработчику страницы указать строку подключения базы данных через `Web.config` вместо использования жестко заданный значение в коде поставщика s. Поставщик карты пользовательского узла, мы выполним сборку на шаге 6 не переопределяет это `Initialize` метод. Пример использования `Initialize` метода, см. [Джефф Просайз](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [хранения карты сайта в SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) статьи.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Шаг 6: Создание поставщика карты

Чтобы создать поставщика карты, создающий карта сайта из категорий и продуктов в базе данных Northwind, необходимо создать класс, расширяющий `StaticSiteMapProvider`. На шаге 1 появляется вопрос о необходимости добавления `CustomProviders` папки в `App_Code` папку - добавьте новый класс в эту папку с именем `NorthwindSiteMapProvider`. Добавьте следующий код в класс `NorthwindSiteMapProvider` :


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Позволяет начать с изучения этого класса s s `BuildSiteMap` метод, который начинается с [ `lock` инструкции](https://msdn.microsoft.com/library/c5kehkcz.aspx). `lock` Инструкция допускает только один поток одновременно для ввода, тем самым доступ к его код сериализации и предотвращать шаг с заходом друг с другом s пальцам двух одновременных потоков.

Уровня класса `SiteMapNode` переменной `root` используется для кэширования структуре карты сайта. При создании карты узла в первый раз или в первый раз после изменения базовых данных `root` будет `null` будут создаваться структура карты сайта. Корневой узел карты s назначается `root` во время создания процесса, чтобы в следующий раз, этот метод вызывается, `root` не будет `null`. Следовательно при условии, что `root` не `null` структуре карты сайта будет возвращаться вызывающему объекту без необходимости ее повторного создания.

Если корневой `null`, структура карты сайта создается из продукта и сведения о категории. Карта сайта создается путем создания `SiteMapNode` экземпляров и затем формирует иерархию через вызовы `StaticSiteMapProvider` класса s `AddNode` метод. `AddNode`выполняет внутренней бухгалтерского учета, хранение различные `SiteMapNode` экземпляров в `Hashtable`. Прежде чем начать создание иерархии, начнем с вызова `Clear` метод, который удаляет элементы из внутренней `Hashtable`. Далее, `ProductsBLL` класса s `GetProducts` метод и итоговый `ProductsDataTable` хранятся в локальных переменных.

Конструирование s карты сайта начинается с создания корневого узла и присвоения его `root`. Перегрузка [ `SiteMapNode` конструктор s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) используется здесь и в пределах это `BuildSiteMap` передается следующие сведения:

- Ссылка на поставщика карты веб-узла (`this`).
- `SiteMapNode` s `Key`. Это необходимое значение должно быть уникальным для каждого `SiteMapNode`.
- `SiteMapNode` s `Url`. `Url`является необязательным, но если указано, каждый `SiteMapNode` s `Url` значение должно быть уникальным.
- `SiteMapNode` s `Title`, что является обязательным.

`AddNode(root)` Добавляет вызов метода `SiteMapNode` `root` карты сайта, который является корнем. Затем каждый `ProductRow` в `ProductsDataTable` перечисления. Если уже существует `SiteMapNode` для данной категории продукта s, ссылки на него. В противном случае новый `SiteMapNode` для категории создается и добавляется в качестве дочернего элемента `SiteMapNode``root` через `AddNode(categoryNode, root)` вызова метода. После соответствующей категории `SiteMapNode` найден узел или создания `SiteMapNode` создается для текущего продукта и добавлен в качестве дочернего категории `SiteMapNode` через `AddNode(productNode, categoryNode)`. Обратите внимание, что категория `SiteMapNode` s `Url` значение свойства `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` while продукта `SiteMapNode` s `Url` присваивается `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Продукты, имеющие базы данных `NULL` значение для их `CategoryID` группируются в категории `SiteMapNode` которого `Title` свойство имеет значение None, для которых `Url` свойству пустую строку. Я решил задать `Url` на пустую строку после `ProductBLL` класса s `GetProductsByCategory(categoryID)` метод в настоящее время отсутствует возможность возвращать только те продукты с `NULL` `CategoryID` значение. Кроме того, хотели продемонстрировать отрисовку элементов управления навигацией `SiteMapNode` , у которого отсутствует значение для его `Url` свойство. Я рекомендую для расширения этого учебника, чтобы None `SiteMapNode` s `Url` свойство указывает на `ProductsByCategory.aspx`, еще отображаются только соответствующие продуктам с `NULL` `CategoryID` значения.


После создания карты сайта, произвольный объект добавляется в кэш данных, используя зависимость кэша SQL на `Categories` и `Products` таблиц через `AggregateCacheDependency` объекта. Мы использовать, с помощью зависимости кэша SQL в предыдущем учебнике *с помощью кэш-зависимости SQL*. Поставщика карты, однако используется перегруженная версия кэша данных s `Insert` метод, мы хранять еще для изучения. Эта перегрузка принимает в качестве окончательного входного параметра делегата, который вызывается при удалении объекта из кэша. В частности, мы передаем в новом [ `CacheItemRemovedCallback` делегировать](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) , указывающий `OnSiteMapChanged` метод определен ниже в `NorthwindSiteMapProvider` класса.

> [!NOTE]
> Кэшируются в памяти представление карты сайта посредством переменной уровня класса `root`. Так как существует только один экземпляр класса поставщика карты пользовательского узла, и, поскольку этот экземпляр является общим для всех потоков в веб-приложении, эту переменную класса служит в качестве кэша. `BuildSiteMap` Метод также использует кэш данных, но только как средство получать уведомления при основной базы данных данные в `Categories` или `Products` таблиц изменений. Обратите внимание, что значение поместить в кэш данных только текущую дату и время. Данные карты сайта *не* поместить в кэш данных.


`BuildSiteMap` Метод завершения, возвращая корневой узел карты веб-узла.

Все остальные методы довольно просты. `GetRootNodeCore`отвечает за возвращение корневого узла. Поскольку `BuildSiteMap` возвращает корень, `GetRootNodeCore` просто возвращает `BuildSiteMap` s возвращаемое значение. `OnSiteMapChanged` Метода задает `root` к `null` при удалении элемента кэша. С корнем снова установить значение `null`, при очередном `BuildSiteMap` — вызове структуре карты сайта, будет создан заново. Наконец `CachedDate` свойство возвращает значение даты и времени, хранящиеся в кэше данных, если такое значение существует. Это свойство может использоваться разработчиком страницы для определения после последнего кэширования данных карты сайта.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Шаг 7: Регистрация`NorthwindSiteMapProvider`

В порядке для наших веб-приложения для использования `NorthwindSiteMapProvider` поставщика карты веб-узла, созданные на шаге 6, необходимо зарегистрировать его в `<siteMap>` раздел `Web.config`. В частности, добавьте следующую разметку в `<system.web>` элемент в `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Эта разметка выполняет две функции: во-первых, он указывает, что встроенная `AspNetXmlSiteMapProvider` поставщик карты сайта по умолчанию; во-вторых, он регистрирует поставщик карты пользовательский сайт создан на шаге 6 с именем понятную Northwind.

> [!NOTE]
> Для поставщиков карты сайта, расположенном в s приложения `App_Code` папки, значение `type` атрибут — это просто имя класса. Кроме того, поставщика карты может были созданы в отдельном проекте библиотеки классов в скомпилированной сборке, помещаются в s приложения web `/Bin` каталога. В этом случае `type` значение атрибута будет *имен*. *ClassName*, *AssemblyName* .


После обновления `Web.config`, занять некоторое время для просмотра из учебников любой страницы в браузере. Обратите внимание, что интерфейс навигации в левой части по-прежнему отображаются все разделы учебников определенные в `Web.sitemap`. Это так, как мы оставили `AspNetXmlSiteMapProvider` как поставщик по умолчанию. Чтобы создать элемент пользовательского интерфейса навигации, который использует `NorthwindSiteMapProvider`, нам нужно будет явно указать, что следует использовать поставщика карты веб-узла "Борей". Мы рассмотрим, как это сделать в шаге 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Шаг 8: Отображение сведений о карте сайта с помощью поставщика карты

Пользовательский узел поставщика карты, создан и зарегистрирован в `Web.config`, мы готов для добавления элементов управления навигацией для `Default.aspx`, `ProductsByCategory.aspx`, и `ProductDetails.aspx` страниц в `SiteMapProvider` папки. Сначала откройте `Default.aspx` страницы и перетащите `SiteMapPath` из области элементов в конструктор. Элемент управления SiteMapPath находится в области навигации панели элементов.


[![Добавление Default.aspx SiteMapPath](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**На рисунке 16**: Добавление SiteMapPath для `Default.aspx` ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))


Элемент управления SiteMapPath отображает навигатором, указывающий текущее положение s страницы в карты сайта. Мы добавили SiteMapPath в верхней части главной страницы обратно в *главные страницы и переходов* учебника.

Теперь пора просматривать эту страницу через браузер. SiteMapPath, добавленных в рисунке 16 использует поставщик карты сайта по умолчанию, извлекать данные из `Web.sitemap`. Поэтому навигации отображает Главная &gt; Настройка карты сайта, так же, как навигации в правом верхнем углу.


[![Поставщик карты сайта по умолчанию использует навигации](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Рисунок 17**: навигации использует поставщик карты сайта по умолчанию ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))


Чтобы SiteMapPath, добавлены на рисунке 16 использовать поставщика карты мы создали в шаге 6, установите его [ `SiteMapProvider` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) Northwind, имя мы присвоено `NorthwindSiteMapProvider` в `Web.config`. К сожалению конструктор продолжается использование поставщика карты веб-узла по умолчанию, но если посетите страницу через браузер после внесения этого изменения свойств вы увидите, что навигации теперь использует поставщика карты.


[![Навигации теперь использует NorthwindSiteMapProvider поставщика карты пользовательского узла](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**На рисунке 18**: навигации теперь использует настраиваемого поставщика карты сайта `NorthwindSiteMapProvider` ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))


Элемент управления SiteMapPath отображает более работы пользовательского интерфейса в `ProductsByCategory.aspx` и `ProductDetails.aspx` страниц. Добавить SiteMapPath к этим страницам, параметр `SiteMapProvider` свойства в обеих в Northwind. Из `Default.aspx` щелкните ссылку Просмотр продуктов для напитков и просмотр сведений о связи, чтобы чай. Как показано на рисунке 19, навигации включает текущего раздела карты сайта (чай) и его предков: Напитки и всех категорий.


[![Навигации теперь использует NorthwindSiteMapProvider поставщика карты пользовательского узла](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**На рисунке 19**: навигации теперь использует настраиваемого поставщика карты сайта `NorthwindSiteMapProvider` ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))


Другие элементы пользовательского интерфейса навигации можно использовать в дополнение к SiteMapPath, такие как элементы управления Menu и TreeView. `Default.aspx`, `ProductsByCategory.aspx`, И `ProductDetails.aspx` страниц в загрузка для этого учебника, например, включают в себя элементы управления меню (см. рис. 20). В разделе [s проверки ASP.NET 2.0 возможности переходов по узлу](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) и [с помощью элементов управления навигацией сайта](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) раздел [примеры использования ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) более подробное рассмотрение элементы управления навигацией и система карты узла в ASP.NET 2.0.


[![Элемент управления меню отображается список всех категорий и продуктов](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Рис. 20**: меню управления перечислены Each категорий и продуктов ([Просмотр полноразмерное изображение](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))


Как упоминалось ранее в этом учебнике структуре карты сайта может осуществляться программными средствами с помощью `SiteMap` класса. Следующий код возвращает корень `SiteMapNode` поставщика по умолчанию:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Поскольку `AspNetXmlSiteMapProvider` является поставщиком по умолчанию для нашего приложения этот код возвращает корневой узел, определенный в `Web.sitemap`. Для поставщика карты сайта, используемый по умолчанию следует использовать `SiteMap` класса s [ `Providers` свойства](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) следующим образом:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Где *имя* имя поставщика карты (Northwind для наших веб-приложения).

Для доступа к члену, относящиеся к поставщика карты веб-узла, используйте `SiteMap.Providers["name"]` для получения экземпляра поставщика и затем приведите его к соответствующему типу. Например, для отображения `NorthwindSiteMapProvider` s `CachedDate` свойство на странице ASP.NET, используйте следующий код:


[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Не забудьте проверить функцией зависимости кэша SQL. После посетив `Default.aspx`, `ProductsByCategory.aspx`, и `ProductDetails.aspx` страниц, на одном из учебников по редактирования, вставки и удаления разделов и измените имя категории или продукта. Затем вернитесь к одной из страниц в `SiteMapProvider` папки. При условии, что прошло достаточно времени для механизма опроса, обратите внимание на изменение в основную базу данных, карты узла должен быть обновлен для отображения нового продукта или имя категории.


## <a name="summary"></a>Сводка

ASP.NET 2.0 включает в себя функции карты узла s `SiteMap` класса, количество встроенных навигации Web элементов управления, и поставщика карты сайта по умолчанию, который ожидает данные карты сайта сохраняются в XML-файл. Чтобы использовать данные карты сайта из другого источника как из базы данных, поставщик карты s архитектуры приложения или удаленного веб-службы, необходимо создать пользовательский узел. Это предполагает создание класс, производный прямо или косвенно от `SiteMapProvider` класса.

В этом учебнике мы узнали, как создать поставщика карты на основании карты сайта сведения о продукте и категории, вызванных из архитектуры приложения. Наш поставщик расширенных `StaticSiteMapProvider` класс и затрат создание `BuildSiteMap` метод, который извлекает данные, составить иерархии карты узла, а в кэше структуры, полученный в переменной уровня класса. Мы использовали зависимость кэша SQL с помощью функции обратного вызова, чтобы сделать недействительными кэшированные структуры, когда базовый `Categories` или `Products` изменения данных.

Программирование довольны!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Хранение карты сайта в SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) и [поставщик карты узла SQL можно хранить ожидал](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Модели поставщика s взглянуть на ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Набор средств поставщика](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Изучение ASP.NET 2.0 s возможности переходов по узлу](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Dave Gardner, Зак Джонс, Мерфи Тереза д и Екатерина Leigh. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Вперед](building-a-custom-database-driven-site-map-provider-vb.md)
