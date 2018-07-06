---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Отображение данных с помощью элементов управления DataList и Repeater (C#) | Документация Майкрософт
author: rick-anderson
description: В предшествующих учебных курсах мы использовали элемент управления GridView для отображения данных. Начиная с этим руководством, мы рассматриваем распространенные шаблоны отчетов с...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 6368580d43e6216ea75d3013fb13aa59d9eeb6ba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805950"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Отображение данных с помощью элементов управления DataList и Repeater (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) или [скачать PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> В предшествующих учебных курсах мы использовали элемент управления GridView для отображения данных. Начиная с этим руководством, мы рассматриваем распространенные шаблоны отчетов с элементами управления DataList и Repeater, начиная с основ отображения данных с помощью этих элементов управления.


## <a name="introduction"></a>Вступление

Во всех примерах на протяжении предыдущих 28 учебных курсов, если нужно отобразить несколько записей из источника данных, мы обращались к элементу управления GridView. GridView отображает по строке для каждой записи в источнике данных, отображения поля данных записи s в столбцах. Хотя GridView можно быстро установить для отображение, постраничный просмотр сортировка, редактирование и удаление данных, его внешний вид угловат. Кроме того, разметка ответственность для она имеет фиксированную структуру s GridView включает HTML `<table>` со строкой таблицы (`<tr>`) для каждой записи и ячейки таблицы (`<td>`) для каждого поля.

Чтобы обеспечить большую степень настройки внешнего вида и визуализированной разметке при отображении нескольких записей, ASP.NET 2.0 предлагаются элементы управления DataList и Repeater (которые также были доступны в ASP.NET версии 1.x). Элементы управления DataList и Repeater визуализации их содержимого, с помощью шаблонов, а не полей BoundField CheckBoxFields, полей Buttonfield и так далее. Как и GridView, элемент управления DataList визуализируется как таблица HTML `<table>`, но позволяет несколько записей источника для отображения на каждую строку таблицы. Элементу управления Repeater, с другой стороны, не визуализирует иной разметки чем что явным образом, после чего хорошо подходят в том случае, когда нужен точный контроль над создаваемой разметкой.

Над руководствах следующего десятка, или так мы рассмотрим создание распространенные шаблоны отчетов с элементами управления DataList и Repeater, начиная с основ отображения данных с помощью этих шаблонов элементов управления. Узнаете, как форматировать эти элементы управления, как изменять компоновку записей источника данных в DataList, распространенные сценарии основной/подробности, способов изменения и удаления данных, как постраничного просмотра и т. д.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Шаг 1: Добавление элементов управления DataList и Repeater учебника веб-страниц

Прежде чем приступать к этом руководстве, позволяют s сначала Отвлекитесь и добавим страницы ASP.NET, которые потребуются для изучения этого учебника и нескольких последующих учебных курсов дело отображение данных с помощью элементов управления DataList и Repeater. Начнем с создания новой папки в проект с именем `DataListRepeaterBasics`. Добавьте следующие пять страниц ASP.NET в этой папке, в которых настроено использование главной страницы `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Создание папки DataListRepeaterBasics и добавление страниц учебника по ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Рис. 1**: создание `DataListRepeaterBasics` папки и добавление страниц учебника по ASP.NET


Откройте `Default.aspx` страницы и перетащите `SectionLevelTutorialListing.ascx` пользовательского элемента управления с `UserControls` папку в область конструктора. Данный пользовательский элемент управления, созданный в учебном курсе [главные страницы и структуру переходов узла](../introduction/master-pages-and-site-navigation-cs.md) , просматривает карту узла и отображает руководства из текущего раздела в виде маркированного списка.


[![Добавление элемента управления Sectionleveltutoriallisting.ascx к странице Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Рис. 2**: добавление `SectionLevelTutorialListing.ascx` для пользовательского элемента управления `Default.aspx` ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


Чтобы маркированный список отображал элементов управления DataList и Repeater учебники, которые будут созданы, необходимо добавить их в карту узла. Откройте `Web.sitemap` файл и добавьте следующую разметку после разметки узла карты сайта Добавление настраиваемых кнопок:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Обновление карты узла для добавления новых страниц ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Рис. 3**: обновите карту узла, чтобы включить новые страницы ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Шаг 2: Отображение информации о продукте в элементе управления DataList

Подобно FormView, s, выводимые данные элемента управления DataList зависит от шаблонов вместо полей BoundField, CheckBoxFields и т. д. В отличие от FormView DataList предназначена для отображения набора записей, а не одиночной. Позвольте s работы с этим руководством с рассмотрения привязки информации о продукте, чтобы элемент управления DataList. Сначала откройте `Basics.aspx` странице в `DataListRepeaterBasics` папку. Затем перетащите элемент управления DataList из инструментария в конструктор. Как показано на рис. 4, перед указанием шаблонов элементов управления DataList s, он отображается в конструкторе как серый квадрат.


[![Перетащите элемент DataList из инструментария в конструктор](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Рис. 4**: перетащите элемент управления DataList из панели элементов на конструктор ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


Из элемента управления DataList s смарт-теге, добавьте новый элемент управления ObjectDataSource и настроить его для использования `ProductsBLL` класс s `GetProducts` метод. Так как мы повторно создание элементов управления DataList только для чтения в этом руководстве установлено стрелку раскрывающегося списка (нет) в s мастер вставки, обновления и удаления вкладок.


[![Создадим новый элемент управления ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Рис. 5**: решили создать новый элемент управления ObjectDataSource ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Настройка ObjectDataSource на использование класса ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Рис. 6**: Настройка ObjectDataSource для использования `ProductsBLL` класс ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Получение информации обо всех продуктах с помощью метода GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Рис. 7**: получить сведения о всех продуктах с помощью `GetProducts` метод ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


После настройки ObjectDataSource и привязать его к DataList через его смарт-тег, Visual Studio автоматически создает `ItemTemplate` в элементе управления DataList, отображающий имя и значение каждого поля данных, возвращаемых источником данных (см. в разделе Разметка ниже). Это значение по умолчанию `ItemTemplate` вида идентична шаблонов, автоматически созданных при привязке источника данных к элементу FormView через конструктор.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Помните, что при привязке источника данных к элементу управления FormView через смарт-тега FormView s, Visual Studio будет создан `ItemTemplate`, `InsertItemTemplate`, и `EditItemTemplate`. С помощью элемента управления DataList, однако только `ItemTemplate` создается. Это обусловлено DataList не имеет встроенной поддержки, предлагаемой FormView вставки и правки. Элемент управления DataList содержать события, связанные с edit и delete и поддержки правки и удаления можно добавить немного кода, но здесь s не простую поддержку out of box, как с помощью FormView. Узнаете, как для включения поддержки правки и удаления с помощью элемента управления DataList в дальнейших учебных курсах.


Позвольте s Отвлекитесь и улучшить внешний вид этого шаблона. Вместо отображения всех полей данных, позволяют отображать только название продукта s, поставщику, категории, количество за единицу и цена за единицу s. Кроме того, позволяют s отобразим имя в `<h4>` и скомпонуем оставшиеся поля, используя `<table>` под заголовком.

Для внесения этих изменений, которые вы можете либо использовать функции в конструкторе на основе смарт-теге щелкните ссылку Изменить шаблоны или изменить шаблон напрямую через декларативный синтаксис страницы s s DataList редактирования шаблона. При использовании параметра Правка шаблонов в конструкторе Получившаяся разметка может не полностью совпадают следующую разметку, но при просмотре через обозреватель покажется очень похожа на снимок экрана, показанный на рис. 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> В примере выше используется метка, для свойств `Text` свойству присваивается значение синтаксиса привязки данных. Кроме того можно было опустить метки полностью, введя в только что синтаксис привязки данных. То есть вместо использования `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` можно было бы вместо этого использовать декларативный синтаксис `<%# Eval("CategoryName") %>`.


Тем не менее, оставляя в элементах управления Label Web, предлагают два преимущества. Во-первых он предоставляет более простые средства для форматирования данных, на основе данных, как мы увидим в следующем учебном курсе. Во-вторых параметр Правка шаблонов в конструкторе t отображения декларативный синтаксис привязки данных, которая отображается за пределами некоторые веб-элемента управления. Вместо этого интерфейса Правка шаблонов предназначен для упрощения работы со статической разметкой и веб-элементы управления и предполагается, что все привязки данных будет производиться через диалоговое окно Edit DataBindings, доступное из смарт-тегов элементов управления Web.

Таким образом при работе с DataList, которая предоставляет возможность правки шаблонов через конструктор, я предпочитаю использовать элементы управления Label Web, чтобы содержимое было доступно через интерфейс редактирования шаблонов. Как скоро можно будет увидеть, элементу управления Repeater требует, что содержимое шаблона s редактироваться в представлении источника. Следовательно при берет на себя создание шаблонов s по элементу управления Repeater, я часто не будем Label Web элементов управления, если я знаю, что необходимо форматировать внешний вид данных привязан текста на основе программной логики.


[![Каждый продукт s выходные данные — к просмотру с помощью s ItemTemplate элемента управления DataList](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Рис. 8**: s Each продукта выходные данные — к просмотру с помощью элементов управления DataList s `ItemTemplate` ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Шаг 3: Улучшение внешнего вида элемента управления DataList

Например GridView, DataList имеют ряд свойств, относящихся к стилю, таких как `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`, и т. д. При работе с элементами управления GridView и DetailsView, мы создавали файлы обложек в `DataWebControls` темы, который предварительно определен `CssClass` свойства для этих двух элементов управления и `CssClass` для нескольких из их подсвойств (`RowStyle` `HeaderStyle`, и так далее). Позвольте s сделайте то же самое для элемента управления DataList.

Как уже говорилось в [отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) учебника, файл обложки указывает связанные свойства по умолчанию веб-элемент управления; тема является коллекция файлы обложки, CSS, изображения и JavaScript, которые определяют определенный внешний вид и поведение веб-сайта. В *отображение данных с ObjectDataSource* мы создали `DataWebControls` темы (который реализуется как папка внутри `App_Themes` папку), имеет, в настоящее время два файла обложки - `GridView.skin` и `DetailsView.skin`. Позвольте s добавить третий файл обложки для указания параметров заранее определенный стиль для элемента управления DataList.

Чтобы добавить файл обложки, щелкните правой кнопкой мыши `App_Themes/DataWebControls` папку, выберите команду Добавить новый элемент и выберите файл обложки из списка. Назовите файл `DataList.skin`.


[![Создание нового файла обложки, именуемого DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Рис. 9**: Создание нового файла обложки с именем `DataList.skin` ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Использовать следующую разметку для `DataList.skin` файла:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Эти параметры назначьте те же классы CSS к соответствующим свойствам элемента управления DataList, как использовались с элементами управления GridView и DetailsView. Классы CSS, используемый здесь `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`, и так далее определяются в `Styles.css` файл и были добавлены в предыдущих учебных курсах.

С добавлением этого файла обложки внешний вид элемента управления DataList обновляется в конструкторе (может потребоваться обновить представление конструктора, чтобы увидеть воздействие нового файла обложки; из меню "Вид", выберите "Обновить"). Как показано на рис. 10, каждого второго продукта имеет Светло-розовый фон цвет.


[![Создание нового файла обложки, именуемого DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Рис. 10**: Создание нового файла обложки с именем `DataList.skin` ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Шаг 4: Изучение элементов управления DataList s других шаблонов

В дополнение к `ItemTemplate`, DataList поддерживает шесть других необязательных шаблонов:

- `HeaderTemplate` Если предоставляется, добавляет строку заголовка в выходные данные и используется для визуализации этой строки
- `AlternatingItemTemplate` используется для визуализации переменных элементов
- `SelectedItemTemplate` используется для визуализации выбранного элемента; Выбранный элемент является элементом, чей индекс соответствует относящемуся к DataList s [ `SelectedIndex` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` использовать для отображения редактируемого элемента
- `SeparatorTemplate` Если предоставляется, служит разделителем между каждым элементом и используется для визуализации этого разделителя
- `FooterTemplate` — Если предоставляется, добавляет строку нижнего колонтитула в выходные данные и используется для визуализации этой строки

При указании `HeaderTemplate` или `FooterTemplate`, DataList добавляет дополнительную строку верхнего или нижнего колонтитула выводимых данных. Как и с GridView s колонтитулы строк, заголовка и нижнего колонтитула в элементе управления DataList не привязанных к данным. Таким образом, любой синтаксис привязки данных в `HeaderTemplate` или `FooterTemplate` , попытки доступа к привязанных данных вернет пустую строку.

> [!NOTE]
> Как мы видели в [отображение сведения о в нижнем колонтитуле GridView s](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) учебник, хотя строки верхнего и нижнего колонтитулов кое синтаксис привязки данных для поддержки t, сведения, относящиеся к данным можно внедрить непосредственно в эти строки из GridView s `RowDataBound` обработчик событий. Этот метод можно использовать для вычисления итогов или другие сведения из данных привязанных к элементу управления, а также назначить эту информацию в нижнем колонтитуле. То же самое может применяться к элементам управления DataList и Repeater; Единственное отличие заключается в, для элементов управления DataList и Repeater создать обработчик событий для `ItemDataBound` событий (а не для `RowDataBound` событий).


В нашем примере позволяют s имеют заголовок отображается в верхней части элемента управления DataList s приводит сведения о продукте `<h3>` заголовок. Для этого добавьте `HeaderTemplate` соответствующая разметка. В конструкторе, это можно сделать, щелкнув ссылку Изменить шаблоны в смарт-теге элемента управления DataList s, выбрав шаблон заголовка из раскрывающегося списка и введя текст, когда вы выберете параметр заголовок 3 из раскрывающегося списка стиля списка (см. рис. 11).


[![Добавление шаблона HeaderTemplate с сведения о продукте текста](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Рис. 11**: добавление `HeaderTemplate` с сведения о продукте текст ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


Кроме того, можно добавить декларативно, введя следующую разметку внутри тегов `<asp:DataList>` теги:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Чтобы добавить свободное пространство между каждым списком продуктов, позволяют добавить s `SeparatorTemplate` , включающий линию между каждым разделом. Тег горизонтальной чертой (`<hr>`), добавляет такой разделитель. Создание `SeparatorTemplate` таким образом, чтобы он имеет следующую разметку:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Как и `HeaderTemplate` и `FooterTemplates`, `SeparatorTemplate` не привязан к записям из источника данных и таким образом не может иметь непосредственного доступа к записи, привязанные к элементу управления DataList источника данных.


После этого добавления при просмотре страницы в обозревателе это должно выглядеть на рис. 12. Обратите внимание на то, строку заголовка и линейку между каждым списком продуктов.


[![В элементе DataList имеются строка заголовка и горизонтальные линии между перечислениями продуктов](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Рис. 12**: В элементе DataList имеются строка заголовка и горизонтальные правило между Each списка продуктов ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Шаг 5: Визуализация конкретной разметки с элементом управления Repeater

В этом случае просмотр исходного кода из браузера при просмотре в DataList примере на рис. 12, вы увидите, что элемент управления DataList создает таблицу HTML `<table>` , содержащий строку таблицы (`<tr>`) с одной ячейки таблицы (`<td>`) для каждого элемента, привязанного к Элемент управления dataList. Эти выходные данные, на самом деле, идентична что создавались бы элемента GridView с одной TemplateField. Как мы увидим в дальнейших учебных курсах, элемент управления DataList допускает дальнейшую настройку выходных данных, благодаря чему мы можем отображать несколько записей источника данных на каждую строку таблицы.

Что делать, если нигде не требуется t таблицы HTML `<table>`, хотя? Для полного и абсолютного контроля над разметкой, создаваемой веб-элемент данных необходимо использовать элемент управления Repeater. Как и элемент управления DataList Repeater конструируется на основе шаблонов. Тем не менее, элементу управления Repeater, предлагает только следующие пять шаблонов:

- `HeaderTemplate` Если предоставлен, добавляет указанную разметку перед элементами
- `ItemTemplate` используется для визуализации элементов
- `AlternatingItemTemplate` Если указано, используется для визуализации переменных элементов
- `SeparatorTemplate` Если предоставлен, добавляет указанную разметку между элементами
- `FooterTemplate` — Если предоставлен, добавляет указанную разметку после элементов

В ASP.NET 1.x, элементу управления Repeater, часто использовался для отображения маркированного списка, данные которого происходили из какого-либо источника данных. В таком случае `HeaderTemplate` и `FooterTemplates` будет содержать открывающим и закрывающим `<ul>` теги, соответственно, тогда как `ItemTemplate` будет содержать `<li>` элементов при помощи синтаксиса привязки данных. Такой подход по-прежнему может использоваться в среде ASP.NET 2.0, как мы видели в двух примерах [главные страницы и переходы по узлу](../introduction/master-pages-and-site-navigation-cs.md) руководства:

- В `Site.master` главной страницы элемента управления Repeater был использован для отображения маркированного списка содержимого карты сайта верхнего уровня (Basic Reporting, Filtering Reports, Настройка форматирования и т. д.); в другой, вложенный элемент управления Repeater использовался для отображения дочерних разделов разделы верхнего уровня
- В `SectionLevelTutorialListing.ascx`, элемент управления Repeater был использован для отображения маркированного списка дочерних разделов текущего раздела карты сайта

> [!NOTE]
> ASP.NET 2.0 введен новый [управления BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), который можно привязать к элементу управления источником данных для отображения простого маркированного списка. С элементом управления BulletedList, нам не нужно указывать какие-либо список связанных с HTML-код; Вместо этого мы просто указать поле данных для отображения в виде текста для каждого элемента списка.


Элементу управления Repeater служит в качестве catch все данные веб-элемента управления. Если не существующего элемента управления, необходимую разметку, можно использовать элемент управления Repeater. Чтобы проиллюстрировать использование элемента управления Repeater, позвольте s Возьмем список категорий, отображенных над DataList сведения продукт, созданный на шаге 2. В частности, позволяют s Возьмем категории, отображенные в таблице HTML `<table>` с каждой категорией, отображаться в виде столбца в таблице.

Для этого запустите, перетащив элемент управления Repeater из области элементов в конструктор над DataList сведения продукта. Как и в DataList, Repeater изначально отображается как серый квадрат пока его шаблоны определены.


[![Добавление элемента управления Repeater в конструктор](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Рис. 13**: Добавление элемента управления Repeater в конструктор ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


Здесь s только один вариант в смарт-теге элемента управления Repeater s: Выбор источника данных. Необязательно для создания нового источника ObjectDataSource и настроить его для использования `CategoriesBLL` класс s `GetCategories` метод.


[![Создайте новый ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Рис. 14**: создайте новый ObjectDataSource ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Настройка ObjectDataSource на использование класса CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Рис. 15**: Настройка ObjectDataSource для использования `CategoriesBLL` класс ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Получение информации обо всех категориях с помощью метода GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Рис. 16**: получить сведения о всех категорий с помощью `GetCategories` метод ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


В отличие от элементов управления DataList Visual Studio не создает автоматически ItemTemplate для элемента управления Repeater после ее привязки к источнику данных. Кроме того шаблоны элемента управления Repeater s не могут быть настроены через конструктор и должны быть указаны декларативно.

Для отображения категорий как однострочный `<table>` со столбцом для каждой категории, нам нужно элементу управления Repeater должен создавать разметку, аналогичную следующей:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Так как `<td>Category X</td>` текста — это часть, которая повторяется, оно будет отображаться в s ItemTemplate элемента управления Repeater. Разметка, появляющаяся перед этим - `<table><tr>` -будут помещены в `HeaderTemplate` при завершающая разметка — `</tr></table>` -будет располагаться в `FooterTemplate`. Чтобы ввести параметры шаблона, перейдите в декларативный раздел страницы ASP.NET, щелкнув «источник» в левом нижнем углу и введите следующий синтаксис:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Repeater создает точную разметку в соответствии с шаблонами, не более и не меньше. Рис. 17 показаны выходные данные элемента управления Repeater s при просмотре через браузер.


[![HTML однострочный &lt;таблицы&gt; перечислены каждой категории в отдельном столбце](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Рис. 17**: одной строки HTML `<table>` перечислены каждой категории в отдельном столбце ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Шаг 6: Улучшение внешнего вида элемента Repeater

Так как Repeater создает точно такую разметку, указано его шаблонами, должен содержать как неудивительно, что нет относящихся к стилю свойств для элемента управления Repeater. Чтобы изменить внешний вид содержимого, создаваемого элементом управления Repeater, мы необходимо вручную добавить необходимое содержимое HTML или CSS непосредственно на шаблоны элемента управления Repeater s.

В нашем примере позволяют s, которые имеют столбцы категорий имеют различные цвета фона, подобно чередующимся строкам в элементе управления DataList. Для этого нам нужно назначить `RowStyle` класс CSS для каждого элемента управления Repeater и `AlternatingRowStyle` класс CSS для каждого переменного элемента Repeater через `ItemTemplate` и `AlternatingItemTemplate` примерно так:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Позвольте s также добавить строку заголовка в выходные данные с текстом категорий продуктов. Так как мы кое t знать количество столбцов итоговой таблице `<table>` будет состоять, самый простой способ создания строки заголовка, которая гарантированно охватит все столбцы, — использовать *два* `<table>` s. Первый `<table>` будет содержать две строки, строки заголовка и строку, которая будет содержать во-вторых, однострочный `<table>` которой имеется столбец для каждой категории в системе. То есть мы хотим создать следующую разметку:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Следующие `HeaderTemplate` и `FooterTemplate` приведет к желаемой разметке:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

Рис. 18 показана элементу управления Repeater, после внесения этих изменений.


[![Столбцы категорий имеют чередующийся фоновый цвет и включает строку заголовка](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Рис. 18**: категория столбцы альтернативного цвет фона и включает строку заголовка ([Просмотр полноразмерного изображения](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Сводка

Хотя элемент управления GridView упрощает для отображения, изменения, удаления, сортировки и перемещение по данным, внешний вид весьма угловат и напоминает сетку. Для большего контроля над внешним видом нам нужно включить для элементов управления DataList или Repeater. Эти элементы управления отображения набора записей при помощи шаблонов вместо полей BoundField, CheckBoxFields и т. д.

Элемент управления DataList визуализируется как таблица HTML `<table>` , по умолчанию отображается каждая запись источника данных в одну строку таблицы, так же, как элемент управления GridView с одной TemplateField. Как мы увидим в дальнейших учебных курсах, тем не менее, элемент управления DataList пропускать несколько записей для отображения на каждую строку таблицы. Элементу управления Repeater, с другой стороны, создает исключительно разметку, указанную в его шаблонах; он не добавляет дополнительной разметки и поэтому обычно используется для отображения данных в HTML-элементов, отличных от `<table>` (например, в виде маркированного списка).

Хотя элемент управления DataList и Repeater предлагают дополнительную гибкость в отображении выходных данных, них нет многих встроенных функций в GridView. Как мы увидим в последующих учебных курсах, некоторые из этих функций можно подключить обратно без особых усилий, но следует помнить, использовать DataList или Repeater вместо GridView ограничив набор функций, которые можно использовать без необходимости реализации таких возможностей самостоятельно.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Эллис (Yaakov Ellis), (Liz Shulok), Рэнди Шмидт и Стейси парк, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Вперед](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
