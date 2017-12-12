---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: "Отображение сводки в нижнем колонтитуле GridView (C#) | Документы Microsoft"
author: rick-anderson
description: "Сводная информация часто отображается в нижней части отчета в строки итогов. Элемент управления GridView может включать строкой нижнего колонтитула, в которых ячейки, мы можем pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 0d3df976181a4641dbfffe77875989c77ece059d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="displaying-summary-information-in-the-gridviews-footer-c"></a>Отображение сводки в нижнем колонтитуле GridView (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) или [скачать PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Сводная информация часто отображается в нижней части отчета в строки итогов. Элемент управления GridView может включать строкой нижнего колонтитула, в которых ячейки мы программным путем вставки статистической обработки данных. В этом учебнике будет показано, как для отображения сводных данных в этой строке нижнего колонтитула.


## <a name="introduction"></a>Вступление

Помимо просмотра каждого цены продуктов, единиц на складе, единицы заказа и переупорядочить уровни, пользователь может также быть интересны статистические сведения, такие как среднюю цену, общее число единиц на складе и так далее. Такие сводные данные часто отображается в нижней части отчета в строки итогов. Элемент управления GridView может включать строкой нижнего колонтитула, в которых ячейки мы программным путем вставки статистической обработки данных.

Эта задача дает нам три проблемы:

1. Настройка GridView для отображения его строкой нижнего колонтитула
2. Определение сводных данных; то есть как мы вычислений выполните среднюю цену или общее количество единиц на складе?
3. Добавление сводных данных в соответствующие ячейки в строке нижнего колонтитула

В этом учебнике будет показано, как преодолеть эти трудности. В частности мы создадим страница, на которой перечислены категории в раскрывающемся списке с выбранной категории продуктов, отображаемый в GridView. GridView будет включать строку нижнего колонтитула, отображает среднюю цену и общее число единиц на складе, а также на порядок продуктов в этой категории.


[![Сводная информация отображается в строке нижнего колонтитула GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Рис. 1**: Сводка информация отображается в строке нижнего колонтитула GridView ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))


Этот учебник, с его категория продуктов интерфейс главного и подчиненного представлений, основан на вопросы, рассмотренные в предыдущем [иерархического фильтрации с DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) учебника. Если вы еще не работали в более ранних по, сделайте это перед продолжением работы с этим параметром.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Шаг 1: Добавление DropDownList категорий и продуктов GridView

Перед с добавлением нижний колонтитул элемента GridView сводные сведения, касающиеся сами, давайте сначала просто для построения иерархического отчета. После завершения первого шага мы рассмотрим способ включения сводных данных.

Сначала откройте `SummaryDataInFooter.aspx` страницы в `CustomFormatting` папки. Добавьте элемент управления DropDownList и задайте его `ID` для `Categories`. Далее, щелкните ссылку в смарт-теге DropDownList Выбор источника данных и по желанию добавьте новый элемент управления ObjectDataSource с именем `CategoriesDataSource` , вызывающий `CategoriesBLL` класса `GetCategories()` метод.


[![Добавить новый элемент управления ObjectDataSource CategoriesDataSource с именем](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**На рисунке 2**: добавить новый элемент управления ObjectDataSource с именем `CategoriesDataSource` ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))


[![Иметь ObjectDataSource вызвать метод GetCategories() класса CategoriesBLL](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Рис. 3**: У вызова ObjectDataSource `CategoriesBLL` класса `GetCategories()` метод ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))


После настройки ObjectDataSource, этот мастер возвращает нам конфигурации источника данных DropDownList, мастер, из которого необходимо указать значение поля данных должны отображаться и какой из них должен соответствовать значению DropDownList `ListItem` s. У `CategoryName` поле отображаются и использование `CategoryID` как значение.


[![Используйте поля CategoryID и CategoryName как текст, а значение для элементов ListItem, соответственно](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Рис. 4**: использование `CategoryName` и `CategoryID` полей `Text` и `Value` для `ListItem` s, соответственно ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))


На этом этапе у нас есть DropDownList (`Categories`), в которой перечислены категории в системе. Теперь необходимо добавить GridView, в которой перечислены продукты, относящиеся к выбранной категории. Прежде чем мы делаем, однако теперь пора флажок Включить AutoPostBack в DropDownList смарт-тега. Как было сказано в *иерархического фильтрации с DropDownList* учебник, установив DropDownList `AutoPostBack` свойства `true` страницы будут публиковаться обратно при каждом изменении значения DropDownList. Это приведет к GridView к обновлению, показывающая те продукты, для новой выбранной категории. Если `AutoPostBack` свойству `false` (по умолчанию), изменение категории не вызывает обратную передачу и поэтому не будет обновлять перечисленных продуктов.


[![Установите флажок Enable AutoPostBack в DropDownList смарт-тег](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Рис. 5**: установите флажок "Включить" AutoPostBack в DropDownList смарт-тег ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))


