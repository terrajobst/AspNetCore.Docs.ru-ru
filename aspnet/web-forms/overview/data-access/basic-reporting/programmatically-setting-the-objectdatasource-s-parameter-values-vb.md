---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Программная установка значений параметров ObjectDataSource (VB) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы рассмотрим добавление метода к DAL и BLL, который принимает один входной параметр и возвращает данные. Этот параметр будет установлен пример...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: d779de4f5bd0d03f413237689e5a64330fcb491d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825786"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Программная установка значений параметров ObjectDataSource (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) или [скачать PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> В этом руководстве мы рассмотрим добавление метода к DAL и BLL, который принимает один входной параметр и возвращает данные. Этот параметр будет установлен пример программным образом.


## <a name="introduction"></a>Вступление

Как мы видели в [предыдущем учебном курсе](declarative-parameters-vb.md), доступны несколько вариантов декларативной передачи значений параметров методам ObjectDataSource. Если значение параметра жестко, поступают из веб-элемент управления на странице или в любой другой источник, доступный для чтения в источнике данных `Parameter` объекта, например, что значение может быть привязано входному параметру без написания кода.

Иногда, тем не менее, если значение параметра поступает из источника не учитываемого одним из источника данных, встроенные `Parameter` объектов. Если узел поддерживает учетные записи пользователей, мы может потребоваться задать параметр, в зависимости от текущего зарегистрированного посетителя идентификатор пользователя Или же может потребоваться настройка значения параметра перед его отправкой методу нижележащего объекта для ObjectDataSource.

