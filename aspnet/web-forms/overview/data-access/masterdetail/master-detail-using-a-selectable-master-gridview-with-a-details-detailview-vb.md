---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: Главного и подчиненного представлений с помощью выбираемого основного элемента GridView с DetailView (VB) | Документы Microsoft
author: rick-anderson
description: Этот учебник будет GridView, строки которого необходимо указать имя и цены каждого продукта, а также кнопка выбора. При нажатии кнопки Select для particu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: 80db1589de901f7364c05c5bb67829145579b6c0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Главного и подчиненного представлений с помощью выбираемого основного элемента GridView с DetailView (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) или [скачать PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> Этот учебник будет GridView, строки которого необходимо указать имя и цены каждого продукта, а также кнопка выбора. При нажатии кнопки "Select" для конкретного продукта приведет к его полные сведения для отображения в элементе управления DetailsView на одной странице.


## <a name="introduction"></a>Вступление

В [с предыдущим учебником](master-detail-filtering-across-two-pages-vb.md) мы узнали, как создание главного и подчиненного представлений отчета с помощью двух веб-страницы: «главный» веб-страницы, из которой мы отображается список поставщиков; и веб-страницы «подробности», эти продукты, предоставляемые выбранного в списке Поставщик. Данный формат отчета на две страницы могут включены в одну страницу. Этот учебник будет GridView, строки которого необходимо указать имя и цены каждого продукта, а также кнопка выбора. При нажатии кнопки "Select" для конкретного продукта приведет к его полные сведения для отображения в элементе управления DetailsView на одной странице.


[![При нажатии кнопки Select отображает сведения о продукте](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Рис. 1**: при нажатии кнопки Select отображает сведения о продукте ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))


## <a name="step-1-creating-a-selectable-gridview"></a>Шаг 1: Создание выбираемым GridView

Напомним, что две страницы главного и подчиненного представлений отчета, каждый главный запись включена гиперссылка, при нажатии отправлено пользователя на страницу сведений, передав имя выбранной строки `SupplierID` значение в строке запроса. Такие гиперссылки был добавлен к каждой строке GridView, с помощью HyperLinkField. Для одной страницы основной/подробности отчета, мы должны кнопка для каждого GridView строк, при нажатии показывает подробные сведения. Чтобы включить кнопку выбора для каждой строки, которая вызывает обратную передачу и помечает эту строку как GridView можно настроить элемента управления GridView [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx).

Начните с добавления элемента управления GridView для `DetailsBySelecting.aspx` страницы в `Filtering` папки, установка его `ID` свойства `ProductsGrid`. Добавьте новый элемент управления ObjectDataSource с именем `AllProductsDataSource` , вызывающий `ProductsBLL` класса `GetProducts()` метод.


[![Создайте элемент управления ObjectDataSource AllProductsDataSource с именем](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**На рисунке 2**: создание ObjectDataSource с именем `AllProductsDataSource` ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))


[![Используйте класс ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Рис. 3**: использование `ProductsBLL` класса ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))


[![Настройка ObjectDataSource для вызова метода GetProducts()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Рис. 4**: настройте элемент управления ObjectDataSource Invoke `GetProducts()` метод ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))


Изменить поля элемента GridView удаление всех, за исключением `ProductName` и `UnitPrice` стояли. Кроме того, вы можете настроить эти стояли, при необходимости, таких как формат `UnitPrice` BoundField виде валюты и изменение `HeaderText` свойства стояли. Эти действия могут быть выполнены в графическом виде, щелкнув ссылку Правка столбцов GridView смарт-теге или вручную настроив декларативного синтаксиса.


[![Удалите все, кроме ProductName и UnitPrice стояли](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Рис. 5**: удалите все, кроме `ProductName` и `UnitPrice` стояли ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))


Окончательную разметку элемента GridView является:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

Далее нам нужно пометить как доступный для выбора, GridView, который будет добавлять кнопку выбора для каждой строки. Чтобы сделать это, просто установите флажок Разрешить выделение в смарт-теге элемента GridView.


[![Сделать доступным для выбора строки GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Рис. 6**: Убедитесь, выбираемых строк GridView ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))


Проверка включения пункта добавляет CommandField для `ProductsGrid` GridView с его `ShowSelectButton` свойством, имеющим значение True. В результате кнопка выбора для каждой строки GridView, как показано на рис. 6. По умолчанию, выберите кнопки отображаются как элементов управления LinkButton, но также можно использовать кнопки или ImageButtons через CommandField `ButtonType` свойство.


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

При нажатии кнопки выберите строке GridView выполняется обратная и GridView `SelectedRow` свойство обновляется. В дополнение к `SelectedRow` предоставляет свойство, GridView [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx), и [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) свойства. `SelectedIndex` Свойство возвращает индекс выбранной строки, тогда как `SelectedValue` и `SelectedDataKey` свойства возвращают значений на основе GridView [свойство DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx).

`DataKeyNames` Свойство используется для связывания одного или несколько полей данных значения с каждой строкой и обычно используется для атрибута, однозначно идентифицирующие сведения из базовых данных с каждой строки в GridView. `SelectedValue` Свойство возвращает значение первого `DataKeyNames` поля данных для выбранной строки при этом `SelectedDataKey` свойство возвращает выбранную строку `DataKey` объект, который содержит все значения для ключевых полей, указанные данные для этой строки.

`DataKeyNames` Свойству автоматически присваивается уникальное определение полей данных при привязке источника данных к GridView, DetailsView и FormView через конструктор. Пока это свойство задана нам автоматически в предыдущем учебники, примеры могло бы работать без `DataKeyNames` указанное свойство. Однако для GridView, доступный для выбора в этом учебнике, а также для будущих учебников, в которых мы будем подробнее вставки, обновления и удаления `DataKeyNames` свойства должен быть задан правильно. Теперь пора убедитесь, что к GridView `DataKeyNames` свойству `ProductID`.

Давайте просмотрите ход работы сих через браузер. Обратите внимание, что GridView указаны имя и цену всех продуктов вместе с LinkButton выберите. При нажатии кнопки Select вызывает обратную передачу. На шаге 2 будет рассмотрена возможность DetailsView отвечают на этом обратной передачи, отображая сведения для выбранного продукта.


[![Каждая строка продукта содержит выберите LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Рис. 7**: каждая строка продукта содержит LinkButton выберите ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))


## <a name="highlighting-the-selected-row"></a>Выделение цветом выбранной строки

`ProductsGrid` GridView имеет `SelectedRowStyle` свойство, которое может использоваться для диктовки визуальный стиль для выбранной строки. Используется неправильно, это может улучшить взаимодействие с пользователем отображая более четко выбранные какие строки GridView. В этом учебнике давайте выбранного строка была выделена с желтым фоном.

Как и в более ранних учебные видеоматериалы, давайте стараться, чтобы уровень улучшения внешнего вида связанные параметры, определенные как классы CSS. Таким образом, создайте новый класс CSS в `Styles.css` с именем `SelectedRowStyle`.


[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Чтобы применить этот класс CSS для `SelectedRowStyle` свойство *все* изменить GridViews в серии учебника `GridView.skin` обложки в `DataWebControls` темы для включения `SelectedRowStyle` параметры, как показано ниже:


[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

В результате этого добавления выбранной строки GridView теперь выделено на желтом фоне.


[![Настройка внешнего вида выбранной строки с помощью свойства SelectedRowStyle GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Рис. 8**: Настройка выбранные строки с помощью внешнего вида элемента GridView `SelectedRowStyle` свойство ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))


## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Шаг 2: Вывод сведений о выбранном продукте в элементе управления DetailsView

С `ProductsGrid` завершить GridView, все, что остается только добавление DetailsView, в которой отображаются сведения о выбранных конкретного продукта. Добавление элемента управления DetailsView над элементом управления GridView и создать новый элемент управления ObjectDataSource с именем `ProductDetailsDataSource`. Поскольку мы хотим этого DetailsView для отображения определенного сведений о выбранного продукта, настройте `ProductDetailsDataSource` использовать `ProductsBLL` класса `GetProductByProductID(productID)` метод.


[![Вызвать метод GetProductByProductID(productID) класса ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Рис. 9**: вызов `ProductsBLL` класса `GetProductByProductID(productID)` метод ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))


У *`productID`* значение параметра, полученное из элемента управления GridView `SelectedValue` свойство. Как мы уже упоминали, GridView `SelectedValue` свойство возвращает значение для выбранной строки ключа первых данных. Таким образом, важно, GridView `DataKeyNames` свойству `ProductID`, после чего выбранную строку `ProductID` возвращается значение `SelectedValue`.


[![Значение productID параметра к свойству SelectedValue GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Рис. 10**: задать *`productID`* параметр к GridView `SelectedValue` свойство ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))


Один раз `productDetailsDataSource` ObjectDataSource был настроен правильно и привязан к DetailsView, завершения этого учебника! При первом посещении страницы не выбрана строка, поэтому элемента GridView `SelectedValue` возвращает `Nothing`. Поскольку нет ни одного продукта с `NULL` `ProductID` значение, записи не возвращаются `GetProductByProductID(productID)` метод, это означает, что не отображается DetailsView (см. рис. 11). После нажатия кнопки "Select" строке GridView обратная передача и обновлении DetailsView. Это время GridView `SelectedValue` возвращает `ProductID` выбранной строки `GetProductByProductID(productID)` возвращает метод `ProductsDataTable` с информацией об этом конкретного продукта и DetailsView отображает эти подробные сведения (см. рис. 12).


[![При отображении первой посещением только GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Рис. 11**: отображается при первом посещении только GridView ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))


[![После выбора строки, отображаются сведения о продукте](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Рис. 12**: после выбора строки, отображаются сведения о продукте ([Просмотр полноразмерное изображение](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))


## <a name="summary"></a>Сводка

В этом и предыдущем три учебника мы видели ряд методов для отображения иерархического отчеты. В данном руководстве, мы рассмотрели использование выбираемым GridView для размещения основных записей и DetailsView для отображения сведений о выбранной записи master на одной странице. В предыдущих учебниках мы рассматривали как отобразить сведения об образце отчеты с помощью элементами управления DropDownList и отображение основных записей в одной веб-страницы и записи в области данных на другом.

Этот учебник завершает изучение главного и подчиненного представлений отчетов. Начиная с следующее руководство для начала рассмотрим наши исследования настраиваемого форматирования с GridView, DetailsView и FormView. Мы рассмотрим, как для настройки отображения этих элементов управления на основе данных, связанных с ними, как создать сводку данных в нижнем колонтитуле GridView и использование шаблонов для получения повышенный контроль над макетом.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было – Хилтон Гизнау. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](master-detail-filtering-across-two-pages-vb.md)
