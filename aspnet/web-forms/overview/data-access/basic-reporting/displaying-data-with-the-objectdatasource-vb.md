---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Отображение данных с ObjectDataSource (VB) | Документы Microsoft
author: rick-anderson
description: В этом учебнике рассматривается в элемент управления ObjectDataSource, с помощью этого элемента управления можно привязать данные из МЕТОДА, созданные в предыдущем учебнике без havi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: ec3be56e1bb4294402351ff05e9209fe97510748
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877350"
---
<a name="displaying-data-with-the-objectdatasource-vb"></a>Отображение данных с ObjectDataSource (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) или [скачать PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> В этом учебнике рассматривается на элемент управления ObjectDataSource, с помощью этого элемента управления можно привязать данные из МЕТОДА, созданные в предыдущем учебнике без необходимости написания строка кода!


## <a name="introduction"></a>Вступление

С нашей приложения веб-сайта и архитектура макет страниц мы все готово для начала изучения того, как выполнять набор общих данных и отчетов-задач, связанных с. На предыдущих уроках мы рассмотрели программные методы привязки данных DAL и уровень бизнес-ЛОГИКИ на веб-страницу ASP.NET элемента управления данными. Этот синтаксис назначение данных веб-элемент управления `DataSource` свойства к данным для отображения и последующего вызова элемента управления `DataBind()` метод использовался в приложениях ASP.NET 1.x и можно продолжать использовать в приложениях 2.0. Однако в ASP.NET 2.0 новых источников данных предоставляют декларативный способ работы с данными. Использование этих элементов управления можно привязать данные из МЕТОДА, созданные в предыдущем учебнике без необходимости написания строка кода!

ASP.NET 2.0 есть пять встроенных источников данных [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), и [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) несмотря на то, что можно создавать собственные [элементами управления источниками данных пользовательских](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), при необходимости. Так как мы уже создали архитектуру нашего учебного приложения, мы будем работать с ObjectDataSource от наших классов уровень бизнес-ЛОГИКИ.


![ASP.NET 2.0 включает пять встроенных источников данных](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Рис. 1**: ASP.NET 2.0 включает пять встроенных источников данных


Элемент управления ObjectDataSource выступает в качестве прокси-сервер для работы с некоторыми объектами. Чтобы настроить элемент управления ObjectDataSource мы укажите это базовый объект и его методы сопоставлении ObjectDataSource `Select`, `Insert`, `Update`, и `Delete` методы. После этого объекта указан, и его методы сопоставлен ObjectDataSource, мы затем привязать к данным веб-элемент управления ObjectDataSource. ASP.NET поставляется с многие данных веб-элементов управления, включая GridView, DetailsView, RadioButtonList и DropDownList, среди прочего. Во время жизненного цикла страницы данных веб-элемент управления может потребоваться доступ к данным он привязан к, которой он предстоит выполнить путем вызова его ObjectDataSource `Select` метода; Если данных веб-элемент управления поддерживает вставку, обновления или удаления, может вызывать его ObjectDataSource `Insert`, `Update`, или `Delete` методы. Как показано на следующей диаграмме, эти вызовы ObjectDataSource методам соответствующего базового объекта затем направляются.


[![Элемент управления ObjectDataSource выступает в качестве учетной записи-посредника](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**На рисунке 2**: ObjectDataSource выступает в качестве прокси-сервера ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image4.png))


Хотя элемент управления ObjectDataSource можно использовать для вызова методов для вставки, обновления или удаления данных, мы рассмотрим только возврат данных. с помощью ObjectDataSource и веб-элементы управления, которые изменяют данные будут посвящены следующие учебники.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Шаг 1: Добавление и настройка элемента управления ObjectDataSource

Сначала откройте `SimpleDisplay.aspx` страницы в `BasicReporting` папки, перейдите в представление конструктора и перетащите элемент управления ObjectDataSource из панели элементов в область конструктора страницы. ObjectDataSource отображается как серый прямоугольник в рабочей области конструирования, так как он не создает разметку; он просто получает доступ к данным путем вызова метода из указанного объекта. Данные, возвращенные ObjectDataSource могут отображаться в данных веб-элемента управления GridView, DetailsView, FormView и т. д.

> [!NOTE]
> Кроме того, можно сначала добавить данные веб-элемент управления на страницу и выберите его в смарт-теге &lt;новый источник данных&gt; параметр из раскрывающегося списка.


Чтобы указать базовый объект и способ сопоставления методы этого объекта в элемент управления ObjectDataSource ObjectDataSource, щелкните ссылку настройки источника данных из элемента управления ObjectDataSource смарт-тега.


[![Нажмите кнопку настроить связь с источником данных из смарт-тег](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Рис. 3**: нажмите кнопку настроить связь с источником данных из смарт-тега ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image7.png))


Появится мастер настройки источника данных. Во-первых мы необходимо указать объект, который является работа с ObjectDataSource. Если установлен флажок «Показывать только компоненты данных», в раскрывающемся списке на этом экране перечислены только объекты, имеющие атрибут `DataObject` атрибута. В настоящее время наш список включает адаптеров таблиц в типизированный набор данных и классы уровень бизнес-ЛОГИКИ, созданный в предыдущем учебнике. Если вы не добавляли `DataObject` атрибут классам уровня бизнес-логики не увидят их в этот список. В этом случае, снимите флажок «Показывать только компоненты данных» для просмотра всех объектов, которые следует включить классы уровень бизнес-ЛОГИКИ (вместе с другими классами типизированных наборов данных DataTables, объектов DataRow и так далее).

В первом экране мастера выберите `ProductsBLL` класса из раскрывающегося списка и нажмите кнопку Далее.


[![Укажите объект для использования с элементом управления ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Рис. 4**: Укажите объект для использования с элементом управления ObjectDataSource ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image10.png))


На следующем экране мастера предлагается выбрать, какой метод следует вызвать элемент управления ObjectDataSource. В раскрывающемся списке перечислены эти методы, которые возвращают данные объекта, выбранного на предыдущем экране. Здесь мы видим `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, и `GetProductsBySupplierID`. Выберите `GetProducts` метода из раскрывающегося списка и нажмите кнопку Готово (Если вы добавили `DataObjectMethodAttribute` для `ProductBLL`методы, как показано в предыдущем учебнике, этот параметр будет выбран по умолчанию).


[![Выберите метод для возвращения данных на вкладке SELECT](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Рис. 5**: выберите метод для возвращения данных на вкладке ВЫБЕРИТЕ ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Вручную настроить элемент управления ObjectDataSource

Мастер настройки источника данных ObjectDataSource предоставляет быстрый способ для указания объекта, к которому он использует и связать вызываются какие методы объекта. Можно однако настроить ObjectDataSource через его свойства в окне «Свойства» или непосредственно в декларативной разметкой. Просто установите `TypeName` свойство в тип базового объекта для использования и `SelectMethod` на метод, который вызывается при получении данных.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Даже если вы предпочитаете мастер настройки источника данных, которые могут возникнуть ситуации, когда необходимо вручную настроить элемент управления ObjectDataSource, как мастер выводит только классы, созданные разработчиком. Если вы хотите привязать ObjectDataSource на класс в .NET Framework, такие как [класс членства](https://msdn.microsoft.com/library/system.web.security.membership.aspx), чтобы получить доступ к учетной записи пользователя или [класс Directory](https://msdn.microsoft.com/library/system.io.directory.aspx) для работы с сведения о файловой системе необходимо будет вручную настроить свойства элемента управления ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Шаг 2: Добавление веб-элемент управления данными и привязке его ObjectDataSource

После добавлен на страницу и настройки ObjectDataSource все готово для добавления данных веб-элементов управления страницы, чтобы отобразить данные, возвращенные ObjectDataSource `Select` метод. Все данные веб-элемент управления можно привязать к элементу ObjectDataSource; Давайте взглянем на отображение данных ObjectDataSource в GridView, DetailsView и FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Привязка элемента управления GridView в элемент управления ObjectDataSource

Добавление элемента управления GridView с панели элементов для `SimpleDisplay.aspx`в область конструктора. В смарт-теге элемента GridView выберите элемент управления ObjectDataSource, который мы добавили в шаге 1. Автоматически создается поле BoundField в GridView для каждого свойства, возвращенных данных из элемента управления ObjectDataSource `Select` метод (а именно, свойства, определенные DataTable продуктов).


[![Был добавлен на страницу элемент управления GridView и связывать элемент управления ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Рис. 6**: GridView будет добавлен на страницу и связанных ObjectDataSource ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image16.png))


Затем можно настроить, изменить или удалить стояли GridView, выбрав пункт Правка столбцов из смарт-тега.


[![Управление стояли GridView через диалоговое окно Изменение столбцов](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Рис. 7**: диалоговое окно Управление GridView стояли через изменение столбцов ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image19.png))


Теперь пора изменить стояли GridView, удаление `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, и `ReorderLevel` стояли. Выберите из списка в левом нижнем BoundField и нажмите кнопку "Удалить" (красный крест) для их удаления. После этого изменения порядка стояли, чтобы `CategoryName` и `SupplierName` стояли перед `UnitPrice` BoundField, выбрав эти стояли и нажав кнопку со стрелкой вверх. Задать `HeaderText` свойства оставшиеся стояли для `Products`, `Category`, `Supplier`, и `Price`соответственно. Затем попросите `Price` денежный формат валюты, задав BoundField `HtmlEncode` свойству значение False и его `DataFormatString` свойства `{0:c}`. И наконец, выровняем `Price` справа и `Discontinued` — по центру через `ItemStyle` / `HorizontalAlign` свойство.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]


[![Настраивались стояли GridView](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Рис. 8**: GridView настраивались стояли ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Использование тем для согласованного внешнего вида

Эти учебники стремятся удалит все параметры на уровне управления, вместо использование каскадных таблиц стилей, определенных во внешнем файле, когда это возможно. `Styles.css` Файл содержит `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, и `AlternatingRowStyle` классы CSS, которые должны использоваться для определяют внешний вид данных, веб-элементы управления, используемые в этих учебниках. Для этого нужно присвоить свойству `CssClass` свойства `DataWebControlStyle`и его `HeaderStyle`, `RowStyle`, и `AlternatingRowStyle` свойств `CssClass` свойства соответствующим образом.

Если настроить перечисленные `CssClass` свойства на веб-элемент управления, необходимо явно установить значения этих свойств для всех данных, веб-элемент управления, добавленный к нашим руководствам. Более удобный подход состоит в определении свойства CSS по умолчанию для GridView, DetailsView и FormView контролирует использование тем. Тема — это совокупность свойств уровня для элемента управления, изображения и классов CSS, которые могут быть применены к страницам на сайте для принудительного применения общий внешний вид и поведение.

Нашей теме не будет ни изображений, ни файлов CSS (Мы оставим таблицу стилей `Styles.css` как-определяется в корневой папке веб-приложения), но будет содержать две обложки. Обложка — файл, определяющий свойства по умолчанию для веб-элемента управления. В частности, у нас будет файл обложки для элементов управления GridView и DetailsView, указывающее значение по умолчанию `CssClass`-связанных свойств.

Начните с добавления в проект с именем файла обложки `GridView.skin` , щелкнув правой кнопкой мыши имя проекта в обозревателе решений и выбрав команду Добавить новый элемент.


[![Добавление файла обложки GridView.skin](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Рис. 9**: добавить файл обложки с именем `GridView.skin` ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image25.png))


Файлы обложки должны быть помещены в тему, которая хранится в `App_Themes` папки. Так как мы еще нет такая папка, Visual Studio окна предложит создать его нам при добавлении первого файла обложки. Нажмите "Да", чтобы создать `App_Theme` папку и сохранить новый `GridView.skin` файл существует.


[![Позволить Visual Studio, создайте папку App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Рис. 10**: позволить Visual Studio создать `App_Theme` папку ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image28.png))


Это создаст новую тему в `App_Themes` папку с именем GridView с файл обложки `GridView.skin`.


![Темы GridView имеет был добавлен в папку App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Рис. 11**: темы GridView был добавлен в `App_Theme` папки


Переименуйте темы GridView в DataWebControls (щелкните правой кнопкой мыши в папке GridView `App_Theme` папку и выберите Переименовать). Затем введите следующую разметку в `GridView.skin` файла:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Определяет свойства по умолчанию для `CssClass`-связанных свойств для всех элементов GridView все страницы, использующие DataWebControls темы. Теперь добавим обложку для DetailsView данных веб-элемент управления, мы будем использовать ближайшее время. Добавить новую обложку DataWebControls тему, с именем `DetailsView.skin` и добавьте следующую разметку:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

С нашей определена тема последним шагом является применение темы странице ASP.NET. Темы можно применять для страницу по отдельности или для всех страниц веб-сайта. Используем эту тему для всех страниц веб-сайта. Чтобы сделать это, добавьте следующую разметку, чтобы `Web.config` `<system.web>` разделе:


[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

Это все, чтобы он! `styleSheetTheme` Параметр указывает, что свойства, заданные в теме *не* свойства, определенные на уровне элемента управления. Чтобы указать, что параметры темы следует заменяют параметры управления, используйте `theme` атрибута вместо `styleSheetTheme`; к сожалению, параметры темы не отображаются в конструкторе Visual Studio. См. [Обзор обложки и темы ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx) и [темы с помощью серверных стили](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) на тем и обложек; Подробнее [How To: применение тем ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) для получения дополнительных сведений о Настройка страницы для использования темы.


[![GridView отображает имя продукта, категории, поставщика, цены и информации о поддержке](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Рис. 12**: GridView отображает имя продукта, категории, поставщика, цены и неподдерживаемые сведения ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Отображение одной записи за раз в DetailsView

GridView отображает одну строку для каждой записи элемента управления источником данных, к которому он привязан. Бывают случаи, тем не менее, если необходимо отобразить записи или только одной записи за раз. [Управления DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) предлагает функциональные возможности, Подготовка к просмотру как HTML `<table>` с двумя столбцами и одной строке для каждого столбца или свойства, связанного с элементом управления. DetailsView можно считать GridView с одной записью поворачиваются на 90 градусов.

Начните с добавления элемента управления DetailsView *выше* GridView в `SimpleDisplay.aspx`. Затем привяжите его к тому же элементу управления ObjectDataSource как GridView. Как и с GridView, BoundField будут добавлены в DetailsView для каждого свойства объекта, возвращаемого элемента управления ObjectDataSource `Select` метод. Единственное отличие заключается в том, что стояли DetailsView располагаются по горизонтали, а не по вертикали.


[![Добавление DetailsView на страницу и привязать его к ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Рис. 13**: добавление DetailsView на страницу и привязать его к коллекции ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image35.png))


Как GridView DetailsView стояли может быть оптимизировано для более специализированного отображают данные, возвращенные ObjectDataSource. На рисунке 14 показана DetailsView после его стояли и `CssClass` свойства были настроены, чтобы сделать его внешний вид, аналогичный приведенному в примере GridView.


[![Отображение одной записи при DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Рис. 14**: DetailsView. Отображение одной записи ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image38.png))


Обратите внимание, что DetailsView отображает только первую запись, полученную из источника данных. Дающим пользователю осуществлять пошаговое выполнение всех записей, одновременно, нам необходимо включить разбиение на страницы DetailsView. Чтобы сделать это, вернитесь в Visual Studio и установите флажок Включить постраничный просмотр в DetailsView смарт-тега.


[![Включить постраничный просмотр в элементе управления DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Рис. 15**: включить разбиение по страницам в элементе управления DetailsView ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image41.png))


[![Включении разбиения на страницы DetailsView позволяет пользователю просматривать любые из этих продуктов](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**На рисунке 16**: С включено разбиение на страницы DetailsView позволяет пользователю просматривать любые из этих продуктов ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image44.png))


Мы поговорим о разбиение по страницам в будущих учебниках.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Более гибкий макет для отображения одной записи за раз

DetailsView довольно жесткая в его отображения каждой записи возвращаются из коллекции. Мы можете более гибкий представление данных. Например, вместо отображается название продукта, категории, поставщика, цены и информация о поддержке в отдельной строке, мы может потребоваться отображать название продукта и цена в `<h4>` заголовком категорию и поставщика информация, представленная под именем и цены в меньший размер шрифта. И мы не имеет значения отобразить имена свойств (продукта, категории и т. д.) рядом со значениями.

[Управления FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) предоставляет такую настройку. Вместо использования поля (как GridView и DetailsView), FormView использует шаблоны, которые позволяют смешивать веб-элементов управления, статические HTML и [синтаксис привязки данных](http://www.15seconds.com/issue/040630.htm). Если вы знакомы с элементом управления повторителем из ASP.NET 1.x, можно считать из FormView повторителя для отображения одной записи.

Добавить элемент управления FormView `SimpleDisplay.aspx` область конструктора страницы. Сначала FormView появится в виде блока серый говорит о том, что нам нужно указать, как минимум, элемент управления `ItemTemplate`.


[![FormView должен включать ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Рисунок 17**: необходимо включить FormView `ItemTemplate` ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image47.png))


Можно привязать FormView непосредственно к элементу управления источником данных через FormView смарт-тег, который создает значение по умолчанию `ItemTemplate` автоматически (вместе с `EditItemTemplate` и `InsertItemTemplate`, если элемент управления ObjectDatatSource `InsertMethod` и `UpdateMethod` свойств). Однако для этого примера давайте привязка данных к FormView и указать его `ItemTemplate` вручную. Начать с настройки FormView `DataSourceID` свойства `ID` элемента управления ObjectDataSource `ObjectDataSource1`. Создайте `ItemTemplate` , чтобы он отображал имя продукта и цены в `<h4>` имен элементов и категории и shipper ниже, позволяет уменьшить размер шрифта.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]


[![На первом продукта (Chai) отображается в пользовательский формат](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**На рисунке 18**: первого продукта (Chai) отображается в формате ([Просмотр полноразмерное изображение](displaying-data-with-the-objectdatasource-vb/_static/image50.png))


`<%# Eval(propertyName) %>` — Это синтаксис привязки данных. `Eval` Метод возвращает значение указанного свойства для текущего объекта привязки к элементу управления FormView. См. в статье Алекс Гомера [упрощенное и расширенных данных привязки синтаксис в ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) Дополнительные сведения о подробно привязки данных.

Как DetailsView FormView отображаются только первая запись из коллекции. Вы можете включить постраничного просмотра в FormView, чтобы пошагово одного продукта за раз.

## <a name="summary"></a>Сводка

Доступ и отображение данных на уровне бизнес-логики может осуществляться без написания кода, благодаря элементу управления ObjectDataSource ASP.NET 2.0. ObjectDataSource вызывает указанный метод класса и возвращает результаты. Эти результаты могут отображаться в данных веб-элемент управления, к которому привязан элемент управления ObjectDataSource. В этом учебнике мы рассмотрели привязку элементов управления GridView, DetailsView и FormView в элемент управления ObjectDataSource.

Теперь мы только знаете способ использования ObjectDataSource для вызова метода без параметров, но что делать, если нужно вызвать метод, который ожидает, что входные параметры, такие как `ProductBLL` класса `GetProductsByCategoryID(categoryID)`? Чтобы вызвать метод, который ожидает один или несколько параметров настройте ObjectDataSource для указания значений для этих параметров. Мы рассмотрим, как выполнить это в нашем [следующее руководство](declarative-parameters-vb.md).

Программирование довольны!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Создание собственных элементов управления источниками данных](https://msdn.microsoft.com/library/ms364049.aspx)
- [Примеры GridView для ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Простой и расширенный синтаксис привязки данных в ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Темы в ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Стили на стороне сервера с использованием тем.](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Практическое руководство: Применение тем ASP.NET программными средствами](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было – Хилтон Гизнау. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [Вперед](declarative-parameters-vb.md)
