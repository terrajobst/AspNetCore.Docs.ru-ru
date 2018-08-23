---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: Отображение сводной информации в нижнем колонтитуле элемента GridView (VB) | Документация Майкрософт
author: rick-anderson
description: Сводная информация часто отображается в нижней части отчета в строке сводки. Элемент управления GridView может включать строку нижнего колонтитула, в которых ячейки, мы можем запроса на Вытягивание...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: dfb78ee1e5da2774254cbe685b8dfd3dc7d46af9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835970"
---
<a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Отображение сводной информации в нижнем колонтитуле элемента GridView (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) или [скачать PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> Сводная информация часто отображается в нижней части отчета в строке сводки. Элемент управления GridView может включать строку нижнего колонтитула в ячейки которого мы программно внедрить статистической обработки данных. В этом руководстве будет показано, как для отображения сводных данных в этой строке нижнего колонтитула.


## <a name="introduction"></a>Вступление

Помимо отображения каждого из продуктов цены, единиц на складе, заказанных единиц для изменения порядка уровней и порядок, пользователь может также заинтересовать статистические сведения, такие как средняя цена, общее количество единиц на складе и так далее. Такие сведения часто отображается в нижней части отчета в строке сводки. Элемент управления GridView может включать строку нижнего колонтитула в ячейки которого мы программно внедрить статистической обработки данных.

Эта задача дает нам три проблемы:

1. Настройка для отображения его строку нижнего колонтитула GridView
2. Определение сводных данных; то есть как мы вычисляет среднюю цену или общее количество единиц на складе?
3. Добавление сводных данных в соответствующие ячейки таблицы строки нижнего колонтитула

В этом руководстве будет показано, как преодолеть эти трудности. В частности мы создадим страницу со списком категорий в раскрывающемся списке с продуктами выбранной категории, отображаемый в GridView. GridView будет включать строку нижнего колонтитула, отображает среднюю цену и общее количество единиц на складе, а также на порядок продуктов в этой категории.


[![Сводная информация отображается в строке нижнего колонтитула GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Рис. 1**: сведения о отображается в строке нижнего колонтитула GridView ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))


Этот учебник, с его категории продуктов «основной/подробности» интерфейс, основан на понятия, описанные в предыдущих ["основной/подробности" Фильтрация с помощью элемента управления DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) руководства. Если вы еще не работали с более ранней руководством, сделайте это перед продолжением работы с данным элементом.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Шаг 1: Добавление элемента управления DropDownList категорий и GridView для продуктов

Прежде чем добавить сводные данные для нижнего колонтитула GridView, касающиеся сами, давайте сначала просто построить отчет «основной/подробности». Когда мы изучили этот первый шаг, мы рассмотрим способ включения сводных данных.

Сначала откройте `SummaryDataInFooter.aspx` странице в `CustomFormatting` папку. Добавьте элемент управления DropDownList и задайте его `ID` для `Categories`. Затем щелкните ссылку выберите источник данных смарт-теге DropDownList и необязательно, чтобы добавить новый ObjectDataSource, именуемый `CategoriesDataSource` , вызывающий `CategoriesBLL` класса `GetCategories()` метод.


[![Добавьте новый ObjectDataSource, именуемый CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Рис. 2**: добавьте новый ObjectDataSource с именем `CategoriesDataSource` ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))


[![У элемента управления ObjectDataSource вызова метода GetCategories() класса categoriesbll](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Рис. 3**: иметь элемент управления ObjectDataSource вызывает `CategoriesBLL` класса `GetCategories()` метод ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))


После настройки ObjectDataSource, этот мастер возвращает нам конфигурации источника данных элемента управления DropDownList, мастер, из которого необходимо указать значение поля данных должны отображаться и какой из них должен соответствовать значению DropDownList `ListItem` s. У `CategoryName` поле, отображаемое и использование `CategoryID` как значение.


[![Используйте поля CategoryID и CategoryName текста и значения для элементов ListItem, соответственно](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Рис. 4**: использование `CategoryName` и `CategoryID` полей `Text` и `Value` для `ListItem` s, соответственно ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))


На этом этапе у нас есть элемента управления DropDownList (`Categories`), содержащая список категорий в системе. Теперь нам нужно добавить элемент GridView со списком продуктов, принадлежащих выбранной категории. Сначала, однако Отвлекитесь и установите флажок Включить AutoPostBack в смарт-тега DropDownList. Как уже говорилось в *"основной/подробности" Фильтрация с помощью элемента управления DropDownList* учебника, задав DropDownList `AutoPostBack` свойства `True` страницы будут передаваться обратно при каждом изменении значения элемента управления DropDownList. Это приведет к GridView к обновлению, с этими продуктами для новой выбранной категории. Если `AutoPostBack` свойству `False` (по умолчанию), изменение категории не вызывает обратную передачу и таким образом, не обновляются продукты.


[![Установите флажок Enable AutoPostBack в смарт-тега DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Рис. 5**: установите флажок "Включить" AutoPostBack в смарт-теге DropDownList ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))


Добавление элемента управления GridView на страницу для отображения продуктов для выбранной категории. Значение элемента GridView `ID` для `ProductsInCategory` и привязать его к элементу управления ObjectDataSource с именем `ProductsInCategoryDataSource`.


[![Добавьте новый ObjectDataSource, именуемый ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Рис. 6**: добавьте новый ObjectDataSource с именем `ProductsInCategoryDataSource` ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))


Настройте элемент управления ObjectDataSource, таким образом, чтобы он вызывает `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод.


[![У элемента управления ObjectDataSource вызвать метод GetProductsByCategoryID(categoryID)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Рис. 7**: иметь элемент управления ObjectDataSource вызывает `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))


