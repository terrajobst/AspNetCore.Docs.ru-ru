---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Использование полей TemplateField в элементе управления GridView (VB) | Документация Майкрософт
author: rick-anderson
description: Для обеспечения гибкости GridView предлагает TemplateField, на которой отображается с помощью шаблона. Шаблон может быть смесь статического кода HTML, веб-элементы управления, и...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6090c97e12736fb83938e9da5991d19ed50463b6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837076"
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>Использование полей TemplateField в элементе управления GridView (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) или [скачать PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Для обеспечения гибкости GridView предлагает TemplateField, на которой отображается с помощью шаблона. Шаблон может быть смесь статического кода HTML, веб-элементы управления и синтаксис привязки данных. В этом руководстве мы рассмотрим использование TemplateField для достижения большего уровня настройки с помощью элемента управления GridView.


## <a name="introduction"></a>Вступление

GridView представляет собой набор полей, которые определяет, какие свойства `DataSource` должны включаться в выводимые данные, а также способ отображения данных. Простейший тип поля — поле типа BoundField, который отображает значение данных в виде текста. Поля других типов при отображении данных используют различные элементы HTML. CheckBoxField, например, отображаемое как флажок, состояние которого зависит от значения поля данных; ImageField выводит на экран изображение, источник которого создается на основе поля данных. Гиперссылки и кнопки, состояние которого зависит от базового значения поля данных могут быть представлены с помощью поля HyperLinkField и ButtonField типы полей.

Если типы полей CheckBoxField, ImageField, HyperLinkField и ButtonField необходимы для альтернативного представления данных, они все же весьма ограниченны с точки зрения форматирования. По полю CheckBoxField отображаются только одного флажка, тогда как ImageField можно отобразить только одно изображение. Что делать, если необходимо включать текст, флажок, определенного поля *и* изображения, все зависимости от значений полей данных? Или, что делать, если мы захотели бы отображать данные с помощью веб-элемент управления CheckBox, изображения, гиперссылки или кнопку? Кроме того поле типа BoundField ограничивает область отображения одного поля данных. Что делать, если нам нужно вывести несколько значений полей данных в одном столбце GridView?

Для обеспечения такого уровня гибкости GridView предлагает TemplateField, на которой отображается с помощью *шаблона*. Шаблон может быть смесь статического кода HTML, веб-элементы управления и синтаксис привязки данных. Кроме того TemplateField имеется широкий набор шаблонов, которые можно использовать для настройки отображения различных ситуациях. Например `ItemTemplate` используется по умолчанию для отображения ячейки в каждой строке, но `EditItemTemplate` шаблон можно использовать для настройки интерфейса при изменении данных.

В этом руководстве мы рассмотрим использование TemplateField для достижения большего уровня настройки с помощью элемента управления GridView. В [предыдущем учебном курсе](custom-formatting-based-upon-data-vb.md) мы научились изменять форматирование в зависимости от нижележащих данных при помощи `DataBound` и `RowDataBound` обработчики событий. Другой способ настройки форматирования в зависимости от базовых данных путем вызова методов форматирования из шаблона. В этом руководстве также мы рассмотрим этот метод.

В этом руководстве мы будем использовать поля TemplateField для оформления списка сотрудников. В частности, мы составим список всех сотрудников, но отобразит его имя и фамилию в один столбец, дате найма в элемента управления календарем, а также состояние столбца, который указывает, сколько дней они был принят на работу компании.


[![Три поля TemplateField используются для настройки отображения](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Рис. 1**: три поля TemplateField используются для настройки отображения ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Шаг 1: Привязка данных к GridView

В службах reporting сценариях, где необходимо использование полей TemplateField для настройки внешнего вида, я нахожу проще всего начать с создания элемента управления GridView, который содержит только поля BoundField, кроме сначала и затем для добавления нового поля TemplateField или преобразовать существующие поля BoundFields в Поля TemplateField, при необходимости. Таким образом давайте начнем этот учебник, путем добавления элемента управления GridView к странице через конструктор и ее привязки к элементу ObjectDataSource, который возвращает список сотрудников. Эти действия создадут GridView с помощью полей BoundField для каждого из полей employee.

Откройте `GridViewTemplateField.aspx` странице и перетащите элемент управления GridView с панели инструментов в конструктор. Смарт-теге элемента GridView выберите Добавление нового элемента управления ObjectDataSource, который вызывает `EmployeesBLL` класса `GetEmployees()` метод.


[![Добавить новый элемент управления ObjectDataSource, вызывающего метод GetEmployees()](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Рис. 2**: добавьте новый элемент управления ObjectDataSource, Invokes `GetEmployees()` метод ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


Привязка элемента GridView таким образом автоматически добавит поле BoundField для каждого свойства сотрудника: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, и `Country`. Для этого отчета давайте не возиться с отображением `EmployeeID`, `ReportsTo`, или `Country` свойства. Чтобы удалить эти поля BoundField, вы можете:

- Используйте поля диалоговом окне щелкните ссылку Правка столбцов смарт-теге элемента GridView, чтобы открыть это диалоговое окно. Далее выберите полей BoundField в нижнем левом углу списка и щелкните красный значок X кнопку для удаления BoundField.
- Изменить декларативный синтаксис элемента GridView вручную из представления источников, удалите `<asp:BoundField>` элемента для типа BoundField, нужно удалить.

После удаления `EmployeeID`, `ReportsTo`, и `Country` полей BoundField, разметку элемента GridView должен выглядеть так:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Отвлекитесь и просмотрите ход работы в браузере. На этом этапе вы должны увидеть таблицу, содержащую запись для каждого сотрудника и четыре столбца: один для сотрудника Фамилия, один для их имя, один для названий и один к дате найма.


[![LastName, FirstName, Title и HireDate поля отображаются для каждого сотрудника](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Рис. 3**: `LastName`, `FirstName`, `Title`, и `HireDate` отображаются поля для каждого сотрудника ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Шаг 2: Отображение имени и фамилии в одном столбце

В настоящее время каждого сотрудника и фамилии, отображаются в отдельном столбце. Возможно, желательно объединить их в один столбец. Для этого нам нужно использовать поле TemplateField. Мы можете Добавление нового поля TemplateField, добавить в него нужную разметку и синтаксис привязки данных и затем удалить `FirstName` и `LastName` можно преобразовать полю BoundFields или мы `FirstName` BoundField в поле TemplateField, изменить TemplateField для включения `LastName` значение, а затем удалите `LastName` BoundField.

Тот же результат в обоих случах, но лично я предпочитаю преобразование полей BoundField в поля TemplateField, когда это возможно, так как преобразование автоматически добавляет `ItemTemplate` и `EditItemTemplate` с веб-элементов управления и синтаксис привязки данных для моделирования внешнего вида и поле типа BoundField функциональные возможности. Это удобно, что нам потребуется меньше работы по созданию TemplateField как процесс преобразования выполняет часть работы за нас.

Чтобы преобразовать существующий BoundField в поле TemplateField, щелкните ссылку Изменить столбцы смарт-теге элемента GridView, открывая диалоговое окно "поля". Выберите поле типа BoundField предназначенным для преобразования в списке в левом нижнем углу и затем щелкните ссылку «Convert это поле в TemplateField» в правом нижнем углу.


[![Преобразование поля BoundField в поле TemplateField в диалоговом окне поля](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Рис. 4**: преобразование типа BoundField в поле TemplateField в диалоговом окне поля ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


Преобразуйте `FirstName` BoundField в поле TemplateField. После этого изменения есть нет никаких видимых перемен в конструкторе. Это обусловлено преобразование BoundField в поле TemplateField создает поддерживает вид BoundField поле TemplateField. Несмотря на то, что никакой разницы в этот момент в конструкторе, процессе преобразования декларативный синтаксис BoundField декларативный синтаксис - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` — со следующим синтаксисом TemplateField:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Как вы видите, TemplateField состоит из двух шаблонов `ItemTemplate` , имеет метку, `Text` свойству присваивается значение `FirstName` поля данных и `EditItemTemplate` с элементом TextBox со свойством `Text` также установлено свойство Чтобы `FirstName` поля данных. Синтаксис привязки данных — `<%# Bind("fieldName") %>` -указывает, что поле данных *`fieldName`* привязан к указанному свойству элемента управления Web.

Чтобы добавить `LastName` значение этого поля TemplateField, нам нужно добавить другой элемент управления Label Web в поля данных `ItemTemplate` и привязать его `Text` свойства `LastName`. Это можно сделать вручную или с помощью конструктора. Чтобы сделать это вручную, просто добавьте соответствующий декларативный синтаксис для `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Чтобы добавить его в конструкторе, щелкните ссылку Изменить шаблоны смарт-теге элемента GridView. Отображает интерфейс редактирования шаблона элемента GridView. В этом смарт-теге этого интерфейса является список шаблонов в GridView. Поскольку у нас имеется только один TemplateField на этом этапе, только шаблоны, использованные в раскрывающемся списке представлены эти шаблоны `FirstName` TemplateField вместе с `EmptyDataTemplate` и `PagerTemplate`. `EmptyDataTemplate` Шаблон, если указано, используется для отображения выходных данных элемента GridView в том случае, если данные, привязанные к элементу GridView; нет результатов `PagerTemplate`, если он указан, используется для отображения интерфейса разбиения на страницы для элемента управления GridView, который поддерживает разбиение по страницам.


[![В конструкторе можно изменить шаблоны элемента GridView](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Рис. 5**: элемента GridView шаблоны могут быть изменены через конструктор ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


Чтобы отобразить также `LastName` в `FirstName` TemplateField перетащите элемент управления Label из области элементов в `FirstName` TemplateField `ItemTemplate` в GridView изменения шаблонов интерфейса.


[![Добавить веб-элемент управления Label ItemTemplate FirstName TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Рис. 6**: Добавление веб-элемент управления Label к `FirstName` TemplateField ItemTemplate ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


На этом этапе содержит элемент управления Label Web, добавленный TemplateField его `Text` , имеющим значение «Label». Нам нужно изменить таким образом, чтобы это свойство привязано к значению `LastName` поле данных. Для выполнения этого щелкнуть смарт-тега элемента управления Label и выберите пункт Edit DataBindings.


[![Выберите параметр "Изменить" Привязка данных из смарт-тега метки](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Рис. 7**: выберите Редактирование привязок данных с смарт-тега метки ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


Откроется диалоговое окно Привязка данных. Отсюда можно выбрать свойство участвовать в привязку данных из списка слева и выберите поле для привязки данных в раскрывающемся списке справа. Выберите `Text` в левом и `LastName` справа и нажмите кнопку ОК.


[![Привязки свойства Text к полю данных LastName](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Рис. 8**: привязка `Text` свойства `LastName` поля данных ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> Привязка данных диалоговое окно позволяет указать, следует ли выполнять двусторонней привязки данных. Если оставить этот флажок снят, синтаксис привязки данных `<%# Eval("LastName")%>` будет использоваться вместо `<%# Bind("LastName")%>`. Для этого учебника подходит любой из этих подходов. Двусторонняя привязка к данным также важно при вставке или изменении данных. Для отображения данных, однако оба подхода будут работать одинаково хорошо. Мы обсудим двусторонней привязки данных, подробно в последующих учебных курсах.


Отвлекитесь и просмотреть эту страницу через обозреватель. Как вы видите, GridView по-прежнему имеет четыре столбца; Тем не менее `FirstName` теперь стоят *оба* `FirstName` и `LastName` значений полей данных.


[![FirstName и LastName значения отображаются в одном столбце](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Рис. 9**: как `FirstName` и `LastName` значения отображаются в одном столбце ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


Чтобы завершить этот первый шаг, удалите `LastName` BoundField и переименуйте `FirstName` TemplateField `HeaderText` значения свойства «Имя». После внесения этих изменений декларативная разметка GridView должен выглядеть следующим образом:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![Первый и фамилии каждого сотрудника отображаются в одном столбце](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Рис. 10**: первый и фамилии каждого сотрудника отображаются в одном столбце ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Шаг 3: Использование элемента управления Calendar для отображения`HiredDate`поля

Отображение значения поля данных в виде текста в элементе управления GridView, как просто использовать BoundField. В некоторых сценариях тем не менее, данные удобнее представить в конкретной веб-элемента управления текста, а. Подобная настройка отображения данных возможна с помощью поля TemplateField. Например, а не отобразить дату приема сотрудника как текст, можно было показать календаря (с помощью [элемента управления Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) с дате найма выделены.

Для этого сначала преобразуйте `HiredDate` BoundField в поле TemplateField. Просто перейдите к смарт-теге элемента GridView и щелкните ссылку Изменить столбцы, открывая диалоговое окно "поля". Выберите `HiredDate` BoundField и нажмите кнопку «Преобразовать это поле в TemplateField.»


[![Преобразовать HiredDate BoundField в поле TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Рис. 11**: преобразовать `HiredDate` BoundField в поля TemplateField ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


Как мы видели в шаге 2, это приводит к замене BoundField поле TemplateField содержит `ItemTemplate` и `EditItemTemplate` с метку и текстовое поле, `Text` свойства привязаны к `HiredDate` значение, используя синтаксис привязки данных `<%# Bind("HiredDate")%>`.

Чтобы заменить текст элемента управления календарем, измените шаблон путем удаления метки и добавления элемента управления календаря. В конструкторе щелкните Изменить шаблоны смарт-теге элемента GridView и выберите `HireDate` TemplateField `ItemTemplate` из раскрывающегося списка. Затем удалите элемент управления Label и перетащите элемент управления календаря с панели инструментов в интерфейс редактирования шаблона.


[![Добавьте элемент управления "Календарь" для HireDate TemplateField ItemTemplate](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Рис. 12**: добавьте элемент управления "Календарь" для `HireDate` TemplateField `ItemTemplate` ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


На этом этапе каждая строка в GridView будет содержать элемент управления "Календарь" в его `HiredDate` TemplateField. Однако дата приема сотрудника фактическое `HiredDate` значение не задано в любом месте в элементе управления календаря, вызывая каждый элемент управления Calendar показывают текущий месяц и дату по умолчанию. Чтобы исправить это, нам нужно назначить каждому сотруднику `HiredDate` для элемента управления Calendar [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) и [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) свойства.

В смарт-тега элемента управления Calendar выберите пункт Edit DataBindings. Затем привяжите `SelectedDate` и `VisibleDate` свойства `HiredDate` поля данных.


[![Привязка свойства SelectedDate и VisibleDate к полю данных HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Рис. 13**: привязка `SelectedDate` и `VisibleDate` свойства `HiredDate` поля данных ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> Календарь совсем не обязательно видимыми. Например, может быть 1 августа календаре<sup>st</sup>, 1999 выделенной датой, а показывать текущий месяц и год. Выделенная и видимая Дата указываются с элемента управления Calendar `SelectedDate` и `VisibleDate` свойства. Поскольку нам требуется, выберите оба сотрудника `HiredDate` и убедитесь, что он отображается, мы должны привязать оба свойства к `HireDate` поля данных.


При просмотре страницы в браузере, в календаре теперь Показывать дату приема сотрудника и выбирает этой определенной даты.


[![HiredDate сотрудника отображается в элементе управления Calendar](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Рис. 14**: сотрудника `HiredDate` отображается в элементе управления Calendar ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> В отличие от во всех примерах, мы видели в данный момент, в этом руководстве мы сделали *не* задать `EnableViewState` свойства `False` для этого GridView. Причина такого подхода обусловлено тем, когда пользователь щелкает даты элемента управления Calendar вызывает обратную передачу, значение даты просто нажать кнопку выбранной даты календаря. При отключении состояния представления элемента GridView, тем не менее, при каждой обратной передаче элемента GridView привязываются к его базового источника данных, что приводит к выбранной даты календаря устанавливается *обратно* сотрудника `HireDate`, перезапись Дата, выбранного пользователем.


В этом руководстве это более чем рассуждения, поскольку пользователь не сможет обновить сотрудника `HireDate`. Что, вероятно, будет лучше для настройки элементов управления календарем, таким образом, чтобы его даты не могут быть выбраны. Тем не менее в этом руководстве показано, что в некоторых случаях состояние представления должно быть включено для предоставления определенных функциональных возможностей.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Шаг 4: Отображается число дней Сотрудник работает в компании

Пока мы рассмотрели два варианта из полей TemplateField:

- Объединение двух или нескольких значений полей данных в один столбец и
- Выражения значения поля данных, с помощью веб-элемент управления вместо текста

Третий использование полей TemplateField в отображении метаданные о GridView базовые данные. Кроме возможности просматривать дату приема сотрудника, например, мы может также потребоваться столбец, отображающий общее число дней, они имели на задание.

Еще другой использование полей TemplateField возникает в сценариях, когда базовые данные должны отображаться по-разному в веб-страницы отчета в формате, он хранится в базе данных. Представим, что `Employees` таблица имела `Gender` поле, хранятся в виде букв `M` или `F` для указания пола сотрудника. При отображении этой информации на веб-странице мы хотим видеть полные как «Male» или «Female», а не просто «M» или «F».

Оба этих сценария могут быть обработаны создание *метод форматирования* в классе фонового кода страницы ASP.NET (или в отдельной библиотеке классов, реализованы в виде `Shared` метод), вызываемой из шаблона. Этот метод форматирования вызывается из шаблона, используя тот же синтаксис привязки данных, который использовался раньше. Метод форматирования может занять в любое количество параметров, но должен вернуть строку. Эта строка представляет HTML-код, который вставляется в шаблон.

Для иллюстрации этой концепции, попробуем создать столбец, в которой перечислены общее число дней, которое сотрудник провел на работе. Этот метод форматирования будет принимать `Northwind.EmployeesRow` и не возвращать число дней, сотрудник принят на работу как строка. Этот метод можно добавить вспомогательном классе страницы ASP.NET, но *необходимо* быть помечен как `Protected` или `Public` чтобы быть доступным из шаблона.


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Так как `HiredDate` поле может содержать `NULL` базы данных значения, нам сначала нужно убедиться, что значение не `NULL` перед продолжением вычисления. Если `HiredDate` значение `NULL`, мы просто возвращают строку «Unknown»; Если это не `NULL`, мы вычисляем разницу между текущей датой и `HiredDate` значение и возврат числа дней.

Чтобы использовать этот метод необходимо вызвать из TemplateField в GridView, используя синтаксис привязки данных. Начните с добавления нового поля TemplateField к GridView, щелкнув ссылку Изменить столбцы в смарт-теге элемента GridView и Добавление нового поля TemplateField.


[![Добавление нового поля TemplateField GridView](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Рис. 15**: Добавление нового поля TemplateField к GridView ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


Значение этого нового поля TemplateField `HeaderText` свойства «Дней на задание» и его `ItemStyle` `HorizontalAlign` свойства `Center`. Для вызова `DisplayDaysOnJob` метод на основе шаблона, добавить `ItemTemplate` и используйте следующий синтаксис привязки данных:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` Возвращает `DataRowView` объект, соответствующий `DataSource` привязанной к `GridViewRow`. Его `Row` свойство возвращает строго типизированный `Northwind.EmployeesRow`, который передается `DisplayDaysOnJob` метод. Этот синтаксис привязки данных может стоять непосредственно в `ItemTemplate` (как показано в декларативном синтаксисе ниже) или могут быть назначены `Text` свойства элемента управления Label Web.

> [!NOTE]
> Кроме того, вместо передачи в `EmployeesRow` экземпляр, можно просто передать в `HireDate` с использованием синтаксиса `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Тем не менее `Eval` возвращает `Object`, поэтому мы должны внести изменения наших `DisplayDaysOnJob` сигнатуру метода, чтобы принять входной параметр типа `Object`вместо нее. Мы не можем `Eval("HireDate")` вызов `DateTime` поскольку `HireDate` столбца в `Employees` таблица может содержать `NULL` значения. Таким образом, необходимо принять `Object` как входной параметр для `DisplayDaysOnJob` метод, проверьте, были ли базы данных `NULL` значение (которого может выполняться с помощью `Convert.IsDBNull(objectToCheck)`), а затем продолжайте соответствующим образом.


Из-за этих тонкостей мы решили передать во всем `EmployeesRow` экземпляра. В следующем учебном курсе мы рассмотрим более удачный пример использования `Eval("columnName")` синтаксис для передачи входного параметра в метод форматирования.

Ниже приведен декларативный синтаксис для наших GridView после добавления TemplateField и `DisplayDaysOnJob` метод, вызываемый из `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

Рис. 16 показаны результаты всей работы, выполненной работы в браузере.


[![Отображается число дней, сотрудник провел на работе](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Рис. 16**: число дней сотрудник провел на работе отображается ([Просмотр полноразмерного изображения](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>Сводка

TemplateField в элементе управления GridView позволяет более высокий уровень гибкости в отображении данных, чем другие элементы управления полями. Поля TemplateField идеальны для ситуаций, где:

- Несколько полей данных должны отображаться в одном столбце GridView
- Данные удобнее представить в веб-элемента управления, а не обычный текст
- Результат зависит от базовых данных, такие как отображение метаданных или переформатирования данных

Помимо настройки внешнего вида данных, поля TemplateField также используются для настройки пользовательских интерфейсов для правки и вставки данных, как мы увидим в последующих руководствах.

Следующих двух учебных курсах мы продолжим, изучение шаблонов, начиная с взгляда на использование полей TemplateField в элементе управления DetailsView. После этого мы перейдем к элементу FormView, который использует шаблоны вместо полей для обеспечения большей гибкости при размещении и структурировании данных.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был (Dan Jagers). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](custom-formatting-based-upon-data-vb.md)
> [Вперед](using-templatefields-in-the-detailsview-control-vb.md)
