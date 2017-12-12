---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: "Добавление элементов управления проверки для редактирования и вставка интерфейсов (Visual Basic) | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы рассмотрим, как просто можно добавить элементы управления проверки к EditItemTemplate и InsertItemTemplate веб-элемента управления данными, для обеспечения более..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: a181be8d07ff15c33818077dc75b5cc6c6bbf65d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Добавление элементов управления проверки для редактирования и вставка интерфейсов (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) или [скачать PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> В этом учебнике вы узнаете, как просто можно добавить элементы управления проверки к EditItemTemplate и InsertItemTemplate веб-элемента управления данными, для обеспечения более надежный пользовательского интерфейса.


## <a name="introduction"></a>Вступление

Элементы управления GridView и DetailsView в примерах мы рассматривали за последние три учебника состояли стояли и CheckBoxFields (типы полей автоматически добавляется в Visual Studio при привязке к источнику данных GridView и DetailsView Управление с помощью смарт-тег). При изменении строки в GridView и DetailsView этих стояли, которые не только для чтения, преобразуются в текстовые поля, из которого конечный пользователь может изменять существующие данные. Аналогичным образом, при вставке новой записи в элемента управления DetailsView этих стояли которого `InsertVisible` свойству `True` (по умолчанию), отображаются как пустые текстовые поля, в которое пользователь может вставить значения полей новой записи. Аналогичным образом CheckBoxFields, которые отключены в интерфейсе стандартные, только для чтения, преобразуются в включена флажки в интерфейсах правки и вставки.

Значение по умолчанию интерфейсы правки и вставки для BoundField и CheckBoxField могут быть полезными, интерфейс не имеет каких-либо проверки. Если пользователь допустил ошибку данных ввода - например пропуск `ProductName` или ввод недопустимое значение для `UnitsInStock` (например, -50) будет вызвано исключение из в глубине архитектуры приложения. Хотя это исключение может быть верно обработано, как показано в [с предыдущим учебником](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), в идеале правки или вставки пользовательского интерфейса, включит проверяющие элементы управления, чтобы запретить пользователю ввести недопустимые данные в Первым делом.

Чтобы указать настраиваемый редактирования или интерфейс для вставки, нам нужно заменить BoundField или CheckBoxField TemplateField. TemplateFields, в котором были предметом в [с помощью TemplateFields в элементе управления GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) и [с помощью TemplateFields в элементе управления DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) учебниках, могут состоять из нескольких Определение шаблонов отдельные интерфейсы для различных состояний строки. TemplateField `ItemTemplate` используется при подготовке к просмотру только для чтения поля или строки в элементах управления DetailsView или GridView, тогда как `EditItemTemplate` и `InsertItemTemplate` указывают интерфейсы, используемые для правки и вставки режимы, соответственно.

