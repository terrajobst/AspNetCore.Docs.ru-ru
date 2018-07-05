---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Настройка интерфейса изменения данных (Visual Basic) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы рассмотрим способы настройки интерфейса изменяемого элемента управления GridView, заменяя стандартные текстовые поля и CheckBox альтернатив...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 5eea1f226cbcfa07c2fcca3d68fb5ee1cb88ab61
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370381"
---
<a name="customizing-the-data-modification-interface-vb"></a>Настройка интерфейса изменения данных (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) или [скачать PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> В этом руководстве мы рассмотрим способы настройки интерфейса изменяемого элемента управления GridView, заменяя стандартные текстовые поля и CheckBox альтернативный веб-элементами управления ввода.


## <a name="introduction"></a>Вступление

Поля BoundFields и CheckBoxFields, используемые элементами управления GridView и DetailsView упрощают процесс изменения данных из-за возможности для подготовки к просмотру интерфейсы только для чтения, редактирования и пригодным для вставки. Эти интерфейсы могут подготавливаться без необходимости добавления дополнительных декларативная разметка и код. Однако интерфейсы BoundField и CheckBoxField элемента не хватает настройки, часто требуется в сценарии из реальной жизни. Чтобы настроить интерфейс редактирования или пригодным для вставки в GridView или DetailsView, нам нужно вместо этого использовать TemplateField.

В [предыдущем учебном курсе](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) мы узнали, как настройка интерфейса изменения данных путем добавления веб-элементов управления проверки. В этом руководстве мы рассмотрим способы настройки фактические данные коллекции веб-элементов управления, заменив BoundField и стандартные текстовые поля в CheckBoxField и элементов управления CheckBox, альтернативный веб-элементами управления ввода. В частности мы создадим изменяемого элемента управления GridView, позволяющий продукта имя, категорию, поставщика и снят с производства обновляться. При редактировании конкретной строки, категорию и поставщика поля будут отображаться как элементов управления DropDownList, содержащую набор доступных категорий и поставщиков для выбора. Кроме того, мы заменим CheckBoxField по умолчанию флажок элемент управления RadioButtonList, который предлагает два варианта: «Активный» и «Более не поддерживается».


[![Интерфейс правки элемента GridView включает элементов управления DropDownList и переключатели](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Рис. 1**: редактирование интерфейс включает элементов управления DropDownList элемента GridView и переключатели ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Шаг 1: Создание соответствующего`UpdateProduct`перегрузки

В этом руководстве мы построим изменяемого элемента управления GridView, позволяющий редактирование имя продукта, категории, поставщика, который более не поддерживается состояние. Таким образом, нам нужно `UpdateProduct` перегрузку, которая принимает пять входных параметров, значения этих четырех продукта, а также `ProductID`. Как и в наших предыдущих перегрузок, это будет один:

1. Получения сведений о продуктах из базы данных для указанного `ProductID`,
2. Обновление `ProductName`, `CategoryID`, `SupplierID`, и `Discontinued` поля, и
3. Отправить запрос на обновление DAL через TableAdapter `Update()` метод.

Для краткости для Данная конкретная перегрузка я опустил правило проверки бизнеса, которая гарантирует, что единственный продукт, предлагаемых поставщик не помечаются как снятые с продажи продукта. Вы можете добавить его в, если вы предпочитаете, или, в идеальном случае Реструктурируйте логику в отдельный метод.

В следующем коде показано новое `UpdateProduct` перегрузки в `ProductsBLL` класса:


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Шаг 2: Создание изменяемого элемента управления GridView

С помощью `UpdateProduct` добавлена перегрузка, мы готовы к созданию наших изменяемого элемента управления GridView. Откройте `CustomizedUI.aspx` странице в `EditInsertDelete` папку и добавьте элемент управления GridView в конструктор. Создайте новый ObjectDataSource смарт-теге элемента GridView. Настройка ObjectDataSource для получения сведений о продуктах с помощью `ProductBLL` класса `GetProducts()` метода и обновления данных с помощью продукта `UpdateProduct` мы только что созданной перегрузки. На вкладках INSERT и DELETE выберите (нет) из раскрывающихся списков.


[![Настройка ObjectDataSource на использование только что созданной перегрузки UpdateProduct](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Рис. 2**: Настройка ObjectDataSource для использования `UpdateProduct` перегрузки только что создали ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image6.png))


Как мы видели на протяжении всего руководства изменения данных, декларативный синтаксис для элемента управления ObjectDataSource, созданные с помощью Visual Studio назначает `OldValuesParameterFormatString` свойства `original_{0}`. Это, само собой, не будет работать с нашей уровня бизнес-логики, поскольку наши методы не рассчитывают на получение исходного `ProductID` передать в значение. Таким образом, как мы делали в предыдущих учебных курсах, Отвлекитесь и удалить это назначение свойства из декларативного синтаксиса или вместо этого установить значение этого свойства в `{0}`.

После этого изменения декларативная разметка ObjectDataSource должен выглядеть следующим образом:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Обратите внимание, что `OldValuesParameterFormatString` свойство была удалена и что `Parameter` в `UpdateParameters` для каждого входного параметра, наши `UpdateProduct` перегрузки.

Хотя элемент управления ObjectDataSource настроен для обновления только подмножества значений продукта, в настоящее время показывает GridView *все* поля продукта. Отвлекитесь и измените GridView, чтобы:

- Он включает только `ProductName`, `SupplierName`, `CategoryName` поля BoundFields и `Discontinued` CheckBoxField
- `CategoryName` И `SupplierName` полей для отображения перед (слева от) `Discontinued` CheckBoxField
- `CategoryName` И `SupplierName` поля BoundField, кроме `HeaderText` свойство имеет значение «Category» и «Поставщик», соответственно
- Включена поддержка редактирования (установите флажок Enable Editing в смарт-теге элемента GridView)

После внесения этих изменений конструктор будет выглядеть на рис. 3 с помощью декларативного синтаксиса элемента GridView, показано ниже.


[![Удалить ненужные поля из GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Рис. 3**: удалить ненужные поля из GridView ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

На этом этапе завершена GridView поведение только для чтения. При просмотре данных, каждого продукта отображается в виде строки в GridView, Отображение названия продукта, категории, поставщика и более не поддерживается состояние.


[![Интерфейс только для чтения элемента GridView — завершено](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Рис. 4**: интерфейс только для чтения элемента GridView — Complete ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image12.png))


> [!NOTE]
> Как уже говорилось в [Обзор Вставка, обновление и удаление данных учебника](an-overview-of-inserting-updating-and-deleting-data-cs.md), очень важно, чтобы GridView состояние представления s быть включены (поведение по умолчанию). Если задать GridView s `EnableViewState` свойства `false`, возникает риск случайного удаления или изменения записей параллельно работающими пользователями. См. в разделе [предупреждение: проблемы параллелизма с помощью ASP.NET 2.0 элементов управления GridView/DetailsView/FormView среды, поддерживающих правку и/или удаление и которых состояние просмотра отключено](http://scottonwriting.net/sowblog/posts/10054.aspx) Дополнительные сведения.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Шаг 3: С помощью элемента управления DropDownList для категорию и поставщика, редактирования интерфейсы

Помните, что `ProductsRow` объект содержит `CategoryID`, `CategoryName`, `SupplierID`, и `SupplierName` свойства, которые содержат фактические значения идентификатора внешнего ключа в `Products` базы данных, таблицы и соответствующий `Name` значения в `Categories` и `Suppliers` таблицы. `ProductRow` `CategoryID` И `SupplierID` могут быть чтение из и записывать данные, тогда как `CategoryName` и `SupplierName` свойства отмечены как доступные только для чтения.

Из-за состояния только для чтения `CategoryName` и `SupplierName` свойства, имели соответствующие поля BoundField, кроме их `ReadOnly` свойство значение `True`, предотвращая изменение при редактировании строки эти значения. Хотя можно установить `ReadOnly` свойства `False`, подготовки к просмотру `CategoryName` и `SupplierName` поля BoundField, кроме как текстовые поля во время редактирования, такой подход приведет к исключение при попытке пользователя обновить продукт, так как нет `UpdateProduct` перегрузку, принимающую в `CategoryName` и `SupplierName` входных данных. На самом деле мы не хотим создавать такие перегрузки по двум причинам:

- `Products` В таблице отсутствует `SupplierName` или `CategoryName` поля, но `SupplierID` и `CategoryID`. Таким образом мы хотим наш метод передаваемые этими конкретными значениями идентификатора, не их таблиц подстановки значений.
- Пользователю нужно ввести имя поставщика или категории не является идеальным, так как требует от пользователя запоминать доступные категории и поставщики и их правильное написание.

Поставщика и категории поля следует отображать, категории и имена поставщиков в режиме только для чтения (как это делается сейчас) и выберите в раскрывающемся списка применимые параметры при редактировании. С помощью раскрывающегося списка, конечному пользователю можно быстро просмотреть, какие категории и поставщики будут доступны для выбора одного из и многое другое вносить свой выбор.

Чтобы обеспечить такое поведение, нам нужно преобразовать `SupplierName` и `CategoryName` полей BoundField в поля TemplateField которого `ItemTemplate` выдает `SupplierName` и `CategoryName` значения и для которой `EditItemTemplate` использует элемент управления DropDownList для списка Доступные категории и поставщиков.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Добавление`Categories`и`Suppliers`элементов управления DropDownList

Сначала преобразуйте `SupplierName` и `CategoryName` полей BoundField в поля TemplateField,: щелкните ссылку Изменить столбцы смарт-теге элемента GridView; выбрав BoundField в списке в нижней левой; и нажав кнопку «Преобразовать это поле в Ссылка на поле TemplateField». Процесс преобразования создаст TemplateField с обоими `ItemTemplate` и `EditItemTemplate`, как показано в декларативном синтаксисе ниже:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Так как поле типа BoundField был помечен как только для чтения, как `ItemTemplate` и `EditItemTemplate` содержат Label Web свойство `Text` свойство привязано к полю данных применимо (`CategoryName`, в приведенном выше синтаксисе). Нам нужно изменить `EditItemTemplate`, заменив элемент управления Label Web элемент управления DropDownList.

Как мы видели в предыдущих учебных курсах, шаблон можно изменить в конструкторе или непосредственно из декларативного синтаксиса. Чтобы изменить его в конструкторе, щелкните ссылку Изменить шаблоны смарт-теге элемента GridView и работы с помощью поля «Категория» `EditItemTemplate`. Удалить элемент управления Label Web и замените элемент управления DropDownList, присвоить значение DropDownList свойство ID `Categories`.


[![Удалите текстовое поле и добавьте к EditItemTemplate элемента управления DropDownList](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Рис. 5**: удалить текстовое поле и добавление элемента управления DropDownList для `EditItemTemplate` ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image15.png))


Далее нам нужно заполнить DropDownList с доступные категории. Щелкните ссылку выберите источник данных смарт-теге DropDownList и создадим новый ObjectDataSource, именуемый `CategoriesDataSource`.


[![Создать новый элемент управления ObjectDataSource с именем CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Рис. 6**: Создание нового элемента управления ObjectDataSource с именем `CategoriesDataSource` ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image18.png))


Чтобы данный элемент управления ObjectDataSource возвращает все категории, привяжите его к `CategoriesBLL` класса `GetCategories()` метод.


[![Привязать ObjectDataSource к метода GetCategories() CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Рис. 7**: привязать ObjectDataSource к `CategoriesBLL` `GetCategories()` метод ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image21.png))


Наконец, настройте параметры DropDownList таким образом, чтобы `CategoryName` поле должно отображаться на каждой DropDownList `ListItem` с `CategoryID` поле, используемое в качестве значения.


[![Отображается в поле «Категория» и CategoryID использовать в качестве значения](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Рис. 8**: У `CategoryName` поле отображается и `CategoryID` используется в качестве значения ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image24.png))


После внесения этих изменений декларативная разметка для `EditItemTemplate` в `CategoryName` TemplateField будет включать DropDownList и элементу ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> DropDownList в `EditItemTemplate` должен иметь состояние представления включено. Декларативный синтаксис серверного элемента управления DropDownList скоро будет добавлен синтаксис привязки данных и привязки данных команды, такие как `Eval()` и `Bind()` может присутствовать только в элементах управления, состояние представления включено.


Повторите эти шаги для добавления элемента управления DropDownList с именем `Suppliers` для `SupplierName` TemplateField `EditItemTemplate`. Это потребует добавления элемента управления DropDownList для `EditItemTemplate` и создание другой элемент управления ObjectDataSource. `Suppliers` DropDownList ObjectDataSource, тем не менее, должны вызывать `SuppliersBLL` класса `GetSuppliers()` метод. Кроме того, настроить `Suppliers` DropDownList для отображения `CompanyName` поле и использовать `SupplierID` как значение для его `ListItem` s.

После добавления двух элементов управления DropDownList `EditItemTemplate` s, загрузки страницы в браузере и нажмите кнопку Edit для Chef Anton's Cajun Seasoning продукта. Как показано на рис. 9, категорию и поставщика столбцы продукта, отображаются как раскрывающиеся списки, содержащий доступные категории и поставщики для выбора. Тем не менее, обратите внимание, что *первый* элементы в обоих раскрывающихся списках выбраны по умолчанию («напитки» для категории) и Exotic Liquids как поставщик, несмотря на то, что Chef Anton's Cajun Seasoning — это Condiment, предоставляемые New Orleans Cajun Радует.


[![По умолчанию выбран первый элемент в раскрывающемся списке указаны](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Рис. 9**: по умолчанию выбран первый элемент в раскрывающемся списке указаны ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image27.png))


Кроме того, если нажать кнопку обновления, вы обнаружите, что продукта `CategoryID` и `SupplierID` значения устанавливаются в значение `NULL`. Эти нежелательные поведения вызываются в том случае, так как элементов управления DropDownList в `EditItemTemplate` s не привязаны к полям данных из базовых данных продукта.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Привязка элементов управления DropDownList для`CategoryID`и`SupplierID`полей данных

Чтобы имеют категорию и поставщика отредактированный продукта раскрывающиеся списки задать соответствующие значения и с учетом этих значений, отправляемые BLL `UpdateProduct` метод после нажатия кнопки обновления, нам нужно выполнить привязку элементов управления DropDownList `SelectedValue` свойства `CategoryID` и `SupplierID` полей данных, использование двусторонней привязки данных. Для этого с помощью `Categories` DropDownList, вы можете добавить `SelectedValue='<%# Bind("CategoryID") %>'` непосредственно к декларативный синтаксис.

Кроме того можно задать DropDownList databindings, изменив шаблон через конструктор и щелкнув ссылку Edit DataBindings в смарт-теге DropDownList. Затем указать, что `SelectedValue` свойство должно быть связано с `CategoryID` поле использование двусторонней привязки данных (см. рис. 10). Повторите декларативным или конструктора процесса для привязки `SupplierID` поле данных `Suppliers` DropDownList.


[![Привязка CategoryID к DropDownList свойство SelectedValue, использование двусторонней привязки данных](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Рис. 10**: привязка `CategoryID` в элемент управления DropDownList `SelectedValue` двусторонняя привязка с помощью свойства данных ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image30.png))


После привязки были применены к `SelectedValue` свойства двух элементов управления DropDownList, категорию и поставщика столбцы отредактированный продукта будет по умолчанию значения данного продукта. После нажатия кнопки обновления, `CategoryID` и `SupplierID` передаются значения выбранных стрелку раскрывающегося списка элемента `UpdateProduct` метод. Рис. 11 показана руководства, после добавления инструкции привязки данных; Обратите внимание на то, как элементы раскрывающегося списка, выбранного для Chef Anton's Cajun Seasoning будут правильно Condiment и New Orleans Cajun Delights.


[![Изменить текущий категорию и поставщика значения, выбираемые по умолчанию](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Рис. 11**: изменить текущий категорию и поставщика значения, выбираемые по умолчанию ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image33.png))


## <a name="handlingnullvalues"></a>Обработка`NULL`значения

`CategoryID` И `SupplierID` столбцов в `Products` таблица может быть `NULL`, но элементов управления DropDownList в `EditItemTemplate` s не включайте элемент списка для представления `NULL` значение. Это имеет два последствия:

- Пользователь не может использовать наш интерфейс для изменения категории или поставщику продукта, отличным от`NULL` значение `NULL` один
- Если для разработки продукта `NULL` `CategoryID` или `SupplierID`, нажав кнопку "Изменить" приведет к возникновению исключения. Это обусловлено `NULL` значение, возвращенное `CategoryID` (или `SupplierID`) в `Bind()` инструкция не соответствует значению в DropDownList (DropDownList возникло исключение при его `SelectedValue` свойству присваивается значение *не* в его коллекции элементов списка).

Чтобы обеспечить поддержку `NULL` `CategoryID` и `SupplierID` значения, необходимо добавить другой `ListItem` каждый элемент управления DropDownList для представления `NULL` значение. В ["основной/подробности" Фильтрация с помощью элемента управления DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) было показано, как добавить дополнительный `ListItem` для databound элемента управления DropDownList, который участвующих параметр DropDownList `AppendDataBoundItems` свойства `True` и вручную добавлять дополнительный `ListItem`. В этой с предыдущим учебником, тем не менее, мы добавили `ListItem` с `Value` из `-1`. Логика привязки данных в ASP.NET, однако автоматически преобразует пустую строку для `NULL` значение и a наоборот. Таким образом, в этом руководстве мы хотим `ListItem`в `Value` быть пустой строкой.

Начните с установки значения обоих элементов управления DropDownList `AppendDataBoundItems` свойства `True`. Затем добавьте `NULL` `ListItem` , добавив следующий `<asp:ListItem>` элемент для каждого элемента управления DropDownList, чтобы декларативная разметка будет выглядеть как:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Я решил использовать «(None)» как текстовое значение для данного `ListItem`, но его также быть пустой строкой, при желании можно изменить.

> [!NOTE]
> Как мы видели в *"основной/подробности" Фильтрация с помощью элемента управления DropDownList* руководстве `ListItem` s можно добавить через конструктор элемента управления DropDownList, щелкнув DropDownList `Items` свойства в окне «Свойства» (который Отображает `ListItem` редактор коллекции). Тем не менее, не забудьте добавить `NULL` `ListItem` в этом руководстве через декларативный синтаксис. Если вы используете `ListItem` редактор коллекции, созданный декларативный синтаксис не будет включать `Value` параметр полностью, если назначить содержит пустую строку, создание декларативная разметка, таких как: `<asp:ListItem>(None)</asp:ListItem>`. Хотя это может показаться безопасным, отсутствующее значение приводит к DropDownList для использования `Text` значение свойства на его месте. Что означает, что если это `NULL` `ListItem` — флажок установлен, значение «(None)» будет предпринята попытка присвоить `CategoryID`, что приведет к возникновению исключения. При явном задании `Value=""`, `NULL` присваивается значение `CategoryID` при `NULL` `ListItem` выбран.


Повторите эти шаги для список Suppliers DropDownList.

С этим дополнительных `ListItem`, интерфейс редактирования теперь можно назначить `NULL` значения в продукт `CategoryID` и `SupplierID` поля, как показано на рис. 12.


[![Выберите (нет) для присвоения значения NULL для категории или поставщику продукта](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Рис. 12**: выберите значение (None) для назначения `NULL` значение для категории или поставщику продукта ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Шаг 4: Использование переключатели неподдерживаемые состояния

В настоящее время продукты `Discontinued` поля данных выражается с помощью CheckBoxField, который отображает Снятый флажок для строк только для чтения и поддержкой флажок для редактируемой строки. Хотя этот пользовательский интерфейс подход часто применяется, мы можно настроить при необходимости с помощью TemplateField. В этом учебнике давайте изменим CheckBoxField в поле TemplateField, используется элемент управления RadioButtonList с двумя вариантами «Активный» и «Больше» из которого пользователь может указать продукта `Discontinued` значение.

Сначала преобразуйте `Discontinued` CheckBoxField в поле TemplateField, которая создаст TemplateField с `ItemTemplate` и `EditItemTemplate`. Оба шаблоны включают флажок с его `Checked` , привязанное к `Discontinued` поле данных, единственное различие между ними, что `ItemTemplate`элемента CheckBox `Enabled` свойство имеет значение `False`.

Замените флажок в оба `ItemTemplate` и `EditItemTemplate` с элементом управления RadioButtonList, параметр оба RadioButtonLists `ID` свойства `DiscontinuedChoice`. Затем указывается, что RadioButtonLists должны каждый содержат два переключателя, один с меткой «активно» со значением «False» и надписью «Более не поддерживается» со значением «True». Для этого вы можете ввести `<asp:ListItem>` элементов в напрямую через декларативный синтаксис или используйте `ListItem` редактор коллекции из конструктора. Рис. 13 показан `ListItem` редактор коллекции после двух переключателей кнопку параметры были указаны.


[![Добавить в активных и неподдерживаемые параметры RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Рис. 13**: Добавление Active и неподдерживаемые параметры RadioButtonList ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image39.png))


С момента RadioButtonList в `ItemTemplate` не должно быть закрытым для правки, задайте его `Enabled` свойства `False`, оставляя `Enabled` свойства `True` (по умолчанию) для RadioButtonList в `EditItemTemplate`. Это позволит упростить переключателей в строке не изменять только для чтения, но разрешает пользователю изменять значения RadioButton для редактируемой строки.

Мы по-прежнему необходимо назначать элементы управления RadioButtonList `SelectedValue` свойства, чтобы выделить соответствующий переключатель на основе продукта `Discontinued` поля данных. С помощью элементов управления DropDownList, проверить ранее в этом учебнике, этот синтаксис привязки данных можно добавить непосредственно в декларативной разметке или с помощью ссылки Edit DataBindings в смарт-тегов RadioButtonLists.

После добавления двух RadioButtonLists и настройки их, `Discontinued` TemplateField декларативная разметка должна выглядеть так:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Благодаря этим изменениям `Discontinued` был преобразован столбец из списка флажков в список пар кнопку radio (см. рис. 14). При правке продукта, соответствующий переключатель выбран, и неподдерживаемые состояние продукта можно изменить, выбрав переключатель «другие» и нажав кнопку обновления.


[![Неподдерживаемые флажки были заменены пары кнопку переключателя](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Рис. 14**: неподдерживаемые флажки были заменены пары кнопку Radio ([Просмотр полноразмерного изображения](customizing-the-data-modification-interface-vb/_static/image42.png))


> [!NOTE]
> Так как `Discontinued` столбца в `Products` базы данных не может иметь `NULL` значения, нам не нужно беспокоиться о записи `NULL` сведения в интерфейсе. IF, однако `Discontinued` столбец может содержать `NULL` значения, может понадобиться добавить третий переключатель в список с его `Value` задать пустую строку (`Value=""`), подобно тому, как с помощью категорию и поставщика элементов управления DropDownList.


## <a name="summary"></a>Сводка

Хотя BoundField и CheckBoxField автоматически отображать интерфейсы только для чтения, правки и вставки, они не в состоянии для настройки. Часто, однако нам нужно будет настроить правки или вставки интерфейса, возможно добавление элементов управления проверки (как мы видели в предыдущем учебном курсе) или путем настройки пользовательского интерфейса сбора данных (как мы видели в этом руководстве). Настройка интерфейса с TemplateField можно свести к следующим образом:

1. Добавление поля TemplateField или преобразовать существующий BoundField или CheckBoxField в поле TemplateField
2. Дополнить интерфейс, при необходимости
3. Привязка поля данных вновь добавленный элементами управления веб-использование двусторонней привязки данных

Помимо использования встроенных элементов управления ASP.NET Web, также можно настроить шаблоны TemplateField специальными, скомпилированными серверных элементов управления и пользовательские элементы управления.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Вперед](implementing-optimistic-concurrency-vb.md)
