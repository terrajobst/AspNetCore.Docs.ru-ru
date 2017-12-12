---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: "Добавление столбца GridView переключателей (C#) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике рассматривается, как добавить столбец кнопок-переключателей в элементе управления GridView предоставить пользователю более интуитивно понятный способ выбора одной строки..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: ba6b163078bbab1bda302676e7e4c8a1d07f3c98
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>Добавление столбца GridView переключателей (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) или [скачать PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> Этот учебник рассказывается, как добавление столбца кнопок-переключателей в элементе управления GridView предоставить пользователю более интуитивно понятный способ выбора одной строки GridView.


## <a name="introduction"></a>Вступление

Элемент управления GridView предоставляет большое количество встроенных функциональных возможностей. Он включает несколько различных полей для отображения текста, изображения, гиперссылки и кнопки. Он поддерживает шаблоны для дальнейшей настройки. С помощью нескольких щелчков мышью она s сделать GridView, где можно выбрать каждую строку по нажатию кнопки или включить изменение или удаление возможностей невозможно. Несмотря на множество функций, предоставленного часто будет ситуаций, в которых дополнительных, неподдерживаемые функции будет необходимо добавить. В этом учебнике и двух следующих мы изучим, как расширить функциональные возможности s GridView, чтобы включить дополнительные функции.

Этот учебник и другая сосредоточиться на улучшение процесса выбора строк. Как проверить в [главный/Detail, с помощью выбираемого основного элемента GridView с DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), мы можем добавить CommandField GridView, который включает кнопку выбора. При нажатии выполняется обратная и GridView s `SelectedIndex` обновляется индекс строки, в которых выберите кнопка была нажата. В [главный/Detail, с помощью выбираемого основного элемента GridView с DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) учебника, мы узнали, как использовать эту функцию для отображения подробных сведений для выбранной строки GridView.

Кнопка выбора работает во многих ситуациях, она может не работать также для других пользователей. Вместо использования кнопки, два других элементах пользовательского интерфейса обычно используются для выбора: переключатель и флажок. Можно расширить GridView, чтобы вместо кнопка выбора, каждая строка содержит переключатель или флажок. В сценариях, где пользователь может выбрать только одну из записей GridView переключатель может находиться предпочтительный над кнопка выбора. В ситуациях, где пользователь может выбрать несколько записей, например, в приложении веб сервера электронной почты, где пользователь может потребоваться выбрать несколько сообщений, чтобы удалить флажок предоставляет функциональные возможности, не доступные с помощью кнопки "выбрать" или "переключатель" пользовательские интерфейсы.

Этот учебник рассказывается, как добавить столбец кнопок-переключателей в GridView. В учебнике продолжением изучает с помощью флажков.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Шаг 1: Создание улучшение GridView веб-страниц

Прежде чем начать, улучшение GridView, чтобы включить столбец кнопок-переключателей, позволяют s сначала занять некоторое время для создания страниц ASP.NET в данном проекте веб-сайта, он понадобится для этого учебника и двух следующих. Начните с добавления в новую папку с именем `EnhancedGridView`. Добавьте следующие страницы ASP.NET в этой папке, убедитесь, что связать каждую страницу с `Site.master` главной страницы:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Добавление страниц ASP.NET для руководств SqlDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Рис. 1**: Добавление страницы ASP.NET для руководств SqlDataSource


Как и в других папках, `Default.aspx` в `EnhancedGridView` папки будут перечислены учебники в своем разделе. Помните, что `SectionLevelTutorialListing.ascx` пользовательский элемент управления предоставляет следующие функциональные возможности. Таким образом, добавьте этот пользовательский элемент управления для `Default.aspx` , перетащив его из обозревателя решений на странице s представление конструктора.


[![Добавление Default.aspx SectionLevelTutorialListing.ascx пользовательского элемента управления](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**На рисунке 2**: добавление `SectionLevelTutorialListing.ascx` пользовательского элемента управления на `Default.aspx` ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


И, наконец, добавьте эти четыре страницы как операции для `Web.sitemap` файла. В частности, добавьте следующую разметку после с помощью элемента управления SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

После обновления `Web.sitemap`, занять некоторое время для просмотра учебников веб-сайт с помощью браузера. В левом меню теперь включает элементы для редактирования, вставки и удаления учебники.


![Карта сайта теперь включает операции для повышения учебники GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Рис. 3**: карты сайта теперь включает операции для улучшения учебники GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Шаг 2: Отображение поставщиков в элементе управления GridView

Для этого учебника позволяют строить GridView, s список поставщиков из США с каждой строкой GridView, предоставляя типа "переключатель". После выбора поставщика через переключатель, пользователь может просмотреть продукты s поставщика путем нажатия кнопки. Пока эта задача может показаться тривиальными, существует ряд оттенков, благодаря которым она особенно сложная. Прежде чем углубиться в этих тонкостей позволяют s сначала нужно получить список поставщиков GridView.

Сначала откройте `RadioButtonField.aspx` страницы в `EnhancedGridView` папки, перетащив элемент управления GridView с панели элементов в конструктор. Набор GridView s `ID` для `Suppliers` и выберите его смарт-тег создать новый источник данных. В частности, создать ObjectDataSource с именем `SuppliersDataSource` , его данные извлекаются из `SuppliersBLL` объекта.


[![Создать новый элемент управления ObjectDataSource SuppliersDataSource с именем](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Рис. 4**: создать новый именованный ObjectDataSource `SuppliersDataSource` ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![Настройка ObjectDataSource с помощью класса SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Рис. 5**: Настройка ObjectDataSource для использования `SuppliersBLL` класса ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


Поскольку нам требуется только список этих поставщиков из США, выберите `GetSuppliersByCountry(country)` метода из раскрывающегося списка на вкладке "SELECT".


[![Настройка ObjectDataSource с помощью класса SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Рис. 6**: Настройка ObjectDataSource для использования `SuppliersBLL` класса ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


На вкладке «обновление», выберите (нет) и нажмите кнопку Далее.


[![Настройка ObjectDataSource с помощью класса SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Рис. 7**: Настройка ObjectDataSource для использования `SuppliersBLL` класса ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


Поскольку `GetSuppliersByCountry(country)` метод принимает параметр, мастер настройки источника данных запрашивает источник этого параметра. Для указания жестко запрограммированные значения (в этом примере США) оставьте параметр источника раскрывающегося списка значение None и введите значение по умолчанию в текстовом поле. Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Использовать США в качестве значения по умолчанию для страны, параметр](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Рис. 8**: США используется как значение по умолчанию для `country` параметра ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


По завершении работы мастера GridView будет включать поле BoundField для каждого из полей данных поставщика. Удалите все, кроме `CompanyName`, `City`, и `Country` стояли и переименуйте `CompanyName` стояли `HeaderText` свойства поставщика. После этого GridView и ObjectDataSource должен выглядеть следующим образом.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

В этом учебнике позволяют s разрешить пользователю просмотр выбранного поставщика продукты, на ту же страницу списка поставщиков или на другую страницу. Чтобы решить эту проблему, добавьте две кнопки веб-элементов управления на страницу. Я хранить набор `ID` s из этих двух кнопок `ListProducts` и `SendToProducts`, с принципом что при `ListProducts` нажатии обратная передача будет выполняться и выбранного поставщика s продукты отображаются на одной странице, но когда `SendToProducts` нажатии пользователь будет перейти на другую страницу, которая перечисляет продукты.

Рис. 9 показана `Suppliers` GridView и два веб-кнопка управляет при просмотре через браузер.


[![Поставщики из США имеют свои имя, Город и материалами страны](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Рис. 9**: тех поставщиков из США имеют их имя города и страны в списке сведения ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Шаг 3: Добавление столбца кнопок-переключателей

На этом этапе `Suppliers` GridView имеет три стояли отображение название компании, города и страны всех поставщиков в США. Он по-прежнему хватает столбца кнопок-переключателей, однако. К сожалению t GridView включают встроенные RadioButtonField, в противном случае мы просто добавить, в сетке и выполняться. Вместо этого можно добавить TemplateField и настроить его `ItemTemplate` для подготовки к просмотру типа "переключатель", что приводит к типа "переключатель" для каждой строки в GridView.

Изначально может предположить, что нужного пользовательского интерфейса могут быть реализованы путем добавления веб-управления RadioButton для `ItemTemplate` из TemplateField. Хотя на самом деле это повысит отдельного переключателя к каждой строке GridView, переключатели не могут быть сгруппированы и поэтому не являются взаимоисключающими. Пользователь является возможность одновременный выбор нескольких переключателей GridView.

Несмотря на то, что использование TemplateField RadioButton веб-элементов управления не предлагает функциональность, нам нужно, s позволяют реализовать этот подход, как оно s, имеет смысл выяснить, почему полученный переключателей не группируются. Начните с добавления TemplateField к GridView поставщики, сделав его крайнего левого поля. Затем в смарт-теге s GridView, щелкните ссылку Изменить шаблоны и перетащите элемент управления RadioButton Web из области элементов в TemplateField s `ItemTemplate` (см. рис. 10). Набор RadioButton s `ID` свойства `RowSelector` и `GroupName` свойства `SuppliersGroup`.


[![Добавление элемента управления RadioButton Web ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Рис. 10**: Добавление веб-элемент управления RadioButton для `ItemTemplate` ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


После внесения этих изменений в конструкторе, разметку s GridView должен выглядеть примерно следующее:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

RadioButton s [ `GroupName` свойство](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) используется для группирования рядов кнопок-переключателей. Все элементы управления RadioButton с тем же `GroupName` считаются сгруппированных; из группы одновременно можно выбрать только один переключатель. `GroupName` Задает значение для отображаемой переключателя s `name` атрибута. Браузер проверяет переключателей `name` атрибуты для определения радио кнопку группирований.

С помощью элемента управления RadioButton Web добавлен `ItemTemplate`, посетите эту страницу через браузер и щелкните переключатели в строках таблицы s. Обратите внимание, как переключателей не сгруппированы, что позволяет выбрать все строки, как на рис. 11 показано.


[![Переключатели GridView s не группируются](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Рис. 11**: переключателей s GridView не группируются ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


Переключатели не сгруппированы, так как их готовый для просмотра `name` атрибуты различны, несмотря на то, имеющих одинаковый `GroupName` значение свойства. Чтобы просмотреть эти различия, выполните исходный из браузера и проверьте разметку кнопки переключателя.


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Обратите внимание как обе `name` и `id` атрибуты не точные значения, указанные в окне «Свойства», но предваряются символом количество других `ID` значения. Дополнительный `ID` значения, добавленные в начало отображаемого объекта `id` и `name` атрибуты являются `ID` s переключателя кнопок родительские элементы управления `GridViewRow` s `ID` s, GridView s `ID`, Элемент управления s содержимым `ID`и веб-формы s `ID`. Эти `ID` s добавляются выполнялась каждый веб-элемент управления GridView имеет уникальное `id` и `name` значения.

Всех преобразуемых другой элемент управления `name` и `id` так, как это как браузер уникально идентифицирует каждый элемент управления на стороне клиента и каким образом он идентифицирует веб-сервер какое действие или было изменено при обратной передаче. Например предположим, мы хотим выполнить код для сервера, каждый раз, когда RadioButton s проверить состояние было изменено. Это можно сделать, задав RadioButton s `AutoPostBack` свойства `true` и Создание обработчика событий для `CheckChanged` события. Однако если создаваемые `name` и `id` значения всех переключателей были одинаковыми, на обратную передачу нам не удалось определить, какие именно RadioButton, выбранного щелчком мыши.

Короткие он является, не удается создать столбец кнопок-переключателей в элементе управления GridView с помощью элемента управления RadioButton. Вместо этого необходимо использовать вместо устаревшей методы чтобы убедиться, что соответствующая разметка вставляется в каждой строке GridView.

> [!NOTE]
> Как веб-управления RadioButton, переключатель HTML-элемент управления, при добавлении в шаблон будет содержать уникальный `name` атрибута, что переключатели в сетке несгруппированные. Если вы не знакомы с HTML-элементы управления, вы можете пропустить эту заметку, как HTML-элементы управления используются редко, особенно в ASP.NET 2.0. Но если вы заинтересованы в дополнительной информацией, [к. Скотт Аллен](http://odetocode.com/blogs/scott/default.aspx) запись в блоге s [веб-элементов управления и элементов управления HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Использование литерала элемента управления для добавления переключателей кнопка разметки

Чтобы правильно сгруппировать все переключателей в GridView, необходимо вручную ввести разметки кнопок переключателей в `ItemTemplate`. Каждый переключатель должен же `name` атрибута, но должен иметь уникальный `id` атрибута (в случае мы хотим доступ типа "переключатель" посредством клиентского скрипта). После пользователь выбирает типа "переключатель" и в блогах резервного страницу, браузер будет возвращаемое значение выбранного переключателя s `value` атрибута. Таким образом, каждый переключатель потребуется уникальный `value` атрибута. Наконец, при обратной передаче нужно обязательно добавьте `checked` атрибут один переключатель, который выбран, в противном случае после пользователь делает выделение и обратно в блогах, переключателей вернет состояние по умолчанию (все невыбранные).

Существует два подхода, которые могут быть выполнены для внедрения низкоуровневые разметки в шаблон. Один заключается в осуществлении сочетание разметки и вызывает форматирование методы, определенные в классе кода. Этот метод сначала обсуждались в [с помощью TemplateFields в элементе управления GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) учебника. В нашем случае он может выглядеть примерно так:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

Здесь `GetUniqueRadioButton` и `GetRadioButtonValue` бы методы, определенные в классе кода, который возвращается соответствующий `id` и `value` значения для каждого переключателя атрибутов. Этот подход хорошо работает для назначения `id` и `value` атрибутов, но не хватает, при необходимости указания `checked` значение атрибута, так как синтаксис привязки данных выполняется только в том случае, когда данные сначала привязать к GridView. Таким образом, если GridView включено состояние просмотра, методы форматирования возникает только при первой загрузке страницы (или когда GridView явно привязывается к источнику данных) и поэтому функция, которая задает `checked` вызывать атрибут выиграл t обратной передачи. Он s вместо незначительные проблемы и немного выходит за рамки данной статьи, поэтому он сейчас. Я выполните, тем не менее, рекомендуем вам попробуйте использовать описанный подход и работать через точку, где вы будете возникли затруднения. Хотя такие t упражнения выиграл получить все ближе рабочую версию, он будет повысить более глубокое представление о GridView и жизненный цикл привязки данных.

Другой подход для добавления пользовательских низкоуровневые разметки в шаблон и подход, который будет использоваться для этого учебника является добавление [управления Literal](https://msdn.microsoft.com/en-us/library/sz4949ks(VS.80).aspx) в шаблон. Затем в GridView s `RowCreated` или `RowDataBound` обработчик событий текстовый элемент управления может осуществляться программными средствами и его `Text` значение свойства в разметку для выпуска.

Удалите элемент управления RadioButton из TemplateField s `ItemTemplate`, заменив текстовым элементом управления. Значение элемента управления для вывода s `ID` для `RadioButtonMarkup`.


[![Добавление элемента управления литерала ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Рис. 12**: Добавление литерала элемента управления для `ItemTemplate` ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


Создайте обработчик событий для GridView s `RowCreated` событий. `RowCreated` Событие возникает, когда для каждой добавляемой строки или нет привязываются к GridView. Это означает, что даже при обратной передаче при повторной загрузке данных из состояния представления `RowCreated` событие по-прежнему запускается, и вот почему мы используем его вместо `RowDataBound` (которого срабатывает, только когда данные явным образом связан с данным веб-элемент управления).

В этом обработчике событий нам нужен только если мы re задействования строку данных. Для каждой строки данных, мы хотим программно ссылаться `RadioButtonMarkup` текстовый элемент управления и задайте его `Text` свойства в разметку для выпуска. В следующем коде показано, создается разметка создает переключатель кнопку с заданным `name` атрибута задано значение `SuppliersGroup`, чей `id` атрибута задано значение `RowSelectorX`, где *X* — это индекс строки GridView и которого `value` атрибута задано значение индекс строки GridView.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

При выборе строки GridView и обратной передаче, мы будем рады получить `SupplierID` выбранного поставщика. Таким образом, может показаться, что значение каждого переключателя должно быть фактического `SupplierID` (а не индекс строки GridView). Хотя в некоторых случаях это может работать нормально, было бы угроза безопасности слепо принимать и обрабатывать `SupplierID`. Наш GridView, например, список только поставщики в США. Однако если `SupplierID` передается непосредственно из переключатель какие s, чтобы остановить вредоносные пользователя из обработки `SupplierID` значение отправляются обратно на обратную передачу? С помощью строки индекса в качестве `value`и получения `SupplierID` при обратной передаче из `DataKeys` коллекции, можно обеспечить, пользователь использует только один из `SupplierID` значения, связанные с одной из строк GridView.

После добавления этого кода обработчика событий, внимательно проверить страницы в браузере. Во-первых, обратите внимание, что только один переключатель одновременно можно выбрать кнопку в сетке. Тем не менее, когда при выборе типа "переключатель", а также выбрать одну из кнопок, обратная передача и переключателей все восстановить исходное состояние (то есть, при обратной передаче выбранного переключателя больше не выбран). Чтобы устранить эту проблему, необходимо расширить `RowCreated` обработчик событий, чтобы проверяет выбранного переключателя индекс кнопки, отправленные обратной передачи и добавляет `checked="checked"` атрибут порожденную разметки совпадений индекса строки.

При обратной передаче, браузер отправляет обратно `name` и `value` выбранного переключателя. Значение можно программно получить с помощью `Request.Form["name"]`. [ `Request.Form` Свойство](https://msdn.microsoft.com/en-us/library/system.web.httprequest.form.aspx) предоставляет [ `NameValueCollection` ](https://msdn.microsoft.com/en-us/library/system.collections.specialized.namevaluecollection.aspx) представляющий переменные формы. Переменные формы являются имена и значения полей формы на веб-странице и отправляются обратно в веб-обозревателя, каждый раз, когда выполняется обратная. Поскольку создаваемые `name` атрибут переключателей в GridView `SuppliersGroup`, когда веб-страница выполняет обратный браузер будет отправлять `SuppliersGroup=valueOfSelectedRadioButton` веб-сервере (а также другие поля формы). Эта информация может осуществляться из `Request.Form` свойства с помощью: `Request.Form["SuppliersGroup"]`.

Так как мы будем требуется для определения выбранного переключателя индекса не только в `RowCreated` обработчик событий, но в `Click` добавить обработчики событий для кнопки веб-элементов управления, позволяют s `SuppliersSelectedIndex` свойство классу кода, который возвращает `-1`Если был выбран переключатель "Нет" и выбранного индекса, если выбран один из переключателей.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Это свойство добавлено, мы знаем, чтобы добавить `checked="checked"` разметки в `RowCreated` обработчик событий при `SuppliersSelectedIndex` равняется `e.Row.RowIndex`. Обновите этот обработчик событий, включение этой логики:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

С этим изменением выбранного переключателя остается выбранным после обратной передачи. Теперь, когда у нас есть возможность указать, какие переключателя, мы может изменить поведение, если сначала была посещенные страницы, был выбран первый переключатель строк s GridView (а не having не переключателей, по умолчанию, который является текущим поведение). Чтобы для первого переключателя, по умолчанию, измените `if (SuppliersSelectedIndex == e.Row.RowIndex)` следующим образом: `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

На этом этапе мы добавили столбец групп переключателей к GridView, позволяющий для одной строки GridView выделить и запоминать. Наш следующие действия, чтобы отобразить продукты, предоставляемые выбранного поставщика. На шаге 4 будет рассмотрена перенаправлять пользователя на другую страницу, отправка вдоль выбранного `SupplierID`. В шаге 5 мы рассмотрим способы отображения продуктов s выбранного поставщика в GridView на одной странице.

> [!NOTE]
> Вместо использования TemplateField (фокус этой длительной шаг 3), можно создать настраиваемый `DataControlField` класс, который отображает соответствующий пользовательский интерфейс и функциональные возможности. [ `DataControlField` Класс](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.datacontrolfield.aspx) является базовым классом, от которого наследуют BoundField, CheckBoxField, TemplateField и другие встроенные поля GridView и DetailsView. Создание пользовательской `DataControlField` означает столбец кнопок-переключателей может быть добавлен только с помощью декларативного синтаксиса что приведет также к тому, предоставляющие функции на других веб-страниц и других веб-приложений значительно упрощает класса.


Если хранить создавали пользовательские, скомпилирован элементов управления ASP.NET, тем не менее, известно, что исключение требует большого объема подготовительная работа и несет с узла тонкостей и крайние случаи, которые должны обрабатываться тщательно. Таким образом, мы будет отказаться от реализации столбца кнопок-переключателей как строка настраиваемого `DataControlField` класса сейчас и пользуйтесь параметром TemplateField. Возможно у нас будет возможность изучить создание, использование и развертывание пользовательских `DataControlField` классов в будущем учебника!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Шаг 4: Отображение продуктов s выбранного поставщика на отдельной странице

После выбора строки GridView пользователю необходимо показать продукты выбранного поставщика. В некоторых случаях необходимо отобразить эти продукты на отдельной странице, в других мы, возможно, предпочтете делать на одной странице. Разрешить s сначала проверьте способ отображения продуктов в отдельную страницу; на шаге 5 будет рассмотрена добавить GridView к `RadioButtonField.aspx` для отображения выбранного поставщика s товаров.

В настоящее время имеется два веб-кнопка элементов управления на странице `ListProducts` и `SendToProducts`. Когда `SendToProducts` кнопки, мы хотим отправить пользователю `~/Filtering/ProductsForSupplierDetails.aspx`. Эта страница была создана в [иерархического фильтрации по две страницы](../masterdetail/master-detail-filtering-across-two-pages-cs.md) учебник и отображает продукты для поставщика, `SupplierID` передается через поля строки запроса с именем `SupplierID`.

Для поддержки этой функции, создайте обработчик событий для `SendToProducts` кнопку s `Click` событий. На шаге 3 мы добавили `SuppliersSelectedIndex` свойство, которое возвращает индекс строки, переключатель установлен. Соответствующий `SupplierID` могут быть получены из GridView s `DataKeys` коллекции и пользователь может затем отправляться `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` с помощью `Response.Redirect("url")`.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Этот код прекрасно работает, до тех пор, пока выбран один из переключателей из GridView. Если изначально GridView не поддерживает все переключатели выбран, и пользователь щелкает `SendToProducts` кнопки, `SuppliersSelectedIndex` будет `-1`, которое может вызвать исключение с момента `-1` выходит за пределы диапазона индексов `DataKeys`коллекции. Это не столь важно, тем не менее, если вы решили обновить `RowCreated` обработчик событий, как описано в шаге 3, таким образом, чтобы иметь для первого переключателя в GridView изначально установлен.

Для размещения `SuppliersSelectedIndex` значение `-1`, добавить веб-управления Label на страницу над элементом управления GridView. Задайте его `ID` свойства `ChooseSupplierMsg`, ее `CssClass` свойства `Warning`, ее `EnableViewState` и `Visible` свойства `false`и его `Text` свойство до сначала выберите поставщика из сетки. Класс CSS `Warning` текст отображается в красный, курсив, полужирный, больших шрифта и определяется в `Styles.css`. Задав `EnableViewState` и `Visible` свойства `false`, метка не отображается за исключением того, для тех обратные передачи where управления s `Visible` программно свойству `true`.


[![Добавить веб-элемент управления Label над элементом управления GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Рис. 13**: добавить метку Web управления выше GridView ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


Затем дополнить `Click` обработчик событий для отображения `ChooseSupplierMsg` меток, если `SuppliersSelectedIndex` — меньше нуля и перенаправлять пользователя для `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` в противном случае.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Перейдите на страницу в браузере и щелкните `SendToProducts` кнопку перед выбором поставщиков из GridView. Как показано на рис. 14, отобразится `ChooseSupplierMsg` метки. Затем выберите поставщика и нажмите кнопку `SendToProducts` кнопки. Это будет whisk страница, на которой перечислены продукты, предоставляемые выбранного поставщика. На рисунке 15 показано `ProductsForSupplierDetails.aspx` странице был выбран поставщик Bigfoot Breweries.


[![Метка ChooseSupplierMsg отображается в том случае, если поставщик не установлен](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Рис. 14**: `ChooseSupplierMsg` метка отображается в том случае, если поставщик не установлен ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![В ProductsForSupplierDetails.aspx отображаются продукты s выбранного поставщика](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Рис. 15**: выбран поставщик продукты отображаются в `ProductsForSupplierDetails.aspx` ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Шаг 5: Отображение продуктов s выбранного поставщика на одной странице

На шаге 4 мы узнали, как отправить пользователю продукты другую веб-страницу для отображения выбранного поставщика. Кроме того продукты s выбранного поставщика могут отображаться на одной странице. Чтобы проиллюстрировать это, мы добавим другой GridView для `RadioButtonField.aspx` для отображения выбранного поставщика s товаров.

Поскольку нам нужен только этот GridView продуктов для отображения после выбора других поставщиков, добавьте веб-панели управления под `Suppliers` GridView, установка его `ID` для `ProductsBySupplierPanel` и его `Visible` свойства `false`. На панели, добавьте текст продуктов для выбранного поставщика, за которым следует GridView с именем `ProductsBySupplier`. Выберите смарт-тег s GridView привязать его к новой ObjectDataSource с именем `ProductsBySupplierDataSource`.


[![Привязка ProductsBySupplier GridView к новый элемент управления ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**На рисунке 16**: привязка `ProductsBySupplier` GridView, чтобы новый элемент управления ObjectDataSource ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


Настройте элемент управления ObjectDataSource для использования `ProductsBLL` класса. Поскольку нам нужен только для получения этих продуктов, предоставляемые выбранного поставщика, указать, следует вызвать элемент управления ObjectDataSource `GetProductsBySupplierID(supplierID)` метод для извлечения данных. Выделите (нет) из раскрывающихся списков в UPDATE, INSERT и DELETE вкладок.


[![Настройка ObjectDataSource можно использовать метод GetProductsBySupplierID(supplierID)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Рисунок 17**: Настройка ObjectDataSource для использования `GetProductsBySupplierID(supplierID)` метод ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![Установите раскрывающиеся списки (нет) в UPDATE, INSERT и удаление вкладок](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**На рисунке 18**: задать раскрывающихся списков (нет) в обновления, вставки и удаления вкладок ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


После настройки SELECT, вставка и удаление вкладок, нажмите кнопку Далее. Поскольку `GetProductsBySupplierID(supplierID)` метода требуется входной параметр, мастер создания источников данных предлагает нам нужно указать источник для значения параметра s.

У нас есть две возможности здесь в Указание источника значения параметра s. Мы могли бы использовать объект параметра по умолчанию и программным путем присвоения значения `SuppliersSelectedIndex` свойства параметр s `DefaultValue` свойства в элемент управления ObjectDataSource s `Selecting` обработчика событий. Обращаться к [программно заданию значений параметров ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) учебника в памяти программным путем присвоения значений параметров ObjectDataSource s.

Кроме того, можно использовать ControlParameter и ссылаться на `Suppliers` GridView s [ `SelectedValue` свойство](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (см. на рисунке 19). GridView s `SelectedValue` возвращает `DataKey` значение, соответствующее [ `SelectedIndex` свойства](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Чтобы этот параметр действовал, необходимо программно задать GridView s `SelectedIndex` свойства выбранной строки, если `ListProducts` кнопки. Дополнительным преимуществом, установив `SelectedIndex`, будут присутствовать выбранную запись `SelectedRowStyle` определенные в `DataWebControls` темы (желтый фон).


[![Позволяет указать GridView s SelectedValue как источник параметра ControlParameter](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**На рисунке 19**: позволяет указать GridView s SelectedValue как источник параметра ControlParameter ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


После завершения работы мастера, Visual Studio автоматически добавит поля для поля данных s продукта. Удалите все, кроме `ProductName`, `CategoryName`, и `UnitPrice` стояли и измените `HeaderText` свойства с продуктом, категории и цены. Настройка `UnitPrice` BoundField, чтобы его значение форматируются как денежное значение. После внесения этих изменений, панели, GridView и ObjectDataSource s декларативная разметка должна выглядеть следующим образом:


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Для этого упражнения, нам нужно указать GridView s `SelectedIndex` свойства `SelectedSuppliersIndex` и `ProductsBySupplierPanel` панель s `Visible` свойства `true` при `ListProducts` кнопки. Чтобы сделать это, создайте обработчик событий для `ListProducts` веб-управления Button s `Click` событий и добавьте следующий код:


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Если поставщик не была выбрана из GridView, `ChooseSupplierMsg` метка будет отображаться и `ProductsBySupplierPanel` панели скрыты. В противном случае, если был выбран поставщик `ProductsBySupplierPanel` отображается и GridView s `SelectedIndex` свойство обновляется.

Рис. 20 показаны результаты после был выбран поставщик Bigfoot Breweries и Показать продуктов на кнопки страницы был выполнен щелчок.


[![Продукты предоставляемые Bigfoot Breweries отображаются на одной странице](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Рис. 20**: продуктов предоставляемые Bigfoot Breweries отображаются на одной странице ([Просмотр полноразмерное изображение](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>Сводка

Как было сказано в [главный/Detail, с помощью выбираемого основного элемента GridView с DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) руководства записей, которые могут быть выбраны из элемента управления GridView с использованием CommandField которого `ShowSelectButton` свойству `true`. Но CommandField отображает ее кнопок регулярного кнопок, ссылки или изображения. Пользовательский интерфейс выбора альтернативных строк — предоставить переключатель или флажок в каждой строке GridView. В этом учебнике мы рассмотрели, как добавить столбец кнопок-переключателей.

К сожалению как можно предположить, что добавление столбца относящийся к t кнопки переключатель как простой или простой. Отсутствует нет встроенных RadioButtonField, которые могут быть добавлены при нажатии кнопки, и использование веб-управления RadioButton в TemplateField создает свой собственный набор проблем. В конечном итоге, для обеспечения такой интерфейс мы либо придется создать настраиваемый `DataControlField` класса и не прибегая к вводится соответствующего HTML в TemplateField во время `RowCreated` событий.

Изучена как добавить столбец кнопок-переключателей, сообщите нам можем обратиться к Добавление столбца флажков. Со столбцом флажки пользователь может выбрать один или несколько строк GridView и затем выполнить некую операцию на всех выбранных строк (например, выбрать набор сообщений электронной почты из веб сервера электронной почты клиента, а затем выберите Удалить все выбранные сообщения электронной почты). В следующем уроке мы рассмотрим способы добавления такого столбца.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было David Suru. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Вперед](adding-a-gridview-column-of-checkboxes-cs.md)
