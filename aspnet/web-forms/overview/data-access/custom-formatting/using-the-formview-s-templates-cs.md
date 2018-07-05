---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: Использование шаблонов FormView (C#) | Документация Майкрософт
author: rick-anderson
description: В отличие от элемента управления DetailsView FormView не состоит из полей. Вместо этого FormView подготавливается к просмотру с помощью шаблонов. В этом руководстве мы рассмотрим использование F....
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: cea8cad7c3e049ac5b608a51840bde98c825a90f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366732"
---
<a name="using-the-formviews-templates-c"></a>Использование шаблонов FormView (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) или [скачать PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> В отличие от элемента управления DetailsView FormView не состоит из полей. Вместо этого FormView подготавливается к просмотру с помощью шаблонов. В этом учебном курсе мы рассмотрим использование элемента управления FormView для представления менее строгое представление данных.


## <a name="introduction"></a>Вступление

В последних двух учебных курсах мы научились изменять выходные данные управления GridView и DetailsView, использование полей TemplateField. Разрешить полей TemplateField для содержимого для конкретных полей настраиваться высокой, но в конечном счете GridView и DetailsView имеют вид довольно коробкоподобный, действующие по принципу сетки. Во многих случаях такой сеткоподобной компоновки является идеальным, но в некоторых случаях требуется более гибкое отображение. При отображении единственной записи, легко изменяемая компоновка возможна с помощью элемента управления FormView.

В отличие от элемента управления DetailsView FormView не состоит из полей. Невозможно добавить BoundField и TemplateField в элементе управления FormView. Вместо этого FormView подготавливается к просмотру с помощью шаблонов. Представить себе элемент FormView как элемент управления DetailsView, содержащий один TemplateField. FormView поддерживает следующие шаблоны:

- `ItemTemplate` используемый для визуализации конкретной записи, отображенной в FormView
- `HeaderTemplate` используется для указания необязательной строки заголовка
- `FooterTemplate` используется для указания необязательной строки
- `EmptyDataTemplate` Когда FormView `DataSource` не имеет записей, `EmptyDataTemplate` используется вместо `ItemTemplate` для визуализации разметки элемента управления
- `PagerTemplate` можно использовать для настройки интерфейса разбиения по страницам для FormView среды, у которых разбиение включено
- `EditItemTemplate` / `InsertItemTemplate` используется для настройки интерфейса правки или вставки интерфейса для FormView среды, поддерживающих такие функции

В этом учебном курсе мы рассмотрим использование элемента управления FormView для представления менее строгое отображения продуктов. Вместо полей для имени, категории, поставщика и т. д FormView `ItemTemplate` покажет эти значения, с помощью сочетания элемента заголовка и `<table>` (см. рис. 1).


[![FormView разрыва Сеткоподобной компоновки, Наблюдавшейся в DetailsView](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Рис. 1**: FormView преодолевает ограничений Grid-Like макета, видны в DetailsView ([Просмотр полноразмерного изображения](using-the-formview-s-templates-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Шаг 1: Привязка данных к элементу FormView

Откройте `FormView.aspx` страницы и перетащите с панели инструментов в конструктор элемента FormView. При первом добавлении FormView отображается как серый квадрат, указывая, `ItemTemplate` необходим.


[![FormView не визуализируется в конструкторе, пока предоставлен ItemTemplate](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**Рис. 2**: The FormView невозможно отобразить в конструкторе до `ItemTemplate` предоставляется ([Просмотр полноразмерного изображения](using-the-formview-s-templates-cs/_static/image6.png))


`ItemTemplate` Могут быть созданы вручную (через декларативный синтаксис) или может быть автоматически созданная по привязке FormView к элементу управления источником данных через конструктор. Автоматически создаваемый `ItemTemplate` содержит HTML, что списки имя каждого поля и метки, свойство `Text` свойство привязано к значению поля. Этот подход также автоматически создает `InsertItemTemplate` и `EditItemTemplate`, которые заполняются элементы управления вводом для каждого поля данных, возвращаемых элементом управления источника данных.

Если вы хотите автоматически создавать шаблон, с помощью смарт-тега FormView добавить новый элемент управления ObjectDataSource, который вызывает `ProductsBLL` класса `GetProducts()` метод. Это создаст FormView с `ItemTemplate`, `InsertItemTemplate`, и `EditItemTemplate`. В представлении источника, удалите `InsertItemTemplate` и `EditItemTemplate` так, как мы не интересует создание элемента FormView, который поддерживает правки или вставки пока. Затем очистите разметку внутри `ItemTemplate` таким образом, чтобы у нас есть для работы с нуля.

Если вам предпочтительнее создать `ItemTemplate` вручную, можно добавить и настроить элемент управления ObjectDataSource, перетащив его с панели инструментов в конструктор. Однако не устанавливайте FormView источника данных из конструктора. Вместо этого перейдите в представление источника и вручную установить значение `DataSourceID` свойства `ID` значение элемента управления ObjectDataSource. Вручную добавьте `ItemTemplate`.

Какой бы подход был выбран, на этом этапе декларативная разметка элемента FormView должна выглядеть следующим образом:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

Отвлекитесь и установите флажок Enable Paging в смарт-тега FormView; При этом будет добавлено `AllowPaging="True"` атрибут декларативный синтаксис элемента FormView. Кроме того, задайте `EnableViewState` значение False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Шаг 2: Определение`ItemTemplate`элемента разметки

FormView привязанной к элементу управления ObjectDataSource и настроен для поддержки разбиения по страницам мы готовы указывать содержимое для `ItemTemplate`. Для этого руководства, пусть название продукта, отображаемый в `<h3>` заголовок. После этого мы используем HTML `<table>` для отображения остающихся свойств продукта в четырехстолбцовой таблице, где первый и третий столбцы перечисляют имена свойств, а второй и четвертый списке их значения.

Эта разметка может быть вводить в через интерфейс редактирования шаблона элемента FormView в конструкторе или вручную через декларативный синтаксис. При работе с шаблонами я обычно нахожу проще работать напрямую с декларативным синтаксисом, но вы можете использовать ту же методику, вам наиболее подходит.

В следующем примере показана декларативная разметка FormView после `ItemTemplate`его завершения структуры:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Обратите внимание, что синтаксис привязки данных — `<%# Eval("ProductName") %>`, для примера можно внедрить непосредственно в выходные данные шаблона. То есть, его не нужно назначать в элемент управления Label `Text` свойство. Например, у нас есть `ProductName` значением, отображаемым в `<h3>` элемента с помощью `<h3><%# Eval("ProductName") %></h3>`, что для продукта Chai, будет визуализироваться как `<h3>Chai</h3>`.

`ProductPropertyLabel` И `ProductPropertyValue` классы CSS используются для указания стиля продукта имена свойств и значений в `<table>`. Эти классы CSS определены в `Styles.css` и делают имена свойств полужирного шрифта и по правому краю и добавляют правое заполнение к значениям свойств.

Так как используются не CheckBoxFields с FormView, чтобы можно было отобразить `Discontinued` значение как флажок необходимо добавить собственный элемент управления CheckBox. `Enabled` Свойство имеет значение False, что только для чтения и флажок `Checked` свойство привязано к значению `Discontinued` поля данных.

С помощью `ItemTemplate` завершения, сведения о продукте отображается в гораздо более гибкой форме. Сравните выходные данные DetailsView из последнего курса (рис. 3) с выходными данными, создаваемыми FormView в этом руководстве (рис. 4).


[![Выводимые данные DetailsView](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Рис. 3**: выводимые данные DetailsView ([Просмотр полноразмерного изображения](using-the-formview-s-templates-cs/_static/image9.png))


[![Гибкие выводимые данные FormView](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Рис. 4**: жидкостей выводимые данные FormView ([Просмотр полноразмерного изображения](using-the-formview-s-templates-cs/_static/image12.png))


## <a name="summary"></a>Сводка

Хотя элементы управления GridView и DetailsView можно настроить использование полей TemplateField выходные данные, и по-прежнему представить свои данные в формате сетки, угловат. В тех случаях, когда необходимо отобразить одну запись с помощью более гибкую компоновку, FormView является идеальным выбором. Как и элемент DetailsView, FormView отображает одну запись из его `DataSource`, но в отличие от элемента управления DetailsView состоит только из шаблонов и не поддерживает поля.

Как мы видели в этом руководстве, FormView позволяет более гибкой компоновки при отображении единственной записи. В будущих учебных курсах мы изучим элементы управления DataList и Repeater, которые предоставляют тот же уровень гибкости в качестве FormsView, но могут отображать несколько записей (например GridView).

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного курса – е.р Gilmore. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](using-templatefields-in-the-detailsview-control-cs.md)
> [Вперед](displaying-summary-information-in-the-gridview-s-footer-cs.md)