Так как `GetProductsByCategoryID(categoryID)` метод принимает входной параметр, на последнем шаге мастера можно указать источник значения параметра. Для отображения этих продуктов выбранной категории, имеют параметр, извлеченных из `Categories` DropDownList.


[![Получение categoryID значение параметра из выбранной категории DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Рис. 8**: получение *`categoryID`* значение параметра из выбранной категории DropDownList ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))


После завершения работы мастера GridView будет иметь BoundField для каждого из свойств продукта. Давайте очистки, чтобы только эти поля BoundField `ProductName`, `UnitPrice`, `UnitsInStock`, и `UnitsOnOrder` отображаются поля BoundFields. Вы можете добавить любые параметры на уровне полей для оставшихся полей BoundFields (например при форматировании `UnitPrice` как денежная единица). После внесения этих изменений декларативная разметка GridView должен выглядеть следующим образом:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

На этом этапе у нас есть полнофункционального отчета «основной/подробности» в которой отображается имя, цена за единицу, единиц на складе и единицы измерения для этих продуктов, принадлежащих выбранной категории.


[![Получение categoryID значение параметра из выбранной категории DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Рис. 9**: получение *`categoryID`* значение параметра из выбранной категории DropDownList ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))


## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Шаг 2: Отображение нижний колонтитул в GridView

Элемент управления GridView может отображать строки нижнего колонтитула и заголовка. Эти строки отображаются в зависимости от значения `ShowHeader` и `ShowFooter` свойства, соответственно, с помощью `ShowHeader` по умолчанию принимается `True` и `ShowFooter` для `False`. Чтобы включить нижний колонтитул в GridView, просто установите для его `ShowFooter` свойства `True`.


[![Свойства элемента GridView ShowFooter задано значение True](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Рис. 10**: присвоить свойству `ShowFooter` свойства `True` ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))


Строки нижнего колонтитула содержит ячейку для каждого из полей, определенных в GridView; Тем не менее эти ячейки являются пустыми по умолчанию. Отвлекитесь и просмотрите ход работы в браузере. С помощью `ShowFooter` свойство `True`, GridView включает пустая строка.


[![GridView теперь включает в себя строку нижнего колонтитула](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Рис. 11**: GridView теперь включает строку нижнего колонтитула ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))


Строки нижнего колонтитула на рис. 11 не выделялись, так как она содержит белый фон. Давайте создадим `FooterStyle` класса CSS, в `Styles.css` , указывающий Темно-красный фон и затем настройте `GridView.skin` файл обложки в `DataWebControls` темы, чтобы назначить этот класс CSS в GridView `FooterStyle`в `CssClass` свойство. Если вы хотите освежить в обложки и темы, обращаться к [отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) руководства.

Начните с добавления следующий класс CSS `Styles.css`:


[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

`FooterStyle` Класс CSS в стиль, который напоминает `HeaderStyle` класса, несмотря на то что `HeaderStyle`цвет фона имеет один важный нюанс темнее и его текст отображается полужирным шрифтом. Кроме того текст в нижнем колонтитуле по правому краю, тогда как текст заголовка выравнивается по центру.

Далее, чтобы связать этот класс CSS с нижним колонтитулом для каждого элемента GridView, откройте `GridView.skin` файл `DataWebControls` темы и набор `FooterStyle`в `CssClass` свойство. После этого добавления файла разметки должен выглядеть так:


[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

Как на снимке экрана ниже показано, это изменение делает нижний колонтитул выделяться сильнее.


[![Строки нижнего колонтитула GridView теперь имеет красно-фонового цвета](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Рис. 12**: строки нижнего колонтитула элемента GridView теперь имеет красно-цвет фона ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))


## <a name="step-3-computing-the-summary-data"></a>Шаг 3: Вычисление сводных данных

С помощью отображения нижнего колонтитула GridView со следующей проблемой, с которыми мы, как для вычисления сводных данных. Чтобы вычислить эти статистические сведения двумя способами:

1. С помощью запроса SQL мы выполним дополнительный запрос к базе данных для вычисления сводных данных для определенной категории. SQL включает ряд агрегатные функции вместе с `GROUP BY` предложение для указания данных, по которому должны быть получены сводные данные. Следующий SQL-запрос может вернуть необходимой информации:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    Конечно вы не захотите выполнить этот запрос непосредственно из `SummaryDataInFooter.aspx` страницы, а лишь путем создания метода в `ProductsTableAdapter` и `ProductsBLL`.
2. Вычислить эту информацию, как оно добавляется к элементу GridView рассматривался в [Custom форматирование данных на базе при](custom-formatting-based-upon-data-cs.md) учебника, GridView `RowDataBound` обработчик события запускается один раз для каждой строки, добавляемый к элементу GridView после его были с привязкой к данным. Создать обработчик событий для этого события можно хранить текущее общее значений, мы хотим статистической обработки. После последней строки данных привязан к GridView, у нас есть итоговые значения и сведения, необходимые для вычисления среднего значения.

Я обычно применяется второй подход, как она сохраняет поездку в базу данных и усилий для реализации функциональности сводки в уровне доступа к данным и бизнес-логики, но было бы достаточно любой из этих подходов. В этом руководстве давайте использовать второй вариант и отслеживать нарастающий итог с помощью `RowDataBound` обработчик событий.

Создание `RowDataBound` обработчик событий для элемента GridView, выбрав GridView в конструкторе, щелкнув значок с молнией в окне «Свойства» и дважды щелкнув `RowDataBound` событий. Кроме того можно выбрать GridView и его событие RowDataBound из раскрывающихся списков в верхней части файла класса кода ASP.NET. Это создаст новый обработчик событий с именем `ProductsInCategory_RowDataBound` в `SummaryDataInFooter.aspx` вспомогательном классе страницы.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Чтобы поддерживать текущее общее нам нужно определить переменные за пределами области обработчика событий. Создайте следующие четыре переменные уровня страницы:

- `_totalUnitPrice`, типа `Decimal`
- `_totalNonNullUnitPriceCount`, типа `Integer`
- `_totalUnitsInStock`, типа `Integer`
- `_totalUnitsOnOrder`, типа `Integer`

Теперь напишите код, чтобы увеличить эти три переменные для каждой строки данных обнаружена в `RowDataBound` обработчик событий.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

`RowDataBound` Запускает обработчик событий, гарантируя, что мы имеем дело с DataRow. После того как, было установлено, `Northwind.ProductsRow` , который просто был привязан к `GridViewRow` объекта в `e.Row` хранится в переменной `product`. Далее, работающей общее переменных увеличиваются на соответствующие значения для данного продукта (при условии, что они не содержат базу данных `NULL` значение). Мы хранить список запущенных `UnitPrice` всего» и «количество отличных`NULL` `UnitPrice` записывает так, как средняя цена является частным этих двух чисел.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Шаг 4: Отображение сводных данных в нижнем колонтитуле

Со сводными данными сгруппированы осталось для отображения в строке нижнего колонтитула GridView. Эту задачу, кроме того, можно выполнить программно с помощью `RowDataBound` обработчик событий. Помните, что `RowDataBound` запускает обработчик событий для *каждые* строку, которая привязывается к GridView, включая строки нижнего колонтитула. Таким образом мы можем расширить наш обработчик событий для отображения данных в строке нижнего колонтитула, используя следующий код:


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Так как строки нижнего колонтитула добавляется к элементу GridView после добавления всех строк данных, может быть уверены в том, что к моменту, мы готовы для отображения сводных данных в нижнем колонтитуле, которые будут выполнены вычисления промежуточных итогов. Последний шаг, то для задания этих значений в ячейках нижнего колонтитула.

Для отображения текста в ячейке определенного нижний колонтитул, используйте `e.Row.Cells(index).Text = value`, где `Cells` индексация начинается с 0. Следующий код вычисляет среднюю цену (Общая цена, деленное на количество продуктов) и отображает его, а также общее число единиц на складе и заказанных в ячейках соответствующие нижнего колонтитула элемента управления GridView единиц.


[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

Рис. 13 показан отчет, после добавления этого кода. Обратите внимание как `ToString("c")` среднюю цену сводные сведения о форматироваться как денежная единица.


[![Строки нижнего колонтитула GridView теперь имеет красно-фонового цвета](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Рис. 13**: строки нижнего колонтитула элемента GridView теперь имеет красно-цвет фона ([Просмотр полноразмерного изображения](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))


## <a name="summary"></a>Сводка

Отображение сводных данных является общим требованием отчет и элемент управления GridView облегчает входит такая информация в его строку нижнего колонтитула. Строка нижнего колонтитула отображается при GridView `ShowFooter` свойству `True` и может содержать текст в ячейках, задать программно с помощью `RowDataBound` обработчик событий. Вычисление сводных данных можно либо сделать путем повторного запроса к базе данных или с помощью кода в классе фонового кода страницы ASP.NET для программно вычисления сводных данных.

Этот учебник завершает изучение настраиваемое форматирование с элементами управления GridView, DetailsView и FormView. Нашем следующем учебном курсе запускает исследование Вставка, обновление и удаление данных с помощью этих же элементов управления.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](using-the-formview-s-templates-vb.md)