В этом учебнике мы рассмотрим, как просто можно добавить элементы управления проверки TemplateField `EditItemTemplate` и `InsertItemTemplate` для обеспечения более надежный пользовательского интерфейса. В частности, этот учебник берется пример, созданный в [проверки события, связанные с вставки, обновления и удаления](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) учебник и делается дополнение правки и вставки интерфейсы соответствующей проверкой.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Шаг 1: Репликация в примере из[изучить события, связанные с вставки, обновления и удаления](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

В [проверки события, связанные с вставки, обновления и удаления](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) учебника мы создали страницу, в списке названия и цены продуктов в изменяемого элемента управления GridView. Кроме того, страница включены DetailsView которого `DefaultMode` было задано значение `Insert`, тем самым всегда подготовки к просмотру в режиме вставки. Из этого DetailsView пользователь может ввести имя и цену для нового продукта, щелкните "Вставить" и его добавлении в систему (см. рис. 1).


[![Предыдущий пример дает пользователям возможность добавлять новые продукты и изменять существующие](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Рис. 1**: предыдущего примера позволяет пользователям добавлять новые продукты и изменять существующие ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Наша цель этого учебника — дополнять DetailsView и GridView для предоставления элементов управления проверки. В частности наша логика проверки будет:

- Требовать указывались имя при вставке или изменении продукта
- Требуется предоставить цену при вставке записи; При изменении записи, мы по-прежнему потребуется цена, но будет использовать программную логику в GridView `RowUpdating` уже присутствовавшая из более ранних учебника
- Убедитесь, что введенное значение для цены является допустимым форматом валюты

Прежде чем мы рассмотрим предыдущего примера с целью включения проверки, сперва необходимо реплицировать в примере из `DataModificationEvents.aspx` страницы на страницу в этом учебнике `UIValidation.aspx`. Для этого необходимо скопировать и `DataModificationEvents.aspx` декларативная разметка страницы и его исходный код. Сначала скопируйте декларативная разметка, выполнив следующие действия:

1. Откройте `DataModificationEvents.aspx` страницы в Visual Studio
2. Перейдите на страницы с декларативной разметкой (щелкните «источник» в нижней части страницы)
3. Копирование текста в пределах `<asp:Content>` и `</asp:Content>` (строки с 3 по 44), как показано на рис. 2.


[![Копирование текста в пределах &lt;asp: Content&gt; управления](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**На рисунке 2**: копирование текста в пределах `<asp:Content>` управления ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Откройте `UIValidation.aspx` страницы
2. Перейти к декларативная разметка страницы
3. Вставьте текст внутри `<asp:Content>` элемента управления.

Чтобы скопировать исходный код, откройте `DataModificationEvents.aspx.vb` страницы и скопировать текст *в* `EditInsertDelete_DataModificationEvents` класса. Скопируйте три обработчика событий (`Page_Load`, `GridView1_RowUpdating`, и `ObjectDataSource1_Inserting`), в отличие от **не** скопируйте объявления класса. Вставьте скопированный текст *в* `EditInsertDelete_UIValidation` в класс `UIValidation.aspx.vb`.

После перемещения содержимого и кода из `DataModificationEvents.aspx` для `UIValidation.aspx`, теперь пора проверить ход выполнения в браузере. Вы увидите выходные данные и возникают те же функциональные возможности, в каждой из этих двух страниц (ссылаться на рис. 1 для снимок экрана `DataModificationEvents.aspx` в действие).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Шаг 2: Преобразование стояли TemplateFields

Добавление элементов управления проверки к интерфейсам правки и вставки, должны преобразовываться в TemplateFields стояли, используемым элементами управления DetailsView и GridView. Для этого нажмите кнопку Правка столбцов и изменить поля ссылки в GridView и DetailsView смарт-теги, соответственно. Выберите каждое стояли и щелкните ссылку «Преобразовать это поле в TemplateField».


[![Преобразования каждого элемента GridView и DetailsView стояли в TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Рис. 3**: преобразования каждого элемента GridView и DetailsView стояли в TemplateFields ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Преобразования BoundField в TemplateField через диалоговое окно поля приводит к возникновению ошибки TemplateField, выполняющего те же интерфейсы только для чтения, правки и вставки, BoundField, сам. В следующем примере показана декларативного синтаксиса для `ProductName` в DetailsView после его преобразования в TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Обратите внимание, что это TemplateField имеет три шаблона автоматически создается `ItemTemplate`, `EditItemTemplate`, и `InsertItemTemplate`. `ItemTemplate` Отображает значение поля данных single (`ProductName`) с помощью веб-управления Label, тогда как `EditItemTemplate` и `InsertItemTemplate` текущее значение поля данных в элементе управления TextBox Web, который связывает поле данных с текстовым полем `Text` свойство с помощью двухсторонней привязкой данных. Так как мы используем DetailsView на этой странице для вставки, вы можете удалить `ItemTemplate` и `EditItemTemplate` из двух TemplateFields, несмотря на то, что нет никакого вреда оставляя их.

Поскольку GridView не поддерживает встроенные вставки объектов DetailsView, преобразование GridView `ProductName` поле в TemplateField приводит только `ItemTemplate` и `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Нажмите кнопку «Преобразовать это поле в TemplateField», создаваемые в Visual Studio TemplateField шаблоны которого имитируют преобразованный BoundField пользовательского интерфейса. Это можно проверить, посетив эту страницу через браузер. Вы обнаружите, что внешний вид и поведение TemplateFields аналогичны таковым при использовании стояли вместо.

> [!NOTE]
> Вы можете настроить интерфейсы правки и шаблоны. Например, необходимо иметь текстовое поле в `UnitPrice` TemplateFields к просмотру в виде текстового поля меньше, чем `ProductName` текстового поля. Для этого можно задать текстовое поле `Columns` свойства соответствующее значение или предоставить абсолютную ширину с помощью `Width` свойство. В следующем уроке мы рассмотрим, как полностью настроить интерфейс редактирования, заменяя запись данных на альтернативный веб-элемента управления текстового поля.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Шаг 3: Добавление элементов управления проверки к GridView`EditItemTemplate` s

При создании формы для ввода данных, очень важно, что пользователь ввел все необходимые поля и, юридические, правильно отформатированные значения всех указанных входных данных. Чтобы быть уверенным, что входные данные пользователя являются допустимыми, ASP.NET предоставляет пять встроенных элементов управления проверки, разработанных для использования для проверки значения одного элемента управления ввода:

- [RequiredFieldValidator](https://msdn.microsoft.com/en-us/library/5hbw267h(VS.80).aspx) гарантирует, что значение было предоставлено
- [CompareValidator](https://msdn.microsoft.com/en-us/library/db330ayw(VS.80).aspx) проверяет значение постоянное значение или значения элементов управления Web или гарантирует, что формат значения является допустимым для указанного типа данных
- [RangeValidator](https://msdn.microsoft.com/en-us/library/f70d09xt.aspx) гарантирует, что значение в диапазоне значений
- [RegularExpressionValidator](https://msdn.microsoft.com/en-US/library/eahwtc9e.aspx) проверяет значение на соответствие [регулярных выражений](http://en.wikipedia.org/wiki/Regular_expression)
- [Проверяющий](https://msdn.microsoft.com/en-us/library/9eee01cx(VS.80).aspx) проверяет значение на соответствие настраиваемых, пользовательских метод

Дополнительные сведения для этих пяти элементов управления см. [раздел проверяющие элементы управления](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) из [ASP.NET по основам](https://asp.net/QuickStart/aspnet/).

В нашем примере нам нужно будет использовать RequiredFieldValidator в GridView и DetailsView `ProductName` TemplateFields и RequiredFieldValidator в DetailsView `UnitPrice` TemplateField. Кроме того, нам нужно будет добавить элемент управления CompareValidator для обоих элементов управления `UnitPrice` TemplateFields, гарантирует, что введенная цена имеет значение больше или равно 0 и представлена в допустимом формате валюты.

> [!NOTE]
> Хотя ASP.NET 1.x имели эти же пять проверяющие элементы управления, ASP.NET 2.0 добавлен ряд улучшений, главный, два сценария на стороне клиента, поддержка браузеров, отличных от Internet Explorer и возможность секционирования проверяющие элементы управления на странице в группы проверки. Дополнительные сведения о новых функциях элементов управления проверки в 2.0 посвящены [разбор элементов управления проверки в ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Давайте начнем с добавления необходимых элементов управления проверки для `EditItemTemplate` s в TemplateFields GridView. Для этого щелкните ссылку Изменить шаблоны из GridView смарт-тег, чтобы открыть интерфейс редактирования шаблона. Здесь можно выбрать шаблон для правки из раскрывающегося списка. Поскольку мы хотим расширить интерфейс редактирования, нам нужно добавить элементы управления проверки `ProductName` и `UnitPrice` `EditItemTemplate` s.


[![Нам необходимо расширить ProductName и UnitPrice в EditItemTemplates](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Рис. 4**: нам необходимо расширить `ProductName` и `UnitPrice` `EditItemTemplate` s ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


В `ProductName` `EditItemTemplate`, добавьте RequiredFieldValidator, перетащив его из области элементов в интерфейс редактирования шаблона, поместив после текстового поля.


[![Добавление ProductName EditItemTemplate RequiredFieldValidator](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Рис. 5**: Добавление RequiredFieldValidator для `ProductName` `EditItemTemplate` ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Все проверяющие элементы управления работают путем проверки входных данных одного элемента управления ASP.NET Web. Таким образом, необходимо убедиться, что RequiredFieldValidator, мы только что добавили следует проверить в текстовое поле в `EditItemTemplate`; это делается путем присвоения свойству проверяющего элемента управления [свойство ControlToValidate](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) для `ID` соответствующего веб-элемента управления. Текстовое поле в настоящее время имеет довольно неопределенный `ID` из `TextBox1`, но давайте на что-нибудь более подходящее. Щелкните текстовое поле в шаблоне и в окне свойств измените `ID` из `TextBox1` для `EditProductName`.


[![Измените идентификатор текстовое поле для EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Рис. 6**: изменить текстовое поле `ID` для `EditProductName` ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Затем задайте RequiredFieldValidator `ControlToValidate` свойства `EditProductName`. Задайте [свойство ErrorMessage](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) на «Необходимо указать название продукта» и [свойства Text](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) для «\*». `Text` Значение свойства, если указано, является текст, отображаемый элементом управления проверки в том случае, если проверка завершается с ошибкой. `ErrorMessage` Значение свойства, которое является обязательным, используется элемент управления ValidationSummary; Если `Text` значение свойства задано, `ErrorMessage` также является текст, отображаемый элементом управления проверки в случае недопустимого ввода.

После задания этих трех свойств RequiredFieldValidator экран должен выглядеть примерно как на рис.


[![Задайте RequiredFieldValidator ControlToValidate, сообщение об ошибке и текст](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Рис. 7**: задать RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, и `Text` свойства ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


С RequiredFieldValidator, добавляемый `ProductName` `EditItemTemplate`, что все остается только добавить необходимую проверку к `UnitPrice` `EditItemTemplate`. Поскольку мы решили, что для этой страницы `UnitPrice` обязательно при правке записи, нам не нужно добавлять RequiredFieldValidator. Тем не менее, необходимо добавить CompareValidator, чтобы убедиться, что `UnitPrice`, если указано, отформатированы в виде валюты и больше или равно 0.

Прежде чем добавлять CompareValidator для `UnitPrice` `EditItemTemplate`, давайте сначала измените идентификатор элемента управления TextBox Web из `TextBox2` для `EditUnitPrice`. После этого изменения CompareValidator, добавьте параметр его `ControlToValidate` свойства `EditUnitPrice`, ее `ErrorMessage` свойства «цена должна быть больше или равно нулю и не может включать символ валюты» и его `Text` свойства "\*".

Чтобы указать, что `UnitPrice` значение должно быть больше или равно 0, значение CompareValidator [свойство оператор](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) для `GreaterThanEqual`, ее [свойства ValueToCompare](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) «0» и его [ Свойство типа](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) для `Currency`. В следующем показан декларативный синтаксис `UnitPrice` в TemplateField `EditItemTemplate` после внесения этих изменений:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

После внесения этих изменений, откройте страницу в браузере. При попытке пропустить имя или ввести недопустимое значение цены при изменении продукта, звездочка рядом с текстового поля. Как показано на рис. 8, значение цены, который включает символ валюты, например $19.95 считается недействительным. CompareValidator `Currency` `Type` допускает разделители между цифрами (например, запятые и точки, в зависимости от настроек языка и региональных параметров) и плюса или минуса, но *не* допускает символ валюты. Это поведение может озадачить пользователей, поскольку в настоящий момент отображает интерфейс редактирования `UnitPrice` в формате валюты.

> [!NOTE]
> Помните, что в *события, связанные с вставки, обновления и удаления* учебника мы устанавливаем BoundField `DataFormatString` свойства `{0:c}` для формат денежной единицы. Кроме того, мы устанавливаем `ApplyFormatInEditMode` равным true, что приводит к GridView Правка форматировать `UnitPrice` как денежное значение. При преобразовании BoundField в TemplateField, Visual Studio отметила эти параметры и текстовое поле в формате `Text` как денежную единицу, используя синтаксис привязки данных `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Звездочка рядом с текстовых полей с недопустимые входные данные](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Рис. 8**: звездочка появляется "Далее" для текстовых полей с недопустимые входные данные ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Когда проверка работает как-является, пользователь должен вручную удалить символ валюты при изменении записи, которая является недопустимым. Чтобы исправить это, у нас есть три варианта:

1. Настройка `EditItemTemplate` , чтобы `UnitPrice` значения не форматируются как денежное значение.
2. Разрешить пользователю ввести символ валюты, удаляя CompareValidator и заменив RegularExpressionValidator, что правильно проверяет значения валюты в правильном формате. Проблема здесь регулярное выражение для проверки денежного значения не является довольно и потребует написания кода, если необходимо включить параметры языка и региональных параметров.
3. Удалите проверяющий элемент управления полностью и полагаться на логику проверки на стороне сервера в GridView `RowUpdating` обработчика событий.

Давайте рассмотрим параметр #1 для этого упражнения. В настоящее время `UnitPrice` форматируются как денежные значения, из-за выражение привязки данных для текстового поля в `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Измените оператор привязки на `Bind("UnitPrice", "{0:n2}")`, который форматирует результат как число с двумя цифрами точности. Это можно сделать напрямую через декларативный синтаксис или щелкнув ссылку изменить привязки данных в `EditUnitPrice` текстовое поле в `UnitPrice` в TemplateField `EditItemTemplate` (см. рис).


[![Щелкните ссылку изменить привязки данных в текстовое поле](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Рис. 9**: щелкните ссылку изменить привязки данных в текстовое поле ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Указать описатель формата в операторе Bind](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Рис. 10**: указать описатель формата в `Bind` инструкции ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


После этого изменения в отформатированную цену в интерфейс редактирования включает запятые как разделители групп и точкой в качестве десятичного разделителя, но оставляет символ валюты.

> [!NOTE]
> `UnitPrice` `EditItemTemplate` Не включает RequiredFieldValidator, позволяя обратным передачам выходить и логике обновлений для начала. Тем не менее `RowUpdating` скопированный из *проверки события, связанные с вставки, обновления и удаления* учебник включает программный убедитесь, что гарантирует, что `UnitPrice` предоставляется. Вы можете удалить эту логику, оставьте его в виде-, или добавьте RequiredFieldValidator для `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Шаг 4: Сводка проблем с вводом данных

В дополнение к пяти проверяющие элементы управления, включает ASP.NET [управления ValidationSummary](https://msdn.microsoft.com/en-US/library/f9h59855(VS.80).aspx), который отображает `ErrorMessage` s этих элементов управления проверки, которые обнаружены недопустимые данные. Эти сводные данные могут отображаться в виде текста на веб-странице или с помощью модального, клиентские messagebox. Давайте усовершенствовать этого учебника, чтобы включить клиентский messagebox суммирования проблемы с проверкой.

Для этого перетащите элемент управления ValidationSummary из области элементов в конструктор. Расположение проверяющего элемента управления не имеет значения, так как мы собираемся задана Отображение сводки только в качестве messagebox. После добавления элемента управления, задать его [свойство ShowSummary](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) для `False` и его [свойство ShowMessageBox](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) для `True`. В результате этого добавления в messagebox клиентские описаны ошибок проверки.


[![Ошибки проверки, приведены в Messagebox стороны клиента](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Рис. 11**: ошибок проверки, приведены в Messagebox клиентского ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Шаг 5: Добавление элементов управления проверки в DetailsView`InsertItemTemplate`

В этом учебнике остается добавить элементы управления проверки к интерфейсу вставки DetailsView. Процесс добавления элементов управления проверки к шаблонам DetailsView идентичен описанному на этапе 3; Таким образом мы проскочим через задач на этом шаге. Как мы сделали с GridView `EditItemTemplate` s, я рекомендую Переименовать `ID` s текстовых полей из неопределенный `TextBox1` и `TextBox2` для `InsertProductName` и `InsertUnitPrice`.

Добавить RequiredFieldValidator для `ProductName` `InsertItemTemplate`. Задать `ControlToValidate` для `ID` текстового поля в шаблоне, его `Text` свойства «\*» и его `ErrorMessage` свойства на «Необходимо указать название продукта».

Поскольку `UnitPrice` — требуется для этой страницы при добавлении новой записи, добавьте RequiredFieldValidator для `UnitPrice` `InsertItemTemplate`, параметр его `ControlToValidate`, `Text`, и `ErrorMessage` свойства соответствующим образом. Наконец, добавьте CompareValidator для `UnitPrice` `InsertItemTemplate` , настроив его `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, и `ValueToCompare` свойства так же, как мы сделали с `UnitPrice`элемента CompareValidator в GridView `EditItemTemplate`.

После добавления этих элементов управления проверки, новый продукт не может добавить к системе, если его имя не указано или если цена является отрицательным числом или находится в недопустимом формате.


[![Логику проверки добавлена к интерфейсу вставки DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Рис. 12**: логику проверки добавлена к интерфейсу вставки DetailsView ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Шаг 6: Разделение элементов управления проверки на группы проверки

Наша страница состоит из двух логически неравных наборов элементов управления проверки: те, которые соответствуют GridView интерфейс пользователя для редактирования и те, которые соответствуют DetailsView интерфейсу правки. По умолчанию при обратной передаче *все* проверяются проверяющие элементы управления на странице. При изменении записи мы не хотим DetailsView интерфейса вставки проверяющие элементы управления для проверки. Рис. 13 показано нашей текущей дилемму, когда пользователь изменяет продукт, вводя вполне допустимые значения, нажав кнопку обновления приводит к ошибке проверки, поскольку пустые значения в интерфейсе вставки название и цену.


[![Обновление продукта вызывает проверяющие элементы управления интерфейса вставки пожара](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Рис. 13**: обновление продукта вызывает проверяющие элементы управления интерфейса Вставка Fire ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Элементы управления проверки в ASP.NET 2.0 может быть разделена на группы проверки по их `ValidationGroup` свойство. Чтобы связать набор проверяющих элементов управления в группе, задайте их `ValidationGroup` свойство то же значение. В этом руководстве задать `ValidationGroup` проверяющих элементов управления в GridView TemplateFields для `EditValidationControls` и `ValidationGroup` свойства TemplateFields DetailsView для `InsertValidationControls`. Эти изменения можно выполнить непосредственно в декларативной разметке или в окне «Свойства» при использовании конструктора редактирование интерфейса шаблона.

Дополнение к проверке элементов управления, кнопки и элементы управления Button в ASP.NET 2.0 также включать `ValidationGroup` свойство. Проверки группы проверки проверяются на допустимость только при обратной передаче вызываемые с помощью кнопки с таким же `ValidationGroup` значение свойства. Например, в порядке для DetailsView вставки кнопки для вызова `InsertValidationControls` группы проверки, нам нужно указать CommandField `ValidationGroup` свойства `InsertValidationControls` (см. рис. 14). Кроме того, присвоить свойству в CommandField `ValidationGroup` свойства `EditValidationControls`.


[![Присваивает DetailsView свойству в CommandField ValidationGroup InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Рис. 14**: задать DetailsView в CommandField `ValidationGroup` свойства `InsertValidationControls` ([Просмотр полноразмерное изображение](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


После внесения этих изменений DetailsView и GridView TemplateFields и CommandFields должен выглядеть следующим образом:

TemplateFields и CommandField DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField и TemplateFields GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

На этом этапе элементов управления проверки редактирования срабатывают только при нажатии кнопки обновления GridView и fire элементы управления конкретного insert проверки только в том случае, когда нажата кнопка вставки DetailsView, устранения неполадки, выделены на рис. 13. Тем не менее это изменение наш элемент управления ValidationSummary больше не отображается при вводе недопустимых данных. Также содержит элемент управления ValidationSummary `ValidationGroup` свойства и отображает сводку информации только для тех элементов управления проверки в его группу проверки. Таким образом, необходимо иметь два проверяющих элементов управления на этой странице, один для `InsertValidationControls` и второй для `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

На этом дополнении наш учебный курс завершается.

## <a name="summary"></a>Сводка

Хотя стояли обеспечивают как интерфейс вставки и редактирования, интерфейс не поддерживает настройку. Как правило мы хотим добавить проверяющие элементы управления для редактирования и интерфейсам, чтобы убедиться, что пользователь вводит требуемые данные в допустимом формате. Для выполнения этой задачи необходимо преобразовать в TemplateFields стояли и добавить соответствующие шаблоны элементов управления проверки. В этом учебнике мы расширили пример из *проверки события, связанные с вставки, обновления и удаления* правки учебник, добавление элементов управления проверки в DetailsView оба интерфейса и GridView интерфейс для редактирования. Кроме того мы узнали, как отобразить сведения сводки проверки с помощью элемента управления ValidationSummary и разбиение на разделы по отдельным группам проверки проверяющие элементы управления на странице.

Как было показано в этом учебнике, TemplateFields позволяют дополнять включения элементов управления проверки интерфейсы правки и вставки. TemplateFields также можно расширить для включения дополнительных входных веб-элементы управления, включение текстовое поле будет заменена более подходящий веб-элемент управления. В нашем следующем уроке мы рассмотрим, как заменить элемент управления TextBox с элементом управления DropDownList с привязкой к данным, который идеально подходит при изменении внешнего ключа (такие как `CategoryID` или `SupplierID` в `Products` таблице).

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были (Liz Shulok) и Зак Джонс. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
[Вперед](customizing-the-data-modification-interface-vb.md)
