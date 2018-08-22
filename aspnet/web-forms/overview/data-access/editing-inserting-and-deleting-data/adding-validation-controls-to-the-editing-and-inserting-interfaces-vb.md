---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Добавление элементов управления проверки правки и вставки интерфейсы (Visual Basic) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы видим, насколько это просто добавлять элементы управления проверки к EditItemTemplate и InsertItemTemplate веб-элемента управления данными, чтобы предоставить более...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: d06408717bdf5e7446597ae4330ffb32cf943e7f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837318"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Добавление элементов управления проверки к редактированию интерфейсы и вставки (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) или [скачать PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> В этом руководстве мы рассмотрим, насколько это просто добавлять элементы управления проверки к EditItemTemplate и InsertItemTemplate веб-элемента управления данными для обеспечения пользовательского интерфейса, более защищенного от непреднамеренных ошибок.


## <a name="introduction"></a>Вступление

Элементы управления GridView и DetailsView в примерах, курсах было продемонстрировано на последних трех учебных курса, состояли из полей BoundField и CheckBoxFields (типы полей автоматически добавляется в Visual Studio при привязке к источнику данных GridView или DetailsView Управление с помощью смарт-тег). При правке строки в GridView или DetailsView, эти поля BoundField, кроме, не только для чтения, преобразуются в текстовые поля, из которого конечный пользователь может изменять существующие данные. Аналогичным образом, при вставке нового записи в элементе управления DetailsView, эти поля BoundField, кроме, `InsertVisible` свойству `True` (по умолчанию), отображаются как пустые текстовые окна, в котором пользователь может вставить значения полей новой записи. Аналогичным образом CheckBoxFields, которые отключены в интерфейсе стандартные "," только для чтения, преобразуются в включенные флажки в интерфейсы правки и вставки.

По умолчанию интерфейсы правки и вставки для BoundField и CheckBoxField могут быть полезными, интерфейс не имеет каких-либо проверки. Если пользователь делает ошибку запись данных — такую как пропуск `ProductName` поля или ввод недопустимого значения для `UnitsInStock` (скажем, -50), будет ли вызываться исключение из в глубине архитектуры приложения. Хотя это исключение может быть верно обработано, как показано в [предыдущем учебном курсе](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), в идеале правки или вставки пользовательского интерфейса будет включать элементы управления проверки к запрещает пользователю ввести недопустимые данные в Первым делом.

Чтобы предоставить настраиваемый редактирования или вставки интерфейса, нам нужно заменить BoundField или CheckBoxField TemplateField. Поля TemplateField, которой были предметом в [использование полей TemplateField в элементе управления GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) и [использование полей TemplateField в элементе управления DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) руководства и может состоять из нескольких шаблонов, определяющих отдельные интерфейсы для различных состояний строки. TemplateField `ItemTemplate` используется при подготовке к просмотру только для чтения поля или строки в элементах управления DetailsView и GridView, тогда как `EditItemTemplate` и `InsertItemTemplate` указывают интерфейсы, используемые для правки и вставки режимы, соответственно.

