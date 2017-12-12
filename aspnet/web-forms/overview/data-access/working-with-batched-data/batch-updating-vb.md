---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: "Пакет обновления (VB) | Документы Microsoft"
author: rick-anderson
description: "Узнайте, как для обновления нескольких записей базы данных в одной операции. В уровень пользовательского интерфейса, мы создаем GridView, где каждая строка представляет для редактирования. В данных..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 02df858a7ad2ccefce4717e9bb7b08fc4c8d6ace
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="batch-updating-vb"></a>Пакет обновления (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) или [скачать PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Узнайте, как для обновления нескольких записей базы данных в одной операции. В уровень пользовательского интерфейса, мы создаем GridView, где каждая строка представляет для редактирования. В слое доступа к данным мы вокруг нескольких операций обновления в пределах транзакции, чтобы гарантировать, что все обновления успешно или будет выполнен откат всех изменений.


## <a name="introduction"></a>Вступление

В [предыдущего учебника](wrapping-database-modifications-within-a-transaction-vb.md) мы узнали, как расширить уровня доступа к данным, чтобы добавить поддержку для транзакций базы данных. Транзакции базы данных гарантирует, что ряд инструкций изменения данных будут считаться одной атомарной операции, который гарантирует, что все изменения не удастся или все успешно завершится. Это низкоуровневые функциях DAL в сторону мы re готовы можем обратиться к созданию пакета интерфейсов изменения данных.

В этом учебнике мы создадим GridView, где каждая строка представляет для редактирования (см. рис. 1). Так как каждая строка подготавливается в его интерфейс редактирования s здесь нет необходимости изменить столбец, обновления и кнопки отмены. Вместо этого существует две кнопки обновления продуктов на странице, при нажатии перечисления строк GridView и обновления базы данных.


[![Каждая строка в GridView — Editable](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Рис. 1**: каждой строки в GridView является Editable ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image2.png))


S позволяют начать работу!

> [!NOTE]
> В [выполнении пакетных обновлений](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) учебника мы создали редактирование пакета интерфейса с помощью элемента управления DataList. Этот учебник отличается от предыдущего тем, что он использует GridView и пакетное обновление выполняется в пределах области транзакции. После завершения этого учебника я рекомендую для возврата к ранее учебника и обновить его для использования функции базы данных связанные с транзакциями добавлено в предыдущем учебнике.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Изучение действий по созданию редактируемые все строки в GridView

Как было сказано в [Обзор Вставка, обновление и удаление данных](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) учебнике GridView предлагает встроенную поддержку редактирования свои данные на основе данных в ряду. На внутреннем уровне GridView отмечает какие строки редактируемой через его [ `EditIndex` свойства](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Как осуществляется привязка GridView к источнику данных, он проверяет каждую строку, чтобы увидеть, если индекс строки равен значению `EditIndex`. В этом случае интерфейсы этой строки s, которые обрабатываются с помощью их редактирования поля. Для стояли, интерфейс редактирования — текстовое поле, `Text` присваивается значение данных поля, заданные BoundField s `DataField` свойство. Для TemplateFields `EditItemTemplate` используется вместо `ItemTemplate`.

Помните, что редактирования рабочий процесс запускается, когда пользователь нажимает кнопку Правка строк. Это вызывает обратную передачу, задает GridView s `EditIndex` свойств индекса выбранной строки s и повторных привязок данных в сетке. При нажатии кнопки "Отмена" s строк при обратной передаче `EditIndex` присвоено значение `-1` перед повторной привязкой данных к сетке. Так как строки GridView s запустить индексацию с нуля, установка `EditIndex` для `-1` приводит к отображению GridView в режиме только для чтения.

`EditIndex` Свойство хорошо подходит для редактирования в ряду, но не предназначены для редактирования пакета. Чтобы сделать доступным для редактирования всего GridView, необходимо иметь визуализации с помощью его интерфейс редактирования строки. Самый простой способ выполнения этой задачи является создание, где реализуется каждого редактируемые поля в соответствии с его интерфейс редактирования TemplateField `ItemTemplate`.

На следующем несколько шагов мы создадим полностью изменяемого элемента управления GridView. На первом шаге мы начнем с создания GridView и его ObjectDataSource и преобразовать его стояли и CheckBoxField в TemplateFields. На шагах 2 и 3 мы перейдем редактирования интерфейсы из TemplateFields `EditItemTemplate` s, чтобы их `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Шаг 1: Отображение сведений о продукте

Прежде чем мы беспокоиться о создании элемента управления GridView строк в которых можно изменять, позволяют s сначала просто Отображение сведений о продукте. Откройте `BatchUpdate.aspx` страницы в `BatchData` папки и перетащите элемент управления GridView с панели элементов в конструктор. Набор GridView s `ID` для `ProductsGrid` и в его смарт-тег, выберите, чтобы привязать его к новой ObjectDataSource с именем `ProductsDataSource`. Настройка ObjectDataSource для извлечения данных из `ProductsBLL` класса s `GetProducts` метод.


[![Настройка ObjectDataSource с помощью класса ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**На рисунке 2**: Настройка ObjectDataSource для использования `ProductsBLL` класса ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image4.png))


[![Получить данные продукта, с помощью метода GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Рис. 3**: получение данных продукта с помощью `GetProducts` метод ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image6.png))


Как GridView возможности изменения s ObjectDataSource предназначены для работы на основе данных в ряду. Чтобы обновить набор записей, нам нужно будет написать большой объем кода в классе ASP.NET страницы s кода, который создает пакеты данных и передает ее МЕТОДА. Таким образом задайте раскрывающихся списков в ObjectDataSource s вкладки UPDATE, INSERT и DELETE (нет). Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Рис. 4**: задать раскрывающихся списков в обновления, вставки и удаления вкладок (нет) ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image8.png))


После завершения работы мастера настройки источника данных декларативная разметка ObjectDataSource s должен выглядеть следующим образом:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Завершение работы мастера настройки источника данных, также вызывает Visual Studio для создания стояли и CheckBoxField для полей данных продукта в GridView. В этом учебнике позволяют s только пользователи могут просматривать и изменять s название продукта, категории, цены и состояния больше не поддерживаются. Удалите все, кроме `ProductName`, `CategoryName`, `UnitPrice`, и `Discontinued` поля и переименования `HeaderText` свойства из первых трех полей с продуктом, категории и цену, соответственно. Наконец установите флажки включить разбиение на страницы и включить сортировку в GridView s смарт-тег.

На этом этапе GridView имеет три стояли (`ProductName`, `CategoryName`, и `UnitPrice`) и CheckBoxField (`Discontinued`). Нам нужно преобразовать в TemplateFields эти четыре поля, а затем переместите интерфейс редактирования из TemplateField s `EditItemTemplate` для его `ItemTemplate`.

> [!NOTE]
> Мы изучена Создание и настройка TemplateFields в [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) учебника. Мы рассмотрим действия преобразования в TemplateFields стояли и CheckBoxField и определение их редактирования интерфейсов в их `ItemTemplate` s, но если возникли затруднения или необходимости обновитель Дон t каких-либо опасений ссылать на этот учебник более ранних.


В смарт-теге s GridView нажмите кнопку Правка столбцов ссылку, чтобы открыть диалоговое окно «поля». Затем выберите каждое из полей и щелкните преобразовать это поле в TemplateField ссылку.


![Преобразовать существующий стояли и CheckBoxField в TemplateFields](batch-updating-vb/_static/image5.gif)

**Рис. 5**: преобразовать существующий стояли и CheckBoxField в TemplateFields


Теперь, когда каждое поле TemplateField, мы готов для перемещения редактирование интерфейса из `EditItemTemplate` s, чтобы `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Шаг 2: Создание`ProductName`,`UnitPrice`, и`Discontinued`редактирования интерфейсов

Создание `ProductName`, `UnitPrice`, и `Discontinued` редактирование интерфейсы являются темы этот шаг и довольно просто, как каждый интерфейс уже определена в TemplateField s `EditItemTemplate`. Создание `CategoryName` интерфейс редактирования немного сложнее, так как нам нужно создать DropDownList применимые категории. Это `CategoryName` интерфейс редактирования разобраться в шаге 3.

Позволить приступить к s `ProductName` TemplateField. Щелкните ссылку Изменить шаблоны смарт-теге s GridView и детализации углублением до `ProductName` TemplateField s `EditItemTemplate`. Выберите текстовое поле, скопируйте его в буфер обмена и вставьте его в `ProductName` TemplateField s `ItemTemplate`. Измените текстовое поле s `ID` свойства `ProductName`.

Добавьте RequiredFieldValidator для `ItemTemplate` чтобы убедиться, что пользователь предоставляет значение для каждого названия продукта s. Задать `ControlToValidate` свойства ProductName, `ErrorMessage` свойство вам необходимо указать название продукта. и `Text` свойства \*. После внесения этих изменений `ItemTemplate`, экран должен выглядеть аналогично рис. 6.


[![Теперь TemplateField ProductName содержит текстовое поле и RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Рис. 6**: `ProductName` TemplateField теперь включает текстовое поле и RequiredFieldValidator ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image10.png))


Для `UnitPrice` интерфейс для редактирования, запустить путем копирования из текстового поля `EditItemTemplate` для `ItemTemplate`. Затем поместите $ перед текстового поля и задайте его `ID` свойства «цена» и его `Columns` значение 8.

Также добавьте CompareValidator для `UnitPrice` s `ItemTemplate` для убедитесь, что значение, введенное пользователем значение допустимым валюты, больше или равно 0,00 долларов. Набор s проверяющий элемент управления `ControlToValidate` свойства UnitPrice, его `ErrorMessage` свойство вам необходимо ввести допустимый денежное значение. Можно опустить все валюты символы., его `Text` свойства \*, ее `Type` свойства `Currency`, ее `Operator` свойства `GreaterThanEqual`и его `ValueToCompare` значение 0.


[![Добавления CompareValidator, чтобы убедиться, ввести цена является неотрицательное значение валюты](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Рис. 7**: добавления CompareValidator, чтобы убедиться, ввести цены — неотрицательное значение валюты ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image12.png))


Для `Discontinued` TemplateField Используйте флажок уже определена в `ItemTemplate`. Просто установите его `ID` для Discontinued и его `Enabled` свойства `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Шаг 3: Создание`CategoryName`интерфейс для редактирования

Интерфейс редактирования в `CategoryName` TemplateField s `EditItemTemplate` содержит текстовое поле, которое отображает значение `CategoryName` поля данных. Нам нужно заменить DropDownList, в которой перечислены возможные категории.

> [!NOTE]
> [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) учебник содержит обсуждение более тщательного и завершения настройки шаблона для включения DropDownList в отличие от текстового поля. При выполнении этих шагов они кратко представлены. Более подробное рассмотрение Создание и Настройка категорий DropDownList обращаться к [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) учебника.


Перетащите из области элементов в DropDownList `CategoryName` TemplateField s `ItemTemplate`, параметр его `ID` для `Categories`. На этом этапе мы бы обычно определение элементами управления DropDownList s источника данных через его смарт-тег, создание новых ObjectDataSource. Тем не менее, это будет добавлен в ObjectDataSource `ItemTemplate`, что приведет к экземпляром ObjectDataSource, созданным для каждой строки в GridView. Вместо этого разрешите создать ObjectDataSource за пределами GridView s TemplateFields s. Завершить правку шаблона и перетащите элемент управления ObjectDataSource из области элементов в конструктор под `ProductsDataSource` ObjectDataSource. Назовите новый элемент управления ObjectDataSource `CategoriesDataSource` и настройте его для использования `CategoriesBLL` класса s `GetCategories` метод.


[![Настройка ObjectDataSource с помощью класса CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Рис. 8**: Настройка ObjectDataSource для использования `CategoriesBLL` класса ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image14.png))


[![Получить данные категории с помощью метода GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Рис. 9**: получение данных категории с помощью `GetCategories` метод ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image16.png))


Поскольку этот элемент управления ObjectDataSource используется для получения данных, задайте раскрывающихся списках на вкладках UPDATE и DELETE (нет). Нажмите кнопку Готово, чтобы завершить работу мастера.


[![Набор раскрывающихся списков в UPDATE и DELETE вкладок (нет)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Рис. 10**: установить раскрывающихся списков в обновление и удаление вкладок (нет) ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image18.png))


После завершения работы мастера, `CategoriesDataSource` s должна выглядеть следующим образом:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

С `CategoriesDataSource` создается и настраивается, вернуться к `CategoryName` TemplateField s `ItemTemplate` и смарт-теге DropDownList s, щелкните ссылку, Выбор источника данных. В мастере настройки источника данных выберите `CategoriesDataSource` в первом раскрывающемся списке, а также выбрать возможность `CategoryName` используется для отображения и `CategoryID` как значение.


[![Привязка к CategoriesDataSource DropDownList](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Рис. 11**: привязка DropDownList для `CategoriesDataSource` ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image20.png))


На этом этапе `Categories` DropDownList перечислены все категории, но он не еще автоматически выберите подходящую категорию для продукта, привязанных к строке GridView. Для этого необходимо установить `Categories` DropDownList s `SelectedValue` продукт s `CategoryID` значение. Щелкните ссылку изменить привязки данных в смарт-теге DropDownList s и связать `SelectedValue` свойство с `CategoryID` поля данных, как показано на рис. 12.


![Привязать значение CategoryID продукта s к свойству SelectedValue s DropDownList](batch-updating-vb/_static/image12.gif)

**Рис. 12**: привязка продукт s `CategoryID` значение DropDownList s `SelectedValue` свойство


Один последний проблема остается: Если t продукта `CategoryID` указано значение затем инструкции привязки данных на `SelectedValue` приведет к возникновению исключения. Это происходит потому DropDownList содержит только элементы для категорий и не предоставляет параметр для этих продуктов, имеющих `NULL` базы данных значение `CategoryID`. Чтобы исправить это, задайте DropDownList s `AppendDataBoundItems` свойства `True` и добавить новый элемент в элемент управления DropDownList пропуск `Value` свойство из декларативного синтаксиса. То есть, убедитесь, что `Categories` DropDownList s декларативный синтаксис выглядит следующим образом:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Обратите внимание как `<asp:ListItem Value="">` --выберите один--имеет его `Value` атрибут явно задать равным пустой строке. Обращаться к [Настройка интерфейса изменения данных](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) учебника более подробное описание на Почему этот дополнительный элемент DropDownList необходима для обработки `NULL` случай и почему назначение `Value` Важно свойства равным пустой строке.

> [!NOTE]
> Существует проблема потенциальной производительности и масштабируемости здесь что стоит отметить. Так как каждая строка содержит DropDownList, который использует `CategoriesDataSource` в качестве источника данных, `CategoriesBLL` класса s `GetCategories` будет вызван метод  *n*  посещать раз на каждой странице, где  *n*  — число строк в GridView. Эти  *n*  вызовы `GetCategories` привести  *n*  запросы к базе данных. Влияние этой конфигурации в базе данных может уменьшилось путем кэширования возвращаемый категорий в кэше для каждого запроса или через уровень кэширования с помощью SQL, кэширование зависимостей или истечения срока действия очень короткое время под управлением. Дополнительные сведения о каждом запросе. параметр, кэширования в разделе [ `HttpContext.Items` хранилища кэша в запросе](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Шаг 4: Завершение работы интерфейс редактирования

Мы хранить внесенные ряд изменений в шаблоны s GridView не приостанавливая просмотрите ход работы. Теперь пора просмотрите ход работы через браузер. Как показано на рис. 13, каждая строка подготавливается к просмотру с помощью его `ItemTemplate`, содержащий ячейки s, интерфейс для редактирования.


[![Каждая строка GridView является Editable](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Рис. 13**: каждая строка GridView является Editable ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image22.png))


Существует несколько незначительные проблемы форматирования, которые мы следует выполнять на данном этапе. Во-первых, обратите внимание, что `UnitPrice` значение содержит четырьмя знаками после запятой. Чтобы устранить эту проблему, нужно вернуть `UnitPrice` TemplateField s `ItemTemplate` и смарт-теге s текстовое поле, щелкните ссылку изменить привязки данных. Затем укажите, `Text` свойство форматируются в виде числа.


![Свойство Text Format как число](batch-updating-vb/_static/image14.gif)

**Рис. 14**: формат `Text` свойство как число


Во-вторых, позволяющие s center флажок в `Discontinued` столбцов (а не его по левому краю). Нажмите кнопку Правка столбцов из GridView s смарт-тег и выберите `Discontinued` TemplateField из списка полей в левом нижнем углу. Детализировать `ItemStyle` и задать `HorizontalAlign` свойства Center, как показано на рис. 15.


![Center неподдерживаемые флажок](batch-updating-vb/_static/image15.gif)

**Рис. 15**: центр `Discontinued` флажок


Затем добавьте на страницу элемент управления ValidationSummary и установить его `ShowMessageBox` свойства `True` и его `ShowSummary` свойства `False`. Кроме того, добавить кнопку веб-элементов управления, при нажатии, обновит s изменения пользователя. В частности, добавьте две кнопки веб-элементов одного над элементом управления GridView и под ней задание оба элемента управления `Text` свойства для обновления продуктов.

С момента GridView s интерфейс редактирования определяется в его TemplateFields `ItemTemplate` s, `EditItemTemplate` s избыточной и может быть удален.

После внесения указанных выше упоминалось изменения форматирования, добавление кнопок панели и удаление ненужных `EditItemTemplate` s, на странице s должен выглядеть следующим образом:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

На рисунке 16 показано на этой странице, при просмотре через браузер, после добавления кнопки веб-элементов управления и изменения форматирования.


[![Страница теперь включает две кнопки обновления продуктов](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**На рисунке 16**: теперь включает два обновления продуктов кнопок страниц ([Просмотр полноразмерное изображение](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Шаг 5: Обновление продуктов

При посещении этой страницы будут вносить свои изменения и затем выберите один из двух кнопок обновления продуктов. На этом этапе нам нужно каким-либо образом сохранение введенных пользователем значений для каждой строки в `ProductsDataTable` экземпляра и передайте его методу уровень бизнес-ЛОГИКИ, затем передать этот `ProductsDataTable` экземпляр DAL s `UpdateWithTransaction` метод. `UpdateWithTransaction` Метод, который мы создали [предыдущего учебника](wrapping-database-modifications-within-a-transaction-vb.md), гарантирует, что пакет изменений будут обновлены в виде атомарной операции.

Создайте метод с именем `BatchUpdate` в `BatchUpdate.aspx.vb` и добавьте следующий код:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Этот метод начинает работу, получение всех продуктов в `ProductsDataTable` через вызов МЕТОДА s `GetProducts` метод. Затем выполняется перечисление `ProductGrid` GridView s [ `Rows` коллекции](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). `Rows` Коллекция содержит [ `GridViewRow` экземпляр](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridviewrow.aspx) для каждой строки, отображаемой в GridView. Поскольку мы продемонстрируем не более десяти строк на страницу, GridView s `Rows` коллекции будет иметь не более 10 элементов.

Для каждой строки `ProductID` извлечен из `DataKeys` коллекции и соответствующего `ProductsRow` выбирается из `ProductsDataTable`. Четыре элемента управления вводом TemplateField программно имеются, а их значения присвоены `ProductsRow` s свойства экземпляра. После каждого GridView значения строк s используется для обновления `ProductsDataTable`, он s передаваемый s уровень бизнес-ЛОГИКИ `UpdateWithTransaction` метод, который, как было показано в предыдущем учебнике просто вызывает DAL s `UpdateWithTransaction` метод.

Алгоритм обновления пакета, используемый в этом учебнике обновляет каждую строку в `ProductsDataTable` , соответствующий строки в GridView, независимо от того, менялся ли сведения о продукте s. Хотя такие blind обновляет упорядочивая t обычно проблемы с производительностью, они могут привести к неиспользуемых записей, если вы повторно аудит изменений в таблице базы данных. В [выполнении пакетных обновлений](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) учебника мы изучена обновление интерфейса с DataList пакета и добавить код, который будет обновлять только те записи, которые действительно были изменены пользователем. Вы можете использовать методы из [выполнении пакетных обновлений](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) обновление кода в этом учебнике, при необходимости.

> [!NOTE]
> При привязке источника данных к GridView, до его смарт-тег, Visual Studio автоматически назначает данных источника s первичного ключа значения GridView s `DataKeyNames` свойство. Если не были привязаны ObjectDataSource к GridView по смарт-тег s GridView, как описано в шаге 1, то необходимо вручную задать GridView s `DataKeyNames` ProductID, для доступа к свойству `ProductID` значение для каждой строки с помощью `DataKeys` коллекции.


Код, используемый в `BatchUpdate` аналогично тому, который использовался в s уровень бизнес-ЛОГИКИ `UpdateProduct` методы, основное отличие, в которой `UpdateProduct` методы только один `ProductRow` экземпляр извлекается из архитектуры. Код, который присваивает свойства `ProductRow` одинаково между `UpdateProducts` методы и код внутри `For Each` цикл в `BatchUpdate`, как общий шаблон.

Для завершения этого учебника, необходимо иметь `BatchUpdate` метод вызывается при щелчке либо из кнопок обновления продуктов. Создание обработчиков событий для `Click` событий для этих двух элементов управления и добавьте следующий код в обработчики событий:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Сначала выполняется вызов для `BatchUpdate`. Далее, [ `ClientScript` свойство](https://msdn.microsoft.com/en-us/library/system.web.ui.page.clientscript(VS.80).aspx) используется для добавления JavaScript, который будет отображать messagebox, который считывает продукты были обновлены.

Внимательно протестировать этот код. Посетите `BatchUpdate.aspx` через браузер, изменить число строк и нажмите одну из кнопок обновления продуктов. Предположим, что отсутствуют ошибки проверки входных данных, вы увидите messagebox, который считывает продукты были обновлены. Чтобы проверить атомарность обновления, рассмотрите возможность добавления случайный `CHECK` ограничения как один, которая запрещает режим `UnitPrice` значения 1234,56. Затем из `BatchUpdate.aspx`, изменить количество записей, что задайте один продукт s `UnitPrice` значение запрещенное значение (1234,56). Это должно приведет к ошибке при щелкнув обновления продуктов с другими изменениями во время этой операции пакетного откат к исходным значениям.

## <a name="an-alternativebatchupdatemethod"></a>Альтернативы`BatchUpdate`метод

`BatchUpdate` Метод, мы просто проверить извлекает *все* продуктов из МЕТОДА s `GetProducts` метода, а затем обновляет только те записи, которые отображаются в GridView. Этот подход идеально, если GridView не используется разбиение на страницы, но если это так, может существовать несколько сотен, тысяч или десятков тысяч продуктов, но только десять строки в GridView. В этом случае получение всех продуктов из базы данных только для изменения 10 из них не является идеальным.

Для этих типов ситуаций, рекомендуется использовать следующие `BatchUpdateAlternate` метод вместо:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate`начинается, создав новый пустой `ProductsDataTable` с именем `products`. Затем шаги по GridView s `Rows` коллекции и для каждой строки возвращает сведения о конкретного продукта, с помощью МЕТОДА s `GetProductByProductID(productID)` метод. Полученный `ProductsRow` экземпляр имеет его свойства, обновленные в так же, как `BatchUpdate`, но после обновления строки, он импортируется в `products` `ProductsDataTable` через DataTable s [ `ImportRow(DataRow)` метод](https://msdn.microsoft.com/en-us/library/system.data.datatable.importrow(VS.80).aspx).

После `For Each` завершения цикла, `products` содержит один `ProductsRow` экземпляра для каждой строки в GridView. Поскольку каждое из `ProductsRow` были добавлены экземпляры `products` (а не обновление), если мы просто передать его в `UpdateWithTransaction` метод `ProductsTableAdatper` попытается вставить каждой записи в базе данных. Вместо этого необходимо указать, что каждая из этих строк был изменен (не добавлен).

Это можно сделать, добавив новый метод с именем МЕТОДА `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, показано ниже, наборы `RowState` каждого из `ProductsRow` экземпляров в `ProductsDataTable` для `Modified` , а затем передает `ProductsDataTable` для DAL s `UpdateWithTransaction` метод.


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Сводка

GridView предоставляет возможности редактирования встроенные строки, но не предоставляет поддержку для создания полностью доступно для редактирования интерфейсов. Как было показано в этом учебнике, такие интерфейсы возможны, но требует немного усилий. Чтобы создать GridView, где каждая строка — для редактирования, нам нужно преобразовать поля s GridView в TemplateFields и редактирования интерфейс определяется в `ItemTemplate` s. Кроме того, обновить все - тип кнопки веб-элементов управления необходимо добавить на страницу отдельно от GridView. Эти кнопки `Click` обработчики событий необходимо перечислить GridView s `Rows` коллекции, сохранить изменения в `ProductsDataTable`и передать обновленные данные в соответствующий метод уровень бизнес-ЛОГИКИ.

В следующем уроке мы рассмотрим, как создать интерфейс для удаления пакета. В частности, необходимо добавить флажок каждой строки в GridView и вместо обновления всех-тип кнопки, у нас будет удалить выделенные строки кнопок.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Мерфи Тереза д и Дэвид Suru. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](wrapping-database-modifications-within-a-transaction-vb.md)
[Вперед](batch-deleting-vb.md)
