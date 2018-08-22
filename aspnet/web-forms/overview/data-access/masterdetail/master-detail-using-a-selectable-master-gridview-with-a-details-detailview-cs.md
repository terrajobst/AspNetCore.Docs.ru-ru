---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: «Основной/подробности» с помощью выбираемого основного элемента GridView с DetailView (C#) | Документация Майкрософт
author: rick-anderson
description: Этом учебном курсе будет GridView, строки которого включают название и цену каждого продукта, а также кнопки "выбрать". При нажатии кнопки Select для particu...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9d75c80b4c1bac5011acc896d91ff2fcd5a19298
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829967"
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>«Основной/подробности» с помощью выбираемого основного элемента GridView с DetailView (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe) или [скачать PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> Этом учебном курсе будет GridView, строки которого включают название и цену каждого продукта, а также кнопки "выбрать". При нажатии кнопки Select для конкретного продукта приведет к его полные сведения для отображения в элементе управления DetailsView на этой же странице.


## <a name="introduction"></a>Вступление

В [предыдущем учебном курсе](master-detail-filtering-across-two-pages-cs.md) мы рассмотрели создание отчета «основной/подробности» с помощью двух веб-страницах: «основной» веб-страницы, с которой отображался список поставщиков и веб-странице «Подробности», перечислявшей продукты, предоставляемые выбранным Поставщик. Этот формат отчета могут быть включены в одну страницу. Этом учебном курсе будет GridView, строки которого включают название и цену каждого продукта, а также кнопки "выбрать". При нажатии кнопки Select для конкретного продукта приведет к его полные сведения для отображения в элементе управления DetailsView на этой же странице.


[![При нажатии кнопки Select отображаются сведения о продукте](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**Рис. 1**: при нажатии кнопки Select отображаются сведения о продукте ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Шаг 1: Создание элемента gridviewс возможностью выбора

Вспомним, что две страницы «основной/подробности» сообщить, что каждой основной записи имелась гиперссылка, при щелчке, отправленных пользователя на страницу подробностей, передав имя выбранной строки `SupplierID` значение в строке запроса. Такие гиперссылка добавлялась к каждой строке GridView, использующей HyperLinkField. Для одной страницы основной/подробности отчета, нам понадобится кнопки для каждой строки GridView, при нажатии на ней отображаются сведения. Можно настроить элемент управления GridView для включения кнопки Select для каждой строки, которая вызывает обратную передачу и помечает эту строку как GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Начните с добавления элемента управления GridView для `DetailsBySelecting.aspx` странице в `Filtering` папки, установка его `ID` свойства `ProductsGrid`. Добавьте новый ObjectDataSource, именуемый `AllProductsDataSource` , вызывающий `ProductsBLL` класса `GetProducts()` метод.


[![Создание нового ObjectDataSource, именуемого AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**Рис. 2**: создать элемент управления ObjectDataSource с именем `AllProductsDataSource` ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))


[![Использование класса ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**Рис. 3**: использование `ProductsBLL` класс ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))


[![Настройка ObjectDataSource на вызов метода GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**Рис. 4**: Настройка ObjectDataSource на Invoke `GetProducts()` метод ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))


Измените поля GridView удаления все, кроме `ProductName` и `UnitPrice` полей BoundField. Кроме того, вы можете настроить эти поля BoundField, при необходимости, например при форматировании `UnitPrice` BoundField как денежной единицы и изменив `HeaderText` свойств полей BoundFields. Эти действия можно выполнить графически, щелкнув ссылку Правка столбцов смарт-теге элемента GridView, или путем ручной настройки декларативного синтаксиса.


[![Удалите все, кроме ProductName и UnitPrice полей BoundField](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**Рис. 5**: удалите все, кроме `ProductName` и `UnitPrice` полей BoundField ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))


Окончательная разметка GridView является:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

Далее нам нужно пометить GridView как выбираемый, которая будет добавлена кнопка Select для каждой строки. Для этого просто установите флажок Разрешить выделение в смарт-теге элемента GridView.


[![Убедитесь в доступный для выбора строки GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**Рис. 6**: доступным для выделения строк GridView ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))


Установить флажок Enabling Selection добавлено поле добавляет CommandField для `ProductsGrid` GridView с его `ShowSelectButton` свойство присвоено значение True. В результате кнопка "выбрать" для каждой строки GridView, как показано на рис. 6. По умолчанию, кнопки Select визуализируются как LinkButton, но можно использовать кнопки или ImageButtons вместо через CommandField `ButtonType` свойство.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

При нажатии кнопки Select строки GridView обратная и GridView `SelectedRow` обновляется свойство. В дополнение к `SelectedRow` элемент GridView предоставляет свойства, [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), и [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) свойства. `SelectedIndex` Свойство возвращает индекс выбранной строки, тогда как `SelectedValue` и `SelectedDataKey` свойства возвращают значения, основанные на GridView [свойство DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

`DataKeyNames` Свойство позволяет связать одно или несколько полей данных значения с каждой строкой и часто применяется для соотнесения уникально идентифицирующей информации из базовых данных с каждой строкой GridView. `SelectedValue` Свойство возвращает значение первого `DataKeyNames` поля данных для выбранной строки, тогда как `SelectedDataKey` свойство возвращает выбранной строки `DataKey` объект, содержащий все значения для указанных полей ключа данных для этой строки.

`DataKeyNames` Свойству автоматически присваивается уникальная идентификация поля данных при привязке источника данных к GridView, DetailsView и FormView через конструктор. Хотя это свойство задано для нас автоматически в предшествующих учебных курсах, примеры работали бы без `DataKeyNames` указанное свойство. Тем не менее, для элемента gridviewс возможностью выбора в этом руководстве, а также для последующих учебных курсах, в которых мы будет изучать вставку, обновление и удаление `DataKeyNames` свойства должен быть задан правильно. Отвлекитесь и убедитесь, что элемента GridView `DataKeyNames` свойству `ProductID`.

Давайте просмотрим наши достижения в данный момент через браузер. Обратите внимание на то, что GridView перечисляет названия и цены всех продуктов вместе с кнопкой Select типа LinkButton. При нажатии кнопки Select вызывает обратную передачу. На шаге 2 будет показано, как заставить DetailsView отвечать на эту обратную передачу, отображая подробности о выбранном продукте.


[![Каждая строка продукта содержит кнопку LinkButton SELECT](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**Рис. 7**: каждая строка продукта содержит кнопкой Select типа LinkButton ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))


### <a name="highlighting-the-selected-row"></a>Выделение выбранной строки

`ProductsGrid` GridView есть `SelectedRowStyle` свойство, которое может использоваться для указания стиля оформления выбранной строки. При должном использовании это может повысить удобство работы пользователя с более четко показывая, какая строка GridView выбрана. Для этого руководства пусть выбранная строка будет выделяться желтым фоном.

Как и в предыдущих учебных курсах, постараемся сохранить относящиеся к эстетическим деталям параметры, определенные в виде классов CSS. Таким образом, создайте новый класс CSS в `Styles.css` с именем `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

Чтобы применить этот класс CSS для `SelectedRowStyle` свойство *все* изменение элементов управления GridView в серии учебных `GridView.skin` обложки в `DataWebControls` темы для включения `SelectedRowStyle` параметры, как показано ниже:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

С этим добавлением выбранная строка GridView теперь выделяется желтым цветом.


[![Настройка внешнего вида выбранной строки, с помощью свойства SelectedRowStyle элемента GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**Рис. 8**: Настройка выбранные строки с помощью внешнего вида элемента GridView `SelectedRowStyle` свойство ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Шаг 2: Отображение сведений о выбранном продукте в элементе управления DetailsView

С помощью `ProductsGrid` GridView готов, остается лишь добавить элемент DetailsView, отображающий сведения о конкретном выбранном продукте. Добавьте элемент управления DetailsView над GridView и создайте новый ObjectDataSource, именуемый `ProductDetailsDataSource`. Поскольку мы хотим, чтобы этот DetailsView отображал информацию о выбранном продукте конкретном, Настройка `ProductDetailsDataSource` использовать `ProductsBLL` класса `GetProductByProductID(productID)` метод.


[![Вызов метода класса ProductsBLL GetProductByProductID(productID)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**Рис. 9**: вызвать `ProductsBLL` класса `GetProductByProductID(productID)` метод ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))


У *`productID`* значение параметра, полученные из элемента управления GridView `SelectedValue` свойство. Как уже говорилось ранее, GridView `SelectedValue` свойство возвращает значение первого ключа данных для выбранной строки. Таким образом, крайне важно, GridView `DataKeyNames` свойству `ProductID`, так что выбранной строки `ProductID` значение возвращается `SelectedValue`.


[![Значение параметра productID свойство SelectedValue элемента GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**Рис. 10**: задать *`productID`* параметра к элементу GridView `SelectedValue` свойство ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))


Один раз `productDetailsDataSource` ObjectDataSource правильно настроен и привязан к элементу DetailsView, это руководство представляет собой полный! При первом посещении страницы выбранные строки отсутствуют, поэтому GridView `SelectedValue` возвращает `null`. Так как отсутствуют продукты с `NULL` `ProductID` значение, не возвращает записей `GetProductByProductID(productID)` метода, это означает, что DetailsView не отображается (см. рис. 11). При нажатии кнопки Select строки GridView, обратная передача и DetailsView обновляется. Это раз свойство GridView `SelectedValue` возвращает `ProductID` выбранной строки, `GetProductByProductID(productID)` возвращает метод `ProductsDataTable` с информацией о данном конкретном продукте, а DetailsView показывает эти подробности (см. рис. 12).


[![Отображается при первом посещении страниц по только в GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**Рис. 11**: при первом посещении отображается только GridView ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))


[![При выборе строки, отображаются сведения о продукте](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**Рис. 12**: при выборе строки, отображаются сведения о продукте ([Просмотр полноразмерного изображения](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))


## <a name="summary"></a>Сводка

В этом и трех предшествующих учебных курсах мы рассмотрели ряд приемов отображения отчетов «основной/подробности». В этом учебном курсе мы рассмотрели использование элемента gridviewс возможностью выбора для размещения основных записей и DetailsView для отображения сведений о выбранных основных записях на этой же странице. В предыдущих учебных курсах мы рассмотрели способ отображения обобщенных и подробных отчетов с помощью списков DropDownList и отображение основных записей на одной странице, а также записей на другом.

Данный учебный курс завершает рассмотрение отчетов «основной/подробности». Начиная с Далее tutorialwe начну исследование настраиваемого форматирования с помощью GridView, DetailsView и FormView. Мы увидим, как настроить внешний вид этих элементов управления на основе данных, привязанных к ним, как создать сводку данных в нижнем колонтитуле элемента GridView и как использовать шаблоны, чтобы получить большую степень контроля над компоновкой.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был (Hilton giesenow). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](master-detail-filtering-across-two-pages-cs.md)
> [Вперед](master-detail-filtering-with-a-dropdownlist-vb.md)
