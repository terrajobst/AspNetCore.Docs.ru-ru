---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: "Отображение данных с элементами управления повторителем (C#) и DataList | Документы Microsoft"
author: rick-anderson
description: "В учебниках по выше мы использовали элемента управления GridView для отображения данных. Начиная с этого учебника, мы рассмотрим создание общих шаблонов отчетов с..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f626731e79d83785057498c53cdf49aecb90261
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Отображение данных с помощью DataList и элементы управления повторителем (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) или [скачать PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> В учебниках по выше мы использовали элемента управления GridView для отображения данных. Начиная с этого учебника, мы рассмотрим создание общих шаблонов отчетов с элементами управления DataList и повторителя с основами отображения данных с этими элементами управления.


## <a name="introduction"></a>Вступление

Во всех примерах ранее 28 учебники, если нужно отобразить несколько записей из источника данных, мы выявили для элемента управления GridView. GridView Подготовка строки для каждой записи в источнике данных, отображение запись s полей данных в столбцах. Хотя GridView можно быстро для отображения, просмотра, сортировки, изменить и удалить данные, его внешний вид — немного boxy. Кроме того, отвечает разметку для она имеет фиксированную структуру s GridView включает HTML `<table>` с строки таблицы (`<tr>`) для каждой записи и ячейки таблицы (`<td>`) для каждого поля.

Чтобы обеспечить большую степень настройкам на внешний вид и разметку, при отображении нескольких записей, ASP.NET 2.0 предлагает элементов управления DataList и повторителя (которые также были доступны в ASP.NET версии 1.x). Элементы управления DataList и повторителя отображать свое содержимое с помощью шаблонов, а не стояли CheckBoxFields, ButtonFields и так далее. Как GridView, DataList готовится к просмотру как HTML `<table>`, но позволяет нескольким данных источника записей для отображения на каждую строку таблицы. С другой стороны, повторителя отрисовывает без дополнительной разметки, чем то, что вы явным образом указать и является идеальным кандидатом, когда требуется точный контроль над разметкой создается.

Над рядом около десятка учебники мы рассмотрим создание общих шаблонов отчетов с элементами управления DataList и повторителя с основами отображения данных с помощью этих шаблонов элементов управления. Мы рассмотрим способы форматирования эти элементы управления, как изменить макет записи источника данных в DataList, общие сведения об образце сценарии, способов на изменение и удаление данных, как переходить по записям и т. д.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Шаг 1: Добавление DataList и повторителя учебника веб-страницы

Прежде чем начать этого учебника, позволяют s сначала занять некоторое время для добавления страниц ASP.NET, которые потребуются для этого учебника и далее несколько учебники, посвященные отображение данных с помощью DataList и повторителя. Начните с создания новой папки в проект с именем `DataListRepeaterBasics`. Добавьте пять страниц ASP.NET в этой папке, в которых настроен на использование главной страницы `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Создание папки DataListRepeaterBasics и добавление страницы учебника по ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Рис. 1**: создание `DataListRepeaterBasics` папки и добавления страниц учебника по ASP.NET


Откройте `Default.aspx` страницы и перетащите `SectionLevelTutorialListing.ascx` пользовательского элемента управления с `UserControls` папку в область конструктора. Этот пользовательский элемент управления, который мы создали [главные страницы и переходов](../introduction/master-pages-and-site-navigation-cs.md) учебник, просматривает карту узла и отображает учебники из текущего раздела в виде маркированного списка.


[![Добавление Default.aspx SectionLevelTutorialListing.ascx пользовательского элемента управления](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**На рисунке 2**: добавление `SectionLevelTutorialListing.ascx` пользовательского элемента управления на `Default.aspx` ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


Для отображения маркированного списка DataList и повторителя учебники, которые будут созданы, необходимо добавить их в карту узла. Откройте `Web.sitemap` файл и добавьте следующую разметку после разметки узел карты узла Добавление пользовательских кнопок:


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Обновление карты сайта для включения новых страниц ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Рис. 3**: обновление карты сайта для включения новых страниц ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Шаг 2: Отображение сведений о продукте с DataList

Подобно FormView, s, выводимые данные элемента управления DataList зависит от шаблонов, а не стояли, CheckBoxFields и т. д. В отличие от FormView DataList предназначен для отображения набора записей, а не в одиночные потоке. Позволяют начать работу с учебником с рассмотрения привязки сведения о продукте в DataList s. Сначала откройте `Basics.aspx` страницы в `DataListRepeaterBasics` папки. Затем перетащите DataList из области элементов в конструктор. Как показано на рис. 4, перед тем как указать шаблоны DataList s, конструктор отображает его в виде серого прямоугольника.


[![Перетащите элемент управления DataList из области элементов в конструктор](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Рис. 4**: перетащите DataList из области элементов в конструктор ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


Из DataList s смарт-тег, добавьте новый элемент управления ObjectDataSource и настройте его для использования `ProductsBLL` класса s `GetProducts` метод. Поскольку мы повторного создания DataList только для чтения в этом учебнике значение раскрывающегося списка (нет) в мастер s INSERT, UPDATE и DELETE вкладок.


[![Создадим новый элемент управления ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Рис. 5**: попробовать создать новый элемент управления ObjectDataSource ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Настройка ObjectDataSource с помощью класса ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Рис. 6**: Настройка ObjectDataSource для использования `ProductsBLL` класса ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Получить сведения обо всех продуктах, с помощью метода GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Рис. 7**: получить сведения о всех продуктов с помощью `GetProducts` метод ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


После настройки ObjectDataSource и связывание его с DataList через его смарт-тег, Visual Studio автоматически создает `ItemTemplate` в DataList, в котором отображаются имя и значение каждого поля данных, возвращаемых источником данных (см. Разметка ниже). Это значение по умолчанию `ItemTemplate` внешний вид s идентична шаблонов, автоматически создается, когда привязка источника данных к FormView через конструктор.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Помните, что при привязке источника данных к элементу управления FormView с помощью смарт-тега FormView s, Visual Studio будет создан `ItemTemplate`, `InsertItemTemplate`, и `EditItemTemplate`. С помощью DataList, однако только `ItemTemplate` создается. Это так, как DataList не поддерживает же встроенной правки и вставки поддержка FormView. DataList содержать события, связанные с edit и delete, а изменение и удаление поддержки могут быть добавлены с большой объем кода, но отсутствует s не простой out of box поддерживается как с FormView. Мы рассмотрим, как включить редактирование и удаление поддержки с DataList в будущем учебника.


Разрешить s занять некоторое время, чтобы улучшить внешний вид этого шаблона. Вместо отображения всех полей данных, позволяют s будет отображаться только название продукта s, поставщику, категория, количество за единицу и цена за единицу. Кроме того, позволяют s отображаемое имя в `<h4>` заголовок и макет оставшиеся поля, с помощью `<table>` под заголовком.

Для внесения этих изменений, можно либо использовать возможности в конструкторе из DataList s смарт-тег, щелкните ссылку Изменить шаблоны, или можно изменить шаблон вручную с помощью декларативного синтаксиса s страницы редактирования шаблонов. Если выбран параметр редактирование шаблонов в конструкторе полученный разметке может неточно следующую разметку, но при просмотре через браузер должен выглядят очень похоже на рисунке показано на рисунке 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> В примере выше используется метка веб-элементов управления которого `Text` присваивается значение синтаксис привязки данных. Кроме того мы может опущен меток, введя в выражение привязки данных. То есть вместо `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` можно было бы вместо этого использовать декларативный синтаксис `<%# Eval("CategoryName") %>`.


Однако, оставляя метка веб-элементов управления предоставляют два преимущества. Во-первых он предоставляет простой способ для форматирования данных, на основе данных, как будет показано в следующем уроке. Во-вторых параметр редактирование шаблонов в конструктор t отображения декларативный синтаксис привязки данных, которая отображается за пределами некоторые веб-элемента управления. Вместо этого интерфейса редактирование шаблонов предназначен для упрощения работы с разметки static и веб-элементы управления и предполагается, что все привязки данных, выполняются в диалоговом окне изменить привязки данных, доступный из смарт-тегов элементов управления Web.

Таким образом при работе с DataList, которая предоставляет возможность редактирования шаблонов в конструкторе, я предпочитаю использовать метки веб-элементов управления, чтобы содержимого доступен через интерфейс редактирование шаблонов. Как скоро будет показано, повторителя требует, что содержимое шаблона s редактироваться в представлении источника. Следовательно при создания шаблонов повторителя s, я часто будет пропущен Web метки элементов управления, если я знаю, что необходимо форматировать внешний вид данных привязан текст, основанный на программную логику.


[![Каждый продукт s выходных данных — к просмотру с помощью DataList ItemTemplate s](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Рис. 8**: s каждый продукт выходных данных — к просмотру с помощью DataList s `ItemTemplate` ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Шаг 3: Улучшение внешнего вида элемента управления DataList

Как GridView, DataList предлагает ряд относящихся к стилю свойств, таких как `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`, и т. д. При работе с элементами управления GridView и DetailsView, мы создали файлах обложек в `DataWebControls` тему, предварительно определенные `CssClass` свойства для этих двух элементов управления и `CssClass` свойства для нескольких их подсвойства (`RowStyle` `HeaderStyle`и так далее). Позвольте выполнить то же самое для DataList s.

Как было сказано в [отображение данных с ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) учебного файла обложки определяет внешний вид свойства по умолчанию для веб-элемента управления; темы — это совокупность файлов обложки, CSS, JavaScript и изображения, которые определяют определенный внешний вид и поведение для веб-сайта. В *отображение данных с ObjectDataSource* учебнике мы создали `DataWebControls` темы (реализованная в виде папки внутри `App_Themes` папку), имеет, в настоящее время два файла обложки - `GridView.skin` и `DetailsView.skin`. Позволяет добавить третий файл обложки, чтобы указать параметры предварительно определенных стилей для DataList s.

Чтобы добавить файл обложки, щелкните правой кнопкой мыши `App_Themes/DataWebControls` , выберите команду Добавить новый элемент и выберите файл обложки параметр из списка. Назовите файл `DataList.skin`.


[![Создание нового файла обложки DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Рис. 9**: Создание нового файла обложки с именем `DataList.skin` ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Используйте следующую разметку для `DataList.skin` файла:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Эти параметры назначить соответствующие свойства DataList те же классы CSS, использовавшиеся с элементами управления GridView и DetailsView. Классы CSS, используемые здесь `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`и так далее определяются в `Styles.css` файла и были добавлены в предыдущих учебниках.

При добавлении этого файла обложки внешний s DataList обновляется в конструкторе (может потребоваться обновить представление конструктора, чтобы увидеть эффекты новый файл обложки; из меню «Вид», нажмите кнопку обновления). Как показано на рис. 10, каждый чередующихся продукт имеет светлый розовый цвет фона.


[![Создание нового файла обложки DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Рис. 10**: Создание нового файла обложки с именем `DataList.skin` ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Шаг 4: Изучение DataList s другие шаблоны

В дополнение к `ItemTemplate`, элемент управления DataList поддерживает шесть шаблонов необязательно:

- `HeaderTemplate`Если указано, добавляет строку заголовка в выходные данные и используется для отображения в этой строке
- `AlternatingItemTemplate`используется для отрисовки чередующихся элементов
- `SelectedItemTemplate`используется для отрисовки выбранного элемента; Выбранный элемент является элементом, индекс которого соответствует DataList s [ `SelectedIndex` свойство](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate`использовать для отображения редактируемого элемента
- `SeparatorTemplate`Если указано, добавляет разделитель между каждым элементом и используется для отрисовки этот разделитель
- `FooterTemplate`-Если указано, добавляет строку нижнего колонтитула в выходных данных и используется для отображения в этой строке

При указании `HeaderTemplate` или `FooterTemplate`, DataList добавляется дополнительная строка верхнего или нижнего колонтитула для выводимых данных. Как и с GridView s колонтитулы строки заголовка и нижнего колонтитула в элементе управления DataList не привязанных к данным. Таким образом, любой синтаксис привязки данных в `HeaderTemplate` или `FooterTemplate` , попытки получения доступа к привязанных данных возвратит пустую строку.

> [!NOTE]
> Как мы видели в [отображение сведения о в GridView s нижнего колонтитула](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) учебник, пока строки верхнего и нижнего колонтитулов не хотите синтаксис привязки данных для поддержки t, сведения о конкретных данных, которые могут быть добавлены непосредственно в эти строки из GridView s `RowDataBound` обработчика событий. Этот метод можно использовать для обоих вычисляющих промежуточные итоги или другие сведения из данных привязаны к элементу управления, а также назначить эту информацию в нижний колонтитул. То же самое может применяться к элементам управления DataList и повторителя; Единственным различием является для DataList и повторителя создайте обработчик событий для `ItemDataBound` событий (а не для `RowDataBound` событий).


В нашем примере позволяют s имеют заголовок в верхней части результатов s DataList в сведения о продукте `<h3>` заголовок. Для этого добавьте `HeaderTemplate` соответствующая разметка. В конструкторе, это можно сделать, щелкните ссылку Изменить шаблоны DataList s смарт-тега, выбрав шаблон заголовка из раскрывающегося списка и введя текст после выбора заголовок 3 параметр из раскрывающегося списка стиль списка (см. рис. 11).


[![Добавить HeaderTemplate с сведения о продукте текста](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Рис. 11**: добавление `HeaderTemplate` с сведения о продукте текст ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


Кроме того, можно добавить декларативно, введя следующую разметку в `<asp:DataList>` теги:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Чтобы добавить свободное пространство между каждой список продуктов, позволяют добавить s `SeparatorTemplate` , включает строку между каждого раздела. Тег горизонтальную линию (`<hr>`), добавляет такие разделитель. Создание `SeparatorTemplate` так, что он содержит следующую разметку:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Как `HeaderTemplate` и `FooterTemplates`, `SeparatorTemplate` не привязан к любой записи из источника данных и поэтому не может иметь непосредственного доступа к записи, привязанные к элементу управления DataList источника данных.


После внесения этого. Кроме того, при просмотре страницы через браузер, он должен выглядеть как на рис. 12. Обратите внимание, строку заголовка и строку между вхождение каждого продукта.


[![Элемент управления DataList включает строку заголовка и горизонтальную линию между вхождение каждого продукта](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Рис. 12**: DataList включает строку заголовка и горизонтальное правило между Each списка продуктов ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Шаг 5: Подготовка к просмотру конкретного разметки с помощью элемента управления повторителем

В этом случае исходный из браузера при посещении примера DataList из рис. 12 вы увидите, что элемент управления DataList передает HTML `<table>` , содержащий строку таблицы (`<tr>`) с одну ячейку (`<td>`) для каждого элемента, привязанного к для DataList. Эти выходные данные на самом деле идентична что будет выдаваться в GridView с одной TemplateField. Как будет видно в будущем учебник, DataList разрешить дополнительная настройка выходных данных, позволяющих отображать несколько записи источника данных на каждую строку таблицы.

Что делать, если вы не хотите t требуется создавать HTML `<table>`, хотя? Для общее и полный контроль над разметку, созданную веб-элементе управления данных воспользуйтесь элементе управления повторителем. Как элемент управления DataList повторителя строится на основе шаблонов. Однако повторителя только дает следующие пять шаблонов.

- `HeaderTemplate`Если указано, добавляет указанный разметки перед элементами
- `ItemTemplate`используется для отрисовки элементов
- `AlternatingItemTemplate`Если указано, используется для отрисовки чередующихся элементов
- `SeparatorTemplate`Если указано, добавляет указанный разметки между каждым элементом
- `FooterTemplate`-Если указано, добавляет указанный разметки после элементов

В ASP.NET 1.x, повторителя управления был обычно используется для отображения маркированного списка которого поступают данные из источников данных. В этом случае `HeaderTemplate` и `FooterTemplates` будет содержать открывающим и закрывающим `<ul>` тегов, соответственно, а `ItemTemplate` будет содержать `<li>` элементов при помощи синтаксиса привязки данных. Такой подход по-прежнему может использоваться в ASP.NET 2.0, как было показано в двух примерах в [главные страницы и переходов](../introduction/master-pages-and-site-navigation-cs.md) учебника:

- В `Site.master` главной страницы повторитель был использован для отображения маркированного списка содержимого карты сайта верхнего уровня (Базовые отчеты, фильтрация отчетов, Настройка форматирования и т. д); другой, вложенные повторителя был использован для отображения дочерних разделов разделы верхнего уровня
- В `SectionLevelTutorialListing.ascx`, повторитель был использован для отображения маркированного списка разделов дочерние элементы текущего раздела карты сайта

> [!NOTE]
> ASP.NET 2.0 появился новый [управления маркированный список](https://msdn.microsoft.com/en-us/library/ms228101.aspx), который можно привязать к элементу управления источником данных для отображения простых маркированного списка. С помощью элемента управления BulletedList, нам не нужно указать любой из списка связанных HTML-код; Вместо этого мы просто укажите поле данных для отображения в виде текста для каждого элемента списка.


Повторителя служит catch все данные веб-элемента управления. Если не существующего элемента управления, создающий необходимые разметки, можно использовать элементе управления повторителем. Чтобы проиллюстрировать использование повторителя, позволяют s список категорий, отображенный выше DataList сведения продукта, созданный на шаге 2. В частности, позволяющие s должны категорий, отображаемых в HTML одной строки `<table>` с помощью каждой категории отображаются в виде столбца в таблице.

Для этого запустите путем перетаскивания из области элементов в конструктор выше DataList продукта сведения управления повторителем. Как и в элемент управления DataList повторителя изначально будет отображаться как серые поля свои шаблоны были определены.


[![Добавить в конструктор повторитель](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Рис. 13**: добавить в конструктор повторитель ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


S существует только один параметр в смарт-теге повторителя s: Выбор источника данных. Необязательно для создания нового ObjectDataSource и настройте его для использования `CategoriesBLL` класса s `GetCategories` метод.


[![Создать новый элемент управления ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Рис. 14**: создайте новый элемент управления ObjectDataSource ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Настройка ObjectDataSource с помощью класса CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Рис. 15**: Настройка ObjectDataSource для использования `CategoriesBLL` класса ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Получить сведения обо всех категориях, с помощью метода GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**На рисунке 16**: получить сведения о всех категорий с помощью `GetCategories` метод ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


В отличие от DataList Visual Studio не создает автоматически ItemTemplate для повторителя после ее привязки к источнику данных. Кроме того шаблоны s повторителя нельзя настроить с помощью конструктора и должен быть указан декларативно.

Для отображения категорий в виде одной строки `<table>` со столбцом для каждой категории, нам нужно повторителя для создания разметки, подобную следующей:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Поскольку `<td>Category X</td>` текст — часть, которая повторяется, это имя появится в повторителя s ItemTemplate. Разметка, которая появляется перед ним - `<table><tr>` -будут помещены в `HeaderTemplate` во время окончания разметки - `</tr></table>` -будет располагаться в `FooterTemplate`. Чтобы ввести эти параметры шаблона, перейдите декларативный часть страницы ASP.NET, щелкнув «источник» в левом нижнем углу и введите следующий синтаксис:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Повторителя выдает точно разметку в соответствии с его шаблоны, ничто иное, ничего не меньше. Рисунок 17 показан результат s повторителя при просмотре через браузер.


[![HTML одной строки &lt;таблицы&gt; перечислены каждой категории в отдельном столбце](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Рисунок 17**: одной строки HTML `<table>` перечислены каждой категории в отдельном столбце ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Шаг 6: Улучшение внешнего вида повторителя

Поскольку повторителя выдает точно разметки, заданные свои шаблоны, должен содержать как не удивительно, что нет относящихся к стилю свойств для повторителя. Чтобы изменить внешний вид содержимого, порождаемого повторителя, мы необходимо вручную добавить необходимое содержимое HTML и CSS непосредственно на шаблоны повторителя s.

В нашем примере позволяют s сменяются чередующихся строк в DataList цвета фона, таких как столбцы категории. Для выполнения этой задачи необходимо назначить `RowStyle` класс CSS для каждого элемента повторителя и `AlternatingRowStyle` класс CSS каждого чередующихся повторителя элементом с помощью `ItemTemplate` и `AlternatingItemTemplate` шаблонов, следующим образом:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Разрешить s также добавить строку заголовка в выходные данные с текстом категорий продуктов. Так как мы не хотите t знаете количество столбцов нашей возникающие `<table>` будет состоять, самый простой способ создания строку заголовка, которое гарантированно охватывают все столбцы которой использовать *два* `<table>` s. Первый `<table>` будет содержать две строки, строку заголовка и строку, которая будет содержать во-вторых, одной строки `<table>` , имеется столбец для каждой категории в системе. То есть мы хотим порождение следующую разметку:


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Следующие `HeaderTemplate` и `FooterTemplate` приводят к требуемой разметке:


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

На рисунке 18 показано повторителя после внесения этих изменений.


[![Столбцы категорий поочередно цвет фона и включает строку заголовка](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**На рисунке 18**: категория столбцы альтернативного цвет фона и включает строку заголовка ([Просмотр полноразмерное изображение](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Сводка

Хотя элемент управления GridView делает для отображения, изменения, удаления, сортировки и перемещение по данным, внешний boxy и вид сетки. Для усиления контроля над внешний необходимо включить, чтобы элементы управления DataList или повторителя. Оба эти элемента управления отображения набора записей с помощью шаблонов вместо стояли, CheckBoxFields и т. д.

Отрисовывает HTML-элемент управления DataList `<table>` , по умолчанию отображается каждая запись источника данных в одну строку, так же, как элемент управления GridView с одной TemplateField. Как видно в будущем учебник, тем не менее, DataList пропускать несколько записей для отображения на каждую строку таблицы. Повторителя, с другой стороны, строго создает разметку, указанный в его шаблоны; он не добавляет дополнительной разметки и поэтому обычно используется для отображения данных в HTML-элементов, отличных от `<table>` (например, в виде маркированного списка).

Хотя DataList и повторителя обеспечивают большую гибкость в их выводимых данных, у которых отсутствует многие встроенные функции, доступные в GridView. Как мы изучим в будущих учебниках, некоторые из этих функций можно подключить снова без особых усилий, но следует помнить, использовать DataList или повторителя вместо GridView ограничивать количество функций, которые можно использовать без необходимости реализации этих возможностей самостоятельно.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Yaakov Эллис (Ellis), (Liz Shulok), Рэнди Шмидт и парк Stacy. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Вперед](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
