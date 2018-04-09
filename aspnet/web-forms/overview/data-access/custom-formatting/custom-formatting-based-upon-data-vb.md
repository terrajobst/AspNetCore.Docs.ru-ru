---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Настраиваемое форматирование на основе данных (Visual Basic) | Документы Microsoft
author: rick-anderson
description: Настройка формата элементов GridView, DetailsView и FormView на основе данных, привязанное к нему может осуществляться несколькими способами. В этом учебнике мы будем l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: a5c7f99b863697cc49a5bc9831dae861f51e129d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="custom-formatting-based-upon-data-vb"></a>Настраиваемое форматирование на основе данных (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) или [скачать PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> Настройка формата элементов GridView, DetailsView и FormView на основе данных, привязанное к нему может осуществляться несколькими способами. В этом учебнике мы рассмотрим способы выполнения привязана к данным с помощью обработчиков событий с привязкой к данным и RowDataBound форматирование.


## <a name="introduction"></a>Вступление

Можно настроить внешний вид элементов управления GridView, DetailsView и FormView через множество относящихся к стилю свойств. Такие свойства, как `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, и `Height`, в частности, определяют общий внешний вид отображаемых элементов управления. Свойства в том числе `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, и другие разрешают те же параметры стиля, применяемые к различным разделам. Аналогичным образом эти параметры стиля могут применяться на уровне полей.

Во многих сценариях, форматирования требования зависят от значения отображаемых данных. Например, для привлечения внимания к из купить продукты, отчет, содержащий сведения о продукте задать цвет фона на желтый для этих продуктов, `UnitsInStock` и `UnitsOnOrder` поля являются оба равны 0. Чтобы выделить дороже продуктов, необходимо отобразить цены на продукты стоимостью более $75.00 полужирным шрифтом.

Настройка формата элементов GridView, DetailsView и FormView на основе данных, привязанное к нему может осуществляться несколькими способами. В этом учебнике мы рассмотрим способы выполнения привязана к данным, форматирование использования `DataBound` и `RowDataBound` обработчики событий. В следующем уроке мы изучим Альтернативным подходом.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>С помощью элемента управления DetailsView`DataBound`обработчика событий

При привязке данных к DetailsView, из элемента управления источником данных, либо путем программного связывания данных для элемента управления `DataSource` и вызова его `DataBind()` метода, происходят следующие действия:

1. Данные веб-элемент управления `DataBinding` вызывается событие.
2. Данные, привязанные к данным веб-элемента управления.
3. Данные веб-элемент управления `DataBound` вызывается событие.

Пользовательская логика могут быть добавлены сразу после шаги 1 и 3 посредством обработчика событий. Путем создания обработчика событий для `DataBound` событий, можно программным образом определить данные, которые будут привязаны к веб-управления данными и изменять форматирование при необходимости. Давайте рассмотрим создание DetailsView, будут отображаться общие сведения о продукте, но будет отображаться `UnitPrice` значение в ***шрифта полужирный, курсив*** , если оно превышает $75.00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Шаг 1: Отображение сведений о продукте в элементе управления DetailsView

Откройте `CustomColors.aspx` страницы в `CustomFormatting` папки, перетащите элемент управления DetailsView из области элементов в конструктор, задайте его `ID` значение свойства `ExpensiveProductsPriceInBoldItalic`и привязать его к новый элемент управления ObjectDataSource, который вызывает `ProductsBLL` класса `GetProducts()` метод. Подробные инструкции для выполнения этой задачи здесь для краткости опущены, так как мы рассмотрели их сведений в предыдущих учебниках.

После ObjectDataSource привязан к DetailsView, занять некоторое время для изменения списка полей. Я решил удалить `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, и `Discontinued` стояли переформатированы оставшиеся стояли и переименовать. Я также очищаются `Width` и `Height` параметры. Поскольку DetailsView отображает только одна запись, необходимо включить разбиение на страницы, чтобы разрешить конечным пользователям просматривать все продукты. Сделать, установив флажок Включить постраничный просмотр в DetailsView смарт-тега.


[![Рисунок 1: Установите флажок Enable постраничного просмотра в DetailsView смарт-тег](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Рис. 1**: рис. 1: установите флажок "Включить" разбиение по страницам в DetailsView смарт-тег ([Просмотр полноразмерное изображение](custom-formatting-based-upon-data-vb/_static/image3.png))


После внесения этих изменений будет DetailsView разметку:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Теперь пора проверить эту страницу в браузере.


[![Элемент управления DetailsView отображает одного продукта за раз](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**На рисунке 2**: DetailsView элемент управления отображает одного продукта за раз ([Просмотр полноразмерное изображение](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Шаг 2: Программное определение значений данных в обработчике событий с привязкой к данным

Для правильного отображения цены продуктов для шрифта полужирный, курсив, `UnitPrice` значение превышает $75.00, то необходимо сначала сможет программно определить `UnitPrice` значение. Для DetailsView, это можно сделать в `DataBound` обработчика событий. Чтобы создать событие обработчика щелкните DetailsView в конструкторе, затем перейдите в окно «Свойства». Нажмите клавишу F4, чтобы открыть его, если он не является видимым или перейдите в меню «Вид» и выберите пункт меню в окне "Свойства". В окне свойств щелкните значок молнии, чтобы вывести список событий DetailsView. Затем либо дважды щелкните `DataBound` , либо введите имя обработчика событий, нужно создать.


![Создайте обработчик событий для события с привязкой к данным](custom-formatting-based-upon-data-vb/_static/image7.png)

**Рис. 3**: создайте обработчик событий для `DataBound` событий


> [!NOTE]
> Можно также создать обработчик событий из в коде страницы ASP.NET. Есть два раскрывающихся списках в верхней части страницы. Выберите объект из левого раскрывающегося списка и событие, необходимо создать обработчик для из правого раскрывающегося списка и Visual Studio автоматически создает соответствующий обработчик событий.


Это автоматически создать обработчик событий и перейти к часть кода куда он был добавлен. На этом этапе можно увидеть:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

Данные, привязанные к DetailsView может осуществляться через `DataItem` свойство. Напомним, что элемент управления выполняется привязка к строго типизированный объект DataTable, состоящее из коллекции экземпляров строго типизированных DataRow. Когда DataTable привязан к DetailsView, первый DataRow в DataTable назначается DetailsView `DataItem` свойство. В частности `DataItem` присваивается `DataRowView` объекта. Мы используем `DataRowView` `Row` свойство, чтобы получить доступ к базовому объекту DataRow, который является фактически `ProductsRow` экземпляра. После этого `ProductsRow` мы определить простой проверкой значения свойств объекта экземпляра.

Следующий код иллюстрирует, как определить, является ли `UnitPrice` значение, связанное с элементом управления DetailsView больше $75.00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Так как `UnitPrice` может иметь `NULL` значение в в базе данных, сначала проверьте, чтобы убедиться в том, что мы не дело с `NULL` значение перед обращением к `ProductsRow`в `UnitPrice` свойство. Эта проверка важна так как при попытке доступа к `UnitPrice` со `NULL` значение `ProductsRow` выдаст исключение [StrongTypingException исключение](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Шаг 3: Форматирование значения UnitPrice в DetailsView

На этом этапе можно определить, является ли `UnitPrice` значение, привязанное к DetailsView имеет значение, превышающее $75.00, но у нас пока для ознакомления с программное изменение DetailsView форматирования, соответствующим образом. Чтобы изменить форматирование всей строки в DetailsView, программным образом обратиться к строки с помощью `DetailsViewID.Rows(index)`; чтобы изменения отдельной ячейки, доступ к используйте `DetailsViewID.Rows(index).Cells(index)`. После получения ссылку на строку или ячейку можно затем настроить его внешний вид путем установки его свойств, относящихся к стилю.

Программный доступ к строки необходимо знать индекс строки, который начинается с 0. `UnitPrice` Пятый строки в DetailsView, присвоить ей индекса 4 и предоставления программного доступа с помощью `ExpensiveProductsPriceInBoldItalic.Rows(4)`. На этом этапе мы может иметь всю строку содержимого, отображаются шрифтом полужирный, курсив, используя следующий код:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Тем не менее, это сделает *оба* метки (цена) и значение полужирным шрифтом или курсивом. Если мы хотим сделать только значение полужирный и курсивный нам нужно применить это форматирование во вторую ячейку в строке, в которой можно выполнить при помощи следующих:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Поскольку мы старались сих с помощью таблицы стилей поддерживать четкое разделение отображения разметки и сведений, относящихся к стилю, вместо задания свойства определенного стиля, как показано выше давайте вместо этого используйте класс CSS. Откройте `Styles.css` таблицы стилей и добавьте новый класс CSS `ExpensivePriceEmphasis` со следующим определением:


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Затем в `DataBound` обработчика событий, укажите в ячейке `CssClass` свойства `ExpensivePriceEmphasis`. В следующем коде показано `DataBound` обработчик событий в полном объеме:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

При просмотре Chai, который стоит меньше $75.00, цена отображается обычным шрифтом (см. рис. 4). Однако при просмотре Mishi Kobe Niku имеет цену 97.00 долларов, цена отображается шрифтом полужирный, курсив (см. рис. 5).


[![Цены меньше $75.00 отображаются в обычный шрифт](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Рис. 4**: цены меньше $75.00 отображаются шрифтом Normal ([Просмотр полноразмерное изображение](custom-formatting-based-upon-data-vb/_static/image10.png))


[![Цена дорогих продуктов отображаются в полужирный, курсив шрифта](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Рис. 5**: цена дорогих продуктов отображаются в полужирный, курсив шрифта ([Просмотр полноразмерное изображение](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>С помощью элемента управления FormView`DataBound`обработчика событий

Процедура определения базовых данных, привязанных к FormView идентичны для создания DetailsView `DataBound` , присвойте `DataItem` свойство в тип соответствующего объекта привязки к элементу управления и определения дальнейших действий. FormView и DetailsView различаются, однако обновление внешнего вида собственный пользовательский интерфейс.

FormView не содержит любые стояли и поэтому не имеет `Rows` коллекции. Вместо этого FormView состоит из шаблонов, которые могут содержать смесь статического кода HTML, веб-элементов управления и синтаксис привязки данных. Настройка стиля элемента FormView обычно состоит в настройке стиля из одного или нескольких веб-элементов управления в шаблоны FormView.

Чтобы проиллюстрировать это, давайте FormView для списка продуктов как и в предыдущем примере, но на этот раз давайте отображения используется только имя продукта и единиц на складе с использованием единиц на складе, отображаются красным цветом, если это значение меньше или равно 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Шаг 4: Отображение сведений о продукте в FormView

Добавить FormView для `CustomColors.aspx` странице ниже DetailsView и задайте его `ID` свойства `LowStockedProductsInRed`. Привязать элемент управления ObjectDataSource, созданный на предыдущем шаге FormView. Это создаст `ItemTemplate`, `EditItemTemplate`, и `InsertItemTemplate` для FormView. Удалить `EditItemTemplate` и `InsertItemTemplate` и упрощения `ItemTemplate` для включения только `ProductName` и `UnitsInStock` значений, каждое в своих собственных с соответствующим именем метки элементов управления. С помощью DetailsView из примера выше, также установите флажок Включить постраничный просмотр в FormView смарт-тег.

После этих изменений в FormView должна выглядеть следующим:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Обратите внимание, что `ItemTemplate` содержит:

- **Статическим HTML** текст «продукта:» и «единиц на складе:» вместе с `<br />` и `<b>` элементы.
- **Веб-элементы управления** два элемента управления Label `ProductNameLabel` и `UnitsInStockLabel`.
- **Синтаксис привязки данных** `<%# Bind("ProductName") %>` и `<%# Bind("UnitsInStock") %>` синтаксис, который присваивает значения из этих полей к элементам управления Label `Text` свойства.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Шаг 5: Программное определение значений данных в обработчике событий с привязкой к данным

С FormView готова, следующим шагом является программным способом определить, если `UnitsInStock` значение меньше или равно 10. Это можно сделать с FormView точно так же, как в DetailsView. Начните с создания обработчика событий для FormView `DataBound` событий.


![Создание обработчика событий с привязкой к данным](custom-formatting-based-upon-data-vb/_static/image14.png)

**Рис. 6**: создание `DataBound` обработчика событий


События обработчика приведение FormView `DataItem` свойства `ProductsRow` экземпляр и определить ли `UnitsInPrice` значение — таким образом, что нам нужно отобразить ее красным цветом.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Шаг 6: Форматирование в элементе управления UnitsInStockLabel Label в FormView ItemTemplate

Последним шагом является выделение значения `UnitsInStock` значение отображается красным цветом, если значение не превышает 10. Для этого нужно получить программный доступ к `UnitsInStockLabel` управления `ItemTemplate` и задать его свойства стиля, чтобы его текст отображается красным цветом. Чтобы получить доступ к веб-элемента управления в шаблоне, используйте `FindControl("controlID")` метод следующим образом:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

В нашем примере мы хотим получить доступ к метку элемент управления `ID` значение `UnitsInStockLabel`, мы используем:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

После получения программный ссылку на веб-элемент управления, при необходимости можно изменять его относящихся к стилю свойств. Как в прошлом примере мы создали класс CSS в `Styles.css` с именем `LowUnitsInStockEmphasis`. Чтобы применить этот стиль для элемента управления Label, установите его `CssClass` свойство соответствующим образом.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> Синтаксис для форматирования программный доступ к веб-элемента управления с помощью шаблона `FindControl("controlID")` и задание его свойств, относящихся к стилю может также использоваться при использовании [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) в DetailsView или GridView элементы управления. В следующем учебном курсе мы изучим TemplateFields.


Фигуры. 7 показана FormView при отображении продукта которого `UnitsInStock` значение больше 10, а продукт на рис. 8 имеет его значение меньше 10.


[![Для продуктов с достаточно большой Units In Stock настраиваемое форматирование не применяется](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Рис. 7**: для продуктов с достаточно большой Units In Stock, настраиваемое форматирование не применяется ([Просмотр полноразмерное изображение](custom-formatting-based-upon-data-vb/_static/image17.png))


[![Для тех продуктов со значения 10 или меньше единицы в биржевой номер отображается красным цветом](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Рис. 8**: для тех продуктов со значения 10 или меньше единицы в биржевой номер отображается красным цветом ([Просмотр полноразмерное изображение](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Форматирование с GridView`RowDataBound`событий

Ранее мы рассмотрели последовательность шагов DetailsView и FormView управляет ходом выполнения с помощью во время привязки данных. Давайте посмотрим на эти шаги еще раз ее вспомним.

1. Данные веб-элемент управления `DataBinding` вызывается событие.
2. Данные, привязанные к данным веб-элемента управления.
3. Данные веб-элемент управления `DataBound` вызывается событие.

Эти три простых шагов достаточны для DetailsView и FormView, так как они отображают только одна запись. Для GridView, отображающий *все* записи, привязанные к нему (а не только первую), шаг 2 немного сложнее.

На шаге 2 GridView перечисляет источник данных и создает для каждой записи `GridViewRow` и привязывает текущую запись в него. Для каждого `GridViewRow` добавлен к GridView, возникают два события:

- **`RowCreated`** срабатывает после `GridViewRow` был создан
- **`RowDataBound`** вызывается после привязки текущей записи к `GridViewRow`.

Для GridView затем, привязка данных является более точно описанные следующую последовательность шагов:

1. GridView `DataBinding` вызывается событие.
2. Данные будут привязаны к GridView.   
  
   Для каждой записи в источнике данных 

    1. Создание `GridViewRow` объекта
    2. Пожара `RowCreated` событий
    3. Запись привязывается к `GridViewRow`
    4. Пожара `RowDataBound` событий
    5. Добавить `GridViewRow` для `Rows` коллекции
3. GridView `DataBound` вызывается событие.

Для настройки формата отдельных записей в GridView, после этого необходимо создать обработчик событий для `RowDataBound` события. Чтобы проиллюстрировать это, добавим GridView к `CustomColors.aspx` страницу, которая возвращает имя, категория и цена для каждого продукта, выделите эти продукты, цена которых меньше, чем 10,00 $ с на желтом фоне.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Шаг 7: Отображение сведений о продукте в элементе управления GridView

Добавьте элемент управления GridView под FormView из предыдущего примера и задайте его `ID` свойства `HighlightCheapProducts`. Поскольку у нас уже есть ObjectDataSource, который возвращает все продукты на странице, привязать GridView. Наконец измените стояли GridView для включения только продукта имена, категории и цены. После этого разметка GridView разметки должно иметь вид:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Рис. 9 показан ход работы на данный момент, при просмотре через браузер.


[![GridView указаны имя, категория и цена для каждого продукта](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Рис. 9**: GridView указаны имя, категория и цена для каждого продукта ([Просмотр полноразмерное изображение](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Шаг 8: Программное определение значений данных в обработчике событий RowDataBound

Когда `ProductsDataTable` привязывается к GridView его `ProductsRow` экземпляры, перечисления и для каждого `ProductsRow` `GridViewRow` создается. `GridViewRow` `DataItem` Свойство назначено конкретного `ProductRow`, после чего GridView `RowDataBound` вызывается обработчик события. Чтобы определить `UnitPrice` значение для каждого продукта, привязанное к GridView, после чего необходимо создать обработчик событий для элемента GridView `RowDataBound` событий. В этом обработчике событий можно проверять `UnitPrice` значение для текущего `GridViewRow` и принять решение о форматировании для этой строки.

Этот обработчик событий могут создаваться выполнить те же действия, что с FormView и DetailsView.


![Создайте обработчик событий для события RowDataBound GridView](custom-formatting-based-upon-data-vb/_static/image24.png)

**Рис. 10**: Создание обработчика событий для элемента GridView `RowDataBound` событий


Создание обработчика событий таким образом вызывает следующий код, чтобы автоматически добавить в коде страницы ASP.NET.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Когда `RowDataBound` события обработчик событий передается в качестве второго параметра объект типа `GridViewRowEventArgs`, который имеет свойство с именем `Row`. Это свойство возвращает ссылку на `GridViewRow` это было просто с привязкой к данным. Чтобы получить доступ к `ProductsRow` экземпляра привязан к `GridViewRow` мы используем `DataItem` свойства следующим образом:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

При работе с `RowDataBound` обработчик событий, важно помнить, что GridView состоит из различных типов строк и что это событие вызывается для *все* типов строк. Объект `GridViewRow`на тип можно определить с помощью его `RowType` свойство и может иметь одно из значений:

- `DataRow` строку, которая привязана к записи из GridView `DataSource`
- `EmptyDataRow` строки, отображаемой, если GridView `DataSource` пуст
- `Footer` строки нижнего колонтитула; отображается, если GridView `ShowFooter` свойству `True`
- `Header` Строка заголовка; отображается, если GridView ShowHeader задано значение `True` (по умолчанию)
- `Pager` для элемента GridView, которые реализуют разбиение по страницам, строку, отображающую интерфейс разбиения по страницам
- `Separator` не используется для управления GridView, однако используется `RowType` Свойства DataList и повторителя управляет двумя данных веб-элементов управления, мы обсудим в будущих учебниках

Поскольку `EmptyDataRow`, `Header`, `Footer`, и `Pager` не связаны с `DataSource` записей, они будут всегда иметь значение `Nothing` для их `DataItem` свойство. По этой причине, прежде чем работать с текущим `GridViewRow` `DataItem` свойства, нам нужно убедиться, что мы имеем дело с `DataRow`. Это можно сделать, проверив `GridViewRow` `RowType` свойства следующим образом:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Шаг 9: Выделение строки желтым при значении UnitPrice — меньше, чем 10,00 $

Последний шаг — программными методами выделить весь `GridViewRow` Если `UnitPrice` для этой строки равно меньше $10.00. Синтаксис для доступа к строк или ячеек элемента управления GridView выглядит так же, как DetailsView `GridViewID.Rows(index)` для доступа к всей строки, `GridViewID.Rows(index).Cells(index)` для доступа к отдельной ячейки. Тем не менее, когда `RowDataBound` обработчик событий вызывается с привязкой к данным `GridViewRow` еще не добавлены к GridView `Rows` коллекции. Таким образом не может обращаться к текущей `GridViewRow` экземпляра из `RowDataBound` обработчика событий с помощью коллекции строк.

Вместо `GridViewID.Rows(index)`, мы создаем ссылку на текущий `GridViewRow` экземпляра в `RowDataBound` при помощи команды `e.Row`. То есть, чтобы выделить текущий `GridViewRow` экземпляра из `RowDataBound` мы будет использовать обработчик событий:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Вместо установки `GridViewRow`в `BackColor` свойство напрямую, пока что будем придерживаться с использованием классов CSS. Мы создали класс CSS с именем `AffordablePriceEmphasis` , задает цвет фона на желтый. Завершенный `RowDataBound` будет выглядеть следующим образом:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![Наиболее доступной продуктов, которые выделяются желтым](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Рис. 11**: самое недорогим продуктов, которые выделяются желтым ([Просмотр полноразмерное изображение](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>Сводка

В этом учебнике мы узнали, как форматировать GridView, DetailsView и FormView на основе данных, связанных с элементом управления. Для этого мы создали обработчик событий для `DataBound` или `RowDataBound` событий, где базовых данных было проанализировать вместе с необходимые изменения, при необходимости. Для доступа к данным, привязанным к DetailsView или FormView, мы используем `DataItem` свойство в `DataBound` обработчик событий; для элемента управления GridView каждого `GridViewRow` экземпляра `DataItem` свойство содержит данные, привязанные к этой строке, которая доступна в `RowDataBound` обработчик событий.

Синтаксис программной настройки форматирования веб-элемента управления зависит от веб-элемента управления и отображения данных для форматирования. Элементы управления, строк и ячеек может осуществляться для DetailsView и GridView по порядковому номеру индекса. Для FormView, который использует шаблоны, `FindControl("controlID")` метод часто используется для поиска веб-элемента управления из шаблона.

В следующем уроке мы рассмотрим способы использования шаблонов с GridView и DetailsView. Кроме того мы увидим другой способ настройки форматирования на основе базовых данных.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были E.R. Gilmore, Деннис Патерсона и (Dan Jagers). Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [Вперед](using-templatefields-in-the-gridview-control-vb.md)
