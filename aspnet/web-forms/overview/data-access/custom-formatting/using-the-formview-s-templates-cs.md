---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: Используя шаблоны FormView (C#) | Документы Microsoft
author: rick-anderson
description: В отличие от DetailsView FormView не состоит из полей. Вместо этого FormView подготавливается к просмотру с помощью шаблонов. В этом учебнике мы рассмотрим е. с помощью...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 0e1b36f0bfc244e39bb620c1c066b3e2403722cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875829"
---
<a name="using-the-formviews-templates-c"></a>Используя шаблоны FormView (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) или [скачать PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> В отличие от DetailsView FormView не состоит из полей. Вместо этого FormView подготавливается к просмотру с помощью шаблонов. В данном руководстве, мы изучим с помощью элемента управления FormView для представления менее строгое представление данных.


## <a name="introduction"></a>Вступление

В двух последних учебниках мы узнали, как для настройки выходов GridView и DetailsView элементов управления при помощи TemplateFields. Разрешить TemplateFields к содержимому для конкретного поля высокой настраивать, но в конечном итоге GridView и DetailsView имеют вид вместо boxy, вид сетки. Во многих сценариях макета сетки таких идеально, но в некоторых случаях требуется более гибкий, менее строгое отображения. При отображении одной записи, возможно с помощью элемента управления FormView гибкий макет.

В отличие от DetailsView FormView не состоит из полей. Не удается добавить BoundField и TemplateField в FormView. Вместо этого FormView подготавливается к просмотру с помощью шаблонов. Рассматривать как элемент управления DetailsView, содержащий один TemplateField FormView. FormView поддерживает следующие шаблоны:

- `ItemTemplate` используется для отрисовки конкретной записи, отображаемых в FormView
- `HeaderTemplate` используется для указания строки необязательный заголовок
- `FooterTemplate` используется для указания строка необязательно нижнего колонтитула
- `EmptyDataTemplate` Когда FormView `DataSource` отсутствуют какие-либо записи `EmptyDataTemplate` используется вместо `ItemTemplate` для визуализации разметки для элемента управления
- `PagerTemplate` можно использовать для настройки интерфейса постраничного просмотра для FormViews, имеющих включено разбиение на страницы
- `EditItemTemplate` / `InsertItemTemplate` позволяет настраивать интерфейс редактирования или интерфейс для вставки для FormViews, который поддерживает такие функциональные возможности

В данном руководстве, мы изучим с помощью элемента управления FormView для представления менее строгое отображения продуктов. Вместо того чтобы использовать поля имени, категории, поставщика и т. д., FormView `ItemTemplate` отобразит эти значения, используя сочетание элемент заголовка и `<table>` (см. рис. 1).


[![FormView разрывов вид сетки макета в DetailsView](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Рис. 1**: FormView разбивает Grid-Like макета, видны в DetailsView ([Просмотр полноразмерное изображение](using-the-formview-s-templates-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Шаг 1: Привязка данных к FormView

Откройте `FormView.aspx` страницы и перетащите FormView из области элементов в конструктор. При первом добавлении FormView он отображается как серый прямоугольник, предписывая нам, `ItemTemplate` необходим.


[![FormView невозможно отобразить в конструкторе, пока не предоставлены ItemTemplate](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**На рисунке 2**: FormView невозможно отобразить в конструкторе до `ItemTemplate` предоставляется ([Просмотр полноразмерное изображение](using-the-formview-s-templates-cs/_static/image6.png))


`ItemTemplate` Могут быть созданы вручную (с помощью декларативного синтаксиса) или может быть создан автоматически путем привязки к элементу управления источником данных с помощью конструктора FormView. Это автоматически созданной `ItemTemplate` содержит код HTML, что списки имя каждого поля и метки управления которого `Text` свойство привязано к значению поля. Такой подход также auto создает `InsertItemTemplate` и `EditItemTemplate`, которые заполняются элементы управления вводом для каждого поля данных, возвращаемых элементом управления источника данных.

Если вы хотите автоматически создать шаблон, с помощью смарт-тега FormView добавить новый элемент управления ObjectDataSource, который вызывает `ProductsBLL` класса `GetProducts()` метод. Это создаст FormView с `ItemTemplate`, `InsertItemTemplate`, и `EditItemTemplate`. Удалите из представления источника, `InsertItemTemplate` и `EditItemTemplate` поддерживает, так как мы не интересует создание элемента FormView, изменяемых или вставляемых еще. Далее очистить разметка в рамках `ItemTemplate` таким образом, что мы для работы с нуля.

Если бы вместо надстраивать `ItemTemplate` вручную, можно добавить и настроить элемент управления ObjectDataSource, перетащив его из области элементов в конструктор. Тем не менее не устанавливается FormView источника данных из конструктора. Вместо этого перейдите в представление источника и вручную задать FormView `DataSourceID` свойства `ID` значение ObjectDataSource. Вручную добавьте `ItemTemplate`.

Независимо от того, какой подход решил воспользоваться, на этом этапе в FormView декларативная разметка должна выглядеть следующим образом:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

Поставьте флажок Включить постраничный просмотр в FormView смарт-тегов; При этом будет добавлено `AllowPaging="True"` атрибут декларативный синтаксис FormView. Кроме того, задать `EnableViewState` свойству значение False.

## <a name="step-2-defining-theitemtemplates-markup"></a>Шаг 2: Определение`ItemTemplate`элемента разметки

FormView привязку к элементу управления ObjectDataSource и настроен для поддержки разбиения на страницы мы готовы укажите содержимое `ItemTemplate`. В этом учебнике будет имя продукта, отображаемое в `<h3>` заголовок. После этого воспользуемся HTML `<table>` для отображения оставшихся свойства продукта в таблице четыре столбца, где первый и третий столбцы список имен свойств, а вторая и четвертая перечислить их значения.

Эта разметка может быть введено в через интерфейс редактирования шаблона FormView в конструкторе или введены вручную посредством декларативного синтаксиса. При работе с шаблонами обычно найти его проще работать непосредственно с декларативного синтаксиса, но вы можете использовать ту же методику удобно наиболее с.

В следующем примере показана декларативная разметка FormView после `ItemTemplate`его завершения структуры:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Обратите внимание, что синтаксис привязки данных — `<%# Eval("ProductName") %>`для примера могут быть добавлены непосредственно в выходные данные шаблона. То есть их требуется не назначить элемент управления Label `Text` свойство. Например, у нас есть `ProductName` значению, отображаемому в `<h3>` элемента с помощью `<h3><%# Eval("ProductName") %></h3>`, что для продукта Chai будут отображаться как `<h3>Chai</h3>`.

`ProductPropertyLabel` И `ProductPropertyValue` классы CSS, используются для указания стиль продукта имена и значения свойств в `<table>`. Эти классы CSS определены в `Styles.css` и привести имена свойств полужирный и выравниванием по правому краю, добавьте право заполнение значений свойств.

Так как используются не CheckBoxFields с FormView, чтобы показать `Discontinued` значение как флажок необходимо добавить собственный элемент управления CheckBox. `Enabled` Задано значение False, сделав его только для чтения и этот флажок `Checked` свойство привязано к значению `Discontinued` поля данных.

С `ItemTemplate` завершения продукта информация отображается в виде гораздо более гибкая. Сравните выходные данные DetailsView из последнего курса (рис. 3) с выходными данными, созданные в FormView в этом учебнике (рис. 4).


[![Жесткая вывода DetailsView](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Рис. 3**: жесткая DetailsView Output ([Просмотр полноразмерное изображение](using-the-formview-s-templates-cs/_static/image9.png))


[![Выходные данные жидкости FormView](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Рис. 4**: FormView Output жидкости ([Просмотр полноразмерное изображение](using-the-formview-s-templates-cs/_static/image12.png))


## <a name="summary"></a>Сводка

Хотя элементы управления GridView и DetailsView могут быть настроены при помощи TemplateFields выходные данные, и по-прежнему представить свои данные в формате вид сетки, boxy. В тех случаях, когда должно быть показано одной записи с использованием менее строгое макета, FormView является идеальным выбором. Как и DetailsView FormView отображает одну запись из его `DataSource`, но в отличие от DetailsView состоит только из шаблонов и не поддерживает полей.

Как было показано в этом учебнике, FormView обеспечивает более гибкий макет при отображении одной записи. В будущих учебниках мы изучим элементов управления DataList и повторителя, которые обеспечивают тот же уровень гибкости в качестве FormsView, однако, могут отображать несколько записей (такие как GridView).

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было E.R. Gilmore. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](using-templatefields-in-the-detailsview-control-cs.md)
> [Вперед](displaying-summary-information-in-the-gridview-s-footer-cs.md)
