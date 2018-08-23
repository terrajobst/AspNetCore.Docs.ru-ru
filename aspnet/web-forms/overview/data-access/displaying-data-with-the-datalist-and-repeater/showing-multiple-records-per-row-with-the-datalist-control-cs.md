---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: Отображение нескольких записей в одной строке с помощью элемента управления DataList (C#) | Документация Майкрософт
author: rick-anderson
description: В этом кратком руководстве мы рассмотрим, как настроить макет элемента управления DataList через его свойства RepeatColumns и RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: f79c446a0c9407309ab65cd993df544e883afb22
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838503"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>Отображение нескольких записей в одной строке с помощью элемента управления DataList (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe) или [скачать PDF](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> В этом кратком руководстве мы рассмотрим, как настроить макет элемента управления DataList через его свойства RepeatColumns и RepeatDirection.


## <a name="introduction"></a>Вступление

Примеры элементов управления DataList мы ve в двух предыдущих курсах к просмотру каждой записи из источника данных в виде строки в элементе HTML с одним столбцом `<table>`. Хотя это поведение по умолчанию элемент управления DataList, это очень легко настроить отображение элементов управления DataList, таким образом, чтобы элементов источника данных могут быть распределены между несколькими столбцами, нескольких строк таблицы. Кроме того он может быть все данные источника, элементов, отображаемых в элементе управления DataList одной строкой и несколькими столбцами.

Мы можете настроить макет элементов управления DataList s через его `RepeatColumns` и `RepeatDirection` , обозначающие, соответственно, сколько столбцов визуализируется и расположены ли эти элементы располагаются горизонтально или вертикально. Рис. 1, например, показывает элемент управления DataList, отображающий сведения о продукте в таблице с тремя столбцами.


[![Элемент управления DataList отображается по три продукта на строку](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**Рис. 1**: The DataList показывает три продукта на строку ([Просмотр полноразмерного изображения](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))


Отображая нескольких элементов источников данных на строку, элемент управления DataList можно более эффективно использовать горизонтальное пространство экрана. В этом кратком руководстве мы рассмотрим эти два свойства DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Шаг 1: Отображение информации о продукте в элементе управления DataList

Перед изучением `RepeatColumns` и `RepeatDirection` свойства, let s сначала создайте элемент управления DataList на нашей странице, в котором перечислены сведения о продукте, с использованием макета Стандартная таблица с одним столбцом и несколькими строками. Например позвольте s Отображение s название продукта, категория и цена, используя следующую разметку:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

Мы ve узнали, как привязать данные к DataList в предыдущих примерах, поэтому я покажу эти действия быстро. Сначала откройте `RepeatColumnAndDirection.aspx` странице в `DataListRepeaterBasics` папки и перетащите элемент управления DataList из инструментария в конструктор. В смарт-теге элемента управления DataList s, необязательно, создайте новый ObjectDataSource и настроить его для извлечения данных из `ProductsBLL` класс s `GetProducts` метод, выбрав (нет) на мастер s INSERT, UPDATE, а удаление вкладки.

После создания и привязки нового источника ObjectDataSource к DataList, Visual Studio автоматически создает `ItemTemplate` , отображающий имя и значение для каждого из полей данных продукта. Настройка `ItemTemplate` непосредственно через декларативную разметку либо на основе шаблонов изменить параметр в смарт-теге элемента управления DataList s, заняв разметки, показанной выше, заменив *название продукта*, *имя категории* , и *цена* текста с помощью элементов управления Label, использующих синтаксис привязки данных соответствующие для присваивания значений их `Text` свойства. После обновления `ItemTemplate`, ваши страницы s должна выглядеть следующим образом:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

Обратите внимание, что я ve включены описатель формата в `Eval` синтаксис привязки данных для `UnitPrice`, форматирование возвращаемого значения как денежной единицы - `Eval("UnitPrice", "{0:C}").`

Отвлекитесь и страницу в браузере. Как показано на рис. 2, элемент управления DataList визуализируется как таблица продуктов одним столбцом и несколькими строками.


[![По умолчанию, отображаемое DataList как таблица с одним столбцом и несколькими строками](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**Рис. 2**: по умолчанию, элемент управления DataList визуализируется как одного столбца, таблицы с несколькими строками ([Просмотр полноразмерного изображения](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Шаг 2: Изменение направления макета DataList s

Во время поведение по умолчанию для элемента управления DataList для выведения его элементов по вертикали в таблице с одним столбцом и нескольких строк, это поведение можно легко изменить с помощью элементов управления DataList s [ `RepeatDirection` свойство](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). `RepeatDirection` Свойство может принимать одно из двух возможных значений: `Horizontal` или `Vertical` (по умолчанию).

Изменив `RepeatDirection` свойства из `Vertical` для `Horizontal`, DataList визуализирует свои записи в одну строку, создавая один столбец на элемент источника данных. Чтобы проиллюстрировать этот эффект, щелкните в конструкторе элемента управления DataList и затем из окна свойств измените `RepeatDirection` свойства из `Vertical` для `Horiztonal`. Сразу же после этого конструктор скорректирует компоновку макета DataList s, создание интерфейса с одной строкой и несколькими столбцами (см. рис. 3).


[![RepeatDirection диктует как направление передачи Свойства DataList s их расположения Out](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**Рис. 3**: `RepeatDirection` свойство определяет, как элементы направление DataList s, Laid Out ([Просмотр полноразмерного изображения](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))


При отображении небольших объемов данных, в одну строку, таблица с несколькими столбцами может быть идеальным способом максимально использовать экран. Для более крупных объемов данных тем не менее одной строки потребуется большое количество столбцов, которые отправляют элементы, удается умещается на экране справа. Figure 4 shows the products when rendered in a single-row DataList. Так как многие продукты (более чем 80), пользователю придется прокручивать экран далеко вправо, чтобы просмотреть сведения о каждом из продуктов.


[![Для достаточно больших источников данных элемент управления DataList с одним столбцом будет требовать горизонтальной прокрутки](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**Рис. 4**: для достаточно больших источников данных, один столбец DataList будет требовать горизонтальной прокрутки ([Просмотр полноразмерного изображения](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Step 3: Displaying Data in a Multi-Column, Multi-Row Table

Чтобы создать элемент управления DataList несколькими столбцами, нескольких строк, нам нужно установить [ `RepeatColumns` свойство](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) число столбцов для отображения. По умолчанию `RepeatColumns` задано значение 0, что приведет к DataList отобразить все элементы в одну строку или столбец (в зависимости от значения `RepeatDirection` свойство).

В нашем примере позволяют s отобразим три продукта на строку таблицы. Таким образом, задать `RepeatColumns` значение 3. После внесения этого изменения, Отвлекитесь и просмотрите результаты в браузере. Как показано на рис. 5, продукты теперь перечислены в таблице с тремя столбцами и нескольких строк.


[![Отображается три продукта на строку](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**Рис. 5**: отображается три продукта на строку ([Просмотр полноразмерного изображения](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))


`RepeatDirection` Свойство влияет на расположение элементов в элементе управления DataList. Рис. 5 показаны результаты с `RepeatDirection` свойство значение `Horizontal`. Обратите внимание на то, что три первых продукта Chai, Chang и Aniseed Syrup располагаются слева направо, сверху вниз. Следующие три продукта (начиная с Chef Anton s Cajun Seasoning) отображаются в строке под первыми тремя. Изменение `RepeatDirection` обратно на `Vertical`, тем не менее, располагает Бокс этих продуктов, сверху вниз, слева направо, как показано на рис. 6.


[![Здесь они Laid Out по вертикали](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**Рис. 6**: Здесь они Laid Out по вертикали ([Просмотр полноразмерного изображения](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))


Количество строк, отображаемых в результирующей таблице зависит от общего числа записей привязан к элементу управления DataList. Точнее, ее s, деленное на общее число элементов источника данных, выделенных `RepeatColumns` значение свойства. Так как `Products` в данный момент есть 84 продукта, который делится на 3, имеется 28 строк. Если количество элементов в источнике данных и `RepeatColumns` значение свойства не на значение, то в последней строке или столбце будут черные клетки. Если `RepeatDirection` присваивается `Vertical`, последний столбец будет иметь пустые клетки, если `RepeatDirection` является `Horizontal`, последняя строка будет иметь пустые ячейки.

## <a name="summary"></a>Сводка

Элемент управления DataList, по умолчанию, перечисляет свои элементы в таблицу одним столбцом и несколькими строками, которая повторяет компоновку элемента GridView с одной TemplateField. Хотя такой макет является приемлемой, мы максимально увеличить площадь экрана, отобразив несколько элементов источника данных каждой строки. Относящееся к элементу управления — это просто параметр DataList s `RepeatColumns` количество столбцов для отображения каждой строки. Кроме того, элемент управления DataList s `RepeatDirection` свойство может использоваться для указания, является ли содержимое таблицы несколькими столбцами, многострочная следует компоновать по горизонтали слева направо и сверху вниз или по вертикали сверху вниз, слева направо.

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был (John suru). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
> [Вперед](nested-data-web-controls-cs.md)
