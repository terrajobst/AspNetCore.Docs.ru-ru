---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: "Программное задание значений параметров ObjectDataSource (VB) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы рассмотрим добавление метода к DAL и МЕТОДА, который принимает один входной параметр и возвращает данные. В примере будет задан этот параметр..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f84558bcc59068f2c6cab390c303ebd97953aaa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Программное задание значений параметров ObjectDataSource (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) или [скачать PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> В этом учебнике мы рассмотрим добавление метода к DAL и МЕТОДА, который принимает один входной параметр и возвращает данные. В примере будет задан этот параметр программными средствами.


## <a name="introduction"></a>Вступление

Как мы видели в [с предыдущим учебником](declarative-parameters-vb.md), декларативной передачи значений параметров методам ObjectDataSource доступно несколько параметров. Если значение параметра задано жестко, поступают из веб-элемента управления на странице или в любой другой источник доступен для чтения в источнике данных `Parameter` объекта, например, что значение может быть привязано к входному аргументу; без написания кода.

Иногда, тем не менее, когда значение параметра поступает из источника, не учитываемого одним из источника данных, встроенные `Parameter` объектов. Если узел поддерживает учетные записи пользователей может потребоваться присвоить параметру, основанному на текущего выполнившего вход пользователя посетителя по идентификатору. Или же может потребоваться настройка значения параметра перед его отправкой методу базового объекта ObjectDataSource.

