---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Создание поставщика карт пользовательского сайта, основанных на базах данных (Visual Basic) | Документация Майкрософт
author: rick-anderson
description: По умолчанию поставщик карты узла в ASP.NET 2.0 извлекает свои данные из статических XML-файла. Пока поставщик на основе XML подходит для многих малый и средний размер...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: ecf09fc6adc59e1b6280ed6f3829f0add2b4a299
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815964"
---
<a name="building-a-custom-database-driven-site-map-provider-vb"></a>Создание поставщика карт пользовательского сайта, основанных на базах данных (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) или [скачать PDF](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> По умолчанию поставщик карты узла в ASP.NET 2.0 извлекает свои данные из статических XML-файла. Пока поставщик на основе XML, подходит для многих небольших и средних веб-сайтов, большего размера веб-приложения требуют более динамической карты сайта. В этом учебном курсе мы выполним сборку пользовательского поставщика карт сайтов, извлекает свои данные из уровня бизнес-логики, который в свою очередь извлекает данные из базы данных.


## <a name="introduction"></a>Вступление

ASP.NET 2.0 функция карты веб-сайтов s позволяет разработчику определить карту веб приложение s узла, в некоторых постоянных средних, такие как в XML-файл. После определения данных карты узла может осуществляться программно с помощью [ `SiteMap` класс](https://msdn.microsoft.com/library/system.web.sitemap.aspx) в [ `System.Web` пространства имен](https://msdn.microsoft.com/library/system.web.aspx) или разнообразными навигации веб-элементы управления, такие как Элементы управления SiteMapPath, Menu и TreeView. Система карты сайта использует [модель поставщика](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , чтобы другой узел карты сериализации реализации можно создавать и подключать к веб-приложения. Поставщик карты узла по умолчанию, который поставляется с ASP.NET 2.0 сохраняет структуры карты узла в XML-файл. Вернитесь в [главные страницы и переходы по узлу](../introduction/master-pages-and-site-navigation-vb.md) руководстве мы создали файл с именем `Web.sitemap` , содержится эта структура и комплектации XML с каждый новый раздел учебника.

По умолчанию поставщик карты узла, основанного на XML работает также в том случае, если структуры карты s довольно статические, такие как в этих руководствах. Во многих ситуациях Однако более динамической карты сайта потребуется. Рассмотрите возможность карты узла, показанный на рис. 1, где каждый category и product отображаются как разделы в структуре s веб-сайта. С помощью этой карты узла посещении веб-страницы, соответствующий корневой узел могут быть перечислены все категории, тогда как посещение веб-странице определенной категории s перечислял продукты этой категории и просмотр веб-странице конкретного продукта s будет подробно продукта s.


[![Категории и продукты состав структуры карты s](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Рис. 1**: категорий и продуктов состав s карты узла структуры ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))


Хотя эта структура на основе категории и продукта может быть жестко запрограммированы в `Web.sitemap` файл, файл будет необходимо обновить каждый раз, когда категории или продукт был добавлен, удален или переименован. Следовательно обслуживание карты сайта значительно упрощает если его структуру извлекается из базы данных или, в идеальном случае уровне бизнес-логики s архитектуры приложения. Таким образом, как продукты и категории были добавлены, переименован или удален, карты узла будет автоматически обновляться для отражения этих изменений.

Так как ASP.NET 2.0 s узла карты сериализации обычно строится поверх модели поставщиков, мы можем создать нашего собственного пользовательского поставщика карт сайтов, извлекает данные из альтернативное хранилище данных, таких как базы данных или архитектуры. В этом руководстве мы создадим пользовательский поставщик, который извлекает свои данные из BLL. Позвольте s приступить к работе!

> [!NOTE]
> Пользовательского поставщика карт сайтов в этом учебнике тесно связан с приложением s архитектура и модель данных. Джефф Просайз s [хранения карты сайта в SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) и [поставщик карты узла SQL ve вы ждали](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) статьи изучить обобщенный подход к хранению данных карты узла в SQL Server.


## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Шаг 1: Создание настраиваемый сайт поставщика карты веб-страниц

Прежде чем приступать к созданию пользовательского поставщика карт сайтов, позвольте s сначала добавим страницы ASP.NET, которые потребуются для этого руководства. Начните с добавления новой папки с именем `SiteMapProvider`. Добавьте следующие страницы ASP.NET в этой папке, не забывая связывать каждую с `Site.master` главной страницы:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Кроме того, добавить `CustomProviders` вложенную папку для `App_Code` папки.


![Добавление страниц ASP.NET учебников связанные с поставщиком карты сайта](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Рис. 2**: Добавление страниц ASP.NET для сайта сопоставить учебные материалы, связанные с поставщиком


Так как существует только один руководстве для этого раздела, мы кое необходимость t `Default.aspx` для перечислит учебные секции s. Вместо этого `Default.aspx` будут отображаться категории в элементе управления GridView. Это будет разобран на шаге 2.

Затем обновите `Web.sitemap` включать ссылку на `Default.aspx` страницы. В частности, добавьте следующую разметку после кэширования `<siteMapNode>`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

После обновления `Web.sitemap`, Отвлекитесь и просмотрите учебный веб-узел в обозревателе. В меню слева теперь есть элемент для работы с учебником поставщика единственный узел карты.


![Карта узла теперь включает запись для работы с учебником поставщика карты узла](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Рис. 3**: карта узла теперь включает запись для работы с учебником поставщика карты узла


Этот учебник s главное — проиллюстрировать создание пользовательского поставщика карт сайтов и настройка веб-приложения для использования этого поставщика. В частности мы выполним сборку поставщика, который возвращает карты узла, которая включает корневой узел и узел для каждой категории и продуктов, как показано на рис. 1. Как правило каждый узел карты узла может указать URL-адрес. Карта узла будет URL-адрес корневого узла s `~/SiteMapProvider/Default.aspx`, которой будут перечислены все категории в базе данных. Каждый узел категории в карте узла будет иметь URL-адрес, указывающий `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, которой будут перечислены все продукты в указанном *categoryID*. Наконец, узел карты сайта каждого продукта будет указывать `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, который будет отображать данные конкретного продукта s.

Для запуска необходимо создать `Default.aspx`, `ProductsByCategory.aspx`, и `ProductDetails.aspx` страниц. Эти страницы выполняются в шагах 2, 3 и 4, соответственно. Так как надежность работы с этим руководством на поставщиков карты веб-узла и так как в предыдущих учебных курсах рассмотрели создание отчеты такого рода многостраничных «основной/подробности», мы будет спешке через шаги 2 – 4. Если вы хотите освежить о создании отчетов «основной/подробности», которые охватывают несколько страниц, обращаться к ["основной/подробности" Фильтрация по две страницы](../masterdetail/master-detail-filtering-across-two-pages-vb.md) руководства.

## <a name="step-2-displaying-a-list-of-categories"></a>Шаг 2: Отображение списка категорий

Откройте `Default.aspx` странице в `SiteMapProvider` папки и перетащите элемент управления GridView с панели элементов в конструктор, установив его `ID` для `Categories`. Смарт-теге GridView s, привязать его к элементу управления ObjectDataSource с именем `CategoriesDataSource` и настройте его таким образом, чтобы он извлекает свои данные с помощью `CategoriesBLL` класс s `GetCategories` метод. Так как этот GridView просто отображаются категории и не предоставляет возможностей изменения данных, набор раскрывающиеся списки в UPDATE, INSERT и удалите вкладок (нет).


[![Настройте элемент ObjectDataSource для возврата категории с помощью метода GetCategories](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Рис. 4**: Настройка ObjectDataSource для возврата категории с помощью `GetCategories` метод ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))


[![Установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Рис. 5**: задать раскрывающиеся списки в обновления, вставки и удаления вкладок (нет) ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))


После завершения работы мастера настройки источников данных Visual Studio добавит BoundField для `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, и `BrochurePath`. Измените GridView, таким образом, чтобы он содержит только `CategoryName` и `Description` поля BoundFields и обновить `CategoryName` BoundField s `HeaderText` свойство к категории.

Добавление поля HyperLinkField затем расположите его, так что он s поле слева. Задайте `DataNavigateUrlFields` свойства `CategoryID` и `DataNavigateUrlFormatString` свойства `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Задать `Text` свойства для просмотра продуктов.


![Добавление поля HyperLinkField к элементу GridView категорий](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Рис. 6**: Добавление поля HyperLinkField к `Categories` GridView


После создания элемента управления ObjectDataSource и настройка полей s GridView, два элемента управления декларативная разметка будет выглядеть следующим образом:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

Рис. 7 показан `Default.aspx` при просмотре через браузер. Если щелкнуть имя категории s Просмотр продуктов ссылке вы перейдете к `ProductsByCategory.aspx?CategoryID=categoryID`, который будет создан на шаге 3.


[![Каждая категория имеет перечисленные вместе со ссылкой для представления продуктов](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Рис. 7**: Каждая категория — в списке вместе со ссылкой для представления продуктов ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))


## <a name="step-3-listing-the-selected-category-s-products"></a>Шаг 3: Перечисление s продуктов выбранной категории

Откройте `ProductsByCategory.aspx` странице и добавьте элемент управления GridView, назовите его `ProductsByCategory`. Из его смарт-тега привязки GridView к элементу управления ObjectDataSource с именем `ProductsByCategoryDataSource`. Настройка ObjectDataSource на использование `ProductsBLL` класс s `GetProductsByCategoryID(categoryID)` метод и задайте в раскрывающемся списке перечислены (нет) на вкладках UPDATE, INSERT и DELETE.


[![Используйте метод GetProductsByCategoryID(categoryID) класса ProductsBLL s](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Рис. 8**: использование `ProductsBLL` класс s `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))


Последний шаг в мастере настройки источника данных запрашивает источник для *categoryID*. Так как эта информация передается через поле строки запроса `CategoryID`выберите строки запроса в раскрывающемся списке и введите в текстовом поле QueryStringField CategoryID, как показано на рис. 9. Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Используйте поле Querystring CategoryID для параметра categoryID](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Рис. 9**: использование `CategoryID` поле строки запроса для *categoryID* параметра ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))


После завершения работы мастера, Visual Studio добавит соответствующие поля BoundFields и по полю CheckBoxField GridView для полей данных продукта. Удалите все, кроме `ProductName`, `UnitPrice`, и `SupplierName` полей BoundField. Настроить эти три поля BoundField `HeaderText` свойств для чтения продукта, цену и поставщика, соответственно. Формат `UnitPrice` BoundField как денежная единица.

Добавление поля HyperLinkField затем перемещается в крайней левой позиции. Задайте его `Text` свойство, чтобы просмотреть подробные сведения, его `DataNavigateUrlFields` свойства `ProductID`и его `DataNavigateUrlFormatString` свойства `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.


![Добавление поля HyperLinkField сведения о представлении, указывающий ProductDetails.aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Рис. 10**: Добавление, представление сведений о HyperLinkField, который указывает на `ProductDetails.aspx`


После внесения этих настроек, GridView и ObjectDataSource s декларативная разметка должна выглядеть следующим образом:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Вернуться к просмотра `Default.aspx` через браузер и щелкните Просмотр продуктов ссылку «Напитки». Это требуется для `ProductsByCategory.aspx?CategoryID=1`, отображая имена, цены и поставщиками продуктов в базе данных "Борей", которые принадлежат к категории «Напитки» (см. рис. 11). Вы можете расширить эту страницу, чтобы включить ссылку для возврата на страницу категорию пользователей (`Default.aspx`) и элемент управления DetailsView и FormView, который отображает имя выбранной категории s и описание.


[![Отображаются имена «Напитки», цены и поставщиками](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Рис. 11**: отображаются имена «Напитки», цены и поставщиками ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))


## <a name="step-4-showing-a-product-s-details"></a>Шаг 4: Отображение сведений о продукте s

Последней странице `ProductDetails.aspx`, отображение выбранных продуктов. Откройте `ProductDetails.aspx` и перетащите DetailsView из области элементов в конструктор. Набор DetailsView s `ID` свойства `ProductInfo` и очистите его `Height` и `Width` значения свойств. Из его смарт-тега привязки элемента управления DetailsView новый ObjectDataSource, именуемый `ProductDataSource`, Настройка ObjectDataSource для извлечения данных из `ProductsBLL` класс s `GetProductByProductID(productID)` метод. Как и в предыдущей веб-страниц, созданных в шаги 2 и 3, установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет).


[![Настройка ObjectDataSource на использование метода GetProductByProductID(productID)](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Рис. 12**: Настройка ObjectDataSource для использования `GetProductByProductID(productID)` метод ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))


Последний шаг мастера настройки источников данных запрашивает источник *productID* параметра. Так как эти данные поступают через поле строки запроса `ProductID`, значение строки запроса и текстовое поле QueryStringField на ProductID стрелку раскрывающегося списка. Наконец нажмите кнопку "Готово", чтобы завершить работу мастера.


[![Настройка параметра, чтобы извлечь его значение из поля строки запроса ProductID productID](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Рис. 13**: Настройка *productID* параметр, чтобы извлечь значение из `ProductID` поля строки запроса ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))


После завершения работы мастера настройки источников данных Visual Studio создаст соответствующие поля BoundFields и по полю CheckBoxField в DetailsView для полей данных продукта. Удалить `ProductID`, `SupplierID`, и `CategoryID` поля BoundFields и настройте остальные поля, по своему усмотрению. После небольшое число эстетически конфигураций Мой DetailsView и ObjectDataSource s декларативная разметка выглядело следующим образом:


[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Чтобы протестировать эту страницу, вернитесь к `Default.aspx` и нажимать кнопку Просмотр продуктов для категории «Напитки». Из списка программ Напитки, щелкните ссылку Просмотр сведений на «Чай Chai». Это требуется для `ProductDetails.aspx?ProductID=1`, показывающий s чай сведения (см. рис. 14).


[![Отображается чай s поставщику, категории, цены и другие сведения](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Рис. 14**: отображается чай s поставщику, категории, цены и другие сведения ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))


## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Шаг 5: Основные сведения о внутренней работы поставщик карты узла

Карты узла, представляется как коллекцию в памяти сервера s web `SiteMapNode` экземпляров, которые образуют иерархию. Должно быть ровно один корневой, все узлы, отличного от root должен иметь ровно один родительский узел и все узлы могут иметь произвольное число дочерних элементов. Каждый `SiteMapNode` объект представляет раздел в структуре веб-сайт s; эти разделы часто имеют соответствующие веб-страницы. Следовательно [ `SiteMapNode` класс](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) имеет свойства, такие как `Title`, `Url`, и `Description`, которые предоставляют сведения для раздела `SiteMapNode` представляет. Имеется также `Key` свойство, которое однозначно определяет каждый `SiteMapNode` в иерархии, а также свойства, используемые для установления этой иерархии `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, и т. д.

Рис. 15 показывает общие структуры карты из рис. 1, но с детали реализации, обрисованного более подробно.


[![Каждый экземпляр SiteMapNode имеет свойства, такие как заголовок, URL-адрес, ключ и т. Д.](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Рис. 15**: каждый `SiteMapNode` имеет свойства как `Title`, `Url`, `Key`, и так далее ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))


Карты узла можно получить с помощью [ `SiteMap` класс](https://msdn.microsoft.com/library/system.web.sitemap.aspx) в [ `System.Web` пространства имен](https://msdn.microsoft.com/library/system.web.aspx). Этот класс s `RootNode` возвращает корневой узел карты s `SiteMapNode` экземпляра; `CurrentNode` возвращает `SiteMapNode` которого `Url` свойства совпадает с URL-адрес текущей запрошенной страницы. Этот класс используется внутренне в ASP.NET 2.0 s навигации веб-элементами управления.

Когда `SiteMap` доступа к свойствам класса s, его необходимо выполнить сериализацию структуры карты с некоторых постоянных носителя в память. Тем не менее, логику сериализации карты узла несложно запрограммированы `SiteMap` класса. Вместо этого во время выполнения `SiteMap` класс определяет, какие карты узла *поставщика* должно использоваться для сериализации. По умолчанию [ `XmlSiteMapProvider` класс](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) используется, который читает структуры карты s из XML-файлом правильный формат. Тем не менее приложив немного работы мы создадим нашего собственного пользовательского поставщика карт сайтов.

Все поставщики карты сайта должен быть производным от [ `SiteMapProvider` класс](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), который включает в себя важные методы и свойства, необходимые для сайта поставщиков карты, но опущены многие детали реализации. Второй класс [ `StaticSiteMapProvider` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), расширяет `SiteMapProvider` и который содержит более надежной реализации необходимую функциональность. На внутреннем уровне `StaticSiteMapProvider` хранит `SiteMapNode` экземпляров сайта сопоставления в `Hashtable` и предоставляет методы, такие как `AddNode(child, parent)`, `RemoveNode(siteMapNode),` и `Clear()` , добавления и удаления `SiteMapNode` s к внутреннему `Hashtable`. Класс `XmlSiteMapProvider` является производным от `StaticSiteMapProvider`.

При создании пользовательского поставщика карт сайтов, который расширяет `StaticSiteMapProvider`, существуют два абстрактные методы, которые необходимо переопределить: [ `BuildSiteMap` ](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) и [ `GetRootNodeCore` ](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, как и предполагает его имя, отвечает за загрузку структуре карты узла из постоянного хранилища и создать его самостоятельно в памяти. `GetRootNodeCore` Возвращает корневой узел карты узла.

Прежде чем веб-узла приложение может использовать поставщик карты узла, он должен быть зарегистрирован в конфигурации приложения s. По умолчанию `XmlSiteMapProvider` класс регистрируется с помощью имени `AspNetXmlSiteMapProvider`. Чтобы зарегистрировать поставщики карты дополнительного сайта, добавьте следующую разметку для `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

*Имя* значение присваивает понятное имя поставщика при *тип* указывает полное имя поставщика карты узла. Мы рассмотрим конкретные значения для *имя* и *тип* значения на шаге 7, когда мы ve создаст наших пользовательского поставщика карт сайтов.

Создается экземпляр класса поставщика карты узла при первом доступе из `SiteMap` класса и остается в памяти в течение времени существования веб-приложения. Так как существует только один экземпляр поставщика карты узла, который может вызываться из нескольких одновременных посетителей веб-узла, крайне важно, что методы поставщика s быть *поточно ориентированные*.

Для производительности и масштабируемости он s, важно, что процесс кэширования в памяти узла структуры и возвращают это кэшируются структуры, а не создать его заново каждый раз `BuildSiteMap` вызывается метод. `BuildSiteMap` может вызываться несколько раз для каждого запроса страницы, каждого пользователя, в зависимости от элементов управления навигацией используется на странице и глубину структуры карты. В любом случае, если мы не кэшируют структуре карты узла в `BuildSiteMap` то каждый раз, он вызывается необходимо повторно получить сведения о продукте и категории из архитектуры (что привело бы запрос к базе данных). Как уже говорилось в предыдущих руководствах кэширования, кэшированных данных могут стать устаревшей. Для решения этой проблемы, мы используем время - или кэше на основе зависимостей кэша SQL.

> [!NOTE]
> Поставщик карты узла может при необходимости переопределить [ `Initialize` метод](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` вызывается, когда поставщик карты узла сначала создается и передается все настраиваемые атрибуты, назначенные поставщика в `Web.config` в `<add>` следующего вида: `<add name="name" type="type" customAttribute="value" />`. Это полезно, если вы хотите разрешить разработчику страницы указать различные параметры поставщика карты узла без необходимости изменять код s поставщика. Например, если мы чтение данных категорий и продуктов непосредственно из базы данных в отличие от рассмотрение архитектуры, мы d скорее всего потребоваться позволяют разработчику страницы указать строку подключения базы данных через `Web.config` вместо использования жестко заданный значение в код поставщика s. Пользовательского поставщика карт сайтов мы выполним сборку на шаге 6 не переопределяет это `Initialize` метод. Например, с помощью `Initialize` метод, см. [Джефф Просайз](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [хранения карты сайта в SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) статьи.


## <a name="step-6-creating-the-custom-site-map-provider"></a>Шаг 6: Создание пользовательского поставщика карт сайтов

Для создания пользовательского поставщика карт сайтов, создающий карты узла из категорий и продуктов в базе данных "Борей", необходимо создать класс, расширяющий `StaticSiteMapProvider`. На шаге 1 бы я попросил вас добавить `CustomProviders` папку в `App_Code` папка — добавьте новый класс в эту папку с именем `NorthwindSiteMapProvider`. Добавьте следующий код в класс `NorthwindSiteMapProvider` :


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Позвольте s начнете с рассмотрения этого класса s `BuildSiteMap` метод, который начинается с [ `lock` инструкции](https://msdn.microsoft.com/library/c5kehkcz.aspx). `lock` Инструкция разрешает только один поток за раз, чтобы ввести, тем самым доступ к его код сериализации и предотвращать шаг с заходом на лишена s друг с другом два параллельных потоков.

Уровня класса `SiteMapNode` переменной `root` используется для кэширования структуры карты. При создании карты узла в первый раз или в первый раз после изменения базовых данных, `root` будет `Nothing` и будут созданы структуры карты. Корневой узел карты s назначается `root` во время создания процесса, чтобы при следующем запуске этот метод вызывается, `root` не будет `Nothing`. Следовательно Если `root` не `Nothing` структуры карты будет возвращаться вызывающему объекту без необходимости ее повторного создания.

Если корневой `Nothing`, структуре карты узла создается на основе продукта и сведения о категории. Карты узла создается путем создания `SiteMapNode` экземпляров и затем формирует иерархию через вызовы `StaticSiteMapProvider` класс s `AddNode` метод. `AddNode` выполняет внутренний внутренних операций, хранение различные `SiteMapNode` экземпляров в `Hashtable`. Прежде чем приступать, создав иерархию, мы начнем с вызова `Clear` метод, который удаляет элементы из внутренней `Hashtable`. Далее, `ProductsBLL` класс s `GetProducts` метод, в результате чего `ProductsDataTable` хранятся в локальных переменных.

Конструирование s карты сайта начинается с создания корневого узла и назначить его `root`. Перегрузка [ `SiteMapNode` конструктор s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) используемый здесь и всему этому `BuildSiteMap` передается следующие сведения:

- Ссылка для поставщика карты узла (`Me`).
- `SiteMapNode` s `Key`. Это обязательное значение должно быть уникальным для каждого `SiteMapNode`.
- `SiteMapNode` s `Url`. `Url` является необязательным, но если указано, каждый `SiteMapNode` s `Url` значение должно быть уникальным.
- `SiteMapNode` s `Title`, который является обязательным.

`AddNode(root)` Добавляет вызов метода `SiteMapNode` `root` в карту узла в качестве привилегированного пользователя. Далее, каждый `ProductRow` в `ProductsDataTable` перечисляется. Если уже существует `SiteMapNode` для текущей категории продуктов «s», на него ссылается. В противном случае новый `SiteMapNode` для категории создается и добавляется в качестве дочернего элемента `SiteMapNode``root` через `AddNode(categoryNode, root)` вызова метода. После соответствующей категории `SiteMapNode` узел найден, или создания `SiteMapNode` для текущего продукта и добавляется в качестве дочернего элемента категории `SiteMapNode` через `AddNode(productNode, categoryNode)`. Обратите внимание, что категория `SiteMapNode` s `Url` свойство имеет значение `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` while продукта `SiteMapNode` s `Url` свойству назначается `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Эти продукты, которые имеют базу данных `NULL` значение для их `CategoryID` группируются в категории `SiteMapNode` которого `Title` свойство имеет значение None, для которых `Url` свойству присваивается пустая строка. Я решил задать `Url` пустую строку после `ProductBLL` класс s `GetProductsByCategory(categoryID)` метод на данный момент отсутствует возможность возвращать только те продукты с `NULL` `CategoryID` значение. Кроме того, я хотел продемонстрировать, как отображать элементы управления навигацией `SiteMapNode` , у которого отсутствует значение для его `Url` свойство. Я советую вам расширять это руководство, чтобы None `SiteMapNode` s `Url` указывает свойство `ProductsByCategory.aspx`, еще отображаются только продукты с `NULL` `CategoryID` значения.


После создания карты узла, произвольный объект добавляется в кэш данных, с помощью зависимость кэша SQL в `Categories` и `Products` таблицы через `AggregateCacheDependency` объекта. Мы изучили использование зависимостей кэша SQL в предыдущем учебном курсе, *использование зависимостей кэша SQL*. Пользовательского поставщика карт сайтов, однако используется перегруженная версия кэша данных s `Insert` метод вставать ve еще для изучения. В качестве его последний входной параметр Эта перегрузка принимает делегат, который вызывается, когда объект удаляется из кэша. В частности, мы передаем в новом [ `CacheItemRemovedCallback` делегировать](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) , указывающий `OnSiteMapChanged` метод определен дальше в `NorthwindSiteMapProvider` класса.

> [!NOTE]
> Кэшируются в памяти представление карты веб-узла через переменную уровня класса `root`. Так как существует только один экземпляр класса поставщика карты пользовательского узла и так как этот экземпляр является общим для всех потоков в веб-приложения, эту переменную класса служит в качестве кэша. `BuildSiteMap` Метод также использует кэш данных, но только как средство для получения уведомления, когда основной базы данных данные в `Categories` или `Products` таблиц изменений. Обратите внимание, что значение, помещения в кэш данных только текущую дату и время. Данные карты сайта *не* поместить в кэш данных.


`BuildSiteMap` Метод завершения, возвращая корневой узел карты веб-узла.

Остальные методы довольно просты. `GetRootNodeCore` отвечает за возвращение корневого узла. Так как `BuildSiteMap` возвращает корень, `GetRootNodeCore` просто возвращает `BuildSiteMap` s возвращаемое значение. `OnSiteMapChanged` Метода задает `root` обратно `Nothing` при удалении элемента кэша. С корнем снова установить значение `Nothing`, очередном `BuildSiteMap` — вызове структуре карты узла, будет создан заново. Наконец `CachedDate` свойство возвращает значение даты и времени, хранящиеся в кэше данных, если существует такое значение. Это свойство может использоваться разработчиком страницы, чтобы определить, когда последнего кэширования данных карты узла.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Шаг 7: Регистрация`NorthwindSiteMapProvider`

В порядке для нашего веб-приложения для использования `NorthwindSiteMapProvider` поставщика карты узла, созданные на шаге 6, необходимо зарегистрировать его в `<siteMap>` раздел `Web.config`. В частности, добавьте следующую разметку в `<system.web>` элемент в `Web.config`:


[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Эта разметка делает две вещи: во-первых, это показывает, что встроенная `AspNetXmlSiteMapProvider` является поставщиком карты узла по умолчанию; во-вторых, он регистрирует пользовательского поставщика карт сайтов создана в шаге 6 в понятном имени Northwind.

> [!NOTE]
> Для поставщиков карт веб, расположенный в s приложения `App_Code` папки, значение `type` атрибут — это просто имя класса. В качестве альтернативы пользовательского поставщика карт сайтов можно было создано в отдельном проекте библиотеки классов в скомпилированную сборку, помещаются в s приложения web `/Bin` каталога. В этом случае `type` значение атрибута было бы *пространства имен*. *ClassName*, *AssemblyName* .


После обновления `Web.config`, стоит потратить немного времени на просмотр страниц с помощью учебников, в браузере. Обратите внимание, что интерфейс навигации в левой части по-прежнему показаны разделы и учебники, определенные в `Web.sitemap`. Это обусловлено тем, мы оставили `AspNetXmlSiteMapProvider` как поставщик по умолчанию. Чтобы создать элемент пользовательского интерфейса навигации, который использует `NorthwindSiteMapProvider`, необходимо явно указать, что следует использовать поставщик карты узла "Борей". Узнаете, как для выполнения этой задачи на шаге 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Шаг 8: Отображение информации карты узла, с помощью пользовательского поставщика карт сайтов

С помощью пользовательского сайта поставщика карты, созданных и зарегистрированных в `Web.config`, мы будет готов, чтобы добавить элементы управления навигацией для `Default.aspx`, `ProductsByCategory.aspx`, и `ProductDetails.aspx` страниц в `SiteMapProvider` папку. Сначала откройте `Default.aspx` страницы и перетащите `SiteMapPath` из инструментария в конструктор. Элемент управления SiteMapPath находится в области навигации панели элементов.


[![Добавление к странице Default.aspx SiteMapPath](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Рис. 16**: Добавление SiteMapPath для `Default.aspx` ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))


Элемент управления SiteMapPath отображает навигатором, указывающее, положение s текущей страницы в карте веб-узла. Мы добавили SiteMapPath в верхней части главной страницы в *главные страницы и переходы по узлу* руководства.

Отвлекитесь и просмотреть эту страницу через обозреватель. SiteMapPath, добавлены на рис. 16 использует поставщика карты узла по умолчанию, извлекает данные из `Web.sitemap`. Таким образом, навигатор показывает Главная &gt; Настройка карты узла, как и в строке навигации в правом верхнем углу.


[![Эта панель перехода использует поставщика карты узла по умолчанию](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Рис. 17**: навигации использует поставщик карты узла по умолчанию ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))


Чтобы SiteMapPath, добавлены на рис. 16 использовать пользовательского поставщика карт сайтов мы создали на шаге 6, задайте его [ `SiteMapProvider` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) в Northwind, имя было присвоено `NorthwindSiteMapProvider` в `Web.config`. К сожалению конструктор будет продолжать использовать поставщика карты узла по умолчанию, но если зайти на страницу через браузер после внесения этого изменения свойств вы увидите, что в строке навигации теперь использует пользовательского поставщика карт сайтов.


[![Эта панель перехода теперь использует NorthwindSiteMapProvider поставщика карты пользовательского узла](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Рис. 18**: навигации теперь использует поставщика карты узла Custom `NorthwindSiteMapProvider` ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))


Элемент управления SiteMapPath отображает более функциональным пользовательским интерфейсом в `ProductsByCategory.aspx` и `ProductDetails.aspx` страниц. Добавьте SiteMapPath эти страницы, параметр `SiteMapProvider` свойство в обоих в Northwind. Из `Default.aspx` щелкните ссылку Просмотр продуктов для «Напитки», а затем ссылку Просмотр сведений о «Чай Chai». Как показано на рис. 19, навигации включает текущего раздела карты узла («Чай Chai») и его родительские элементы: «Напитки» и всех категорий.


[![Эта панель перехода теперь использует NorthwindSiteMapProvider поставщика карты пользовательского узла](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Рис. 19**: навигации теперь использует поставщика карты узла Custom `NorthwindSiteMapProvider` ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))


Другие элементы пользовательского интерфейса навигации можно использовать в дополнение к SiteMapPath, такие как элементы управления Menu и TreeView. `Default.aspx`, `ProductsByCategory.aspx`, И `ProductDetails.aspx` страниц в этом руководстве, например, для загрузки всех включают элементы управления меню (см. рис. 20). См. в разделе [s ASP.NET 2.0, изучив возможности переходов по узлу](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) и [с помощью элементов управления навигации узла](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) раздел [краткие руководства по ASP.NET 2.0](https://quickstarts.asp.net/QuickStartv20/aspnet/) более подробно рассмотрено элементы управления для переходов и система карты узла в ASP.NET 2.0.


[![Элемент управления Menu список всех категорий и продуктов](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Рис. 20**: меню управления перечислены Each категориях и продуктах ([Просмотр полноразмерного изображения](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))


Как упоминалось ранее в этом учебнике, структуре карты сайта может осуществляться программно с помощью `SiteMap` класса. Следующий код возвращает корень `SiteMapNode` поставщика по умолчанию:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Так как `AspNetXmlSiteMapProvider` является поставщиком по умолчанию для нашего приложения, приведенный выше код возвращает корневой узел, определенный в `Web.sitemap`. Для поставщика карты узла, кроме порта по умолчанию следует использовать `SiteMap` класс s [ `Providers` свойство](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) следующим образом:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Где *имя* имя пользовательского поставщика карт сайтов ("Борей", для нашего веб-приложения).

Для доступа к члену, относящиеся к поставщик карты узла, используйте `SiteMap.Providers["name"]` для получения экземпляра поставщика, а затем приведите его к соответствующему типу. Например, для отображения `NorthwindSiteMapProvider` s `CachedDate` свойство на странице ASP.NET, используйте следующий код:


[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> Не забудьте протестировать функции зависимость кэша SQL. После см. в статье `Default.aspx`, `ProductsByCategory.aspx`, и `ProductDetails.aspx` страницы, перейдите к одному из учебников по редактирование, вставка и удаление разделов и измените имя категории или продукта. Затем вернитесь к одной из страниц в `SiteMapProvider` папку. При условии, что прошло достаточно времени для механизма опроса для Обратите внимание на изменения в основную базу данных, карты узла следует обновить для отображения нового продукта или имя категории.


## <a name="summary"></a>Сводка

ASP.NET 2.0 включает в себя карты сайта с функциями `SiteMap` класс, управляет ряд встроенных навигации Web и поставщика карты узла по умолчанию, который ожидает, что данные карты сайта сохраняются в XML-файл. Чтобы использовать информации карты узла из другого источника как из базы данных, архитектура s приложения или удаленного веб-службы, нам необходимо создать пользовательского поставщика карт сайтов. Это предполагает создание класса, производный прямо или косвенно, из `SiteMapProvider` класса.

В этом учебнике мы рассмотрели создание пользовательского поставщика карт сайтов, карты узла на основании информации product и category, вызванных из архитектуры приложения. Наш поставщик расширенных `StaticSiteMapProvider` класс и полученный создание `BuildSiteMap` метод, который извлекает данные, создании иерархии карты узла, а также в кэше итоговую структуру в переменную уровня класса. Мы использовали зависимость кэша SQL с функцию обратного вызова, чтобы сделать недействительными кэшированные структуры, когда базовый `Categories` или `Products` изменения данных.

Счастливого вам программирования!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, обсуждавшимся в этом руководстве см. в следующих ресурсах:

- [Хранение карты сайта в SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) и [поставщик карты узла SQL ve вы ждали](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Модели поставщика s краткий обзор ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Набор средств поставщика](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Изучение ASP.NET 2.0 возможности переходов по узлу s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Дейв Gardner, Зак Джонс, Терезой Мерфи и Екатерина Leigh, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](building-a-custom-database-driven-site-map-provider-cs.md)