Добавление элемента управления GridView к странице для отображения продуктов для выбранной категории. Присвоить свойству `ID` для `ProductsInCategory` и привязать его к новой ObjectDataSource с именем `ProductsInCategoryDataSource`.


[![Добавить новый элемент управления ObjectDataSource ProductsInCategoryDataSource с именем](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Рис. 6**: добавить новый элемент управления ObjectDataSource с именем `ProductsInCategoryDataSource` ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))


Настройте элемент управления ObjectDataSource таким образом, чтобы он вызывает `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод.


[![Вызовите метод GetProductsByCategoryID(categoryID) ObjectDataSource имеют](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Рис. 7**: У вызова ObjectDataSource `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))


Поскольку `GetProductsByCategoryID(categoryID)` метод принимает входной параметр, на последнем шаге мастера можно указать источник значения параметра. Чтобы отобразить эти продукты из выбранной категории, имеют параметр, полученных с `Categories` DropDownList.


[![Получение categoryID значение параметра из DropDownList выбранных категорий](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Рис. 8**: получение  *`categoryID`*  значение параметра в выпадающем выбранной категории ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))


После завершения мастера GridView будет иметь поле BoundField для каждого свойства продукта. Давайте очистить, чтобы только эти стояли `ProductName`, `UnitPrice`, `UnitsInStock`, и `UnitsOnOrder` стояли отображаются. Вы можете добавить все параметры уровня полей оставшиеся стояли (таких как формат `UnitPrice` как денежная единица). После внесения этих изменений, GridView должна выглядеть следующим образом:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

На этом этапе у нас есть полнофункциональной главного и подчиненного представлений отчета, отображающего имя, цену за единицу, единиц на складе и единиц заказа для этих продуктов, которые принадлежат выбранной категории.


