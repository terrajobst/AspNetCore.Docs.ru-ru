---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Пакетного обновления (Visual Basic) | Документация Майкрософт
author: rick-anderson
description: Узнайте, как обновление нескольких записей базы данных в рамках одной операции. В слой пользовательского интерфейса, мы создаем GridView, где каждая строка является редактируемой. В данных...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 7e5062898ca683571df2929eba5d824f9d77accd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833405"
---
<a name="batch-updating-vb"></a>Пакетное обновление (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) или [скачать PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Узнайте, как обновление нескольких записей базы данных в рамках одной операции. В слой пользовательского интерфейса, мы создаем GridView, где каждая строка является редактируемой. В уровень доступа к данным мы Подведем краткие несколько операций обновления в рамках транзакции, чтобы гарантировать, что все обновления успешно или откатываются все обновления.


## <a name="introduction"></a>Вступление

В [предыдущем учебном курсе](wrapping-database-modifications-within-a-transaction-vb.md) мы узнали, как расширить уровень доступа к данным, чтобы добавить поддержку транзакций базы данных. Транзакции базы данных гарантирует, что ряд инструкций, изменяющих данные будут рассматриваться как одной элементарной операцией, которая гарантирует, что все изменения завершится ошибкой, или все успешно завершится. Благодаря этой низкого уровня DAL функции в сторону мы повторно готовы обратим наше внимание на создании пакетной службы интерфейсы изменения данных.

В этом руководстве мы создадим GridView, где каждая строка представляет редактируемый (см. рис. 1). Так как каждая строка подготавливается в его интерфейс редактирования там s устраняет потребность в столбец редактирования, обновления и кнопки отмены. Вместо этого есть две кнопки обновления продуктов на странице, при нажатии для перечисления строк GridView, так и для обновления базы данных.


[![Каждая строка в GridView представляет редактируемый](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Рис. 1**: каждая строка в GridView является редактируемое ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image2.png))


Позвольте s приступить к работе!

> [!NOTE]
> В [выполнение пакетных обновлений](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) руководстве мы создали пакетного редактирования интерфейса с помощью элемента управления DataList. Этот учебник отличается от предыдущего тем, что он использует элемент управления GridView и пакетное обновление выполняется в пределах транзакции. После завершения этого учебника я советую вам, чтобы вернуться в предыдущем учебном курсе и обновить ее, чтобы использовать функции базы данных связанные с транзакциями, добавленные в предыдущем учебном курсе.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Проверка действий по созданию редактируемой всех строк GridView

Как уже говорилось в [Обзор Вставка, обновление и удаление данных](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) руководстве GridView предлагает встроенную поддержку для изменения ее базовых данных, на основе строки. На внутреннем уровне GridView отмечает строку редактируются через его [ `EditIndex` свойство](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Как GridView, привязанный к источнику данных, он проверяет каждую строку, чтобы увидеть, если индекс строки равен значению `EditIndex`. Если Да, интерфейсы этой строки преобразования, поля отображаются с использованием их редактирования. Для полей BoundField, интерфейс редактирования является текстовое поле которого `Text` свойству присваивается значение поля данных, определяемое BoundField s `DataField` свойство. Для поля TemplateField `EditItemTemplate` используется вместо `ItemTemplate`.

Помните, что редактирования рабочий процесс начинается, когда пользователь нажимает кнопку редактирования строк. Это вызывает обратную передачу, задает GridView s `EditIndex` индекс выбранной строки s, а для повторных привязок данных в сетку. При нажатии кнопки "Отмена" строк при обратной передаче `EditIndex` присваивается значение `-1` перед повторной привязкой данных в сетку. Поскольку s строк GridView начинаются индексации с нуля, Настройка `EditIndex` для `-1` приводит к отображению GridView в режиме только для чтения.

`EditIndex` Свойство хорошо подходит для редактирования строки, но не предназначен для пакетной службы редактирования. Чтобы сделать доступными для редактирования всего GridView, необходимо иметь строки, отрисовки с помощью его интерфейс редактирования. Самый простой способ выполнения этой задачи является создание, где каждый редактируемое поле реализуется в соответствии с его интерфейс редактирования TemplateField `ItemTemplate`.

Следующие несколько шагов мы создадим полностью изменяемого элемента управления GridView. На шаге 1 мы начнем с создания GridView и его ObjectDataSource и преобразовать его поля BoundFields и CheckBoxField в поля TemplateField. В шагах 2 и 3 мы перейдем в интерфейсы правки из полей TemplateField `EditItemTemplate` s, чтобы их `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Шаг 1: Отображение информации о продукте

Прежде чем думать о создании GridView которых строк являются изменяемыми, позвольте s сначала просто отображение информации о продукте. Откройте `BatchUpdate.aspx` странице в `BatchData` папки и перетащите элемент управления GridView с панели инструментов в конструктор. Набор GridView s `ID` для `ProductsGrid` и в его смарт-тега выберите, чтобы привязать его к элементу управления ObjectDataSource с именем `ProductsDataSource`. Настройте элемент ObjectDataSource для извлечения данных из `ProductsBLL` класс s `GetProducts` метод.


[![Настройка ObjectDataSource на использование класса ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Рис. 2**: Настройка ObjectDataSource для использования `ProductsBLL` класс ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image4.png))


[![Получить данные продукта, с помощью метода GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Рис. 3**: получение данных продукта с помощью `GetProducts` метод ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image6.png))


Например GridView элемент управления ObjectDataSource с изменения функциями предназначены для работы на основе строки. Чтобы обновить набор записей, нам потребуется написать небольшой фрагмент кода в классе фонового кода страницы s ASP.NET, который упаковывает данные и передает его в BLL. Таким образом задайте раскрывающиеся списки в элемент управления ObjectDataSource s вкладках UPDATE, INSERT и DELETE (нет). Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Рис. 4**: задать раскрывающиеся списки в обновления, вставки и удаления вкладок (нет) ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image8.png))


После завершения работы мастера настройки источников данных, декларативная разметка ObjectDataSource s должен выглядеть следующим образом:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Завершение работы мастера настройки источника данных также приводит к Visual Studio для создания поля BoundFields и CheckBoxField для полей данных продукта в GridView. Для этого руководства позволяют s только разрешает пользователю просматривать и редактировать s название продукта, категории, цены и снят с производства. Удалите все, кроме `ProductName`, `CategoryName`, `UnitPrice`, и `Discontinued` поля и переименуйте `HeaderText` свойства из первых трех полей продукта, категория и цена, соответственно. Наконец установите флажки включить разбиение по страницам и включить сортировку в смарт-теге GridView s.

На этом этапе у GridView есть три поля BoundFields (`ProductName`, `CategoryName`, и `UnitPrice`) и по полю CheckBoxField (`Discontinued`). Нам нужно преобразовать эти четыре поля в поля TemplateField, а затем переместите интерфейс правки из TemplateField s `EditItemTemplate` для его `ItemTemplate`.

> [!NOTE]
> Мы изучили создание и настройка полей TemplateField в [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) руководства. Мы рассмотрим действия преобразования поля BoundFields и CheckBoxField в поля TemplateField и определение их редактирования интерфейсов в их `ItemTemplate` s, но если вы уверены, или требуется такая Дон t каких-либо опасений обращаться к этому руководству более ранней.


Смарт-теге GridView s щелкните ссылку Изменить столбцы, чтобы открыть диалоговое окно "поля". Выберите каждое поле затем щелкните преобразовать это поле в TemplateField ссылку.


![Преобразование существующего поля BoundFields и CheckBoxField в поля TemplateField](batch-updating-vb/_static/image5.gif)

**Рис. 5**: преобразования существующего поля BoundFields и CheckBoxField в поля TemplateField


После того, каждое поле TemplateField, мы будет готов для перемещения редактирования интерфейса из `EditItemTemplate` s, чтобы `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Шаг 2: Создание`ProductName`,`UnitPrice`, и`Discontinued`редактирования интерфейсы

Создание `ProductName`, `UnitPrice`, и `Discontinued` редактирования интерфейсы речь этот шаг и работают довольно просто, как каждый интерфейс уже определено в TemplateField s `EditItemTemplate`. Создание `CategoryName` интерфейс редактирования немного сложнее, поскольку нам необходимо создать DropDownList возможно категорий. Это `CategoryName` интерфейс редактирования является взялись за на шаге 3.

Позвольте s начинаются с `ProductName` TemplateField. Щелкните ссылку Изменить шаблоны в смарт-теге GridView s и детализировать `ProductName` TemplateField s `EditItemTemplate`. Выберите текстовое поле, скопируйте его в буфер обмена и вставьте его в `ProductName` TemplateField s `ItemTemplate`. Измените текстовое поле s `ID` свойства `ProductName`.

Добавьте RequiredFieldValidator к `ItemTemplate` чтобы убедиться, что пользователь предоставит значение для каждого названия продукта s. Задайте `ControlToValidate` свойства ProductName, `ErrorMessage` свойство вам необходимо указать название продукта. и `Text` свойства \*. После внесения этих изменений `ItemTemplate`, экран должен выглядеть аналогично рис. 6.


[![Теперь TemplateField ProductName включает текстовое поле и RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Рис. 6**: `ProductName` TemplateField теперь включает текстовое поле и RequiredFieldValidator ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image10.png))


Для `UnitPrice` интерфейс редактирования, запуска путем копирования из текстового поля `EditItemTemplate` для `ItemTemplate`. Затем поместите $ перед в текстовое поле и задайте его `ID` свойство UnitPrice и его `Columns` значение 8.

Также добавьте элемент управления CompareValidator для `UnitPrice` s `ItemTemplate` чтобы убедиться, что введенное пользователем значение является значением валют, больше или равно 0,00 долл. США. Набор s проверяющий элемент управления `ControlToValidate` свойство UnitPrice, его `ErrorMessage` свойство вам необходимо ввести допустимый денежное значение. Можно опустить любые валюты символы., его `Text` свойства \*, ее `Type` свойства `Currency`, ее `Operator` свойства `GreaterThanEqual`и его `ValueToCompare` значение 0.


[![Добавьте элемент управления CompareValidator для проверить цен, указанных это значение валюты не отрицательны](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Рис. 7**: Добавление CompareValidator, чтобы удостовериться в цену, ввода является значение валюты не отрицательны ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image12.png))


Для `Discontinued` TemplateField используется флажок уже определен в `ItemTemplate`. Просто установите для его `ID` для Discontinued и его `Enabled` свойства `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Шаг 3: Создание`CategoryName`интерфейс редактирования

Интерфейс правки в `CategoryName` TemplateField s `EditItemTemplate` содержит текстовое поле, которое отображает значение `CategoryName` поля данных. Нам нужно заменить с помощью элемента управления DropDownList, перечислены возможные категории.

> [!NOTE]
> [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) учебник содержит обсуждение более тщательной и завершения настройки шаблона для включения элемента управления DropDownList в отличие от текстового поля. При выполнении этих шагов они представлены кратко. Более подробно рассмотрено создание и настройка DropDownList категорий, обращаться к [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) руководства.


Перетащите DropDownList с панели элементов в `CategoryName` TemplateField s `ItemTemplate`, устанавливая его `ID` для `Categories`. На этом этапе мы бы обычно определение элементов управления DropDownList s источника данных через его смарт-тег, создание нового источника ObjectDataSource. Тем не менее, это будет добавлен элемент управления ObjectDataSource в `ItemTemplate`, что приведет к ObjectDataSource экземпляром, созданным для каждой строки GridView. Вместо этого разрешите s создайте ObjectDataSource за пределами s GridView полей TemplateField. Завершить редактирование шаблона и перетащите элемент управления ObjectDataSource из панели элементов в конструктор под `ProductsDataSource` ObjectDataSource. Новый ObjectDataSource следует назвать `CategoriesDataSource` и настройте его для использования `CategoriesBLL` класс s `GetCategories` метод.


[![Настройка ObjectDataSource на использование класса CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Рис. 8**: Настройка ObjectDataSource для использования `CategoriesBLL` класс ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image14.png))


[![Получить категории данных с помощью метода GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Рис. 9**: получение данных категории с помощью `GetCategories` метод ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image16.png))


Поскольку данный элемент управления ObjectDataSource используется просто для получения данных, задайте раскрывающихся списках на вкладках UPDATE и DELETE (нет). Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Набор раскрывающиеся списки в UPDATE и DELETE вкладок (нет)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Рис. 10**: задать раскрывающиеся списки в обновление и удаление вкладок (нет) ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image18.png))


После завершения работы мастера, `CategoriesDataSource` s должна выглядеть следующим образом:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

С помощью `CategoriesDataSource` создания и настройки, вернуться к `CategoryName` TemplateField s `ItemTemplate` и смарт-теге DropDownList s, щелкните ссылку Выбор источника данных. В мастере настройки источника данных выберите `CategoriesDataSource` параметр в первом раскрывающемся списке и выбрать `CategoryName` используемый для отображения и `CategoryID` как значение.


[![Привязка к CategoriesDataSource DropDownList](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Рис. 11**: привязка элемента управления DropDownList для `CategoriesDataSource` ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image20.png))


На этом этапе `Categories` DropDownList перечислены все категории, но он не еще автоматически выбирать подходящую категорию для продукта, привязанный к строке GridView. Для этого нам нужно установить `Categories` DropDownList s `SelectedValue` продукт s `CategoryID` значение. Щелкните ссылку Edit DataBindings в смарт-теге DropDownList s и связать `SelectedValue` свойство с `CategoryID` поле данных, как показано на рис. 12.


![Привязать значение CategoryID продукта s к свойству SelectedValue s DropDownList](batch-updating-vb/_static/image12.gif)

**Рис. 12**: привязка продукт s `CategoryID` значение для элемента управления DropDownList s `SelectedValue` свойство


Один последний остается проблема: Если t продукта `CategoryID` значения указаны, то инструкция привязки данных на `SelectedValue` приведет к возникновению исключения. Это обусловлено DropDownList содержит только элементы для категорий и не предлагает вариант для этих продуктов, имеющих `NULL` базы данных значение `CategoryID`. Чтобы исправить это, установите DropDownList s `AppendDataBoundItems` свойства `True` и добавьте новый элемент в элемент управления DropDownList, пропуск `Value` свойство из декларативного синтаксиса. То есть, убедитесь, что `Categories` декларативный синтаксис серверного элемента управления DropDownList s выглядит следующим образом:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Обратите внимание как `<asp:ListItem Value="">` --выберите один--имеет его `Value` атрибут явно задать пустую строку. Вернуться к [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) учебника Зачем нужна это еще один элемент DropDownList для обработки более глубокое обсуждение `NULL` случай и почему назначение `Value` Важно свойству пустую строку.

> [!NOTE]
> Существует проблема потенциальной производительности и масштабируемости здесь, о котором стоит упомянуть. Так как каждая строка содержит элемента управления DropDownList, использующий `CategoriesDataSource` в качестве источника данных, `CategoriesBLL` класс s `GetCategories` будет вызван метод *n* посетите раз на каждой странице, где *n* число строки GridView. Эти *n* вызовы `GetCategories` привести *n* запросы к базе данных. Это влияние на базе удалось уменьшилось путем кэширования возвращенного категорий в кэше для каждого запроса или через уровень кэширования, используя кэширование зависимостей или истечения срока действия очень короткое время под управлением SQL. Дополнительные сведения о каждом запросе параметр, кэширования см. в разделе [ `HttpContext.Items` Store кэша на запрос](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Шаг 4: Завершение работы интерфейс правки

Мы ve внесены несколько изменений шаблонов s GridView не приостанавливая выполнение просмотрите ход работы. Отвлекитесь и просмотрите ход работы через браузер. Как показано на рис. 13, каждая строка подготавливается с помощью его `ItemTemplate`, который содержит ячейку s, интерфейс редактирования.


[![Каждая строка GridView представляет редактируемый](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Рис. 13**: каждая строка GridView представляет редактируемый ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image22.png))


Существует несколько незначительных проблем форматирования, которые мы следует обратить на этом этапе. Во-первых, обратите внимание, что `UnitPrice` значение содержит четыре десятичных запятых. Чтобы устранить эту проблему, нужно вернуть `UnitPrice` TemplateField s `ItemTemplate` и, в текстовое поле s смарт-теге, щелкните ссылку Edit DataBindings. Затем укажите, `Text` свойство форматируются в виде числа.


![Форматировать как число свойства Text](batch-updating-vb/_static/image14.gif)

**Рис. 14**: формат `Text` свойство как число


Во-вторых, позволяющие s center флажок в `Discontinued` столбца (а не его по левому краю). Щелкните смарт-теге GridView s Правка столбцов и выберите `Discontinued` TemplateField из списка полей в левом нижнем углу. Детализировать `ItemStyle` и задайте `HorizontalAlign` свойство центр, как показано на рис. 15.


![Center неподдерживаемые флажок](batch-updating-vb/_static/image15.gif)

**Рис. 15**: Center `Discontinued` флажок


Затем добавьте элемент управления ValidationSummary на страницу и задать его `ShowMessageBox` свойства `True` и его `ShowSummary` свойства `False`. Кроме того, добавить кнопку веб-элементов управления, при щелчке, обновит s изменения, внесенные пользователем. В частности, добавьте две кнопки веб-элементы управления, одного над элементом управления GridView и под ней задание оба элемента управления `Text` свойства для обновления продуктов.

С момента GridView s интерфейс редактирования определен в его полей TemplateField `ItemTemplate` s, `EditItemTemplate` s избыточным и может быть удален.

После внесения указанных выше упоминалось форматирование, добавление элемента управления Button и удаление ненужный `EditItemTemplate` s, ваши страницы s должен выглядеть следующим образом:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

На рисунке 16 показано на этой странице, при просмотре через браузер, после добавления кнопки веб-элементов управления и изменений форматирования.


[![Теперь страница включает в себя две кнопки обновления продуктов](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Рис. 16**: теперь включает два обновления продуктов кнопок страниц ([Просмотр полноразмерного изображения](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Шаг 5: Обновление продуктов

При посещении этой страницы они делает их изменения и затем выберите один из двух кнопок продукты с обновлением. На этом этапе нам нужно каким-либо образом сохранить введенные пользователем значения для каждой строки в `ProductsDataTable` экземпляра и затем передать его в метод BLL, который затем будет передавать, `ProductsDataTable` экземпляра DAL s `UpdateWithTransaction` метод. `UpdateWithTransaction` Метод, который мы создали в [предыдущем учебном курсе](wrapping-database-modifications-within-a-transaction-vb.md), гарантирует, что пакет изменений будет обновляться в виде атомарной операции.

Создайте метод с именем `BatchUpdate` в `BatchUpdate.aspx.vb` и добавьте следующий код:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Этот метод начинается с получения всех продуктов в `ProductsDataTable` через вызов BLL s `GetProducts` метод. Затем выполняется перечисление `ProductGrid` GridView s [ `Rows` коллекции](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). `Rows` Коллекция содержит [ `GridViewRow` экземпляр](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) для каждой строки, отображаемый в GridView. Так как мы продемонстрируем максимум десять строк на одной странице, GridView s `Rows` коллекции будет иметь не более десяти элементов.

Для каждой строки `ProductID` извлечен из `DataKeys` коллекции и соответствующий `ProductsRow` выбирается из `ProductsDataTable`. Программным способом указываются четыре TemplateField ввода элемента управления и их значения присвоены `ProductsRow` s свойств экземпляра. После каждого GridView значений строк используется для обновления `ProductsDataTable`, он передаваемых в BLL s `UpdateWithTransaction` метод, который, как мы видели в предыдущем учебном курсе просто вызывает DAL s `UpdateWithTransaction` метод.

Алгоритм обновления пакетной службы, используемый в этом учебнике обновляет каждую строку в `ProductsDataTable` , соответствует строке в GridView, независимо от того, менялся ли сведения о продукте s. Хотя такие blind обновляет t упорядочивая обычно проблемы с производительностью, они может привести к излишним записей при изменении во время аудита в таблицу базы данных. Вернитесь в [выполнение пакетных обновлений](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) учебнике мы изучили пакета обновление интерфейса с помощью элемента управления DataList и добавить код, который обновит только те записи, которые действительно были изменены пользователем. Вы можете использовать методы из [выполнение пакетных обновлений](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) обновление кода в этом руководстве, при необходимости.

> [!NOTE]
> При привязке источника данных к GridView через его смарт-тег, Visual Studio автоматически назначает данных источника s первичного ключа значения GridView s `DataKeyNames` свойство. Если не удалось привязать ObjectDataSource к GridView через смарт-тега GridView s, как описано на шаге 1, то необходимо вручную задать GridView s `DataKeyNames` значение ProductID, чтобы получить доступ к `ProductID` значение для каждой строки через `DataKeys` коллекции.


Код, используемый в `BatchUpdate` аналогично тому, который использовался в BLL s `UpdateProduct` методы, основным же различием является, в `UpdateProduct` методы только один `ProductRow` извлекается экземпляр из архитектуры. Код, который назначает свойства `ProductRow` является одинаковым в разных `UpdateProducts` методы и код в `For Each` цикл `BatchUpdate`, так как общий шаблон.

Для работы с этим учебником необходимо иметь `BatchUpdate` метод вызывается при нажатии одной из кнопок продукты с обновлением. Создание обработчиков событий для `Click` события этих двух элементов управления и добавьте следующий код в обработчиках событий:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Сначала выполняется вызов для `BatchUpdate`. Далее, [ `ClientScript` свойство](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) используется для добавления JavaScript, который будет отображать окно messagebox, который считывает продукты были обновлены.

Внимательно протестировать этот код. Посетите `BatchUpdate.aspx` через браузер, изменить число строк и нажмите одну из кнопок продукты с обновлением. Предположим, что нет ошибок проверки входных данных, вы должны увидеть окно messagebox, которая считывает продукты были обновлены. Чтобы проверить атомарность обновления, рассмотрите возможность добавления случайный `CHECK` ограничения, например, которая запрещает режим `UnitPrice` значения 1234,56. А затем из `BatchUpdate.aspx`, изменить несколько записей, не забывая задать один из продуктов s `UnitPrice` в запрещенное значение (1234,56). Это должно привести к ошибке, щелкнув продукты с обновлением с другими изменениями во время операции отката к исходным значениям.

## <a name="an-alternativebatchupdatemethod"></a>Альтернативы`BatchUpdate`метод

`BatchUpdate` Метод, мы просто проверить извлекает *все* продуктов из BLL s `GetProducts` метод, а затем обновляет только те записи, которые отображаются в GridView. Этот подход является идеальным, если GridView не использовать разбиение по страницам, но если это так, может существовать несколько сотен, тысяч или десятков тысяч продуктов, но только десять строк в GridView. В этом случае получение всех продуктов из базы данных только для изменения 10 из них является идеальным.

Для этих типов ситуаций, рассмотрите возможность использования следующих `BatchUpdateAlternate` метод вместо:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` начинается, создав новую пустую `ProductsDataTable` с именем `products`. Он, а затем шаги-GridView s `Rows` коллекции и для каждой строки получает сведения конкретного продукта, с помощью FTPS BLL `GetProductByProductID(productID)` метод. Полученный `ProductsRow` экземпляр имеет его свойства, обновленные в так же, как `BatchUpdate`, но после обновления строки, он импортируется в `products` `ProductsDataTable` через DataTable s [ `ImportRow(DataRow)` метод](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

После `For Each` завершения цикла `products` содержит один `ProductsRow` экземпляра для каждой строки GridView. Поскольку каждое из `ProductsRow` были добавлены экземпляры `products` (а не обновление), если мы просто передать его в `UpdateWithTransaction` метод `ProductsTableAdatper` попытается вставить каждой записи в базе данных. Вместо этого нам нужно указать, что каждая из этих строк был изменен (не добавлен).

Это можно сделать, добавив новый метод BLL с именем `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, показано ниже, наборы `RowState` каждого из `ProductsRow` экземпляров в `ProductsDataTable` для `Modified` , а затем передает `ProductsDataTable` в DAL s `UpdateWithTransaction` метод.


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Сводка

GridView предоставляет возможности редактирования встроенные строки, но не предоставляет поддержку для создания полностью доступно для редактирования интерфейсов. Как мы видели в этом руководстве, такие интерфейсы возможны, но требует некоторой работы. Для создания элемента управления GridView, где каждая строка редактируемой, нам нужно преобразовать поля s GridView в поля TemplateField и редактирования интерфейс определяется в `ItemTemplate` s. Кроме того, обновить все - тип кнопки веб-элементов управления необходимо добавить на страницу, отдельно от GridView. Эти кнопки `Click` обработчики событий нужно перечислить GridView s `Rows` сбора, хранения изменений в `ProductsDataTable`и передача обновленную информацию в соответствующий метод BLL.

В следующем учебном курсе будет показано, как создать интерфейс для удаления пакета. В частности, каждая строка GridView будет установить флажок для включения, а вместо-тип кнопки, мы должны удалить выделенные строки кнопок.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Терезой Мерфи и Дэвид Suru, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](wrapping-database-modifications-within-a-transaction-vb.md)
> [Вперед](batch-deleting-vb.md)
