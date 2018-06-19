---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Отображение нескольких записей на строку с элементом управления DataList (VB) | Документы Microsoft
author: rick-anderson
description: Этот краткий учебник мы изучим, как настроить элемент управления DataList макета через свои свойства RepeatColumns и RepeatDirection.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c85e5a1d7b88a9ed53ed8392a300d5118363bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889999"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Отображение нескольких записей на строку с элементом управления DataList (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) или [скачать PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> Этот краткий учебник мы изучим, как настроить элемент управления DataList макета через свои свойства RepeatColumns и RepeatDirection.


## <a name="introduction"></a>Вступление

В примерах DataList мы хранять встречается за последние два учебника к просмотру каждой записи из источника данных в виде строки в HTML один столбец `<table>`. Это поведение по умолчанию DataList, очень легко настроить отображение DataList таким образом, что элементы источника данных распределены между несколькими столбцами, нескольких строк таблицы. Кроме того он s, можно иметь все данные источника элементов, отображаемых в элементе управления DataList одной строки и нескольких столбцов.

Мы можете настроить макет DataList s через его `RepeatColumns` и `RepeatDirection` , обозначающие, соответственно, сколько столбцов подготавливаются к просмотру и ли эти элементы располагаются вертикально или горизонтально. Рис. 1 показано, DataList, в которой отображаются сведения о продукте в таблицу с тремя столбцами.


