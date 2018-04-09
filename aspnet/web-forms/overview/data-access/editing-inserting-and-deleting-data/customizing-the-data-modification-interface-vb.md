---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Настройка интерфейса изменения данных (Visual Basic) | Документы Microsoft
author: rick-anderson
description: В этом учебнике мы рассмотрим способы настройки интерфейса изменяемого элемента управления GridView, заменив стандартные текстовые поля и CheckBox альтернатив...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7d334a86fcf2fbd1069628527c6e89f3ab655dd5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="customizing-the-data-modification-interface-vb"></a>Настройка интерфейса изменения данных (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) или [скачать PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> В этом учебнике мы рассмотрим способы настройки интерфейса изменяемого элемента управления GridView, заменив стандартные текстовые поля и CheckBox альтернативных ввода веб-элементов управления.


## <a name="introduction"></a>Вступление

Стояли и CheckBoxFields, используемым элементами управления GridView и DetailsView упростить процесс изменения данных из-за возможности для отображения только для чтения, для редактирования и вставляемые интерфейсов. Эти интерфейсы может осуществляться без необходимости добавления дополнительных декларативная разметка и код. Однако BoundField и в CheckBoxField интерфейсов не хватает подлежит, часто требуется в реальных сценариях. Чтобы настроить интерфейс для редактирования или вставляемые в GridView и DetailsView, нам нужно вместо этого использовать TemplateField.

В [предыдущего учебника](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) мы узнали, как настроить интерфейсы изменения данных путем добавления веб-элементов управления проверки. В этом учебнике мы рассмотрим процесс настройки фактические данные коллекции веб-элементов управления, заменив BoundField и стандартные текстовые поля в CheckBoxField и элементов управления CheckBox, альтернативный веб-элементами управления ввода. В частности мы выполним сборку изменяемого элемента управления GridView, позволяющий продукта имя категории, поставщиков и неподдерживаемые состояние обновления. При редактировании конкретной строки, категорию и поставщика поля будут отображаться как элементами управления DropDownList, содержащую набор доступных категорий и поставщиков для выбора. Кроме того, мы заменим CheckBoxField по умолчанию флажок элемента управления RadioButtonList, который предлагает два варианта: «Active» и «Больше».


[![Интерфейс редактирования элемента GridView включает элементами управления DropDownList и элементы управления RadioButton](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Рис. 1**: редактирование интерфейса включает элементами управления DropDownList GridView и переключатели ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Шаг 1: Создание соответствующего`UpdateProduct`перегрузки

В этом учебнике мы построим изменяемого элемента управления GridView, позволяющий редактирование название продукта, категория, поставщик и неподдерживаемые состояния. Таким образом, нам нужно `UpdateProduct` перегрузку, которая принимает пять входных параметров, эти четыре значения, а также `ProductID`. Как и в наших предыдущих перегрузки одного будет:

1. Получить сведения о продукте из базы данных для указанного `ProductID`,
2. Обновление `ProductName`, `CategoryID`, `SupplierID`, и `Discontinued` поля, и
3. Отправить запрос на обновление DAL через TableAdapter `Update()` метод.

Для краткости для этой конкретной перегрузки я не Проверка правила бизнеса, что гарантирует, что продукт, отмеченных как неподдерживаемые не только продукта, предоставляемой его поставщиком. Можно добавить его в, если вы предпочитаете, или, в идеальном случае рефакторинга логическую схему отдельный метод.

В следующем коде показан новый `UpdateProduct` перегрузки в `ProductsBLL` класса:


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Шаг 2: Дополнительно изменяемого элемента управления GridView

С `UpdateProduct` перегрузка добавлены все готово для создания нашей изменяемого элемента управления GridView. Откройте `CustomizedUI.aspx` страницы в `EditInsertDelete` папки и добавление элемента управления GridView в конструктор. Создайте новый элемент управления ObjectDataSource смарт-теге элемента GridView. Настройка ObjectDataSource для получения сведений о продукте через `ProductBLL` класса `GetProducts()` метод и обновления данных с помощью продукта `UpdateProduct` перегрузку, мы только что создали. На вкладках INSERT и DELETE выберите (нет) из раскрывающихся списков.


[![Настройка с использованием перегрузки UpdateProduct только что созданный элемент управления ObjectDataSource](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**На рисунке 2**: Настройка ObjectDataSource для использования `UpdateProduct` перегружать только что создали ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image6.png))


Как видно на протяжении учебники по модификации данных декларативного синтаксиса для ObjectDataSource, созданные с помощью Visual Studio назначает `OldValuesParameterFormatString` свойства `original_{0}`. Это, конечно, не будет работать с нашей уровня бизнес-логики с момента наши методы не ожидается, что исходный `ProductID` значение для передачи. Таким образом, как мы использовали в предыдущих учебниках, пора удалить это назначение свойства из декларативного синтаксиса, или вместо этого установить значение этого свойства в `{0}`.

После этого изменения ObjectDataSource должен выглядеть следующим образом:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Обратите внимание, что `OldValuesParameterFormatString` было удалено и нет `Parameter` в `UpdateParameters` коллекции для каждого входного параметра, наши `UpdateProduct` перегрузки.

При ObjectDataSource настроена для обновления только подмножество значений продукта, в настоящий момент отображается GridView *всех* полей продукта. Теперь пора изменить GridView, чтобы:

- Он включает только `ProductName`, `SupplierName`, `CategoryName` стояли и `Discontinued` CheckBoxField
- `CategoryName` И `SupplierName` поля будет отображаться перед (по левому краю) `Discontinued` CheckBoxField
- `CategoryName` И `SupplierName` стояли `HeaderText` свойство имеет значение «Категория» и «Поставщик» соответственно
- Включена поддержка редактирования (флажок Разрешить изменение в смарт-теге элемента GridView)

После внесения этих изменений конструктор будет выглядеть на рис. 3 с помощью декларативного синтаксиса элемента GridView, показано ниже.


[![Удалить ненужные поля из GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Рис. 3**: удалить ненужные поля из GridView ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

На этом этапе завершена GridView поведение только для чтения. При просмотре данных, каждый продукт отображается в виде строки в GridView, показывающая название продукта, категории, поставщика и неподдерживаемые состояния.


[![Интерфейс только для чтения элемента GridView — завершено.](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Рис. 4**: интерфейс только для чтения GridView является Complete ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image12.png))


> [!NOTE]
> Как было сказано в [Обзор Вставка, обновление и удаление данных учебника](an-overview-of-inserting-updating-and-deleting-data-cs.md), жизненно важна, GridView, быть s состояния просмотра включена (по умолчанию). Если задать GridView s `EnableViewState` свойства `false`, увеличивается риск того что параллельно работающие пользователи непреднамеренно удаление или изменение записей. В разделе [предупреждение: проблемы параллелизма с ASP.NET 2.0 GridViews/DetailsView/FormViews, поддерживают редактирование или удаление и с состояние просмотра отключено](http://scottonwriting.net/sowblog/posts/10054.aspx) для получения дополнительной информации.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Шаг 3: С помощью DropDownList для категорию и редактирования интерфейсы поставщика

Помните, что `ProductsRow` объект содержит `CategoryID`, `CategoryName`, `SupplierID`, и `SupplierName` свойства, которые предоставляют фактический идентификатор значения внешнего ключа в `Products` таблицы и соответствующие базы данных `Name` значения в `Categories` и `Suppliers` таблицы. `ProductRow` `CategoryID` И `SupplierID` можно оба считывать и записывать данные, тогда как `CategoryName` и `SupplierName` свойства помечены только для чтения.

Из-за состояния только для чтения `CategoryName` и `SupplierName` свойства, имели соответствующие стояли их `ReadOnly` свойство `True`, предотвращая внесение при редактировании строки эти значения. Хотя можно выбрать вариант `ReadOnly` свойства `False`, подготовки к просмотру `CategoryName` и `SupplierName` стояли как текстовые поля во время редактирования, такой подход приведет к исключение при попытке пользователя обновить продукт, так как она не `UpdateProduct` , принимающую `CategoryName` и `SupplierName` входные данные. На самом деле мы не хотим создать такие перегрузки по двум причинам:

- `Products` Таблица не содержит `SupplierName` или `CategoryName` поля, но `SupplierID` и `CategoryID`. Таким образом мы хотим наш метод для передачи этих определенного значения идентификатора, не соответствующих таблиц подстановки значений.
- Поскольку требует от пользователя введите имя поставщика или категории не является идеальным, как пользователь знать доступные категории и поставщики и их правильное написание.

Отображать поля поставщика и категории, категории и имена поставщиков в режиме только для чтения (что происходит сейчас) и применимые параметры при редактировании список раскрывающегося списка. С помощью раскрывающегося списка, конечный пользователь может быстро просмотреть, какие категории и поставщики доступны для выбора одного из и более легко можно их выбор.

Чтобы обеспечить это поведение, нам нужно преобразовать `SupplierName` и `CategoryName` стояли в TemplateFields которого `ItemTemplate` выдает `SupplierName` и `CategoryName` значения, для которых `EditItemTemplate` использует элемента управления список Доступные категории и поставщиков.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Добавление`Categories`и`Suppliers`элементами управления DropDownList

Запуск путем преобразования `SupplierName` и `CategoryName` стояли в TemplateFields по: щелкните ссылку Правка столбцов GridView смарт-теге; выбрав в списке в нижней левой; BoundField и нажав кнопку «Преобразовать это поле в Ссылка TemplateField». Процесс преобразования создаст TemplateField с обоими `ItemTemplate` и `EditItemTemplate`, как показано в приведенном ниже синтаксисе декларативных:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Поскольку BoundField был помечен как только для чтения, как `ItemTemplate` и `EditItemTemplate` содержат Web метки свойство `Text` свойство привязано к полю применяется данных (`CategoryName`, в приведенном выше синтаксисе). Нам нужно изменить `EditItemTemplate`, заменив управления DropDownList веб-управления Label.

Как видно в предыдущих учебниках, шаблон можно изменить с помощью конструктора или непосредственно из декларативного синтаксиса. Чтобы изменить его в конструкторе, щелкните ссылку Изменить шаблоны смарт-теге элемента GridView и выберите для работы с поля «Категория» `EditItemTemplate`. Удалите элемент управления Label и замените элемент управления DropDownList, присвоив свойству идентификатора DropDownList `Categories`.


[![Удалите TexBox и добавьте в шаблон EditItemTemplate DropDownList](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Рис. 5**: удалите TexBox и добавьте DropDownList для `EditItemTemplate` ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image15.png))


Далее нам нужно заполнить DropDownList с доступных категорий. Щелкните ссылку Выбор источника данных в смарт-теге DropDownList и создадим новый ObjectDataSource с именем `CategoriesDataSource`.


[![Создать новый элемент управления ObjectDataSource CategoriesDataSource с именем](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Рис. 6**: Создание нового элемента управления ObjectDataSource с именем `CategoriesDataSource` ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image18.png))


Чтобы этот элемент управления ObjectDataSource возвращать все категории, привяжите его к `CategoriesBLL` класса `GetCategories()` метод.


[![Привязать элемент управления ObjectDataSource CategoriesBLL GetCategories() методу](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Рис. 7**: привязать элемент управления ObjectDataSource `CategoriesBLL` `GetCategories()` метод ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image21.png))


И наконец, настройте параметры DropDownList таким образом, что `CategoryName` будет отображаться в каждом DropDownList `ListItem` с `CategoryID` используется в качестве значения поля.


[![Отображается в поле «Категория» и код типа используется в качестве значения](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Рис. 8**: У `CategoryName` отображаются поля и `CategoryID` используется в качестве значения ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image24.png))


После внесения этих изменений декларативная разметка для `EditItemTemplate` в `CategoryName` TemplateField будет включать DropDownList и ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> DropDownList в `EditItemTemplate` должен иметь состояние просмотра включено. Чтобы декларативный синтаксис в DropDownList скоро мы добавим синтаксис привязки данных и команд привязки данных, например `Eval()` и `Bind()` может использоваться только в элементах управления состояние представления которого включен.


Повторите эти шаги для добавления с именем DropDownList `Suppliers` для `SupplierName` в TemplateField `EditItemTemplate`. Это будет включать в себя добавление DropDownList для `EditItemTemplate` и создания другой ObjectDataSource. `Suppliers` DropDownList ObjectDataSource, однако, должны быть настроены для вызова `SuppliersBLL` класса `GetSuppliers()` метод. Кроме того, настроить `Suppliers` DropDownList для отображения `CompanyName` поля и использовать `SupplierID` как значение его `ListItem` s.

После добавления двух элементами управления DropDownList `EditItemTemplate` , загрузить страницу в браузере и нажмите кнопку Правка Креольская Cajun Seasoning продукта. Как показано на рис. 9, продукта, категорию и поставщика столбцы отображаются как раскрывающиеся списки, содержащие доступные категории и поставщики для выбора. Тем не менее, обратите внимание, что *первый* элементы обоих списков раскрывающегося списка выбираются по умолчанию (напитки категории) и экзотические жидкости как поставщик, хотя Seasoning Cajun Креольская Condiment, предоставляемые Cajun Новый Орлеан Преимуществах.


[![По умолчанию выбран первый элемент в раскрывающемся списке указаны](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Рис. 9**: по умолчанию выбран первый элемент в раскрывающемся списке указаны ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image27.png))


Кроме того, если нажать кнопку обновления, вы найдете, продукта `CategoryID` и `SupplierID` значения устанавливаются в значение `NULL`. Эти нежелательные поведения вызванные элементами управления DropDownList в `EditItemTemplate` не привязаны к полям данных из базовых данных продукта.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Привязка элементами управления DropDownList для`CategoryID`и`SupplierID`полей данных

Для измененного продукта категорию и поставщика раскрывающиеся списки задать соответствующие значения и иметь эти значения, отправляемые МЕТОДА `UpdateProduct` метод после нажатия на кнопку обновления, нам нужно привязать элементами управления DropDownList `SelectedValue` свойства `CategoryID` и `SupplierID` полей данных, с помощью двухсторонней привязкой данных. Для этого с `Categories` DropDownList, можно добавить `SelectedValue='<%# Bind("CategoryID") %>'` непосредственно к декларативного синтаксиса.

Кроме того можно задать DropDownList привязки данных, изменив шаблон в конструкторе и щелкните ссылку изменить привязки данных в смарт-теге DropDownList. Далее, указывающий, что `SelectedValue` свойства должны быть привязаны к `CategoryID` поля двусторонняя привязка к данным (см. рис. 10). Повторите декларативной или конструктора процесса для привязки `SupplierID` поле данных `Suppliers` DropDownList.


[![Привязка к свойству SelectedValue DropDownList двусторонняя привязка к данным с помощью CategoryID](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Рис. 10**: привязка `CategoryID` в элемент управления DropDownList `SelectedValue` свойства с помощью двусторонней привязки к данным ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image30.png))


После привязки были применены к `SelectedValue` свойства двумя элементами управления DropDownList, категорию и поставщика столбцы измененного продукта будет по умолчанию значения текущего продукта. После нажатия на кнопку обновления, `CategoryID` и `SupplierID` значения выбранного раскрывающегося элемента списка будет передан `UpdateProduct` метод. Рис. 11 показана учебника после добавления инструкции привязки данных; Обратите внимание, как элементы выбранного раскрывающегося списка для Seasoning Cajun Креольская правильно Condiment и преимуществах нового Орлеана Cajun.


[![По умолчанию выбраны текущей категории и значения поставщика продукта изменен](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Рис. 11**: изменить текущий категорию и поставщика значений по умолчанию выбраны ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image33.png))


## <a name="handlingnullvalues"></a>Обработка`NULL`значения

`CategoryID` И `SupplierID` столбцы в `Products` таблица может быть `NULL`, но элементами управления DropDownList в `EditItemTemplate` не включают элемент списка для представления `NULL` значение. Это имеет два последствия:

- Пользователь не может использовать наши интерфейс изменение поставщику или категории продуктов, отличным от`NULL` значение `NULL` один
- Если продукт имеет `NULL` `CategoryID` или `SupplierID`, нажав кнопку «Изменить» будет приведет к исключению. Это вызвано `NULL` значения, возвращенного `CategoryID` (или `SupplierID`) в `Bind()` инструкция не соответствует значение в раскрывающемся списке (DropDownList приводит к возникновению исключения при его `SelectedValue` свойству присвоено значение *не* в его коллекцию элементов списка).

Чтобы обеспечить поддержку `NULL` `CategoryID` и `SupplierID` значения, необходимо добавить другой `ListItem` для каждого DropDownList для представления `NULL` значение. В [иерархического фильтрации с DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) учебника, мы узнали, как добавить дополнительный `ListItem` для привязки к данным DropDownList, который участвующих параметр DropDownList `AppendDataBoundItems` свойства `True` и вручную, добавив дополнительный `ListItem`. В этом с предыдущим учебником, тем не менее, мы добавили `ListItem` с `Value` из `-1`. Логика привязки данных в ASP.NET, однако автоматически преобразует пустую строку для `NULL` значение и a наоборот. Таким образом, в этом учебнике мы хотим `ListItem` `Value` быть пустой строкой.

Запуск, задав обоих элементами управления DropDownList `AppendDataBoundItems` свойства `True`. Добавьте `NULL` `ListItem` , добавив следующий код `<asp:ListItem>` элемент для каждого DropDownList так выглядит декларативная разметка, такие как:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Я решил «(нет)» используется как текстовое значение для этого `ListItem`, но его также быть пустая строка, при желании можно изменить.

> [!NOTE]
> Как мы видели в *иерархического фильтрации с DropDownList* учебнике `ListItem` s могут добавляться в DropDownList в конструкторе, щелкнув DropDownList `Items` в окне свойств (который Отображает `ListItem` редактор коллекции). Тем не менее, не забудьте добавить `NULL` `ListItem` в этом учебнике посредством декларативного синтаксиса. При использовании `ListItem` редактор коллекции будут исключаться созданный декларативный синтаксис `Value` параметр полностью, если назначенный пустые строки, создавая декларативной разметкой, например: `<asp:ListItem>(None)</asp:ListItem>`. Хотя это может выглядеть опасное, отсутствующее значение вызывает DropDownList для использования `Text` значение свойства на его месте. Что означает, что если это `NULL` `ListItem` — флажок установлен, значение «(нет)» будет предпринята попытка должна назначаться `CategoryID`, что приведет к возникновению исключения. После явного задания `Value=""`, `NULL` присваивается значение `CategoryID` при `NULL` `ListItem` выбран.


Повторите эти шаги для DropDownList поставщиков.

С этим дополнительных `ListItem`, теперь можно назначать интерфейс редактирования `NULL` значения с продуктом `CategoryID` и `SupplierID` поля, как показано на рис. 12.


[![Выберите (нет) для присвоения значения NULL для категории продукта или поставщика](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Рис. 12**: параметр (нет), для назначения `NULL` значение для категории продукта или поставщика ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Шаг 4: Использование переключатели неподдерживаемые состояния

В настоящее время продукта `Discontinued` поля данных выражается с помощью CheckBoxField, который отображает Снятый флажок для строк только для чтения и включен флажок для изменяемой строки. Во время этого пользовательского интерфейса часто подходит, мы можно настраивать при необходимости с помощью TemplateField. В этом учебнике изменим CheckBoxField в TemplateField, используется элемент управления RadioButtonList два варианта «Active» и «Неподдерживаемые» из которого пользователь может указать продукта `Discontinued` значение.

Начните с преобразование `Discontinued` CheckBoxField в TemplateField, который создаст TemplateField с `ItemTemplate` и `EditItemTemplate`. Шаблоны включают флажок с его `Checked` свойство привязано к `Discontinued` поля данных, единственное различие между ними выполняется, `ItemTemplate`флажок в `Enabled` свойству `False`.

Замените флажок в обоих `ItemTemplate` и `EditItemTemplate` с элементом управления RadioButtonList установка обоих RadioButtonLists `ID` свойства `DiscontinuedChoice`. Далее означает, что RadioButtonLists следует каждого содержаться два переключателя, один с меткой «активный» со значением «False» и надписью «Неподдерживаемые» со значением «True». Для этого можно либо ввести `<asp:ListItem>` элементов непосредственно с помощью декларативного синтаксиса или используйте `ListItem` редактор коллекции из конструктора. На рисунке 13 показано `ListItem` редактор коллекции после двух переключателей кнопку параметры были указаны.


[![Добавление RadioButtonList активных и неподдерживаемые параметры](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Рис. 13**: Добавление Active и неподдерживаемые параметры RadioButtonList ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image39.png))


С момента RadioButtonList в `ItemTemplate` не должно быть доступно для редактирования, задайте его `Enabled` свойства `False`, покидая `Enabled` свойства `True` (по умолчанию) для RadioButtonList в `EditItemTemplate`. Это сделает переключатели в строке без изменения только для чтения, но разрешает пользователю изменять значения RadioButton для измененной строки.

Мы по-прежнему необходимо назначить элементы управления RadioButtonList `SelectedValue` свойства, чтобы выделить соответствующий переключатель основе продукта `Discontinued` поля данных. Как и в случае с элементами управления DropDownList, просмотренных ранее в этом учебнике, этот синтаксис привязки данных можно добавить непосредственно в декларативной разметкой или через ссылку изменить привязки данных в RadioButtonLists смарт-тегов.

После добавления двух RadioButtonLists и настраивать их, `Discontinued` в TemplateField должна выглядеть как:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Эти изменения `Discontinued` столбец был преобразован из списка флажков в список пар кнопка переключателей (см. рис. 14). При изменении продукта, соответствующий переключатель установлен и неподдерживаемые состояние продукта может быть обновлен, установив соответствующий переключатель команду Update.


[![Неподдерживаемые флажки были заменены пары кнопка переключателей](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Рис. 14**: неподдерживаемые флажки были заменены переключатель кнопка пары ([Просмотр полноразмерное изображение](customizing-the-data-modification-interface-vb/_static/image42.png))


> [!NOTE]
> Поскольку `Discontinued` столбца в `Products` базы данных не может иметь `NULL` значения, нам не нужно беспокоиться о записи `NULL` сведения в интерфейсе. IF, однако `Discontinued` столбец может содержать `NULL` значения, необходимо добавить третий переключатель в список с его `Value` пустой строкой (`Value=""`) точно так же, как и с категорию и поставщика элементами управления DropDownList.


## <a name="summary"></a>Сводка

Хотя BoundField и CheckBoxField автоматически отображать интерфейсы только для чтения, правки и вставки, у которых отсутствует возможность настройки. Часто, хотя, нам нужно будет настроить правки или вставки интерфейса, возможно добавление элементов управления проверки (как было показано в предыдущем учебнике) или путем настройки пользовательского интерфейса сбора данных (как мы видели в этом учебнике). Настройка интерфейса с TemplateField может быть суммирована следующим образом:

1. Добавление TemplateField или преобразовать существующий BoundField или CheckBoxField в TemplateField
2. Расширяют интерфейс, при необходимости
3. Привязка поля данных для вновь добавленного веб-элементов управления с помощью двухсторонней привязкой данных

Помимо использования встроенных элементов управления ASP.NET Web, можно также настроить шаблоны TemplateField специальными, скомпилированными серверных элементов управления и пользовательские элементы управления.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Вперед](implementing-optimistic-concurrency-vb.md)