Каждый раз, когда ObjectDataSource `Select` вызывается метод ObjectDataSource прежде всего создает его [события Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Затем вызывается метод нижележащего объекта для ObjectDataSource. По завершении работы, ObjectDataSource [выбранные события](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) активируется (рис. 1 показана последовательность событий). Значения параметров, передаваемые в метод нижележащего объекта для ObjectDataSource можно установить или настроить в обработчик событий для `Selecting` событий.


[![Выбранные ObjectDataSource и выбрав ObjectDataSource возникают до и после нижележащего объекта метод вызывается](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Рис. 1**: The ObjectDataSource `Selected` и `Selecting` вызывается метод ObjectDataSource возникают до и после нижележащего объекта ([Просмотр полноразмерного изображения](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


В этом учебнике будет рассмотрено Добавление метода к DAL и BLL, который принимает один входной параметр `Month`, типа `Integer` и возвращает `EmployeesDataTable` заполненный сотрудниками, годовщина приема которых их в указанном `Month`. Наш пример будет установить этот параметр, программными средствами на основе текущего месяца, отображая список «Годовщины сотрудников в этом месяце.»

Давайте начнем!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Шаг 1: Добавление метода к`EmployeesTableAdapter`

Для нашего первого примера, нам нужно добавить возможность получения сотрудников, `HireDate` произошла в указанном месяце. Для поддержки этой функции в соответствие с нашей архитектурой необходимо сначала создать метод в `EmployeesTableAdapter` , сопоставляемый надлежащему оператору SQL. Для этого сначала откроем типизированный набор DataSet Northwind. Щелкните правой кнопкой мыши `EmployeesTableAdapter` метки и выберите Добавить запрос.


[![Добавить новый запрос в EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Рис. 2**: Добавление нового запроса к `EmployeesTableAdapter` ([Просмотр полноразмерного изображения](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Выберите инструкцию SQL, которая возвращает строки. По достижении Specify a `SELECT` инструкции на экране по умолчанию `SELECT` инструкции для `EmployeesTableAdapter` уже будет загружен. Просто добавьте в `WHERE` предложение: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) — это функция T-SQL, возвращающая определенную часть даты `datetime` типа; в данном случае мы используем `DATEPART` для возврата месяца столбца `HireDate` столбца.


[![Возвращаемое только тех, где HireDate столбец строки меньше или равно @HiredBeforeDate параметр](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Рис. 3**: возвращает только те строки, у которых `HireDate` столбца меньше или равно `@HiredBeforeDate` параметра ([Просмотр полноразмерного изображения](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Наконец, измените `FillBy` и `GetDataBy` имена метод `FillByHiredDateMonth` и `GetEmployeesByHiredDateMonth`, соответственно.


[![Выбор более подходящих имен методов, чем FillBy и GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Рис. 4**: выберите более соответствующий метод имена чем `FillBy` и `GetDataBy` ([Просмотр полноразмерного изображения](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Нажмите кнопку Готово, чтобы завершить работу мастера и вернуться в область конструктора набора данных. `EmployeesTableAdapter` Должен включать новый набор методов для доступа к сотрудникам, принятым на работу в указанном месяце.


[![В области проектирования DataSet появились новые методы](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Рис. 5**: новые методы, отображаются в области проектирования DataSet ([Просмотр полноразмерного изображения](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Шаг 2: Добавление`GetEmployeesByHiredDateMonth(month)`метод уровня бизнес-логики

Поскольку логики доступа к архитектуре нашего приложения используется отдельный слой для бизнес-логику и данные, необходимо добавить метод BLL, вызов DAL для получения сотрудников на работу до определенной даты. Откройте `EmployeesBLL.vb` файл и добавьте следующий метод:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Как и другие методы в этом классе `GetEmployeesByHiredDateMonth(month)` просто вызывает DAL и возвращает результаты.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Шаг 3: Отображение сотрудников, годовщина приема которых находится в этом месяце

Последним этапом в этом примере является отображение сотрудников, годовщина приема которых находится в этом месяце. Начните с добавления элемент управления GridView для `ProgrammaticParams.aspx` странице в `BasicReporting` папку и добавьте новый ObjectDataSource в качестве источника данных. Настройка ObjectDataSource на использование `EmployeesBLL` класса `SelectMethod` присвоено `GetEmployeesByHiredDateMonth(month)`.


[![Класс EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Рис. 6**: использование `EmployeesBLL` класс ([Просмотр полноразмерного изображения](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Выберите из GetEmployeesByHiredDateMonth(month)-метод](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Рис. 7**: Select From `GetEmployeesByHiredDateMonth(month)` метод ([Просмотр полноразмерного изображения](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


На последнем экране появляется нам предоставить `month` источника значения параметра. Так как мы это значение программно, оставьте значение источника параметра по умолчанию ни один параметр и нажмите кнопку Готово.


[![Оставьте исходный набор параметров значение None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Рис. 8**: оставьте параметру источника значение None ([Просмотр полноразмерного изображения](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Это создаст `Parameter` объекта в коллекции `SelectParameters` коллекции не поддерживает указанное значение.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Чтобы задать это значение программно, необходимо создать обработчик событий для элемента управления ObjectDataSource `Selecting` событий. Для этого перейдите в режим конструктора и дважды щелкните ObjectDataSource. Кроме того выберите элемент управления ObjectDataSource, перейти в окно свойств и щелкните значок с молнией. Затем дважды щелкните в текстовом поле рядом с полем `Selecting` , либо введите имя обработчика событий, которые вы хотите использовать. Как третий вариант, можно создать обработчик событий, выбрав ObjectDataSource и его `Selecting` события из двух раскрывающихся списках в верхней части класса фонового кода страницы.


![Щелкните значок с молнией в окне «Свойства», чтобы получить список событий веб-элемента управления](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Рис. 9**: щелкните значок с молнией в окне «Свойства», чтобы получить список событий веб-элемента управления


Все три подхода добавить новый обработчик событий для элемента управления ObjectDataSource `Selecting` событий к вспомогательному классу страницы. В этом обработчике событий можно чтения и записи значений параметров, используя `e.InputParameters(parameterName)`, где *`parameterName`* значение `Name` атрибут в `<asp:Parameter>` тега ( `InputParameters` коллекция может также быть индексированные порядковое, как показано на `e.InputParameters(index)`). Чтобы задать `month` параметра, равного текущему месяцу, добавьте следующий код в `Selecting` обработчик событий:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

При просмотре страницы через обозреватель мы видим, что только один сотрудник был принят на работу в этом месяце (март) Лора Каллахан, которая была в компании с 1994 года.


[![Показаны сотрудники с годовщиной отображаются в этом месяце](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Рис. 10**: те, сотрудники которых годовщины этот месяц отображаются ([Просмотр полноразмерного изображения](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Сводка

Хотя значения параметров ObjectDataSource обычно могут быть установлены декларативно, без единой строчки кода, это просто установить значения параметров программно. Нам нужно лишь создать обработчик событий для элемента управления ObjectDataSource `Selecting` событий, который запускается до вызова метода базового объекта и вручную установить значения для одного или нескольких параметров через `InputParameters` коллекции.

Этот учебник завершает раздел основной отчетности. [Следующему руководству](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) запускает фильтрации и сценариях основной-подробности раздел, в котором мы рассмотрели методы предоставления посетителям возможностей фильтрации данных и перехода от основного отчета в отчет сведения.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был (Hilton giesenow). Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](declarative-parameters-vb.md)