[![Элемент управления DataList отображаются три продукты на строку](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Рис. 1**: DataList показано три продукта каждой строки ([Просмотр полноразмерное изображение](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


Отображая несколько элементов источника данных для строки, DataList более эффективно использовать места на экране по горизонтали. Этот краткий учебник мы изучим, эти два свойства DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Шаг 1: Отображение сведений о продукте в элементе управления DataList

Перед тем как узнать `RepeatColumns` и `RepeatDirection` свойства, позволяющие s сначала создать DataList на нашей странице, в которой перечислены сведения о продукте, с использованием макета стандартной один столбец и нескольких строк таблицы. В этом примере позволяют s Отображение s название продукта, категория и цена, используя следующую разметку:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Мы хранять показано, как привязать данные к DataList в предыдущих примерах, поэтому будет быстро переместить шаги выполнения этого действия. Сначала откройте `RepeatColumnAndDirection.aspx` страницы в `DataListRepeaterBasics` папки и перетащите DataList из области элементов в конструктор. В смарт-теге DataList s попробовать создать новый элемент управления ObjectDataSource и настроить его для извлечения данных из `ProductsBLL` класса s `GetProducts` метод, выбрав (нет) параметра с помощью мастера s INSERT, UPDATE и удаление вкладок.

После создания и привязки к элементу управления DataList новый элемент управления ObjectDataSource, Visual Studio автоматически создает `ItemTemplate` отображает имя и значение для каждого из полей данных продукта. Настройка `ItemTemplate` непосредственно через декларативная разметка либо из редактирование шаблонов в диалоговом окне DataList s смарт-тег, чтобы он использовал разметки, показанном выше, заменив *название продукта*, *имя категории* , и *цены* текста с помощью элементов управления Label, использующих синтаксис соответствующие привязки данных для присвоения значений для их `Text` свойства. После обновления `ItemTemplate`, ваш s страница должна выглядеть следующим образом:


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Обратите внимание на то что хранить включены спецификатор формата в `Eval` синтаксис привязки данных для `UnitPrice`, форматирование в денежном формате - возвращаемое значение `Eval("UnitPrice", "{0:C}").`

Теперь пора посетите страницу в браузере. Как показано на рисунке 2, DataList готовится к просмотру как один столбец и нескольких строк таблицы продуктов.


[![По умолчанию, отображаемое DataList как один столбец и нескольких строк таблицы](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**На рисунке 2**: по умолчанию, DataList отображается в виде одного столбца, таблицы с несколькими строками ([Просмотр полноразмерное изображение](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Шаг 2: Изменение направление макета DataList s

При поведение по умолчанию для DataList — для размещения элементов по вертикали в один столбец и нескольких строк таблицы, это поведение можно легко изменить с помощью DataList s [ `RepeatDirection` свойства](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). `RepeatDirection` Свойство может принимать одно из двух возможных значений: `Horizontal` или `Vertical` (по умолчанию).

Изменив `RepeatDirection` свойство из `Vertical` для `Horizontal`, DataList отображает ее записи в виде одной строки и создание элемента источника данных для одного столбца. Чтобы проиллюстрировать этот эффект, щелкните в DataList в конструкторе и в окне свойств измените `RepeatDirection` свойство из `Vertical` для `Horiztonal`. Сразу после таким образом, конструктор настраивает DataList s макет, создание интерфейса одной строки, несколькими столбцами (см. рис. 3).


[![Элементы RepeatDirection определяет как направление передачи Свойства DataList s, упорядочивается Out](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Рис. 3**: `RepeatDirection` свойство определяет, как направление DataList s перечислены расположения Out ([Просмотр полноразмерное изображение](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


При отображении небольших объемов данных, в одну строку нескольких столбцов таблицы может быть идеальным способом, чтобы максимально увеличить площадь экрана. Для больших объемов данных Однако одна строка потребуется большое количество столбцов, какие отправлений те элементы, t может поместиться на экране справа. На рисунке 4 показаны продуктов при подготовке к просмотру в DataList одной строки. Поскольку существует множество продуктов (более чем на 80), пользователь будет гораздо прокручивать экран вправо для просмотра сведений о каждом из этих продуктов.


[![Для источников данных, достаточно большой потребуется DataList один столбец горизонтальной полосы прокрутки](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Рис. 4**: для достаточно больших источников данных, один столбец DataList будет требовать горизонтальной прокрутки ([Просмотр полноразмерное изображение](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Шаг 3: Отображение данных в нескольких столбцов, нескольких строк таблицы

Для создания нескольких столбцов, многострочных DataList, нам нужно указать [ `RepeatColumns` свойство](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) число столбцов для отображения. По умолчанию `RepeatColumns` задано значение 0, что вызовет DataList отобразить все элементы в виде одной строки или столбца (в зависимости от значения `RepeatDirection` свойство).

В нашем примере позволяют s отображения трех продуктов на каждую строку таблицы. Таким образом, задать `RepeatColumns` значение, равное 3. После этого изменения занять некоторое время для просмотра результатов в браузере. Как показано на рисунке 5, продукты, появятся в трех столбцов, нескольких строк таблицы.


[![Три продукта, отображаются в строке](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Рис. 5**: три продукта, отображаются в строке ([Просмотр полноразмерное изображение](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


`RepeatDirection` Свойство влияет на расположение элементов в элемент управления DataList. Рис. 5 показан результаты с `RepeatDirection` свойство `Horizontal`. Обратите внимание, что первые три продуктов Chai, изменений и Aniseed Syrup располагаются слева направо, сверху вниз. Следующие три продукты (начиная с s Креольская Cajun Seasoning) отображаются в строке под первых трех. Изменение `RepeatDirection` обратно свойство `Vertical`, однако располагает этих продуктов, сверху вниз, слева направо, как показано на рис. 6.


[![Здесь они упорядочивается Out по вертикали](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Рис. 6**: Здесь они упорядочивается Out по вертикали ([Просмотр полноразмерное изображение](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


Число строк, отображаемых в результирующей таблице зависит от числа общее число записей, привязанный к элементу управления DataList. Точнее, он s, деленное на ceiling общее количество элементов источника данных `RepeatColumns` значение свойства. Поскольку `Products` таблице в данный момент 84 продуктов, который делится на 3, 28 строк. Если количество элементов в источнике данных и `RepeatColumns` значение свойства не являются кратных, то последняя строка или столбец будут иметь пустые ячейки. Если `RepeatDirection` равно `Vertical`, последний столбец будет иметь пустые ячейки; Если `RepeatDirection` — `Horizontal`, последняя строка будет иметь пустые ячейки.

## <a name="summary"></a>Сводка

Элемент управления DataList по умолчанию список его в один столбец и нескольких строк таблицы, которая имитирует макет элемента управления GridView с одной TemplateField. Хотя этот макет по умолчанию приемлемо, мы увеличить площадь экрана путем отображения нескольких элементов источника данных в строке. Решения этой проблемы — просто параметр DataList s `RepeatColumns` количество столбцов для отображения каждой строки. Кроме того, элемент управления DataList s `RepeatDirection` свойство может использоваться для указания, является ли содержимое нескольких строк, нескольких столбцов таблицы должен быть размещен по горизонтали слева направо, сверху вниз или по вертикали сверху вниз, слева направо.

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было Джон Suru. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [Вперед](nested-data-web-controls-vb.md)