[![Получение categoryID значение параметра из DropDownList выбранных категорий](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Рис. 9**: получение  *`categoryID`*  значение параметра в выпадающем выбранной категории ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Шаг 2: Отображение нижнего колонтитула в GridView

Элемент управления GridView можно отобразить строки верхний и нижний колонтитул. Эти строки отображаются в зависимости от значения `ShowHeader` и `ShowFooter` свойства, соответственно, с `ShowHeader` по умолчанию принимается `true` и `ShowFooter` для `false`. Чтобы включить нижний колонтитул в GridView, просто установите его `ShowFooter` свойства `true`.


[![GridView ShowFooter свойство имеет значение true](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Рис. 10**: присвоить свойству `ShowFooter` свойства `true` ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))


В строке нижнего колонтитула имеет ячейку для каждого из полей, определенных в GridView; Тем не менее эти ячейки являются пустыми по умолчанию. Теперь пора просмотрите ход работы в браузере. С `ShowFooter` свойство `true`, GridView включает пустая строка.


[![Теперь GridView включает строку нижнего колонтитула](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Рис. 11**: GridView теперь включает строку нижнего колонтитула ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))


Строки нижнего колонтитула на рис. 11 не выделялись, как с белым фоном. Давайте создадим `FooterStyle` класса CSS в `Styles.css` , указывающий Темно-красный фон и затем настройте `GridView.skin` файл обложки в `DataWebControls` тему, чтобы назначить этот класс CSS в GridView `FooterStyle`в `CssClass` свойство. Если вам нужно подтянуть свои навыки в обложки и темы, обращаться к [отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) учебника.

Начните с добавления следующий класс CSS для `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

`FooterStyle` Класс CSS аналогично стиль `HeaderStyle` класса `HeaderStyle`цвет фона — особенность темнее, а его текст отображается полужирным шрифтом. Кроме того текст в нижний колонтитул по правому краю, а заголовок текст выравнивается по центру.

Далее, чтобы связать этот класс CSS с нижний колонтитул каждого GridView, откройте `GridView.skin` файла в `DataWebControls` темы и набор `FooterStyle`в `CssClass` свойство. После этого добавления файла разметки должно иметь вид:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Как на снимке экрана ниже показано, это изменение делает нижний колонтитул выделяться сильнее.


[![Строка нижнего колонтитула GridView теперь имеет цвет фона красно-](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Рис. 12**: строкой нижнего колонтитула GridView теперь имеет красно-цвет фона ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Шаг 3: Вычисление сводных данных

Отобразить нижний колонтитул элемента GridView Далее важной проблемой нам описана информацию для вычисления сводных данных. Вычисляет эти статистические данные двумя способами.

1. С помощью запроса SQL может выдавать дополнительных запросов к базе данных для вычисления сводных данных для определенной категории. SQL включает несколько агрегатных функций вместе с `GROUP BY` предложение для указания данных, по которому должны быть получены сводные данные. Следующий SQL-запрос может вернуть необходимой информации:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Конечно не требуется выдать этот запрос непосредственно из `SummaryDataInFooter.aspx` страницы, а лишь путем создания метода в `ProductsTableAdapter` и `ProductsBLL`.
2. Вычислить эти сведения, как оно добавляется к GridView как описано в [форматирование на основе по данным](custom-formatting-based-upon-data-cs.md) учебника, GridView `RowDataBound` обработчик события запускается один раз для каждой строки, добавляемые в GridView, после его были с привязкой к данным. Создав обработчик событий для этого события может хранить текущее общее значений, мы хотим статистической обработки. После последней строки данных привязано к GridView, у нас есть итоги и сведения, необходимые для вычисления среднего.

Обычно я применяю второй подход, как она сохраняет маршрут в базе данных и трудозатраты, необходимые для реализации функциональности сводки в уровне доступа к данным и бизнес-логики, но было бы достаточно обоих подходов. В этом учебнике давайте использовать второй параметр и отслеживать использование промежуточных итогов `RowDataBound` обработчика событий.

Создание `RowDataBound` обработчик событий для GridView, выбрав GridView в конструкторе, щелкнув значок молнии из окна «Свойства» и дважды щелкните `RowDataBound` событий. Это создаст новый обработчик событий с именем `ProductsInCategory_RowDataBound` в `SummaryDataInFooter.aspx` классе кода страницы.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Чтобы сохранить текущее общее необходимо определить переменные вне обработчика событий. Создайте следующие четыре переменные уровня страницы:

- `_totalUnitPrice`, тип`decimal`
- `_totalNonNullUnitPriceCount`, тип`int`
- `_totalUnitsInStock`, тип`int`
- `_totalUnitsOnOrder`, тип`int`

Затем напишите код, чтобы увеличить эти три переменные для каждой строки данных обнаружил в `RowDataBound` обработчика событий.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

`RowDataBound` Запускает обработчик событий, гарантируя, что мы имеем дело с DataRow. После того, было установлено, `Northwind.ProductsRow` экземпляр, который только что был привязан к `GridViewRow` объекта в `e.Row` хранится в переменной `product`. Далее, выполнение общего переменных увеличиваются с соответствующими значениями текущего продукта (предполагая, что они не содержат базы данных `NULL` значение). Мы отслеживания объекта как работающих `UnitPrice` общей и количество отличных`NULL` `UnitPrice` записей, поскольку средняя цена является частным этих двух чисел.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Шаг 4: Отображение сводных данных в нижний колонтитул

Со сводными данными суммируются осталось для отображения в строке нижнего колонтитула GridView. Эта задача также можно выполнить программно с помощью `RowDataBound` обработчика событий. Помните, что `RowDataBound` запускает обработчик событий для *каждого* строку, которая привязывается к GridView, включая строки нижнего колонтитула. Таким образом можно расширить обработчиком событий для отображения данных в строке нижнего колонтитула, используя следующий код:


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Так как в строке нижнего колонтитула добавляется к GridView, после добавления всех строк данных, может быть уверенным, что к моменту мы готовы для отображения сводных данных в нижнем колонтитуле, будет завершена вычисления промежуточных итогов. Последний шаг, то для задания этих значений в ячейках нижний колонтитул.

Для отображения текста в ячейке определенного нижнего колонтитула, используйте `e.Row.Cells[index].Text = value`, где `Cells` индексирование начинается с 0. Следующий код вычисляет среднюю цену (Общая цена, деленное на количество продуктов) и отображает его вместе с общее число единиц на складе и модули в порядке в ячейках соответствующие нижний колонтитул элемента управления GridView.


[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

Рис. 13 показывает отчет после добавления этого кода. Обратите внимание как `ToString("c")` среднюю цену сводные сведения о форматироваться как валюты.


[![Строка нижнего колонтитула GridView теперь имеет цвет фона красно-](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Рис. 13**: строкой нижнего колонтитула GridView теперь имеет красно-цвет фона ([Просмотр полноразмерное изображение](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))


## <a name="summary"></a>Сводка

Отображение сводных данных является общим требованием отчетов и управления GridView позволяет легко включать такую информацию, в строке нижнего колонтитула. Отображается строка нижнего колонтитула при GridView `ShowFooter` свойству `true` и может иметь текст в этих ячейках задать программным путем с помощью `RowDataBound` обработчика событий. Вычисление сводных данных можно либо сделать путем повторного запроса к базе данных или с помощью кода в классе кода страницы ASP.NET для вычисления сводных данных программными средствами.

Этот учебник завершает изучение настраиваемое форматирование с элементами управления GridView, DetailsView и FormView. В этом руководстве Далее запускает наши исследования вставки, обновления и удаления данных с помощью этих же элементов управления.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Назад](using-the-formview-s-templates-cs.md)
[Вперед](custom-formatting-based-upon-data-vb.md)
