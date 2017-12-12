---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: "Добавление элементов управления проверки в DataList Правка интерфейса (Visual Basic) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы рассмотрим, как просто можно добавить элемент управления DataList EditItemTemplate проверяющие элементы управления для обеспечения более надежный редактирования int. пользователя..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: b720c7704a9c44e60ed8a9ad1479558376fb5402
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Добавление элементов управления проверки в элемент управления DataList редактирования интерфейса (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) или [скачать PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> В этом учебнике вы узнаете, как просто можно добавлять проверяющие элементы управления DataList EditItemTemplate для предоставления более надежный пользовательский интерфейс для редактирования.


## <a name="introduction"></a>Вступление

В данный момент редактирование учебники DataList DataLists, редактирования интерфейсов не включены все проверки входных данных пользователя упреждающего несмотря на то, что недопустимый вводимые данные, такие как отсутствует имя продукта или отрицательное цена приводят к возникновению исключения. В [предыдущего учебника](handling-bll-and-dal-level-exceptions-vb.md) мы рассмотрели, как добавить код в элемент управления DataList s обработки исключений `UpdateCommand` обработчик события для перехвата и правильно отображать сведения о всех исключений, которые были обнаружены. В идеале тем не менее, интерфейс редактирования будет включать проверяющие элементы управления, чтобы запретить пользователю ввести недопустимые данные в первую очередь.

В этом учебнике мы рассмотрим, как просто можно добавить проверяющие элементы управления DataList s `EditItemTemplate` для обеспечения более надежный пользовательский интерфейс для редактирования. В частности берется пример, созданный в предыдущем учебнике этого учебника и расширяет интерфейс редактирования соответствующей проверкой.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Шаг 1: Репликация в примере из[обработка исключений уровня DAL и уровень бизнес-ЛОГИКИ](handling-bll-and-dal-level-exceptions-vb.md)

В [обработки уровень бизнес-ЛОГИКИ и исключения уровня DAL](handling-bll-and-dal-level-exceptions-vb.md) учебника мы создали страницу, в списке названия и цены продуктов в две колонки, редактируемые DataList. Наша цель этого учебника — расширяют интерфейс редактирования s DataList включения элементов управления проверки. В частности наша логика проверки будет:

- Требовать, чтобы название продукта s был предоставлен
- Убедитесь, что введенное значение для цены является допустимым форматом валюты
- Убедитесь, что значение даты, введенное для цена больше или равно нулю, так как отрицательное `UnitPrice` недопустимое значение

Прежде чем мы рассмотрим предыдущего примера с целью включения проверки, сперва необходимо реплицировать в примере из `ErrorHandling.aspx` страницы в `EditDeleteDataList` папки на страницу в этом учебнике `UIValidation.aspx`. Для этого необходимо скопировать и `ErrorHandling.aspx` декларативная разметка s и его исходный код страницы. Сначала скопируйте декларативная разметка, выполнив следующие действия:

1. Откройте `ErrorHandling.aspx` страницы в Visual Studio
2. Перейдите на страницу s декларативная разметка (щелкните «источник» в нижней части страницы)
3. Копирование текста в пределах `<asp:Content>` и `</asp:Content>` (строки с 3 до 32), как показано на рисунке 1.


[![Копирование текста в пределах &lt;asp: Content&gt; управления](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**На рисунке 2**: копирование текста в пределах `<asp:Content>` управления ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))


1. Откройте `UIValidation.aspx` страницы
2. Перейдите на страницу s декларативная разметка
3. Вставьте текст внутри `<asp:Content>` элемента управления.

Чтобы скопировать исходный код, откройте `ErrorHandling.aspx.vb` страницы и скопировать текст *в* `EditDeleteDataList_ErrorHandling` класса. Скопируйте три обработчика событий (`Products_EditCommand`, `Products_CancelCommand`, и `Products_UpdateCommand`) вместе с `DisplayExceptionDetails` метода, но выполните **не** скопируйте объявления класса или `using` инструкции. Вставьте скопированный текст *в* `EditDeleteDataList_UIValidation` в класс `UIValidation.aspx.vb`.

После перемещения содержимого и кода из `ErrorHandling.aspx` для `UIValidation.aspx`, теперь пора проверить страницы в браузере. Вы должны видеть такие же выходные данные и возникают те же функциональные возможности, в каждой из этих двух страниц (см. рис. 2).


[![На странице UIValidation.aspx имитирует в ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**На рисунке 2**: `UIValidation.aspx` страницы имитирует в `ErrorHandling.aspx` ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Шаг 2: Добавление элементов управления проверки DataList s EditItemTemplate

При создании формы для ввода данных, очень важно, что пользователь ввел все необходимые поля и что их предоставленный входные данные являются значениями юридических, правильный формат. Чтобы обеспечить правильность s данные, вводимые пользователем, ASP.NET предоставляет пять встроенных элементов управления проверки, предназначенных для проверки значения одного входного веб-элемента управления:

- [RequiredFieldValidator](https://msdn.microsoft.com/en-us/library/5hbw267h(VS.80).aspx) гарантирует, что значение было предоставлено
- [CompareValidator](https://msdn.microsoft.com/en-us/library/db330ayw(VS.80).aspx) проверяет значение постоянное значение или значения элементов управления Web или гарантирует, что формат значения s допустим для указанного типа данных
- [RangeValidator](https://msdn.microsoft.com/en-us/library/f70d09xt.aspx) гарантирует, что значение в диапазоне значений
- [RegularExpressionValidator](https://msdn.microsoft.com/en-US/library/eahwtc9e.aspx) проверяет значение на соответствие [регулярных выражений](http://en.wikipedia.org/wiki/Regular_expression)
- [Проверяющий](https://msdn.microsoft.com/en-us/library/9eee01cx(VS.80).aspx) проверяет значение на соответствие настраиваемых, пользовательских метод

Дополнительные сведения для этих пяти элементов управления обращаться к [Добавление проверяющих элементов управления для редактирования и вставка интерфейсов](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) учебник или извлечение [раздел проверяющие элементы управления](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) из [ASP.NET по основам](https://quickstarts.asp.net).

В нашем примере нам нужно будет использовать RequiredFieldValidator, чтобы убедиться, что указано значение для имени продукта и CompareValidator чтобы введенной цены имеет значение больше или равно 0 и представлена в допустимом формате валюты.

> [!NOTE]
> Хотя ASP.NET 1.x имели эти же пять проверяющие элементы управления, ASP.NET 2.0 добавлен ряд улучшений, главный, два сценария на стороне клиента, поддержка браузеров помимо Internet Explorer и возможность секционирования проверяющие элементы управления на странице в группы проверки. Дополнительные сведения о новых функциях элементов управления проверки в 2.0 посвящены [разбор элементов управления проверки в ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Позволяет начать с добавления необходимых проверяющие элементы управления DataList s s `EditItemTemplate`. Эту задачу можно выполнить с помощью конструктора, щелкнув ссылку Изменить шаблоны смарт-теге DataList s, или с помощью декларативного синтаксиса. Разрешить s шаг в процессе, с помощью параметра редактирование шаблонов в режиме конструктора. После выбора для редактирования DataList s `EditItemTemplate`, добавьте RequiredFieldValidator, перетащив его из области элементов в интерфейс редактирования шаблона, поместив его после `ProductName` текстового поля.


[![Добавьте RequiredFieldValidator EditItemTemplate после ProductName текстового поля](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Рис. 3**: Добавление RequiredFieldValidator для `EditItemTemplate After` `ProductName` текстового поля ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))


Все проверяющие элементы управления работают путем проверки входных данных одного элемента управления ASP.NET Web. Таким образом, нам нужно указать, следует проверить RequiredFieldValidator, мы только что добавили `ProductName` TextBox; это можно сделать, настроив проверяющий элемент управления s [ `ControlToValidate` свойство](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) для `ID` из соответствующий веб-управления (`ProductName`, в данном экземпляре). Затем задайте [ `ErrorMessage` свойство](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) для необходимо указать название продукта s и [ `Text` свойство](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) для \*. `Text` Значение свойства, если указано, является текст, отображаемый элементом управления проверки в том случае, если проверка завершается с ошибкой. `ErrorMessage` Значение свойства, которое является обязательным, используется элемент управления ValidationSummary; Если `Text` значение свойства задано, `ErrorMessage` значение свойства отображается с помощью элемента управления проверки недопустимые входные данные.

После задания этих трех свойств RequiredFieldValidator экран должен выглядеть аналогично рис. 4.


[![Задать RequiredFieldValidator s ControlToValidate, сообщение об ошибке и свойства текста](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Рис. 4**: набор RequiredFieldValidator s `ControlToValidate`, `ErrorMessage`, и `Text` свойства ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))


С RequiredFieldValidator, добавляемый `EditItemTemplate`все, что остается только добавить необходимую проверку для цена продукта s текстовое поле. Поскольку `UnitPrice` является необязательным при изменении записи, мы не хотите добавлять RequiredFieldValidator требуется t. Тем не менее, необходимо добавить CompareValidator, чтобы убедиться, что `UnitPrice`, если указано, отформатированы в виде валюты и больше или равно 0.

Добавить CompareValidator в `EditItemTemplate` и задайте его `ControlToValidate` свойства `UnitPrice`, ее `ErrorMessage` свойство цену должно быть больше или равно нулю и не может содержать символ валюты и его `Text` свойства \*. Чтобы указать, что `UnitPrice` значение должно быть больше или равно 0, набор CompareValidator s [ `Operator` свойство](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) для `GreaterThanEqual`, ее [ `ValueToCompare` свойство](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) значение 0, и его [ `Type` свойство](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) для `Currency`.

После добавления этих двух проверяющие элементы управления, элемент управления DataList s `EditItemTemplate` s должен выглядеть следующим образом:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

После внесения этих изменений, откройте страницу в браузере. При попытке пропустить имя или ввести недопустимое значение цены при изменении продукта, звездочка рядом с текстового поля. Как показано на рисунке 5, значение цены, который включает символ валюты, например $19.95 считается недействительным. CompareValidator s `Currency` `Type` допускает разделители между цифрами (например, запятые и точки, в зависимости от настроек языка и региональных параметров) и плюса или минуса, но *не* допускает символ валюты. Это поведение может озадачить пользователей, поскольку в настоящий момент отображает интерфейс редактирования `UnitPrice` в формате валюты.


[![Звездочка рядом с текстовых полей с недопустимые входные данные](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Рис. 5**: звездочка появляется "Далее" для текстовых полей с недопустимые входные данные ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))


Когда проверка работает как-является, пользователь должен вручную удалить символ валюты при изменении записи, которая является недопустимым. Кроме того Если недопустимые входные данные в поле ввода интерфейс ни обновления и "Отмена", не кнопки, если он выбран, вызывает обратную передачу. В идеальном случае кнопка отмены вернет DataList состояние до редактирования независимо от того, допустимость s входные данные пользователя. Кроме того, нам нужно проверить допустимость данных страницы s перед обновлением сведения о продукте в DataList s `UpdateCommand` обработчика событий, как элементы управления проверки, можно обойти, если пользователям в браузерах которых не хотите поддержка t JavaScript или иметь логику на стороне клиента поддержка отключена.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Удаление символа валюты из текстового поля UnitPrice EditItemTemplate s

При использовании CompareValidator s `Currency``Type`, проверяемого входные данные не должны содержать какие-либо символы валют. Наличие таких символов в результате CompareValidator пометить как недопустимые входные данные. Тем не менее, наш интерфейс редактирования в настоящее время включает символ валюты в `UnitPrice` текстовое поле, то есть пользователь должен явно удалить символ валюты перед сохранением изменений. Чтобы устранить эту у нас есть три варианта:

1. Настройка `EditItemTemplate` , чтобы `UnitPrice` значение текстового поля не форматируются как денежное значение.
2. Разрешить пользователю ввести символ валюты, удаляя CompareValidator и заменив RegularExpressionValidator, которое проверяет наличие значения валюты в правильном формате. Проблема здесь регулярное выражение для проверки денежного значения не так просто, как CompareValidator и потребует написания кода, если необходимо включить параметры языка и региональных параметров.
3. Удалите проверяющий элемент управления полностью и полагаться на пользовательскую серверную логику проверки в GridView s `RowUpdating` обработчика событий.

Разрешить переход с параметром 1 в этом учебнике s. В настоящее время `UnitPrice` форматируются как денежное значение, из-за выражение привязки данных для текстового поля в `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Изменение `Eval` инструкции `Eval("UnitPrice", "{0:n2}")`, который форматирует результат как число с двумя цифрами точности. Это можно сделать напрямую через декларативный синтаксис или щелкнув ссылку изменить привязки данных в `UnitPrice` текстовое поле в DataList s `EditItemTemplate`.

После этого изменения в отформатированную цену в интерфейс редактирования включает запятые как разделители групп и точкой в качестве десятичного разделителя, но оставляет символ валюты.

> [!NOTE]
> При удалении формат денежной единицы интерфейс для редактирования, удобно помещать обозначение денежной единицы текста за пределами текстового поля. Это служит в качестве подсказки пользователю о необходимости предоставить обозначение денежной единицы.


## <a name="fixing-the-cancel-button"></a>Исправления кнопки отмены

По умолчанию веб-элементов управления проверки выдавать JavaScript для выполнения проверки на стороне клиента. При нажатии кнопки, LinkButton или ImageButton проверяющие элементы управления на странице проверяются на стороне клиента до обратного вызова. В случае любой недопустимые данные обратной передачи отменяется. Для некоторых кнопок, допустимость данных может быть имеет значения. в этом случае наличие отменена из-за недопустимых данных обратной передачи является неудобство.

Этот пример, определена кнопка отмены. Предположим, что пользователь вводит недопустимые данные, такие как имя продукта s, решает, она t необходимо выполнить после того как все сохранить сведения о продукте и обращений к кнопке "Отмена". В настоящее время кнопки «Отмена» запускает проверяющие элементы управления на странице, которые пропущена имя продукта и предотвращения обратной передачи. Наш пользователь должен ввести текст в `ProductName` текстовое поле, чтобы отменить процесс редактирования.

К счастью, кнопки, LinkButton и ImageButton имеют [ `CausesValidation` свойство](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.causesvalidation.aspx) , можно указать ли щелкнув кнопку должен инициировать логики проверки (по умолчанию — `True`). Набор s кнопку Отмена `CausesValidation` свойства `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Проверка входных данных, допустимых в обработчик события UpdateCommand

Из-за клиентского скрипта, генерируемой проверяющие элементы управления, если пользователь вводит неверный ввод проверяющие элементы управления отменить обратную передачу кнопки LinkButton, в любой ответ или ImageButton, свойства `CausesValidation` свойства `True` ( по умолчанию). Тем не менее если пользователь посещает устарела браузере или JavaScript, поддержку которого была отключена, проверяет проверки на стороне клиента не будет выполняться.

Все проверяющие элементы управления ASP.NET повторите свою логику проверки сразу после обратной передачи и отчет общего допустимость входных данных страницы s через [ `Page.IsValid` свойства](https://msdn.microsoft.com/en-us/library/system.web.ui.page.isvalid.aspx). Тем не менее, поток страниц не прервано или остановлено в каком-либо способом на основе значения из `Page.IsValid`. Как разработчиков, убедитесь, что наши обязанностью является `Page.IsValid` свойство имеет значение `True` перед продолжением программный код, предполагающий допустимые входные данные.

Если у пользователя есть JavaScript отключена, посещает нашу страницу, правки продукта, вводит значение цены за слишком много ресурсов и нажимает кнопку обновления, клиентская проверка не выполняется, и произойдет обратная передача. При обратной передаче ASP.NET страницы s `UpdateCommand` выполняет обработчика событий и вызывает исключение при синтаксическом анализе слишком дорого `Decimal`. Поскольку у нас есть обработки исключений, корректно обрабатываться такого исключения, но мы может помешать недопустимые данные в первую очередь Запаздывающие через, только если с `UpdateCommand` обработчик событий при `Page.IsValid` имеет значение `True`.

Добавьте следующий код в начало `UpdateCommand` обработчика событий, непосредственно перед `Try` блока:


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

В результате этого добавления продукта будет пытаться обновить только в том случае, если объем передаваемых данных является допустимым. Предпочтение было отдано t большинство пользователей смогут обратной передачи недопустимых данных из-за клиентские скрипты элементов управления проверки, но пользователи которого браузеры не хотите поддержка t JavaScript или в которых поддерживает JavaScript отключена, обхода проверки со стороны клиента и отправить недопустимые данные.

> [!NOTE]
> Проницательный читатель будет помните, что при обновлении данных в GridView, мы t нужно явно не проверять `Page.IsValid` свойству в классе кода нашей странице s. Это обусловлено обращается к GridView `Page.IsValid` свойство для нас и продолжить обновление только в том случае, если он возвращает значение `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Шаг 3: Сводка проблем с вводом данных

В дополнение к пяти проверяющие элементы управления, включает ASP.NET [управления ValidationSummary](https://msdn.microsoft.com/en-US/library/f9h59855(VS.80).aspx), который отображает `ErrorMessage` s этих элементов управления проверки, которые обнаружены недопустимые данные. Эти сводные данные могут отображаться в виде текста на веб-странице или с помощью модального, клиентские messagebox. Позволяют повысить этого учебника, чтобы включить клиентский messagebox суммирования проблемы с проверкой s.

Для этого перетащите элемент управления ValidationSummary из области элементов в конструктор. Расположение t управления ValidationSummary важны, так как мы re требуется настроить его для отображения сводки только как messagebox. После добавления элемента управления, задать его [ `ShowSummary` свойство](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) для `False` и его [ `ShowMessageBox` свойство](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) для `True`. В результате этого добавления ошибок проверки, приведены в messagebox клиентского (см. рис. 6).


[![Ошибки проверки, приведены в Messagebox стороны клиента](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Рис. 6**: ошибок проверки, приведены в Messagebox клиентского ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))


## <a name="summary"></a>Сводка

В этом учебнике мы узнали, как уменьшить вероятность возникновения исключений с помощью элементов управления проверки для упреждающего Проверьте правильность наши пользователи входные данные перед использованием их в рабочий процесс обновления. ASP.NET предоставляет пять веб-элементов управления проверки, предназначенных для проверки определенного веб-узла управления входными s и выдавать отчеты о допустимости ввода s. В этом учебнике мы использовали два из этих пяти элементов управления RequiredFieldValidator и CompareValidator для обеспечения, название продукта s был указан и что с ценой в денежном формате со значением больше или равно нулю.

Добавление элементов управления проверки в DataList s, интерфейс редактирования сводится перетаскивая их на `EditItemTemplate` из панели элементов и задание небольшое число свойств. По умолчанию проверяющие элементы управления автоматически создавать скрипт проверки на стороне клиента. они также предоставляют серверную проверку при обратной передаче, сохраняя совокупное результат в `Page.IsValid` свойство. Для обхода проверки на стороне клиента, при нажатии кнопки, LinkButton или ImageButton, настроить кнопку s `CausesValidation` свойства `False`. Кроме того, перед выполнением каких-либо задач с данными, отправленного на обратную передачу, убедитесь, что `Page.IsValid` возвращает `True`.

Все изменения учебники DataList мы хранить, пока проверяется имели очень простого редактирования интерфейсы текстовое поле для имени продукта s, а другой для цены. Интерфейс редактирования, однако может содержать сочетание разных веб-элементов управления, например элементами управления DropDownList, календари, переключатели, флажки и т. д. В следующем учебном курсе мы рассмотрим построение интерфейс, который использует различные веб-элементов управления.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Патерсона Деннис, Алексей Pespisa и (Liz Shulok). Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](handling-bll-and-dal-level-exceptions-vb.md)
[Вперед](customizing-the-datalist-s-editing-interface-vb.md)