В этом руководстве мы рассмотрим, насколько это просто добавлять элементы управления проверки к TemplateField `EditItemTemplate` и `InsertItemTemplate` для обеспечения пользовательского интерфейса, более защищенного от непреднамеренных ошибок. В частности, в этом руководстве пример, созданный в [Проверка события, связанные с вставки, обновления и удаления](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) интерфейсы, руководство и делается дополнение правки и вставки соответствующей проверкой.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Шаг 1: Воспроизведение примера из[обзор событий, связанных со вставкой, обновлением и удалением](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

В [Проверка события, связанные с вставки, обновления и удаления](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) руководстве мы создали страницу, перечислявшую названия и цены продуктов в изменяемого элемента управления GridView. Кроме того, страницы включены в элементе управления DetailsView, `DefaultMode` было установлено на `Insert`, тем самым всегда подготовки к просмотру в режиме вставки. В этот DetailsView, пользователь может введите название и цену для нового продукта, щелкните Insert и его добавления в систему (см. рис. 1).


[![Предыдущий пример позволял пользователям добавлять новые продукты и изменение существующих запросов.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Рис. 1**: предыдущий пример позволяет пользователям добавлять новые продукты и изменять существующие ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Наша цель для этого учебника, — для дополнения DetailsView и GridView для предоставления элементов управления проверки. В частности наша логика проверки будет:

- Требует, что при вставке или правке продукта было предоставлено его название
- Требует, что цена предоставляться при вставке записи; При правке записи, мы цена тоже требуется, но будет использовать программную логику в GridView `RowUpdating` уже присутствовавшая в предыдущем учебном курсе
- Убедитесь, что введенное значение для цены является допустимым форматом валюты

Прежде чем мы рассмотрим дополнение предыдущего примера, включая проверку, необходимо сначала выполнить репликацию в примере из `DataModificationEvents.aspx` страницы на страницу в этом учебнике `UIValidation.aspx`. Для этого нам нужно скопировать оба `DataModificationEvents.aspx` декларативная разметка страницы и ее исходный код. Сначала скопируйте декларативную разметку, выполнив следующие действия:

1. Откройте `DataModificationEvents.aspx` страницы в Visual Studio
2. Перейдите к декларативной разметке страницы (щелкните «источник» в нижней части страницы)
3. Скопируйте текст внутри `<asp:Content>` и `</asp:Content>` (строки с 3 по 44), как показано на рис. 2.


[![Скопируйте текст внутри &lt;asp: Content&gt; элемента управления](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Рис. 2**: скопируйте текст внутри `<asp:Content>` управления ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Откройте `UIValidation.aspx` страницы
2. Перейдите к декларативной разметке страницы
3. Вставьте текст внутри `<asp:Content>` элемента управления.

Чтобы скопировать исходный код, откройте `DataModificationEvents.aspx.vb` странице и скопируйте только текст *в* `EditInsertDelete_DataModificationEvents` класса. Скопируйте три обработчика событий (`Page_Load`, `GridView1_RowUpdating`, и `ObjectDataSource1_Inserting`), в отличие от **не** скопируйте объявления класса. Вставьте скопированный текст *в* `EditInsertDelete_UIValidation` в класс `UIValidation.aspx.vb`.

После перемещения содержимого и кода из `DataModificationEvents.aspx` для `UIValidation.aspx`, Отвлекитесь и своих успехов в браузере. Вы должны увидеть идентичные результаты и те же функциональные возможности, в каждой из этих двух страниц (вернуться к рис. 1 для снимок экрана `DataModificationEvents.aspx` в действии).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Шаг 2: Преобразование полей BoundField в поля TemplateField

Добавление элементов управления проверки к интерфейсам правки и вставки, используемые элементами управления DetailsView и GridView полей BoundFields должны быть преобразованы в поля TemplateField. Для этого щелкните ссылки Правка столбцов и изменить поля в GridView и DetailsView смарт-тегов, соответственно. Выберите каждое из полей BoundFields и щелкните ссылку «Преобразуйте это поле в TemplateField».


[![Преобразования каждого из полей BoundField DetailsView и GridView в поля TemplateField](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Рис. 3**: преобразования каждого элемента DetailsView и GridView полей BoundField в поля TemplateField ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Преобразование BoundField в поле TemplateField в диалоговом окне поля создает TemplateField, в котором видно, те же интерфейсы только для чтения, правки и вставки, BoundField, сам. В следующей разметке показан декларативный синтаксис для `ProductName` в DetailsView после его преобразования в поле TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Обратите внимание, что с этой TemplateField автоматически создается три шаблона `ItemTemplate`, `EditItemTemplate`, и `InsertItemTemplate`. `ItemTemplate` Отображает значение поля данных single (`ProductName`) с помощью элемента управления Label Web, тогда как `EditItemTemplate` и `InsertItemTemplate` представлять значение поля данных в текстовое поле веб-элемент, который связывает поле данных с помощью текстового поля `Text` свойство, использование двусторонней привязки данных. Так как мы только при использовании элемента управления DetailsView на этой странице для вставки, вы можете удалить `ItemTemplate` и `EditItemTemplate` из двух полей TemplateField, несмотря на то, что нет никакого вреда от оставляйте их имена.

Поскольку GridView не поддерживает встроенный вставки объектов DetailsView, преобразование GridView `ProductName` поле в TemplateField приводит только `ItemTemplate` и `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Нажимая кнопку «Преобразовать это поле в TemplateField», Visual Studio создала шаблоны которого имитируют интерфейс пользователя из преобразованного BoundField TemplateField. Это можно проверить, посетив эту страницу через обозреватель. Вы обнаружите, что внешний вид и поведение полей TemplateField аналогичны таковым при использовании полей BoundField вместо этого.

> [!NOTE]
> Вы можете настроить интерфейсы правки и шаблоны. Например, необходимо иметь текстовое поле `UnitPrice` поля TemplateField, отображаемой в качестве текстового поля меньше чем `ProductName` текстового поля. Для выполнения этой задачи можно задать текстового поля `Columns` свойства соответствующее значение или предоставить абсолютную ширину с помощью `Width` свойство. В следующем учебном курсе будет показано, как полностью персонализировать интерфейс правки путем замены текстового поля с другой записью веб-элемента управления.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Шаг 3: Добавление элементов управления проверки к элементу GridView`EditItemTemplate` s

При конструировании форм ввода данных, важно, чтобы пользователи вводили все необходимые поля и что все предоставленные входные данные являются правовых, правильный формат значениями. Чтобы гарантировать, что входные данные пользователя являются допустимыми, ASP.NET предоставляет пять встроенных элементов управления проверки, которые предназначены для использования для проверки значения одного элемента управления ввода:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) гарантирует, что предоставлено значение
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) проверяет значение постоянное значение или значения элемента управления Web или гарантирует, что формат значения является допустимым для указанного типа данных
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) гарантирует, что значение в диапазоне значений
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) проверяет значение на соответствие [регулярных выражений](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) проверяет значение на соответствие пользовательские, определяемый пользователем метод

Дополнительные сведения о этих пяти элементах управления ознакомьтесь [разделе проверяющие элементы управления](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) из [ASP.NET по основам](https://asp.net/QuickStart/aspnet/).

В этом руководстве нам потребуется использовать RequiredFieldValidator в GridView и DetailsView `ProductName` полей TemplateField и RequiredFieldValidator в элементе DetailsView `UnitPrice` TemplateField. Кроме того, нам нужно будет добавить элемент управления CompareValidator для обоих элементов управления `UnitPrice` поля TemplateField, который гарантирует, что введенной цены имеет значение больше или равно 0 и представлен в допустимом формате валюты.

> [!NOTE]
> Хотя в ASP.NET 1.x имеются эти же пять элементов управления проверки, ASP.NET 2.0 добавлен ряд улучшений, основной, два сценария на стороне клиента, поддержка обозревателей, отличных от Internet Explorer и способность разделять элементы управления проверки на странице в группы проверки. Дополнительные сведения о новых функциях управления проверки в 2.0 см. [разбор элементов управления проверки в ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Начнем с добавления необходимых элементов управления проверки к `EditItemTemplate` s в поля TemplateField GridView. Для этого щелкните ссылку Изменить шаблоны смарт-теге элемента GridView для вызова интерфейса изменения шаблонов. На этой странице можно выбрать шаблон для правки из раскрывающегося списка. Поскольку нам требуется дополнить интерфейс правки, нам нужно добавить элементы управления проверки к `ProductName` и `UnitPrice`в `EditItemTemplate` s.


[![Нам нужно расширить ProductName и UnitPrice к шаблонам EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Рис. 4**: нам необходимо расширить `ProductName` и `UnitPrice` `EditItemTemplate` s ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


В `ProductName` `EditItemTemplate`, добавьте RequiredFieldValidator, перетащив его с панели инструментов в интерфейс редактирования шаблона, поместив после текстового поля.


[![Добавьте RequiredFieldValidator к ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Рис. 5**: добавьте RequiredFieldValidator к `ProductName` `EditItemTemplate` ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Все элементы управления проверки работают, проверяя ввод одного веб-ASP.NET элемента управления. Таким образом, нам нужно указать, что RequiredFieldValidator, мы только что добавили следует проверить в текстовое поле в `EditItemTemplate`; это достигается путем установки свойства элемента управления проверки [свойство ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) для `ID` соответствующего веб-элемента управления. Текстовое поле в данный момент имеет довольно неопределенный `ID` из `TextBox1`, но давайте изменим его на что-нибудь более подходящее. Щелкните текстовое поле в шаблоне и затем из окна свойств измените `ID` из `TextBox1` для `EditProductName`.


[![Изменить идентификатор текстового поля, на EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Рис. 6**: изменение текстового поля `ID` для `EditProductName` ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Затем задайте RequiredFieldValidator `ControlToValidate` свойства `EditProductName`. Наконец, установите [свойство ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) на «Необходимо указать название продукта» и [свойство Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) для "\*«. `Text` Значение свойства, если указано, — это текст, отображаемый элементом управления проверки в том случае, если проверка завершается неудачно. `ErrorMessage` Значение свойства, которое является обязательным, используемый элементом управления ValidationSummary; Если `Text` значение свойства задано, `ErrorMessage` также является текст, отображаемый элементом управления проверки в случае недопустимого ввода.

После установки этих трех свойств RequiredFieldValidator, экран должен выглядеть примерно как на рис.


[![Задайте RequiredFieldValidator – ControlToValidate, ErrorMessage и свойства текста](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Рис. 7**: задать RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, и `Text` свойства ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


С помощью RequiredFieldValidator, добавляемый `ProductName` `EditItemTemplate`, что все, что остается только добавить необходимую проверку к `UnitPrice` `EditItemTemplate`. Так как мы решили, что для этой страницы `UnitPrice` обязательно при правке записи, нам не нужно добавить RequiredFieldValidator. Тем не менее, необходимо добавить элемент управления CompareValidator чтобы убедиться, что `UnitPrice`, если предоставлено, верный формат как валюты и больше или равно 0.

Прежде чем добавлять CompareValidator для `UnitPrice` `EditItemTemplate`, сперва стоит изменить идентификатор элемента управления TextBox Web из `TextBox2` для `EditUnitPrice`. После этого изменения добавьте CompareValidator, установка его `ControlToValidate` свойства `EditUnitPrice`, ее `ErrorMessage` свойства «цена должна быть больше или равно нулю и не может включать символ валюты» и его `Text` свойства "\*".

Чтобы указать, что `UnitPrice` значение должно быть больше или равно 0, задайте CompareValidator [свойство оператор](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) для `GreaterThanEqual`, ее [свойства ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) «0», а его [ Свойство типа](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) для `Currency`. В следующем показан декларативный синтаксис `UnitPrice` TemplateField `EditItemTemplate` после внесения этих изменений:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

После внесения этих изменений, откройте страницу в браузере. При попытке пропустить имя или ввести недопустимое значение цены при правке продукта рядом с текстовым полем отображается звездочка. Как показано на рис. 8, значение цены, включающее символ валюты, скажем 19,95 долларов США, считается недопустимым. CompareValidator `Currency` `Type` допускает разделители между цифрами (такие как запятые или точки, в зависимости от параметров языка и региональных параметров) и плюса или минуса, но *не* допускает символ валюты. Это поведение может озадачить пользователей, поскольку интерфейс правки в настоящий момент отображает `UnitPrice` в формате валюты.

> [!NOTE]
> Помните, что в *события, связанные с вставки, обновления и удаления* руководстве, мы устанавливаем BoundField `DataFormatString` свойства `{0:c}` Чтобы отформатировать его как денежная единица. Кроме того, мы устанавливаем `ApplyFormatInEditMode` присваивается значение true, приводит к GridView интерфейсам редактирования и вставки для форматирования `UnitPrice` как денежная единица. При преобразовании типа BoundField в поле TemplateField, Visual Studio отметила эти параметры и текстового поля в формате `Text` как денежную единицу, используя синтаксис привязки данных `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Звездочка рядом с текстовые поля, содержащими Недопустимый ввод](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Рис. 8**: звездочка появляется "Далее" для текстовых полей с недопустимые входные данные ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Когда проверка работает как-является, пользователь должен вручную удалить символ валюты при правке записи, что неприемлемо. Чтобы исправить это, у нас есть три варианта:

1. Настройка `EditItemTemplate` таким образом, чтобы `UnitPrice` значение отформатировано как денежная единица.
2. Разрешает пользователю вводить символ валюты путем удаления CompareValidator и заменив RegularExpressionValidator, который нормально проверяет наличие верно отформатированного денежного значения. Проблема здесь том, что регулярное выражение для проверки денежного значения не красивой и потребует написания кода, если необходимо включить параметры языка и региональных параметров.
3. Полностью удалить проверяющего элемента управления и полагаться на логику проверки на стороне сервера в GridView `RowUpdating` обработчик событий.

Мы выберем параметр #1 для этого упражнения. В настоящее время `UnitPrice` форматируется как денежная единица из-за выражение привязки данных для текстового поля в `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Измените оператор привязки на `Bind("UnitPrice", "{0:n2}")`, который форматирует результат как число с двумя цифрами точности. Это можно сделать напрямую через декларативный синтаксис или щелкнув ссылку Edit DataBindings из `EditUnitPrice` соответствующее текстовое поле в `UnitPrice` TemplateField `EditItemTemplate` (см. рис. 9 и 10).


[![Щелкните ссылку Edit DataBindings текстового поля](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Рис. 9**: щелкните ссылку Edit DataBindings текстового поля ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Указание указателя формата в операторе Bind](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Рис. 10**: Указание указателя формата в `Bind` инструкции ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Благодаря этому изменению в отформатированную цену в интерфейсе правки входят запятые как разделители групп и точка как десятичный разделитель, но оставляет символ валюты.

> [!NOTE]
> `UnitPrice` `EditItemTemplate` Не включает в себя RequiredFieldValidator, позволяя обратным передачам выходить и логике обновлений запускаться. Тем не менее `RowUpdating` обработчик событий, копируемые из *Проверка события, связанные с вставки, обновления и удаления* руководства входит Программная проверка, обеспечивающая `UnitPrice` предоставляется. Вы можете удалить эту логику, оставьте его в виде — является, или добавьте RequiredFieldValidator к `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Шаг 4: Суммирование проблем с вводом данных

В дополнение к пяти проверяющие элементы управления, включает ASP.NET [управления ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), которое отображает `ErrorMessage` s элементов управления проверки, обнаруживших недопустимые данные. Эти сводные данные могут отображаться в виде текста на веб-странице или через модальное, клиентские messagebox. Давайте усовершенствуем для включения клиентского messagebox, обобщение проблем, выявленных проверкой.

Для этого перетащите элемент управления ValidationSummary из области элементов в конструктор. Расположение проверяющий элемент управления не имеет значения, так как мы собираемся настроить его на отображение сводки только в качестве messagebox. После добавления элемента управления, задайте его [свойство ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) для `False` и его [свойство ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) для `True`. В результате этого добавления в messagebox клиентские приведены все ошибки проверки.


[![Проверка ошибки выдаются сводкой в Messagebox стороне клиента](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Рис. 11**: проверки ошибки выдаются сводкой в Messagebox клиентские ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Шаг 5: Добавление элементов управления проверки к элементу управления DetailsView`InsertItemTemplate`

Остается в этом руководстве только добавление элементов управления проверки к интерфейсу вставки DetailsView. Процесс добавления элементов управления проверки к шаблонам DetailsView идентичен описанному на этапе 3; Таким образом здесь мы проскочим через него, на этом шаге. Как мы сделали с GridView `EditItemTemplate` s, я рекомендую Переименовать `ID` s из текстовых полей из неопределенный `TextBox1` и `TextBox2` для `InsertProductName` и `InsertUnitPrice`.

Добавьте RequiredFieldValidator к `ProductName` `InsertItemTemplate`. Задайте `ControlToValidate` для `ID` текстового поля в шаблоне, его `Text` свойства "\*" и его `ErrorMessage` значение «Необходимо указать название продукта».

Так как `UnitPrice` — требуется для этой страницы при добавлении новой записи, добавьте RequiredFieldValidator к `UnitPrice` `InsertItemTemplate`, устанавливая его `ControlToValidate`, `Text`, и `ErrorMessage` свойства соответствующим образом. Наконец, добавьте элемент управления CompareValidator для `UnitPrice` `InsertItemTemplate` , настроив его `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, и `ValueToCompare` свойства, как мы это делали `UnitPrice`элемента CompareValidator в GridView `EditItemTemplate`.

После добавления этих элементов управления проверки, новый продукт не добавлены в систему, если его имя не указано или если цена является отрицательным числом или находится в недопустимом формате.


[![Логика проверки добавлена к интерфейсу вставки DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Рис. 12**: логика проверки добавлена к интерфейсу вставки DetailsView ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Шаг 6: Разделение элементов управления проверки на группы проверки

Наша страница состоит из двух логически неравных наборов элементов управления проверки: те, которые соответствуют GridView редактирования интерфейса и соответствующих DetailsView интерфейсу правки. По умолчанию при обратной передаче *все* проверяются элементы управления проверки на странице. Тем не менее при редактировании записи мы не хотим интерфейсу вставки DetailsView проверяющие элементы управления для проверки. Рис. 13 иллюстрируют текущую дилемму, когда пользователь изменяет продукт, вводя вполне допустимые значения, щелкнув обновления вызывает ошибку проверки, поскольку значения название и цену в интерфейсе правки пусты.


[![Обновление продукта вызывает элементов управления проверки интерфейса вставки для активации](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Рис. 13**: обновление продукта вызывает элементов управления проверки интерфейса вставки для активации ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Элементы управления проверки в ASP.NET 2.0 может быть разделена на группы проверки по их `ValidationGroup` свойство. Чтобы связать набор элементов управления проверки в группу, просто установите их `ValidationGroup` в соответствии с тем же значением. В этом руководстве задайте `ValidationGroup` проверяющих элементов управления в поля TemplateField GridView к `EditValidationControls` и `ValidationGroup` свойств полей TemplateField в элементе DetailsView для `InsertValidationControls`. Эти изменения можно сделать непосредственно в декларативной разметке или в окне свойств при использовании конструктора изменить интерфейс шаблона.

В дополнение к проверке элементы управления, кнопки и связанных с кнопкой в ASP.NET 2.0 также включать `ValidationGroup` свойство. Проверки группы проверки проверяются на верность только при вызове обратной передачи с помощью кнопки, который имеет то же `ValidationGroup` значение свойства. Например, в порядке для кнопки "Вставить" в элементе DetailsView для активации `InsertValidationControls` группы проверки, нам нужно установить поля CommandField и `ValidationGroup` свойства `InsertValidationControls` (см. рис. 14). Кроме того, задать GridView поля CommandField и `ValidationGroup` свойства `EditValidationControls`.


[![Присваивает DetailsView свойству поля CommandField и ValidationGroup на InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Рис. 14**: задать DetailsView поля CommandField и `ValidationGroup` свойства `InsertValidationControls` ([Просмотр полноразмерного изображения](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


После внесения этих изменений DetailsView и GridView в поля TemplateField, так и в CommandFields должен выглядеть следующим образом:

Поля TemplateField в элементе DetailsView и CommandField

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField и CommandField GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

На этом этапе элементов управления проверки редактирования срабатывают только при нажатии кнопки "Обновить" для элемента GridView и огонь проверка с учетом вставки элементов управления, только в том случае, при нажатии кнопки "Вставить" в элементе DetailsView, устранение проблему, очерченную рис. 13. Тем не менее это изменение наш элемент управления ValidationSummary больше не отображается при вводе недопустимых данных. Также содержит элемент управления ValidationSummary `ValidationGroup` свойства и отображает сводку информации только для тех элементов управления проверки в его группу проверки. Таким образом, необходимо иметь два элемента управления проверки на этой странице, один для `InsertValidationControls` и второй для `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

На этом дополнении наш учебный курс завершается.

## <a name="summary"></a>Сводка

Хотя поля BoundField, кроме могут обеспечить как интерфейс вставки и редактирования, интерфейс не настраивается. Как правило мы хотим добавить элементы управления проверки к этим интерфейсам, чтобы убедиться, что пользователь вводит требуемые данные в допустимом формате. Для выполнения этой задачи необходимо преобразование полей BoundField в поля TemplateField и добавление элементов управления проверки к соответствующему шаблону(-ам). В этом руководстве мы расширили пример из *Проверка события, связанные с вставки, обновления и удаления* правки учебник, добавление элементов управления проверки к элементу управления DetailsView оба интерфейса и GridView интерфейс редактирования. Кроме того мы видели, способ отображения сводки проверки данных с помощью элемента управления ValidationSummary и разделением элементов управления проверки на странице по отдельным группам проверки.

Как мы видели в этом руководстве, поля TemplateField разрешить интерфейсы правки и вставки, дополнять включения элементов управления проверки. Поля TemplateField также можно расширить для включения дополнительных входных веб-элементы управления, включение заменяется более подходящий веб-элемента управления текстового поля. В нашем следующем учебном курсе мы рассмотрим, как заменить элемент управления TextBox с элементом управления DropDownList привязкой к данным, что идеально подходит при изменении внешнего ключа (таких как `CategoryID` или `SupplierID` в `Products` таблицы).

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. (Liz Shulok) и Зак Джонс, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [Вперед](customizing-the-data-modification-interface-vb.md)
