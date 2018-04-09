---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: С помощью TemplateFields в элементе управления GridView (C#) | Документы Microsoft
author: rick-anderson
description: Для обеспечения гибкости GridView предлагает TemplateField, на которой отображается с помощью шаблона. Шаблон может включать сочетание статического кода HTML, веб-элементов управления и...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 6485cbda50912920808fc0caf41c888493f210dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="using-templatefields-in-the-gridview-control-c"></a>С помощью TemplateFields в элементе управления GridView (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) или [скачать PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Для обеспечения гибкости GridView предлагает TemplateField, на которой отображается с помощью шаблона. Шаблон может быть смесь статического кода HTML, веб-элементов управления и синтаксис привязки данных. В этом учебнике мы рассмотрим, как использовать TemplateField для дополнительной настройки с помощью элемента управления GridView.


## <a name="introduction"></a>Вступление

GridView состоит из набора полей, которые определяет, какие свойства `DataSource` должны быть включены в выходные данные и способ отображения данных. Простейший тип поля — BoundField, который отображает значение данных в виде текста. Поля других типов при отображении данных используют различные элементы HTML. CheckBoxField, например, отображаемое как флажок, состояние которого зависит от значения поля данных; ImageField отображает изображение, источник которого создается на основе поля данных. Гиперссылки и кнопки, состояние которого зависит от базового значения поля данных может осуществляться с помощью HyperLinkField и ButtonField типы полей.

Хотя типы полей CheckBoxField, ImageField, HyperLinkField и ButtonField разрешить альтернативного представления данных, они по-прежнему весьма ограниченны с точки зрения форматирования. CheckBoxField отображаются только одного флажка, тогда как ImageField можно отобразить только одно изображение. Что делать, если необходимо включать текст, флажок определенного поля *и* изображение все зависимости от значений полей данных? Или, что если нам нужно отобразить данные с помощью веб-элемент управления, отличный от флажок, изображения, гиперссылки или кнопки? Кроме того BoundField ограничивает область отображения одного поля данных. Что делать, если мы хотели Показать двух или нескольких значений полей данных в одном столбце GridView?

Чтобы учесть этот уровень гибкости GridView предлагает TemplateField, на которой отображается с помощью *шаблона*. Шаблон может быть смесь статического кода HTML, веб-элементов управления и синтаксис привязки данных. Кроме того TemplateField имеет множество шаблонов, которые можно использовать для настройки подготовки к просмотру в различных ситуациях. Например `ItemTemplate` используется по умолчанию для отображения ячейки в каждой строке, но `EditItemTemplate` шаблон можно использовать для настройки интерфейса при изменении данных.

В этом учебнике мы рассмотрим, как использовать TemplateField для дополнительной настройки с помощью элемента управления GridView. В [предыдущего учебника](custom-formatting-based-upon-data-cs.md) мы узнали, как настроить форматирование в зависимости от базовых данных с помощью `DataBound` и `RowDataBound` обработчики событий. Другой способ настройки форматирования на основе базовых данных путем вызова методов форматирования из шаблона. Мы рассмотрим этот метод в этом учебнике также.

В этом учебнике мы будем использовать TemplateFields, чтобы настроить внешний вид списка сотрудников. В частности, мы составим список всех сотрудников, но будет отображен сотрудника имя и фамилию в один столбец, дате найма в элементе управления Calendar и состояние столбца, который указывает, сколько дней они был принят на работу в компании.


[![Три TemplateFields используются для настройки отображения](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Рис. 1**: три TemplateFields используются для настройки отображения ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Шаг 1: Привязка данных к GridView

В службах reporting сценариях, где необходимо использовать TemplateFields настройки внешнего вида, найти его проще всего начать с создания элемента управления GridView, содержащий только стояли сначала и затем, чтобы добавить новый TemplateFields или преобразовать существующий стояли для TemplateFields при необходимости. Таким образом давайте начнем этого учебника, добавление элемента управления GridView к странице через конструктор и привяжите его к элементу ObjectDataSource, который возвращает список сотрудников. Эти шаги создаст GridView с стояли для каждого из полей employee.

Откройте `GridViewTemplateField.aspx` страницы и перетащите элемент управления GridView с панели элементов в конструктор. В смарт-теге элемента GridView выбрать и добавить новый элемент управления ObjectDataSource, который вызывает `EmployeesBLL` класса `GetEmployees()` метод.


[![Добавить новый элемент управления ObjectDataSource, который вызывает метод GetEmployees()](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**На рисунке 2**: Добавление нового элемента управления ObjectDataSource, методы Invoke `GetEmployees()` метод ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


Привязка GridView таким образом автоматически добавит поле BoundField для каждого свойства сотрудника: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, и `Country`. Для этого отчета давайте не использоваться с отображением `EmployeeID`, `ReportsTo`, или `Country` свойства. Чтобы удалить эти стояли можно:

- Чтобы открыть это диалоговое окно, используйте поля диалоговом окне щелкните ссылку Правка столбцов GridView смарт-теге. Затем выберите и щелкните красный значок X стояли из нижнего левого угла кнопку для удаления BoundField.
- Вручную изменить декларативного синтаксиса элемента GridView из представления источников, удалите `<asp:BoundField>` элемент для BoundField, нужно удалить.

После удаления `EmployeeID`, `ReportsTo`, и `Country` стояли, разметку элемента GridView должен выглядеть так:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Теперь пора просмотрите ход работы в браузере. На этом этапе появится таблица с помощью записи для каждого сотрудника и четыре столбца: один для сотрудника Фамилия, одному их именами, названий и один для дате найма.


[![LastName, FirstName, заголовок и HireDate полей отображаются для каждого сотрудника.](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Рис. 3**: `LastName`, `FirstName`, `Title`, и `HireDate` отображаются поля для каждого сотрудника ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Шаг 2: Отображение имени и фамилии в одном столбце

В настоящее время каждого сотрудника и фамилии, отображаются в отдельном столбце. Может оказаться удобнее объединить их в один столбец. Для выполнения этой задачи необходимо использовать TemplateField. Мы можно либо добавить новые TemplateField, добавить в него нужную разметку и синтаксис привязки данных, а затем удалите `FirstName` и `LastName` можно преобразовать стояли или мы `FirstName` BoundField в TemplateField, изменить TemplateField для включения `LastName` значение, а затем удалите `LastName` BoundField.

Оба подхода net тот же результат, но мы рекомендуем преобразование стояли TemplateFields, когда это возможно, поскольку преобразование автоматически добавляет `ItemTemplate` и `EditItemTemplate` с веб-элементов управления и синтаксис привязки данных имитацию и функциональность BoundField. Преимущество заключается в том, что нам нужно будет меньше работы по созданию TemplateField, как в процессе преобразования будет выполнена некоторых операций для нас.

Для преобразования существующего BoundField в TemplateField, щелкните ссылку Правка столбцов GridView смарт-теге, открывая диалоговое окно «поля». Выберите BoundField для преобразования из списка в правом нижнем углу и затем щелкните ссылку «Преобразовать это поле в TemplateField» в правом нижнем углу.


[![Преобразовать поле BoundField в TemplateField из диалогового окна поля](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Рис. 4**: преобразование из диалогового окна поля BoundField в TemplateField ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


Преобразуйте `FirstName` BoundField в TemplateField. После этого изменения нет не перемен в конструкторе. Это так, как преобразование BoundField и TemplateField создает TemplateField, который поддерживает внешний вид BoundField. Несмотря на то, что никаких видимых изменений в этот момент в конструкторе, процесс преобразования заменил BoundField декларативного синтаксиса - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` - TemplateField следующим образом:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Как видите, TemplateField состоит из двух шаблонов `ItemTemplate` , имеет метку которого `Text` свойству присвоено значение `FirstName` поля данных и `EditItemTemplate` текстовое поле, куда элемент управления `Text` задано свойство Чтобы `FirstName` поля данных. Синтаксис привязки данных — `<%# Bind("fieldName") %>` -указывает, что поля данных *`fieldName`* привязан к указанному свойству элемента управления Web.

Чтобы добавить `LastName` значение этого TemplateField, необходимо добавить другой веб-управления Label в поля данных `ItemTemplate` и привязать его `Text` свойства `LastName`. Это можно сделать вручную или с помощью конструктора. Чтобы сделать это вручную, просто добавьте соответствующий декларативный синтаксис в `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Чтобы добавить его в конструкторе, щелкните ссылку Изменить шаблоны смарт-теге элемента GridView. Это позволит отобразить интерфейс редактирования шаблона элемента GridView. В этом интерфейса смарт-тегов список шаблонов в GridView. Поскольку у нас имеется только один TemplateField на этом этапе, только шаблоны, в раскрывающемся списке, эти шаблоны для `FirstName` TemplateField вместе с `EmptyDataTemplate` и `PagerTemplate`. `EmptyDataTemplate` Шаблон, если указано, используется для отображения выходных данных GridView в том случае, если данные, привязанные к GridView; нет результатов `PagerTemplate`, если он указан, используется для отображения интерфейса разбиения на страницы для элемента управления GridView, поддерживающий разбиение на страницы.


[![Можно изменить шаблоны элемента GridView с помощью конструктора](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Рис. 5**: GridView шаблоны могут быть изменены через конструктор ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


Чтобы также отобразить `LastName` в `FirstName` TemplateField перетащите элемент управления Label из области элементов в `FirstName` в TemplateField `ItemTemplate` в GridView редактирование шаблона интерфейса.


[![Добавить веб-элемент управления Label FirstName TemplateField ItemTemplate](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Рис. 6**: Добавление веб-элемент управления Label к `FirstName` в TemplateField ItemTemplate ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


В этой точке веб-управления Label добавлен TemplateField содержит его `Text` свойством, имеющим значение «Метка». Нам нужно изменить, чтобы это свойство привязано к значению `LastName` поля данных. Для выполнения этого щелкнуть смарт-тег элемента управления Label и выберите пункт Изменить привязки данных.


[![Выберите Правка привязок данных с метки смарт-тег](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Рис. 7**: выберите изменить привязки данных с метки смарт-тег ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


Откроется диалоговое окно Привязка данных. Здесь можно выбрать свойство для участия в привязки данных в списке в левой части экрана и выберите поле для привязки данных в раскрывающемся списке справа. Выберите `Text` в левом и `LastName` в правом и нажмите кнопку ОК.


[![Привязать свойство текста в поле «Фамилия» данных](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Рис. 8**: привязка `Text` свойства `LastName` поля данных ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> Диалоговое окно Привязка данных позволяет указать, следует ли выполнять двухсторонней привязкой данных. Если оставить этот флажок снят, синтаксис привязки данных `<%# Eval("LastName")%>` будет использоваться вместо `<%# Bind("LastName")%>`. В этом учебнике подойдет любой из этих подходов. Двусторонняя привязка к данным становится важным, при вставке или изменении данных. Для отображения данных, однако оба подхода могут использоваться одинаково хорошо. Двусторонняя привязка к данным, подробно обсуждаются в будущих учебниках.


Теперь пора просматривать эту страницу через браузер. Как видите, GridView по-прежнему включает четыре столбца; Тем не менее `FirstName` теперь стоят *оба* `FirstName` и `LastName` значения поля данных.


[![FirstName и LastName значения отображаются в одном столбце](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Рис. 9**: как `FirstName` и `LastName` значения отображаются в одном столбце ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


Для выполнения первого шага, удалите `LastName` BoundField и переименуйте `FirstName` в TemplateField `HeaderText` свойства «Имя». После внесения этих изменений декларативная разметка GridView должен выглядеть следующим образом:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![Первый и фамилии каждого сотрудника отображаются в одной сохранений](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Рис. 10**: первый и фамилии каждого сотрудника отображаются в одного столбца ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Шаг 3: С помощью элемента календаря для отображения`HiredDate`поля

Отображение значения поля данных в виде текста в элементе управления GridView, достаточно использовать поле BoundField. В определенных случаях тем не менее, данные лучше представить с помощью определенного веб-элемента управления текста, а. Подобная настройка отображения данных возможна с помощью TemplateFields. Например, а не отображать дату найма работника как текст, можно было показать календаре (с помощью [управления Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) с дате найма выделяются.

Для этого запустите путем преобразования `HiredDate` BoundField в TemplateField. Просто перейдите к GridView смарт-тег и щелкните ссылку Изменить столбцы, открывая диалоговое окно «поля». Выберите `HiredDate` BoundField и нажмите кнопку «Преобразовать это поле в TemplateField.»


[![Преобразовать HiredDate BoundField в TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Рис. 11**: преобразование `HiredDate` BoundField в TemplateField ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


Как мы видели в шаге 2, это приведет к замене BoundField TemplateField, содержащий `ItemTemplate` и `EditItemTemplate` с подписью и текстовое поле, `Text` свойства связаны `HiredDate` с использованием синтаксиса databinding `<%# Bind("HiredDate")%>`.

Чтобы заменить текст элемента управления календаря, измените шаблон путем удаления метки и добавление элемента управления Calendar. В режиме конструктора выберите Редактирование шаблонов из GridView смарт-тег и выберите `HireDate` в TemplateField `ItemTemplate` из раскрывающегося списка. Затем удалите элемент управления Label и перетащите элемент управления календаря из области элементов в интерфейсе правки шаблона.


[![Добавить элемент управления календаря HireDate ItemTemplate в TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Рис. 12**: Добавление элемента управления "Календарь" для `HireDate` в TemplateField `ItemTemplate` ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


На этом этапе каждой строки в GridView будет содержать элемент управления "Календарь" в его `HiredDate` TemplateField. Однако сотрудника фактическое `HiredDate` значение не задано в любом месте в элементе управления календаря, вызывая каждый элемент управления календаря показывают за текущий месяц и дату по умолчанию. Чтобы исправить это, необходимо назначить каждому сотруднику `HiredDate` для элемента управления Calendar [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) и [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) свойства.

В смарт-тега элемента управления Calendar выберите команду Изменить привязки данных. Затем привяжите `SelectedDate` и `VisibleDate` свойства `HiredDate` поля данных.


[![Привязать к полю данных HiredDate SelectedDate и VisibleDate свойства](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Рис. 13**: привязка `SelectedDate` и `VisibleDate` свойства `HiredDate` поля данных ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> Выбранная дата элемента управления Calendar не обязательно видимыми. Например, календарь, возможно, 1 августа<sup>st</sup>, 1999 г. как выбранной даты, но отображаться за текущий месяц и год. Выбранная дата и дата отображается задаются с помощью элемента управления Calendar `SelectedDate` и `VisibleDate` свойства. Поскольку нам требуется, выберите оба сотрудника `HiredDate` и убедитесь, что она отображается, нам нужно привязать оба эти свойства, чтобы `HireDate` поля данных.


При просмотре страницы в браузере, календарь теперь показывает месяц дату приема сотрудника и выбирает этой определенной даты.


[![HiredDate сотрудника отображается в элементе управления Calendar](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Рис. 14**: Сотрудник `HiredDate` отображается в элементе управления календаря ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> В отличие от во всех примерах, описанных выше таким образом, в этом учебнике мы сделали *не* задать `EnableViewState` свойства `false` для этого GridView. Для этого решения, поскольку нужно щелкнуть даты элемента управления Calendar вызывает обратную передачу, установите значение выбранной даты по календарю Дата просто нажать кнопку. Если GridView состояние просмотра отключено, однако при каждой обратной передаче GridView привязываются источнике данных, вследствие чего выбранной даты по календарю задаваемый *обратно* для сотрудника `HireDate`, перезапись Дата, выбранного пользователем.


В этом учебнике это более чем рассуждения, поскольку пользователь не сможет обновить сотрудника `HireDate`. Возможно, лучше настроить элемент управления Calendar даты не могут быть выбраны. Независимо от того в этом учебнике показано, что в некоторых случаях состояние представления должно быть включено для предоставления определенных возможностей.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Шаг 4: Отображается число дней сотрудник работал для компании

Таким образом, что мы видели два приложения TemplateFields:

- Объединение двух или нескольких значений полей данных в один столбец и
- Значение поля данных, с помощью веб-элемента управления вместо текста выражения

Третий использование TemplateFields при отображении метаданные о GridView базовым данным. Просмотра дат приема на работу сотрудников, например, мы может также потребоваться столбец, отображающий общее количество дней, они имели на задание.

Еще другое использование оператора TemplateFields возникает в сценариях, если базовые данные должны отображаться по-разному в веб-страницы отчета в формате хранятся в базе данных, чем. Предположим, что `Employees` таблицы была `Gender` поле, хранятся в виде букв `M` или `F` для указания Пол сотрудника. При отображении этой информации на веб-странице мы хотим Показать пол, как «Мужской» или «Женский», а не только «M» или «F».

Оба эти сценарии могут обрабатываться путем создания *метод форматирования* в классе кода страницы ASP.NET (или в отдельной библиотеке классов, реализованы в виде `static` метод), вызываемой из шаблона. Этот метод форматирования вызывается с помощью шаблона, используя тот же синтаксис привязки данных, рассмотренное ранее. Метод форматирования можно выполнять в любое количество параметров, но должен вернуть строку. Эта строка представляет HTML, которая вставляется в шаблон.

Для иллюстрации этой концепции, попробуем чтобы отобразить столбец, содержащий общее число дней, сотрудник была в задании. Этот метод форматирования будет принимать `Northwind.EmployeesRow` объекта и возвращает количество дней, сотрудник принят на работу как строка. Этот метод можно добавить класс кода страницы ASP.NET, но *должен* помечается как `protected` или `public` чтобы быть доступен из шаблона.


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Поскольку `HiredDate` поле может содержать `NULL` базы данных значения, мы сначала убедитесь, что значение не является `NULL` перед продолжением вычисления. Если `HiredDate` значение `NULL`, мы просто возвращают строку «Unknown»; Если это не `NULL`, мы вычисляем разницу между текущим временем и `HiredDate` значение и возвращает количество дней.

Чтобы использовать этот метод необходимо вызвать из TemplateField в GridView, используя синтаксис привязки данных. Начните с добавления новых TemplateField к GridView, щелкнув ссылку Правка столбцов GridView смарт-тег и добавление новых TemplateField.


[![Добавить новый TemplateField в GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Рис. 15**: Добавление новых TemplateField GridView ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


Задайте новый TemplateField `HeaderText` свойства «Дней на задание» и его `ItemStyle` `HorizontalAlign` свойства `Center`. Для вызова `DisplayDaysOnJob` метода на основе шаблона, добавьте `ItemTemplate` и используйте следующий синтаксис привязки данных:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` Возвращает `DataRowView` объект, соответствующий `DataSource` привязанной к `GridViewRow`. Его `Row` свойство возвращает строго типизированный `Northwind.EmployeesRow`, который передается `DisplayDaysOnJob` метод. Этот синтаксис привязки данных может стоять непосредственно в `ItemTemplate` (как показано в приведенном ниже синтаксисе декларативных) или могут быть назначены `Text` свойство метки веб-элемент управления.

> [!NOTE]
> Кроме того, вместо передачи в `EmployeesRow` экземпляр, можно просто передать в `HireDate` с использованием `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Тем не менее `Eval` возвращает `object`, поэтому нам пришлось бы изменить наши `DisplayDaysOnJob` сигнатуру метода, чтобы принять входного параметра типа `object`, вместо этого. Нам не удается привести слепо `Eval("HireDate")` вызов `DateTime` из-за `HireDate` столбца в `Employees` таблица может содержать `NULL` значения. Таким образом, необходимо принять `object` как входной параметр для `DisplayDaysOnJob` метод, проверьте, были ли базы данных `NULL` значение (которого может выполняться с помощью `Convert.IsDBNull(objectToCheck)`), а затем продолжайте соответствующим образом.


Из-за этих тонкостей мы удалили передать во всем `EmployeesRow` экземпляра. В следующем уроке мы рассмотрим более удачный пример использования `Eval("columnName")` для передачи входного параметра в метод форматирования.

Ниже показан декларативный синтаксис для наших GridView, после добавления TemplateField и `DisplayDaysOnJob` метод, вызываемый из `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

На рисунке 16 показано завершенного учебника при просмотре через браузер.


[![Отображается число дней для задания была сотрудника](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**На рисунке 16**: число дней сотрудник был задания отображается ([Просмотр полноразмерное изображение](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>Сводка

TemplateField в элементе управления GridView обеспечивает более высокую степень гибкости в отображении данных, чем другие элементы управления полями. TemplateFields идеально подходят для ситуаций, где:

- Несколько полей данных должны отображаться в одном столбце GridView
- Данные лучше представить с помощью веб-элемент управления, а не обычного текста
- Результат зависит от базовых данных, таких как отображение метаданных или переформатирования данных

Помимо настройки внешнего вида данных, TemplateFields также используются для настройки пользовательских интерфейсов для редактирования и вставка данных, как будет видно в будущих учебниках.

Два следующих учебников поработать шаблоны, начиная с рассмотрим использование TemplateFields в DetailsView. После этого мы перейдем к FormView, который использует шаблоны вместо поля обеспечивают большую гибкость в макет и структуру данных.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было (Dan Jagers). Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](custom-formatting-based-upon-data-cs.md)
> [Вперед](using-templatefields-in-the-detailsview-control-cs.md)