Каждый раз, когда элемент управления ObjectDataSource `Select` вызывается метод сначала вызывает ObjectDataSource его [события Selecting](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Затем вызывается метод ObjectDataSource базового объекта. Завершении ObjectDataSource [выбранные события](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) активируется (рис. 1 иллюстрирует эту последовательность событий). Можно задать или настроить в обработчик событий для значений параметров, передаваемых в метод базового объекта ObjectDataSource `Selecting` событий.


[![Выбранные ObjectDataSource и выбрав события возникают до и после нижележащего объекта метод вызывается](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Рис. 1**: ObjectDataSource `Selected` и `Selecting` вызывается метод Fire события до и после нижележащего объекта ([Просмотр полноразмерное изображение](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


В этом учебнике мы рассмотрим добавление метода к DAL и МЕТОДА, который принимает один входной параметр `Month`, типа `Integer` и возвращает `EmployeesDataTable` заполненный сотрудниками, их найма окончания действия в указанном `Month`. Наш пример будет установить этот параметр, программными средствами на основе текущего месяца, отображая список «Годовщины сотрудников в этом месяце.»

Давайте начнем!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Шаг 1: Добавление метода в`EmployeesTableAdapter`

Для первого примера необходимо добавить возможность получения сотрудников, `HireDate` произошла в указанном месяце. Для поддержки этой функции в соответствии с нашей архитектурой, необходимо сначала создать метод в `EmployeesTableAdapter` сопоставляемого соответствующие инструкции SQL. Чтобы добиться этого, сначала откройте типизированного набора данных "Борей". Щелкните правой кнопкой мыши `EmployeesTableAdapter` метки и выберите Добавить запрос.


[![Добавление нового запроса в EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**На рисунке 2**: Добавление нового запроса к `EmployeesTableAdapter` ([Просмотр полноразмерное изображение](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Выберите добавление инструкции SQL, возвращающей строки. По достижении укажите `SELECT` инструкции на экране по умолчанию `SELECT` инструкции для `EmployeesTableAdapter` уже будет загружен. Просто добавьте в `WHERE` предложение: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/en-us/library/ms174420.aspx) — функция T-SQL, которая возвращает конкретную дату часть `datetime` тип; в этом случае мы используем `DATEPART` для возврата месяц `HireDate` столбца.


[![Возврат только тех строк где HireDate столбца меньше или равно @HiredBeforeDate параметр](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Рис. 3**: возвращает только те строки, у которых `HireDate` столбца меньше или равно `@HiredBeforeDate` параметра ([Просмотр полноразмерное изображение](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Наконец, измените `FillBy` и `GetDataBy` имена метод `FillByHiredDateMonth` и `GetEmployeesByHiredDateMonth`соответственно.


[![Выберите более подходящих имен методов, чем FillBy и GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Рис. 4**: выберите более соответствующий метод имена чем `FillBy` и `GetDataBy` ([Просмотр полноразмерное изображение](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Нажмите кнопку Готово, чтобы завершить работу мастера и вернуться в область конструктора набора данных. `EmployeesTableAdapter` Должен включать новый набор методов для доступа к сотрудников, принятых на работу в указанном месяце.


[![Появились новые методы в область конструктора набора данных](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Рис. 5**: новые методы, отображаются в области конструктора DataSet ([Просмотр полноразмерное изображение](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Шаг 2: Добавление`GetEmployeesByHiredDateMonth(month)`метод уровня бизнес-логики

Поскольку логики доступа к архитектуре нашего приложения используется отдельный слой для бизнес-логику и данные, необходимо добавить метод наш уровень бизнес-ЛОГИКИ, вызов DAL для получения сотрудников на работу до определенной даты. Откройте `EmployeesBLL.vb` файл и добавьте следующий метод:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Как и другие методы в этом классе `GetEmployeesByHiredDateMonth(month)` просто вызывает DAL и возвращает результаты.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Шаг 3: Для отображения сотрудников, чьи найма окончания действия — в этом месяце

Последним этапом в этом примере является отображение сотрудников которого найма окончания действия — в этом месяце. Начните с добавления GridView к `ProgrammaticParams.aspx` страницы в `BasicReporting` папку и добавьте новый элемент управления ObjectDataSource в качестве источника данных. Настройка ObjectDataSource для использования `EmployeesBLL` класса `SelectMethod` значение `GetEmployeesByHiredDateMonth(month)`.


[![Используйте класс EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Рис. 6**: использование `EmployeesBLL` класса ([Просмотр полноразмерное изображение](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Выберите из GetEmployeesByHiredDateMonth(month)-метод](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Рис. 7**: Select From `GetEmployeesByHiredDateMonth(month)` метод ([Просмотр полноразмерное изображение](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


На последнем экране появляется нам для предоставления `month` источник значения параметра. Поскольку нам это значение программными средствами, оставьте значение источника параметра по умолчанию None и нажмите кнопку Готово.


[![Оставьте ни один набор параметров источника](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Рис. 8**: оставьте параметр источника, значение None ([Просмотр полноразмерное изображение](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Это создаст `Parameter` объекта в элемент управления ObjectDataSource `SelectParameters` коллекции не поддерживает указанное значение.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Чтобы установить это значение программными средствами, необходимо создать обработчик событий для элемента управления ObjectDataSource `Selecting` событий. Для этого перейдите в представление конструктора, а затем дважды щелкните элемент управления ObjectDataSource. Кроме того выберите элемент управления ObjectDataSource, перейдите в окне «Свойства» и щелкните значок молнии. Затем дважды щелкните в текстовом поле рядом с `Selecting` , либо введите имя обработчика событий, который вы хотите использовать. Как третий вариант, можно создать обработчик событий, выбрав элемент управления ObjectDataSource и его `Selecting` события из двух раскрывающихся списках в верхней части страницы кода класса.


![Щелкните значок молнии в окне свойств, чтобы вывести список событий веб-элемента управления](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Рис. 9**: щелкните значок молнии в окне свойств, чтобы вывести список событий веб-элемента управления


Все три подхода добавить новый обработчик событий для элемента управления ObjectDataSource `Selecting` событий для классов кода страницы. В этом обработчике событий можно чтение и запись значений параметров, с помощью `e.InputParameters(parameterName)`, где  *`parameterName`*  значение `Name` атрибута в `<asp:Parameter>` тег ( `InputParameters` коллекция может также быть индексированные порядковым, как и в `e.InputParameters(index)`). Чтобы задать `month` параметр текущему месяцу, добавьте следующий код в `Selecting` обработчик событий:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

При посещении этой страницы с помощью браузера мы видим, что только один сотрудник был принят на работу в этом месяце (март) Иванов Мария, который был в компании с 1994 г.


[![Тех сотрудников, годовщин показаны в этом месяце](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Рис. 10**: тех сотрудников с годовщин этот месяц отображаются ([Просмотр полноразмерное изображение](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Сводка

Значения параметров ObjectDataSource обычно может быть задано декларативно, без необходимости написания кода, его легко задавать значения параметров программными средствами. Все, необходимо сделать — создать обработчик событий для элемента управления ObjectDataSource `Selecting` событий, который запускается до вызова метода базового объекта и вручную установить значения для одного или нескольких параметров через `InputParameters` коллекции.

Этот учебник завершает раздел основной отчетности. [Следующее руководство](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) запускает фильтрации и сценариях основной-подробности раздел, в котором мы рассмотрим методы предоставления посетителя для фильтрации данных и перехода от основного отчета в отчет подробные сведения.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было – Хилтон Гизнау. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](declarative-parameters-vb.md)
