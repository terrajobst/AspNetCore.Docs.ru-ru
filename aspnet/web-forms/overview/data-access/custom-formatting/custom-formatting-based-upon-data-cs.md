---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: Пользовательское форматирование на основе данных (C#) | Документация Майкрософт
author: rick-anderson
description: Настройка формата элементов GridView, DetailsView и FormView на основе привязанным к ним данным может быть выполнена несколькими способами. В этом руководстве мы будем l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 505081890ac0eb7a1f248569fda1b14351119275
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397988"
---
<a name="custom-formatting-based-upon-data-c"></a>Пользовательское форматирование на основе данных (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) или [скачать PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> Настройка формата элементов GridView, DetailsView и FormView на основе привязанным к ним данным может быть выполнена несколькими способами. В этом руководстве мы рассмотрим способы выполнения привязан форматирования данных при помощи обработчиков событий DataBound и RowDataBound.


## <a name="introduction"></a>Вступление

Внешний вид элементов управления GridView, DetailsView и FormView можно настроить с помощью множество относящихся к стилю свойств. Свойства, такие как `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, и `Height`, диктуют общее оформление отображаемых элементов управления. Свойства в том числе `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, и другим пользователям те же параметры стиля, применяемые к отдельным частям. Аналогичным образом эти параметры стиля могут применяться на уровне поля.

Во многих сценариях, требованиях к форматированию зависят от значения отображаемых данных. Например, для привлечения внимания к из купить продукты, отчет, содержащий сведения о продукте задать цвет фона на желтый по этим продуктам, `UnitsInStock` и `UnitsOnOrder` равны 0. Чтобы выделить более дорогих продуктов, мы может потребоваться отобразить цены на продукты стоимостью более $75.00 полужирным шрифтом.

Настройка формата элементов GridView, DetailsView и FormView на основе привязанным к ним данным может быть выполнена несколькими способами. В этом руководстве мы рассмотрим способы выполнения данных, привязанных форматирование воспользоваться `DataBound` и `RowDataBound` обработчики событий. В следующем учебном курсе мы рассмотрим альтернативный подход.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>С помощью элемента управления DetailsView`DataBound`обработчик событий

При привязке данных в элементе управления DetailsView, либо из элемента управления источником данных, либо путем программного связывания данных к элементу управления `DataSource` и вызова его `DataBind()` метода, происходят следующие действия:

1. Данные веб-элемента управления в `DataBinding` вызывает событие.
2. Данные привязываются к данным веб-элемента управления.
3. Данные веб-элемента управления в `DataBound` вызывает событие.

Пользовательская логика может быть внесена сразу же после выполнения шагов 1 и 3 через обработчик событий. Создать обработчик событий для `DataBound` событий, можно программно определить данные, которые будут привязаны к веб-управления данными и изменить форматирование нужным образом. В качестве примера давайте создать элемент DetailsView, будут отображаться общие сведения о продукте, но отобразит `UnitPrice` значение в ***шрифт полужирным, курсивом*** если оно превышает $75.00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Шаг 1: Отображение информации о продукте в элементе управления DetailsView

Откройте `CustomColors.aspx` странице в `CustomFormatting` папки, перетащите элемент управления DetailsView из инструментария в конструктор, задайте его `ID` значение свойства `ExpensiveProductsPriceInBoldItalic`и привязать его к новый элемент управления ObjectDataSource, который вызывает `ProductsBLL` класса `GetProducts()` метод. Подробные инструкции для выполнения этой задачи здесь для краткости опущены, так как мы их подробно рассматриваемую в предыдущих учебных курсах.

Когда элемент управления ObjectDataSource привязан к элементу DetailsView, Отвлекитесь и списка полей. Мы решили удалить `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, и `Discontinued` поля BoundField, кроме переформатированы оставшихся полей BoundFields и переименован. Переформатировали `Width` и `Height` параметры. Так как элемент DetailsView отображает только одну запись, необходимо включить разбиение по страницам позволит пользователю просматривать все продукты. Сделать это, установив флажок Enable Paging в смарт-теге DetailsView.


[![Установите флажок Включить разбиение по страницам в смарт-теге DetailsView](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**Рис. 1**: установите флажок "Включить" Paging в смарт-теге DetailsView ([Просмотр полноразмерного изображения](custom-formatting-based-upon-data-cs/_static/image3.png))


После внесения этих изменений будет DetailsView разметки:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

Отвлекитесь и проверьте страницу в браузере.


[![Элемент управления DetailsView отображает одного продукта за раз](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**Рис. 2**: The DetailsView элемент управления отображает одного продукта за раз ([Просмотр полноразмерного изображения](custom-formatting-based-upon-data-cs/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Шаг 2: Программное определение значений данных в обработчике событий DataBound

Для отображения цены шрифтом полужирный, курсив, для этих продуктов, `UnitPrice` значение превышает $75.00, нам нужно сначала быть программно определять `UnitPrice` значение. Для элемента DetailsView, это можно сделать в `DataBound` обработчик событий. Чтобы создать событие обработчика выделите DetailsView в конструкторе, затем перейдите в окно свойств. Нажмите клавишу F4, чтобы открыть его, если это не отображается, или перейдите к меню "Вид" и выберите пункт меню в окно "Свойства". В окне свойств щелкните значок с молнией, чтобы получить список событий DetailsView. Затем либо дважды щелкните `DataBound` , либо введите имя обработчика событий, который вы хотите создать.


![Создайте обработчик событий для события DataBound](custom-formatting-based-upon-data-cs/_static/image7.png)

**Рис. 3**: создайте обработчик событий для `DataBound` событий


Это автоматически создать обработчик событий и перейти к ту часть кода куда он был добавлен. На этом этапе вы увидите:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

Данные, привязанные к элементу управления DetailsView может осуществляться через `DataItem` свойство. Вспомним, что выполняется привязка четыре элемента управления в объект DataTable со строгой типизацией, который представляет собой коллекцию строго типизированных экземпляров строки DataRow. Когда объект DataTable привязан к элементу управления DetailsView, первый объект DataRow в DataTable назначается DetailsView `DataItem` свойство. В частности `DataItem` свойству назначается `DataRowView` объекта. Мы можем использовать `DataRowView` `Row` свойство, чтобы получить доступ к базовому объекту DataRow, который является фактически `ProductsRow` экземпляра. Когда у нас это `ProductsRow` экземпляра, мы можем просто проверив значения свойств объекта.

Следующий код иллюстрирует способ определить, является ли `UnitPrice` значение, привязанное к элементу управления DetailsView больше, чем 75.00 долл. США:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> Так как `UnitPrice` может иметь `NULL` значение в в базе данных, сначала проверьте, чтобы убедиться в том, что мы не имеем дело с `NULL` значение перед доступом к `ProductsRow`в `UnitPrice` свойство. Такая проверка очень важна так как при попытке получить доступ к `UnitPrice` со `NULL` значение `ProductsRow` выдаст исключение [StrongTypingException исключение](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Шаг 3: Форматирование значение UnitPrice в DetailsView

На этом этапе мы можем определить ли `UnitPrice` значение, привязанное к элементу управления DetailsView имеет значение, превышающее $75.00, но необходимо еще, чтобы узнать, как программными методами настроить форматирование элемента управления DetailsView форматирования, соответствующим образом. Чтобы изменить форматирование всей строки в элементе DetailsView, программный доступ к строке с помощью `DetailsViewID.Rows[index]`; для изменения отдельной ячейки, доступ к используйте `DetailsViewID.Rows[index].Cells[index]`. Получив ссылку на строку или ячейку можно затем настроить его внешний вид путем установки его свойств, относящихся к стилю.

Программный доступ к строку необходимо знать индекс строки, который начинается с 0. `UnitPrice` Строки стоит пятой в DetailsView, ее индек равен 4 и делая доступными в программах с помощью `ExpensiveProductsPriceInBoldItalic.Rows[4]`. На этом этапе мы может иметь всю строку содержимого, отображенный на полужирный, курсив, используя следующий код:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

Тем не менее, это сделает *оба* метки (цена) и значение полужирного шрифта и курсива. Если мы хотим сделать просто значение полужирного шрифта и курсива нам нужно применить это форматирование во вторую ячейку в строке, в которой можно выполнить при помощи следующих:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

Поскольку наших учебниках полученный промежуточный результат с помощью таблицы стилей поддерживать четкое разделение между разметку и сведения, относящиеся к стилю, вместо того чтобы задавать свойства определенного стиля, как показано выше давайте вместо этого используйте класс CSS. Откройте `Styles.css` таблицы стилей и добавьте новый класс CSS с именем `ExpensivePriceEmphasis` со следующим определением:


[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

Затем в `DataBound` обработчик событий, установите для свойства `CssClass` свойства `ExpensivePriceEmphasis`. В следующем коде показан `DataBound` обработчик событий в полном объеме:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

При просмотре Chai, который стоит меньше, чем 75.00 долларов, цена отображается обычным шрифтом (см. рис. 4). Тем не менее, при просмотре Mishi Kobe Niku, который имеет цену 97.00 долларов, цена отображается шрифтом полужирный, курсив (см. рис. 5).


[![Цены меньше $75.00 отображаются шрифтом норм.](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**Рис. 4**: цены меньше $75.00 отображаются шрифтом обычный ([Просмотр полноразмерного изображения](custom-formatting-based-upon-data-cs/_static/image10.png))


[![Цена дорогих продуктов отображаются полужирным, курсивом шрифта](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**Рис. 5**: цена дорогих продуктов отображаются полужирным, курсивом шрифта ([Просмотр полноразмерного изображения](custom-formatting-based-upon-data-cs/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>С помощью элемента управления FormView`DataBound`обработчик событий

Процедура определения базовых данных, привязанных к FormView аналогичны используемым для создания в элементе управления DetailsView `DataBound` , присвойте `DataItem` свойство в тип соответствующего объекта привязанной к элементу управления и определите дальнейшие действия. FormView и DetailsView отличаются, однако способ обновления внешний вид их пользовательского интерфейса.

FormView не содержит все поля BoundField, кроме и поэтому не имеет `Rows` коллекции. Вместо этого элемента FormView состоит из шаблонов, которые могут содержать смесь статического кода HTML, веб-элементы управления и синтаксис привязки данных. Настройка стиля элемента FormView обычно состоит в настройке стиля одного или нескольких веб-элементов управления в шаблоны FormView.

Чтобы проиллюстрировать это, используйте FormView для списка продуктов, как в предыдущем примере, но на этот раз, давайте отобразим только название продукта и единиц на складе с количеством единиц на складе, отображаются красным цветом, если он меньше или равно 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Шаг 4: Отображение информации о продукте в элементе управления FormView

Добавление элемента FormView к `CustomColors.aspx` странице под списком в DetailsView и задайте его `ID` свойства `LowStockedProductsInRed`. Привязать FormView к ObjectDataSource, созданному на предыдущем шаге. Это создаст `ItemTemplate`, `EditItemTemplate`, и `InsertItemTemplate` для FormView. Удалить `EditItemTemplate` и `InsertItemTemplate` и упростить `ItemTemplate` для включения только `ProductName` и `UnitsInStock` значений, каждое в своих собственных соответствующим образом с именем метки элементов управления. Как с помощью DetailsView из примера выше, также установите флажок Enable Paging в смарт-тега FormView.

После внесения этих изменений разметка элемента FormView должна выглядеть следующим:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

Обратите внимание, что `ItemTemplate` содержит:

- **Статический HTML** текст «продукта:» и «единиц на складе:» вместе с `<br />` и `<b>` элементы.
- **Веб-элементы управления** два элемента управления Label, `ProductNameLabel` и `UnitsInStockLabel`.
- **Синтаксис привязки данных** `<%# Bind("ProductName") %>` и `<%# Bind("UnitsInStock") %>` синтаксис, который присваивает значения из этих полей к элементам управления Label `Text` свойства.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Шаг 5: Программное определение значений данных в обработчике событий DataBound

С помощью FormView готова, следующим шагом является программно определять, если `UnitsInStock` значение меньше или равно 10. Это достигается с FormView точно так же как в DetailsView. Начните с создания обработчика событий для элемента FormView `DataBound` событий.


![Создание обработчика событий DataBound](custom-formatting-based-upon-data-cs/_static/image14.png)

**Рис. 6**: создание `DataBound` обработчик событий


Событий обработчик приведение FormView `DataItem` свойства `ProductsRow` экземпляра и определить ли `UnitsInPrice` значение — таким образом, что нам нужно отобразить ее красным цветом.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Шаг 6: Форматирование элемента UnitsInStockLabel Label в ItemTemplate элемента FormView

Последним шагом является выделение значения `UnitsInStock` значение красным цветом, если значение не превышает 10. Для этого нам нужно получить программный доступ к `UnitsInStockLabel` контролировать `ItemTemplate` и настроить свойства стиля, чтобы текст выделялся красным цветом. Чтобы получить доступ к веб-элемент управления в шаблоне, используйте `FindControl("controlID")` метод следующим образом:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

В нашем примере мы хотим получить доступ к метку со свойством `ID` значение `UnitsInStockLabel`, мы используем:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

Получив программную ссылку на веб-элемента управления, мы при необходимости можно изменить его свойства, относящиеся к стилю. Как в прошлом примере, я создал класс CSS в `Styles.css` с именем `LowUnitsInStockEmphasis`. Чтобы применить этот стиль к элементу управления Label Web, установите его `CssClass` свойство соответствующим образом.


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> Синтаксис для форматирования программный доступ к веб-элемента управления с помощью шаблона `FindControl("controlID")` и затем настроив его свойства, относящиеся к стилю можно также использовать при использовании [полей TemplateField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) в GridView или DetailsView элементы управления. В нашем следующем учебном курсе мы рассмотрим поля TemplateField.


Рис. 7 показан FormView при отображении продукта, `UnitsInStock` значение больше 10, а его значение меньше 10 продукта на рис. 8.


[![Для продуктов с достаточно больших единиц на складе настраиваемое форматирование не применяется](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**Рис. 7**: для продуктов с достаточно больших единиц на складе, настраиваемое форматирование не применяется ([Просмотр полноразмерного изображения](custom-formatting-based-upon-data-cs/_static/image17.png))


[![Единиц на складе номер отображается красным цветом для тех продуктов с значения 10 или меньше](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**Рис. 8**: единиц на складе номер отображается красным цветом для тех продуктов с значения 10 или меньше ([Просмотр полноразмерного изображения](custom-formatting-based-upon-data-cs/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Форматирование с GridView`RowDataBound`событий

Ранее мы рассмотрели последовательность действий DetailsView и FormView ход процесса при помощи во время привязки данных. Давайте взглянем на эти действия еще раз ее вспомним.

1. Данные веб-элемента управления в `DataBinding` вызывает событие.
2. Данные привязываются к данным веб-элемента управления.
3. Данные веб-элемента управления в `DataBound` вызывает событие.

Эти три простых шага достаточны для DetailsView и FormView, так как они отображают только одну запись. Для элементов управления GridView, отображающий *все* записи, привязанные к нему (а не только первую), этап 2 немного сложнее.

На шаге 2 GridView перечисляет источник данных и для каждой записи создает `GridViewRow` и привязывает текущую запись к нему. Для каждого `GridViewRow` добавлен к GridView, возникают два события:

- **`RowCreated`** срабатывает после `GridViewRow` будет создана
- **`RowDataBound`** вызывается после привязки текущей записи к `GridViewRow`.

Для элементов управления GridView затем привязки данных более точно описан ниже последовательность действий:

1. GridView `DataBinding` вызывает событие.
2. Данные привязываются к GridView.   
  
   Для каждой записи в источнике данных 

    1. Создание `GridViewRow` объекта
    2. Fire `RowCreated` событий
    3. Запись привязывается к `GridViewRow`
    4. Fire `RowDataBound` событий
    5. Добавить `GridViewRow` для `Rows` коллекции
3. GridView `DataBound` вызывает событие.

Для настройки формата отдельных записей элемента GridView, затем необходимо создать обработчик событий для `RowDataBound` событий. Чтобы проиллюстрировать это, давайте добавим элемент управления GridView для `CustomColors.aspx` страницы, содержащий имя, категорию и цену продукта, выделение цветом те продукты, цена которых меньше, чем 10,00 $ с желтым цветом.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Шаг 7: Отображение информации о продукте в элементе управления GridView

Добавьте элемент управления GridView под FormView из предыдущего примера и задайте его `ID` свойства `HighlightCheapProducts`. Поскольку у нас уже есть ObjectDataSource, который возвращает все продукты на странице, привязки GridView к этому. Наконец измените поля BoundFields элемента GridView для включения только закупочного имена, категории и цены. После внесения этих изменений разметка GridView должен выглядеть так:


[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

Рис. 9 показаны результаты для этой точки, при просмотре через браузер.


[![GridView перечисляет имя, категорию и цену продукта](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**Рис. 9**: GridView перечисляет имя, категорию и цену для каждого продукта ([Просмотр полноразмерного изображения](custom-formatting-based-upon-data-cs/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Шаг 8: Программное определение значений данных в обработчике событий RowDataBound

Когда `ProductsDataTable` привязан к элементу GridView его `ProductsRow` экземпляры, перечисления и для каждого `ProductsRow` `GridViewRow` создается. `GridViewRow` `DataItem` Свойство назначено конкретного `ProductRow`, после чего GridView `RowDataBound` вызове обработчика событий. Чтобы определить `UnitPrice` значение для каждого продукта, привязанное к элементу GridView, а затем, нам необходимо создать обработчик событий для элемента GridView `RowDataBound` событий. В этом обработчике событий можно изучить `UnitPrice` значение для текущего `GridViewRow` и принятия решения о форматировании для этой строки.

Этот обработчик событий могут создаваться выполнить те же действия, что с FormView и DetailsView.


![Создайте обработчик событий для события RowDataBound элемента GridView](custom-formatting-based-upon-data-cs/_static/image24.png)

**Рис. 10**: создайте обработчик событий для элемента GridView `RowDataBound` событий


Создание обработчика событий таким образом вызовет следующий код, чтобы автоматически добавить в коде страницы ASP.NET:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

Когда `RowDataBound` вызывает событие, обработчик событий передается в качестве второго параметра объект типа `GridViewRowEventArgs`, который имеет свойство с именем `Row`. Это свойство возвращает ссылку на `GridViewRow` это было просто с привязкой к данным. Чтобы получить доступ к `ProductsRow` привязанный к `GridViewRow` мы используем `DataItem` свойства следующим образом:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

При работе с `RowDataBound` обработчик событий, важно помнить, что GridView состоит из строк разного типа и что это событие вызывается для *все* типов строк. Объект `GridViewRow`его тип можно определить с помощью его `RowType` свойство и может иметь одно из возможных значений:

- `DataRow` строку, которая привязана к записи из элемента GridView `DataSource`
- `EmptyDataRow` строкой, отображаемой, если GridView `DataSource` пуст
- `Footer` строки нижнего колонтитула; отображается, если GridView `ShowFooter` свойство имеет значение `true`
- `Header` строки заголовка; показано, если GridView ShowHeader свойству `true` (по умолчанию)
- `Pager` для элемента GridView, реализовать разбиение по страницам, эта строка отображает интерфейс разбиения по страницам
- `Separator` не используется для элемента GridView, но используются `RowType` управляет свойства для элементов управления DataList и Repeater, два элемента данных веб-элементы управления, будут обсуждаться в последующих руководствах

Так как `EmptyDataRow`, `Header`, `Footer`, и `Pager` не связаны с `DataSource` запись, они будут всегда иметь `null` значение для их `DataItem` свойство. По этой причине, прежде чем работать с текущим `GridViewRow` `DataItem` свойство, нам нужно убедиться, что мы имеем дело с `DataRow`. Это можно сделать, проверив `GridViewRow`в `RowType` свойства следующим образом:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Шаг 9: Выделение строки желтым при значении UnitPrice — меньше, чем 10,00 $

Последний шаг — программными методами выделить весь `GridViewRow` Если `UnitPrice` значение для этой строки равно, меньше, чем 10,00 $. Синтаксис для доступа к строк или ячеек в элементе управления GridView будет таким же, как DetailsView `GridViewID.Rows[index]` для доступа к всей строки, `GridViewID.Rows[index].Cells[index]` для доступа к отдельной ячейке. Тем не менее, если `RowDataBound` обработчик событий вызывается привязкой к данным `GridViewRow` еще должен быть добавлен к элементу GridView `Rows` коллекции. Таким образом вы не может получить доступ к текущим `GridViewRow` экземпляра из `RowDataBound` обработчик событий, с помощью коллекции строк.

Вместо `GridViewID.Rows[index]`, мы создаем ссылку на текущий `GridViewRow` в экземпляре `RowDataBound` обработчика событий с помощью `e.Row`. То есть того, чтобы выделить текущий `GridViewRow` экземпляра из `RowDataBound` обработчик событий, мы бы использовали:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

Вместо установки `GridViewRow`в `BackColor` свойства напрямую, пока что будем придерживаться использовать классы CSS. Я создал класс CSS с именем `AffordablePriceEmphasis` , задает цвет фона на желтый. Завершенные `RowDataBound` будет выглядеть следующим образом:


[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]


[![Наиболее доступной продуктов, которые выделены желтым](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**Рис. 11**: самое доступное продуктов, которые выделены желтым ([Просмотр полноразмерного изображения](custom-formatting-based-upon-data-cs/_static/image27.png))


## <a name="summary"></a>Сводка

В этом учебнике мы рассмотрели возможности форматирования GridView, DetailsView и FormView на основе данных, привязанных к элементу управления. Для этого мы создали обработчики для событий `DataBound` или `RowDataBound` событий, где базовых данных было проверить вместе с изменения форматирования, при необходимости. Для доступа к данным, привязанным к DetailsView или FormView, мы используем `DataItem` свойство в `DataBound` обработчик событий; для элементов управления GridView, каждый `GridViewRow` экземпляра `DataItem` свойство содержит данные, привязанные к данной строке, которая доступна в `RowDataBound` обработчик событий.

Синтаксис программной настройки форматирования веб-элемента управления зависит от веб-элемента управления и отображения данных, которые необходимо отформатировать. Элементы управления, строк и ячеек может осуществляться для DetailsView и GridView по порядковому номеру индекса. Для FormView, который использует шаблоны, `FindControl("controlID")` метод часто используется для поиска веб-элемент управления из шаблона.

В следующем учебном курсе мы рассмотрим способы использования шаблонов с GridView и DetailsView. Кроме того мы увидим другой способ настройки форматирования на основе базовых данных.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. E.R. стали Лиз Шалок в этом руководстве Gilmore, Dennis Patterson и (Dan Jagers). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Вперед](using-templatefields-in-the-gridview-control-cs.md)
