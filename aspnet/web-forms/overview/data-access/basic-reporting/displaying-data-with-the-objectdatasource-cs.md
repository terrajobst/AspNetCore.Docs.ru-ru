---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: Отображение данных с помощью ObjectDataSource (C#) | Документация Майкрософт
author: rick-anderson
description: В этом учебнике рассматривается в элемент управления ObjectDataSource, с помощью этого элемента управления, можно привязать данные, полученные из BLL, созданные в предыдущем учебном курсе без havi...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: f10745eee9f6ac04e670d710a4ac999c9ddda50b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814929"
---
<a name="displaying-data-with-the-objectdatasource-c"></a>Отображение данных с помощью ObjectDataSource (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe) или [скачать PDF](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> В этом учебнике рассматривается в элемент управления ObjectDataSource, с помощью этого элемента управления, можно привязать данные, полученные из BLL, созданные в предыдущем учебном курсе без необходимости написания строки кода!


## <a name="introduction"></a>Вступление

Мы можем приступить к исследованию возможностей для выполнения разных общих данных и отчетов-задач, связанных с нашей приложения веб-сайта и архитектура макет страниц. На предыдущих уроках мы рассмотрели программные методы привязки данных в DAL и BLL данных веб-элемента управления на странице ASP.NET. Этот синтаксис, назначение данных веб-элемента управления `DataSource` свойства к данным для отображения и последующего вызова элемента управления `DataBind()` метод был шаблон, используемый в приложениях ASP.NET 1.x и можно продолжать использовать в приложениях 2.0. Тем не менее ASP.NET 2.0 новых источников данных предлагают также декларативный метод работы с данными. Использование этих элементов управления, можно привязать данные, полученные из BLL, созданных в [предыдущем учебном курсе](../introduction/creating-a-business-logic-layer-cs.md) без необходимости написания строки кода!

ASP.NET 2.0 есть пять встроенных источников данных [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx), и [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx) несмотря на то, что вы можете создавать собственные [элементами управления источниками данных, пользовательских](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), при необходимости. Так как мы уже создали архитектуру нашего учебного приложения, мы будем использовать элемент управления ObjectDataSource к нашим классам BLL.


![ASP.NET 2.0 включает пять встроенных источников данных](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**Рис. 1**: ASP.NET 2.0 включает пять встроенных источников данных


Элемент управления ObjectDataSource выступает в качестве прокси-сервер для работы с некоторыми объектами. Чтобы настроить элемент управления ObjectDataSource мы укажите это базовый объект и его методы сопоставлении ObjectDataSource `Select`, `Insert`, `Update`, и `Delete` методы. После того как был указан этот базовый объект и его методы сопоставляются с ObjectDataSource, можно будет привязать ObjectDataSource к данных веб-элемента управления. ASP.NET имеется немало веб-элемент управления GridView, DetailsView, RadioButtonList и DropDownList, включая, помимо прочих. Во время жизненного цикла страницы, данные веб-элемент управления может потребоваться доступ к данным он привязан, которой он предстоит выполнить путем вызова метода `Select` метода; Если данные веб-элемент управления поддерживает вставку, обновление или удаление, может вызывать его ObjectDataSource `Insert`, `Update`, или `Delete` методы. Эти вызовы перенаправялет к методам соответствующего базового объекта ObjectDataSource как показано на следующей схеме.


[![Элемент управления ObjectDataSource выступает в качестве прокси-сервера](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**Рис. 2**: элемент управления ObjectDataSource выступает в качестве учетной записи-посредника ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image4.png))


Хотя элемент управления ObjectDataSource можно использовать для вызова методов для вставки, обновления или удаления данных, мы рассмотрим только возврат данных. используя элемент управления ObjectDataSource и веб-элементы управления, которые изменяют данные будут посвящены следующие учебники.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Шаг 1: Добавление и настройка элемента управления ObjectDataSource

Сначала откройте `SimpleDisplay.aspx` странице в `BasicReporting` папки, перейдите в представление конструктора и перетащите элемент управления ObjectDataSource из панели элементов в область конструктора страницы. ObjectDataSource появится как серый квадрат в области конструктора, так как он не создает никакой разметки; он просто получает доступ к данным путем вызова метода из указанного объекта. Данные, возвращаемые элементу ObjectDataSource могут отображаться с данным веб-элемента управления GridView, DetailsView, FormView и т. д.

> [!NOTE]
> В качестве альтернативы можно сначала добавить данные веб-элемента управления на страницу, а затем из его смарт-тега выберите &lt;новый источник данных&gt; параметр из раскрывающегося списка.


Чтобы указать базового объекта ObjectDataSource и сопоставлении методы этого объекта ObjectDataSource, щелкните ссылку Настройка источника данных смарт-теге ObjectDataSource.


[![Нажмите кнопку ссылка Configure Data Source в смарт-теге](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**Рис. 3**: нажмите кнопку настроить связь с источником данных в смарт-теге ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image7.png))


Откроется мастер настройки источника данных. Во-первых нам необходимо указать объект, который является элемент управления ObjectDataSource для работы с. Если установлен флажок «Show only data components», выберите в раскрывающемся списке на этом экране перечислены только объекты, имеющие `DataObject` атрибута. В настоящее время нас в списке есть классы TableAdapters в типизированный набор DataSet и классы BLL, которые мы создали в предыдущем учебном курсе. Если вы забыли добавить `DataObject` атрибутов к классам уровня бизнес-логики вы не увидите их в этом списке. В этом случае снимите флажок «Show only data components» для просмотра всех объектов, в том числе классы BLL (а также другие классы в типизированный набор DataSet DataTables, DataRows и т. д.).

В этом первом экране выберите `ProductsBLL` класса из раскрывающегося списка и нажмите кнопку Далее.


[![Укажите объект для использования с элементом управления ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**Рис. 4**: Укажите объект для использования с элементом управления ObjectDataSource ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image10.png))


На следующем экране мастер предложит указать элемент управления ObjectDataSource должен вызывать метод. В раскрывающемся списке перечислены методы, возвращающие данные в объект выбран на предыдущем экране. Здесь мы видим, `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, и `GetProductsBySupplierID`. Выберите `GetProducts` метод в раскрывающемся списке и нажмите кнопку Готово (Если вы добавили `DataObjectMethodAttribute` для `ProductBLL`в методы, как показано в предыдущем учебном курсе, этот параметр будет выбран по умолчанию).


[![Выберите метод для возврата данных на вкладке SELECT](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**Рис. 5**: выберите метод для возвращения данных на вкладке "ВЫБЕРИТЕ" ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Настройте элемент ObjectDataSource вручную

Мастер настройки источника данных элемента управления ObjectDataSource позволяет быстро указать объект, который используется и связывания вызываются какие методы объекта. Тем не менее, можно настроить элемент управления ObjectDataSource через его свойства в окне «Свойства» или непосредственно в декларативной разметке. Просто задайте `TypeName` свойство в тип базового объекта, которое будет использоваться и `SelectMethod` на метод, который вызывается при получении данных.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

Даже если вы предпочитаете мастер настройки источников данных, которые могут возникнуть ситуации, когда вам нужно вручную настроить элемент управления ObjectDataSource, как в мастере отображаются только классы, созданные разработчиком. Если вы хотите привязать ObjectDataSource к классу в .NET Framework, такие как [класс членства](https://msdn.microsoft.com/library/system.web.security.membership.aspx), чтобы получить доступ к учетной записи пользователя, или [класс Directory](https://msdn.microsoft.com/library/system.io.directory.aspx) для работы с сведений о файловой системе необходимо вручную задать свойства элемента ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Шаг 2: Добавление веб-элемент данных и его привязка к элементу ObjectDataSource

После добавления на страницу и настройки ObjectDataSource мы готовы добавить веб-элементы управления для отображения данных, возвращенных ObjectDataSource на страницу `Select` метод. Все данные веб-элемента управления можно привязать к элементу ObjectDataSource; Давайте взглянем на отображение данных в GridView, DetailsView и FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Привязка элемента управления GridView к элементу ObjectDataSource

Добавление элемента управления GridView с панели инструментов для `SimpleDisplay.aspx`в область конструктора. Смарт-теге элемента GridView выберите элемент управления ObjectDataSource, который мы добавили на шаге 1. Автоматически будет создано поле BoundField в GridView для каждого свойства, возвращаемые данные из элемента управления ObjectDataSource `Select` метод (а именно, свойства, определенные в объекте Products DataTable).


[![Элемент GridView добавлен на страницу и привязан к элементу ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**Рис. 6**: GridView добавлен на страницу и привязки к элементу ObjectDataSource ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image16.png))


Затем можно настроить, изменить порядок или удалять поля BoundFields элемента GridView, выбрав пункт Правка столбцов в смарт-теге.


[![Управление BoundFields элемента GridView в диалоговом окне редактирования столбцов](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**Рис. 7**: диалоговое окно управления GridView поля BoundField, кроме через изменение столбцов ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image19.png))


Отвлекитесь и измените поля BoundFields элемента GridView, удаляя `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, и `ReorderLevel` полей BoundField. Просто выберите из списка в нижнем левом поле типа BoundField и нажмите кнопку "Удалить" (красный знак Х) для их удаления. Затем изменим порядок полей BoundFields так, что `CategoryName` и `SupplierName` стояли перед полем `UnitPrice` нужно выделить. для этого и нажав кнопку со стрелкой вверх. Задайте `HeaderText` свойства для оставшихся полей BoundFields `Products`, `Category`, `Supplier`, и `Price`, соответственно. После `Price` установим в денежном формате BoundField `HtmlEncode` значение False и его `DataFormatString` свойства `{0:c}`. И наконец, выровняем `Price` справа и `Discontinued` флажок по центру: воспользуемся `ItemStyle` / `HorizontalAlign` свойство.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![Настроенные поля BoundFields элемента GridView](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**Рис. 8**: элемента GridView настроенные поля BoundFields ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Использование тем для единообразия

Эти учебники стремятся удалить все параметры на уровне управления, вместо этого использование каскадных таблиц стилей, определенных во внешнем файле, когда это возможно. `Styles.css` Файл содержит `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, и `AlternatingRowStyle` классы CSS, которые должны использоваться для оформления данных веб-элементы управления, используемые в этих учебниках. Для этого нужно присвоить GridView `CssClass` свойства `DataWebControlStyle`и его `HeaderStyle`, `RowStyle`, и `AlternatingRowStyle` свойств `CssClass` свойства соответствующим образом.

Если настроить перечисленные свойства `CssClass` свойствами на уровне веб-элемента управления, необходимо явно установить значения этих свойств для всех данных, веб-элемент управления, добавленный в ходе наших уроков. Более удобный подход является определение свойств, связанных с CSS по умолчанию для элементов управления GridView, DetailsView и FormView при помощи темы. Тема — это коллекция свойств уровня управления, изображения и классы CSS, которые можно применять к страницам на сайте для обеспечения общего вида и впечатления.

Нашей теме не будет ни изображений, ни файлов CSS (Мы оставим таблицу стилей `Styles.css` как-, определенную в корневой папке веб-приложения), но будут две обложки. Обложка — это файл, определяющий свойства по умолчанию для веб-элемента управления. В частности, у нас будет файл обложки для элементов управления GridView и DetailsView, указывающее, значение по умолчанию `CssClass`-связанных свойств.

Начните с добавления нового файла обложки в проект с именем `GridView.skin` , щелкнув имя проекта в обозревателе решений и выбрав Add New Item.


[![Добавление файла обложки GridView.skin](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**Рис. 9**: добавить файл обложки, имя `GridView.skin` ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image25.png))


Файлы обложки нужно добавить в тему, которая хранится в `App_Themes` папку. Так как мы еще нет такую папку, Visual Studio любезно предложит ее создать нас при добавлении первого файла обложки. Нажмите "Да", чтобы создать `App_Theme` папку и сохранить новый `GridView.skin` файл существует.


[![Visual Studio создаст папку App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**Рис. 10**: позволить Visual Studio создать `App_Theme` папку ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image28.png))


Это создаст новую тему в `App_Themes` папку с именем GridView в файле обложки `GridView.skin`.


![Темы GridView имеет были добавлены к папке App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**Рис. 11**: имеет темы GridView добавлен `App_Theme` папки


Переименуйте тему GridView в DataWebControls (правой кнопкой мыши папку GridView в `App_Theme` папку и выберите Переименовать). После этого введите следующую разметку в `GridView.skin` файла:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

Определяет свойства по умолчанию `CssClass`-связанных свойств для всех элементов GridView на всех страницах, при помощи темы DataWebControls. Добавим обложку для элемента DetailsView, веб-элемента управления, который мы будем использовать вскоре данных. Добавить новую обложку тему DataWebControls, с именем `DetailsView.skin` и добавьте следующую разметку:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

С помощью наших определена тема осталось применить тему к странице ASP.NET. Можно применить тему, на основе страниц или для всех страниц в веб-сайта. Применим нашу тему для всех страниц на веб-сайте. Для этого добавьте следующую разметку для `Web.config` `<system.web>` разделе:


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

Это все, вот! `styleSheetTheme` Параметр указывает, что свойства, заданные в теме *не* свойства, определенные на уровне управления. Чтобы указать, что параметры темы должные параметры управления, используйте `theme` атрибута вместо `styleSheetTheme`; к сожалению, параметры темы, заданного с помощью `theme` атрибута не отображаются в представлении конструирования Visual Studio. См. [ASP.NET темы и обложки](https://msdn.microsoft.com/library/ykzx33wh.aspx) и [темы с помощью стили на стороне сервера](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) узнать больше о темы и обложки элементов см. в разделе [How To: применение тем ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx) дополнительную информацию о Настройка страницы для использования темы.


[![Элемент GridView отображает по имени продукта, категории, поставщика, цены и информации о поддержке](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**Рис. 12**: элемент GridView отображает имя продукта, категории, поставщика, цены и сведения, более не поддерживается ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Отображение одной записи за раз в DetailsView

Элемент GridView отображает по одной строке для каждой записи, полученной элементом управления источником данных, к которому он привязан. Бывают случаи, тем не менее, когда необходимо отобразить записи или только одной записи за раз. [Управления DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) обеспечивает эти функциональные возможности, создает элемент HTML `<table>` с двумя столбцами и одной строке для каждого столбца или свойства, привязанного к элементу управления. Элемент DetailsView можно считать элемента GridView с одной записью Повернуть на 90 градусов.

Начните с добавления элемента управления DetailsView *выше* GridView в `SimpleDisplay.aspx`. Затем привяжите его к тот же элемент управления ObjectDataSource, элемент GridView. Как и с GridView, BoundField будут добавлены к элементу управления DetailsView для каждого свойства объекта, возвращаемого ObjectDataSource `Select` метод. Единственным различием является то, что поля BoundFields в элементе DetailsView располагаются по горизонтали, а не по вертикали.


[![Добавление элемента DetailsView на страницу и привяжите его к элементу ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**Рис. 13**: Добавление элемента DetailsView на страницу и привяжите его к элементу ObjectDataSource ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image35.png))


Например GridView DetailsView поля BoundFields может быть оптимизировано для более функциональный отображения данных, возвращаемых элементом ObjectDataSource. Рис. 14 показан элемент DetailsView после BoundFields и `CssClass` заданы свойства, чтобы сделать его внешний вид, как в примере GridView.


[![Отображение одной записи при DetailsView](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**Рис. 14**: DetailsView отображение одной записи ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image38.png))


Обратите внимание на то, что элемент DetailsView отображает только первую запись, полученную из источника данных. Чтобы разрешить пользователю перебрать все записи, поочередно, мы должны включить функцию разбиения для элемента DetailsView. Чтобы сделать это, вернитесь в Visual Studio и установите флажок Enable Paging в смарт-теге DetailsView.


[![Включить разбиение по страницам в элементе управления DetailsView](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**Рис. 15**: включить разбиение по страницам в элементе управления DetailsView ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image41.png))


[![Включении разбиения на страницы элемент DetailsView позволяет пользователю просматривать всех продуктов](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**Рис. 16**: С включено разбиение на страницы элемент DetailsView позволяет пользователю просматривать всех продуктов ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image44.png))


Мы поговорим о разбиении на страницы в последующих руководствах.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Более гибкий макет для отображения одной записи за раз

DetailsView не слишком разнообразны, отображается ли он записей, возвращаемых элементом управления ObjectDataSource. Требуется более гибкое представление данных. Например, чтобы не выводить название продукта, категория, поставщик, цена и информация о поддержке в отдельной строке, мы может потребоваться отображения имени продукта и цена стояли в `<h4>` заголовок с категорию и поставщика сведения о пользователях ниже название и цену в меньший размер шрифта. И мы не нужны названия параметров (продукта, категории и т. д.) рядом со значениями.

[Управления FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) обеспечивает настройку такого типа. Вместо полей (как GridView и DetailsView), элемент FormView использует шаблоны, которые позволяют смешивать веб-элементов управления, статический HTML и [синтаксис привязки данных](http://www.15seconds.com/issue/040630.htm). Если вы знакомы с элементом управления Repeater в ASP.NET 1.x, можно представить себе элемент FormView как Repeater, выводящего на экран только одну запись.

Добавьте элемент управления FormView на `SimpleDisplay.aspx` область конструктора страницы. Сначала FormView появится в виде серого, говорит о том, что нам нужно предоставить по меньшей мере, элемента управления `ItemTemplate`.


[![FormView должен включать ItemTemplate](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**Рис. 17**: FormView требуется `ItemTemplate` ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image47.png))


FormView можно привязать непосредственно к элементу управления источника данных через смарт-тега FormView, который автоматически создаст стандартный шаблон `ItemTemplate` автоматически (вместе с `EditItemTemplate` и `InsertItemTemplate`, если элементе objectdatatsource настроены свойства `InsertMethod` и `UpdateMethod` свойств). Тем не менее, для этого примера давайте привязки данных к FormView и указать его `ItemTemplate` вручную. Сначала FormView `DataSourceID` свойства `ID` элемента управления ObjectDataSource, `ObjectDataSource1`. Создайте `ItemTemplate` , чтобы он отображал имя продукта и цену в `<h4>` элемента и категории и shipper имен ниже, позволяет уменьшить размер шрифта.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![Первого продукта (Chai) отображается в пользовательский формат](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**Рис. 18**: первый продукт (Chai) отображается в пользовательский формат ([Просмотр полноразмерного изображения](displaying-data-with-the-objectdatasource-cs/_static/image50.png))


`<%# Eval(propertyName) %>` — Это синтаксис привязки данных. `Eval` Метод возвращает значение указанного свойства для текущего объекта, привязываемого к элементу управления FormView. В статье Алекса Гомера [упрощенное и расширенных данных привязки синтаксис в ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) Дополнительные сведения о протоколах передачи привязки данных.

Как и элемент DetailsView FormView отображает только первую запись, возвращенную элементом управления ObjectDataSource. Вы можете включить разбиение по страницам в FormView, чтобы пошагово одного продукта за раз.

## <a name="summary"></a>Сводка

Доступ и отображение данных на уровне бизнес-логики может осуществляться без написания кода благодаря элементу управления ObjectDataSource ASP.NET 2.0. ObjectDataSource вызывает указанный метод класса и возвращает результаты. Эти результаты могут отображаться в данных веб-элемента управления, к которому привязан элемент управления ObjectDataSource. В этом учебнике мы рассмотрели привязки элементов управления GridView, DetailsView и FormView к ObjectDataSource.

Пока мы видели только способ использования ObjectDataSource для вызова метода без параметров, но что делать, если мы хотим вызвать любой метод, принимающий входные параметры, такие как `ProductBLL` класса `GetProductsByCategoryID(categoryID)`? Чтобы вызвать любой метод, принимающий один или несколько параметров нам нужно настроить элемент ObjectDataSource для указания значений для этих параметров. Узнаете, как для этого в наших [следующему руководству](declarative-parameters-cs.md).

Счастливого вам программирования!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, обсуждавшимся в этом руководстве см. в следующих ресурсах:

- [Создание собственных элементов управления источниками данных](https://msdn.microsoft.com/library/ms364049.aspx)
- [Примеры использования элемента GridView ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Простой и расширенный синтаксис привязки данных в ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Темы в ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Стили на стороне сервера с использованием тем.](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Практическое руководство: Применение тем ASP.NET программными средствами](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был (Hilton giesenow). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Вперед](declarative-parameters-cs.md)
