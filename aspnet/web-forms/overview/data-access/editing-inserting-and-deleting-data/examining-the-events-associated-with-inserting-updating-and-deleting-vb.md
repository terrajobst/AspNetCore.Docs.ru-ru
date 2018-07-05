---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
title: Обзор событий, связанных со вставкой, обновлением и удалением (VB) | Документация Майкрософт
author: rick-anderson
description: В этом учебном курсе мы рассмотрим использование событий, возникающих до, во время и после вставки обновления или удаления веб-элемента управления данными ASP.NET. КО...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: c9bd10a7-eff8-4d8c-bec9-963c2aef2d6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 483ff296bb6fcda14c224c085fc87209bb548700
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387914"
---
<a name="examining-the-events-associated-with-inserting-updating-and-deleting-vb"></a>Обзор событий, связанных со вставкой, обновлением и удалением (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_VB.exe) или [скачать PDF](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/datatutorial17vb1.pdf)

> В этом учебном курсе мы рассмотрим использование событий, возникающих до, во время и после вставки обновления или удаления веб-элемента управления данными ASP.NET. Также будет показано, как настройка интерфейса правки для обновления только подмножества полей продукта.


## <a name="introduction"></a>Вступление

При использовании встроенных Вставка, редактирование или удаление функций элементов управления GridView, DetailsView и FormView, разнообразные действия происходят при упорядочении пользователь не завершит процесс добавления новой записи или удаления существующей записи. Как уже говорилось в [предыдущем учебном курсе](an-overview-of-inserting-updating-and-deleting-data-vb.md), при изменении строки в GridView, "Изменить" заменяется на кнопки "обновления" и "Отмена" и преобразование полей BoundField в текстовые поля. Когда конечный пользователь обновляет данные и последнее обновление, при обратной передаче выполняются следующие действия:

1. GridView заполняет его ObjectDataSource `UpdateParameters` с уникальными идентифицирующими полями измененной записи (с помощью `DataKeyNames` свойства) и значения, введенные пользователем
2. GridView вызывает его ObjectDataSource `Update()` метод, который в свою очередь вызывает соответствующий метод из базового объекта (`ProductsDAL.UpdateProduct`, в нашем предыдущем учебном курсе)
3. Базовые данные, в которых теперь отражены внесенные изменения, повторно привязываются к GridView

Во время этой последовательности операций, ряд событий, благодаря чему мы можем создавать обработчики событий для добавления специальной логики там, где требуется. Например, до шага 1, GridView `RowUpdating` вызывает событие. Мы на этом этапе можно отменить запрос на обновление, если имеется ошибка проверки. Когда `Update()` вызывается метод, ObjectDataSource `Updating` события, предоставляя возможность добавить или настроить значения любого из `UpdateParameters`. После основного элемента управления ObjectDataSource методу объекта, порождается событие ObjectDataSource `Updated` события. Обработчик событий для `Updated` может проверить сведения об операции обновления, например, сколько строк затронуто и ли возникло исключение. Наконец, после шага 2, GridView `RowUpdated` вызывает событие; обработчик этого события может рассмотреть дополнительную информацию о так же, операция обновления выполняется.

Рис. 1 изображена эта последовательность событий и действия, при обновлении элемента управления GridView. Шаблон события на рис.1 не является уникальным для обновления с помощью GridView. Вставка, обновление или удаление данных из GridView, DetailsView и FormView запускает ту же последовательность событий предварительного и последующего уровней для веб-управления данными и элемент управления ObjectDataSource.


[![Серию до и после события запускаются при обновлении данных в элементе управления GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image1.png)

**Рис. 1**: серия из до и после события Fire при обновлении данных в элементе управления GridView ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image3.png))


В этом учебном курсе мы рассмотрим применение этих событй для расширения встроенных Вставка, обновление и удаление возможностей ASP.NET данных веб-элементов управления. Также будет показано, как настройка интерфейса правки для обновления только подмножества полей продукта.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Шаг 1: Обновление продукта`ProductName`и`UnitPrice`поля

В интерфейсы правки из предыдущего учебника *все* полей продукта, которые не были доступны только для чтения полями, доступными. Если требовалось удалить поле из GridView - сказать `QuantityPerUnit` — при обновлении данных веб-управления данными не устанавливал бы значение ObjectDataSource `QuantityPerUnit` `UpdateParameters` значение. Элемент управления ObjectDataSource передавал бы значение `Nothing` в `UpdateProduct` метод слоя бизнес-логики (BLL), который изменит записи измененной базы данных `QuantityPerUnit` столбец `NULL` значение. Аналогичным образом, если требуемое поле, такие как `ProductName`, удаляется из интерфейса правки, операция обновления заканчивается аварийно с "*столбце «ProductName» запрещены значения NULL*" исключение. Причина такого поведения было, поскольку элемент управления ObjectDataSource был настроен для вызова `ProductsBLL` класса `UpdateProduct` метод, который требуется входной параметр для каждого из полей продукта. Таким образом, ObjectDataSource `UpdateParameters` коллекция содержала значение параметра для каждого метода входных параметров.

Если требуется предоставить веб-элемента управления, который позволяет пользователю обновлять только подмножество полей данных, то необходимо либо программно задать отсутствующие значения `UpdateParameters` значения в коллекции `Updating` обработчика событий или создать и вызвать метод BLL, ожидает только подмножество полей. Рассмотрим такой подход адекватен.

В частности, давайте создадим страницу, отображающую только `ProductName` и `UnitPrice` полей в изменяемого элемента управления GridView. Интерфейс правки элемента GridView допускает только пользователю изменить два отображаемых поля, `ProductName` и `UnitPrice`. Поскольку этот интерфейс правки предоставляет только подмножество полей продукта, необходимо либо создание нового ObjectDataSource, использующий существующего слоя BLL `UpdateProduct` метод и содержит недостающие значения полей продукта установить в обработчике его `Updating` событий обработчик, или мы должны создать новый метод BLL, который ожидает только подмножества полей, определенных в GridView. В этом учебнике давайте использовать последний вариант и создадим перегрузку метода `UpdateProduct` метод, который принимает только три входных параметра: `productName`, `unitPrice`, и `productID`:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample1.vb)]

Исходное, такие как `UpdateProduct` метод, данная перегрузка начинает с проверки, если в базе данных с указанным — это продукт `ProductID`. Если нет, он возвращает `False`, означает, что сбой запроса на обновление информации о продукте. В противном случае он обновляет существующую запись продукта `ProductName` и `UnitPrice` поля соответствующим образом и обновление фиксируется посредством вызова TableAdpater `Update()` метод, передавая `ProductsRow` экземпляра.

Сделав это добавление к нашей `ProductsBLL` класс, мы готовы создавать упрощенный интерфейс GridView. Откройте `DataModificationEvents.aspx` в `EditInsertDelete` папку и добавьте элемент управления GridView на страницу. Создайте новый ObjectDataSource и настроить его для использования `ProductsBLL` класса с его `Select()` перегрузке `GetProducts` и его `Update()` перегрузке `UpdateProduct` перегрузку, которая принимает только `productName`, `unitPrice`, и `productID` входных параметров. Рис. 2 показан мастер создания источника данных, при сопоставлении ObjectDataSource `Update()` метод `ProductsBLL` для нового класса `UpdateProduct` перегрузки метода.


[![Сопоставьте метод Update() класса ObjectDataSource новой перегрузке UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image4.png)

**Рис. 2**: сопоставление ObjectDataSource `Update()` метод создать `UpdateProduct` перегрузки ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image6.png))


Так как в нашем примере исходно требуется только возможность править данные, но не вставлять или удалять записи, Отвлекитесь и явно указывать, что ObjectDataSource `Insert()` и `Delete()` методы не следует сопоставлять никаким `ProductsBLL` методы класса, перейдя на вкладках INSERT и DELETE и выбрав (нет) из раскрывающегося списка.


[![Выберите значение (None) из раскрывающегося списка для вставки и удаления вкладок](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image7.png)

**Рис. 3**: выберите значение (None) из раскрывающегося списка для вставки и удаления вкладок ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image9.png))


После завершения работы этого мастера установите флажок Enable Editing смарт-теге элемента GridView.

После выполнения мастера создания источника данных и привязки, к GridView Visual Studio создан декларативный синтаксис для обоих элементов управления. Перейдите к представлению источника для проверки декларативная разметка ObjectDataSource, как показано ниже:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample2.aspx)]

Поскольку нет сопоставлений для ObjectDataSource `Insert()` и `Delete()` методов, существуют не `InsertParameters` или `DeleteParameters` разделы. Кроме того, с момента `Update()` относящимся `UpdateProduct` перегрузку метода, которая принимает только три входных параметра, `UpdateParameters` присутствуют только три экземпляра `Parameter` экземпляров.

Обратите внимание, что ObjectDataSource `OldValuesParameterFormatString` свойству `original_{0}`. Это свойство задается автоматически в Visual Studio при использовании мастера настройки источника данных. Тем не менее поскольку наши методы слоя BLL не рассчитывают на получение исходного `ProductID` значение для передачи в, полностью удалить это назначение свойства из декларативного синтаксиса ObjectDataSource.

> [!NOTE]
> Если вы просто очистите `OldValuesParameterFormatString` значение свойства из окна свойств в режиме конструктора, свойство по-прежнему будет существовать в декларативном синтаксисе, но устанавливается в пустую строку. Либо удалите свойство полностью из декларативного синтаксиса или из окна свойств установите значение по умолчанию, `{0}`.


Хотя элемент управления ObjectDataSource имеет только `UpdateParameters` для имени продукта, цену и идентификатор, Visual Studio добавлены BoundField или CheckBoxField в GridView для каждого из полей продукта.


[![GridView содержит BoundField или CheckBoxField для каждого из полей продукта](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image10.png)

**Рис. 4**: GridView содержит BoundField или CheckBoxField для каждого из полей продукта ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image12.png))


Когда конечный пользователь изменяет продукт и нажимает кнопку ее кнопкой "Обновить", GridView перечисляет те поля, которые не были доступны только для чтения. Затем он устанавливает значение соответствующего параметра в ObjectDataSource `UpdateParameters` коллекции на введенное пользователем значение. Если не создается соответствующий параметр, GridView добавляет ее в коллекцию. Таким образом, если наши GridView содержит поля BoundFields и CheckBoxFields для всех полей продукта, элемент управления ObjectDataSource вызовет перегрузку `UpdateProduct` , принимающую все эти параметры, несмотря на то, ObjectDataSource декларативная разметка указывает только три входных параметра (см. рис. 5). Аналогичным образом, если имеется определенное сочетание не только для чтения продукта поля в GridView, которое не соответствует входным параметрам для `UpdateProduct` перегрузки, будет вызвано исключение при попытке обновить.


[![GridView будет добавить параметры к коллекции UpdateParameters элемента управления ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image13.png)

**Рис. 5**: The GridView будет добавление параметров ObjectDataSource `UpdateParameters` коллекции ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image15.png))


Чтобы убедиться, что элемент управления ObjectDataSource вызывает `UpdateProduct` , принимающую только продукта имя, цену и идентификатор, нам нужно ограничить GridView полями для только что `ProductName` и `UnitPrice`. Это можно сделать, удалив другие поля BoundFields и CheckBoxFields, путем, установкой `ReadOnly` свойства `True`, или с помощью некоторого сочетания этих двух. В этом руководстве Давайте просто удалим все поля GridView, за исключением `ProductName` и `UnitPrice` полей BoundField, после чего декларативная разметка GridView будет выглядеть так:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample3.aspx)]

Несмотря на то что `UpdateProduct` рассчитывает на три входных параметра, у нас есть только два поля BoundFields в нашей GridView. Это обусловлено `productID` входной параметр имеет значение первичного ключа и передается посредством значения `DataKeyNames` свойство для редактируемой строки.

Наши GridView, вместе с `UpdateProduct` перегрузки, пользователь может изменить только имя и цену продукта, не теряя никакие другие поля продукта.


[![Интерфейс дает возможность изменять только имя и цену продукта](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image16.png)

**Рис. 6**: предоставляет интерфейс, редактирования только имя и цену продукта ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image18.png))


> [!NOTE]
> Как уже обсуждалось в предыдущем учебном курсе, очень важно, чтобы GridView состояние представления s быть включены (поведение по умолчанию). Если задать GridView s `EnableViewState` свойства `false`, возникает риск случайного удаления или изменения записей параллельно работающими пользователями. См. в разделе [предупреждение: проблемы параллелизма с помощью ASP.NET 2.0 элементов управления GridView/DetailsView/FormView среды, поддерживающих правку и/или удаление и которых состояние просмотра отключено](http://scottonwriting.net/sowblog/posts/10054.aspx) Дополнительные сведения.


## <a name="improving-theunitpriceformatting"></a>Улучшение`UnitPrice`форматирование

Хотя в примере GridView, показанный на рис. 6, работает, `UnitPrice` не отформатировано вообще, что приводит к отображению цены, который не имеет валют, символов и с четырьмя десятичными позициями. Чтобы применить неизменяемые строки форматирования денежных единиц, просто установите `UnitPrice` BoundField `DataFormatString` свойства `{0:c}` и его `HtmlEncode` свойства `False`.


[![Настройте соответствующим образом DataFormatString и HtmlEncode свойства UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image19.png)

**Рис. 7**: задать `UnitPrice` `DataFormatString` и `HtmlEncode` свойства соответствующим образом ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image21.png))


Благодаря этому изменению неизменяемые строк, цена форматируется как денежное значение; Тем не менее, редактируемой строки, по-прежнему отображается значение без символа денежной единицы и с четырьмя десятичными позициями.


[![Нередактируемые строки являются теперь отформатировано как значения валюты](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image22.png)

**Рис. 8**: нередактируемые Rows, теперь отформатировано как значения валюты ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image24.png))


Команды форматирования, указанные в `DataFormatString` свойства могут применяться к интерфейсу редактирования свойства `ApplyFormatInEditMode` свойства `True` (по умолчанию используется `False`). Отвлекитесь и присвойте этому свойству значение `True`.


[![Значение свойства applyformatineditmode поля UnitPrice типа BoundField значение True](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image25.png)

**Рис. 9**: задать `UnitPrice` BoundField `ApplyFormatInEditMode` свойства `True` ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image27.png))


Благодаря этому изменению значение `UnitPrice` отображаются в измененной строке также форматируется как денежная единица.


[![Значение поля UnitPrice редактирования строки — теперь отформатировано как денежная единица](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image28.png)

**Рис. 10**: изменить строки `UnitPrice` значение — теперь отформатировано как денежная единица ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image30.png))


Тем не менее, обновлении продукта с символом денежной единицы в текстовом поле, например $19,00 вызывает `FormatException`. Если GridView пытается присвоить предоставленные пользователем значения ObjectDataSource `UpdateParameters` коллекции, его не удается преобразовать `UnitPrice` строку «$19,00» в `Decimal` требует параметр (см. рис. 11). Чтобы исправить это можно создать обработчик событий для элемента GridView `RowUpdating` событий, который будет анализировать предоставленное пользователем `UnitPrice` как валюта `Decimal`.

GridView `RowUpdating` событий принимает в качестве второго параметра объект типа [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), который включает `NewValues` в качестве одного из его свойств предоставленные пользователем значения можно назначенные ObjectDataSource `UpdateParameters` коллекции. Мы существующее значение `UnitPrice` значение в `NewValues` синтаксический анализ коллекции с десятичным значением в формате валюты в следующих строках кода в `RowUpdating` обработчик событий:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample4.vb)]

Если пользователь предоставил `UnitPrice` значение (например, «$19,00»), это значение переопределяется десятичным значением, вычисленные поиском решения [Decimal.Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), рассматривающим значение как денежная единица. Это правильно интерпретировать знак десятичного разделителя в случае символов валют, запятых, десятичных запятых и т. д. и использует [перечисление NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) в [System.Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) пространства имен.

Рис. 11 представлена как проблема, из-за символов валют в предоставленной пользователем `UnitPrice`, а также как GridView `RowUpdating` обработчик событий, которые могут использоваться правильно анализа таких входных данных.


[![Значение поля UnitPrice редактирования строки — теперь отформатировано как денежная единица](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image31.png)

**Рис. 11**: изменить строки `UnitPrice` значение — теперь отформатировано как денежная единица ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image33.png))


## <a name="step-2-prohibitingnull-unitprices"></a>Шаг 2: Запрет`NULL UnitPrices`

Хотя база данных настроена так, чтобы разрешить `NULL` значения в `Products` таблицы `UnitPrice` столбца, мы может потребоваться заблокировать пользователи, посещающие страницу вместо `NULL` `UnitPrice` значение. То есть если пользователь не ввел `UnitPrice` значение при правке строки продукта вместо сохранения результатов в базу данных, необходимо отобразить сообщение о том, что на данной странице всех изменяемых продуктов необходимо указывать цену.

`GridViewUpdateEventArgs` , Переданное в GridView `RowUpdating` содержит обработчик событий `Cancel` свойство, если значение `True`, прекращает процедуру обновления. Давайте расширим `RowUpdating` обработчик событий, чтобы задать `e.Cancel` для `True` и выводит сообщение, объясняющее причину, если `UnitPrice` значение в `NewValues` коллекции имеет значение `Nothing`.

Начните с добавления элемента управления Label Web на страницу с именем `MustProvideUnitPriceMessage`. Этот элемент управления метка будет отображаться, если пользователь не указал `UnitPrice` значение при обновлении продукта. Задание метки `Text` значение «Необходимо указать цену продукта.» Я также создал новый класс CSS в `Styles.css` с именем `Warning` со следующим определением:


[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample5.css)]

Наконец, задайте метки `CssClass` свойства `Warning`. На этом этапе конструктор должен вывести предупреждающее сообщение в красный, полужирный, курсив, очень крупный размер над элементом управления GridView, как показано на рис. 12.


[![Label добавлен над элементом управления GridView](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image34.png)

**Рис. 12**: метка имеет были добавлены выше GridView ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image36.png))


По умолчанию эта метка должна быть скрыт, поэтому установите его `Visible` свойства `False` в `Page_Load` обработчик событий:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample6.vb)]

Если пользователь пытается обновить продукт без указания `UnitPrice`, нам нужно отменить обновление и отобразить предупреждение. Дополнения к GridView `RowUpdating` обработчик событий следующим образом:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample7.vb)]

Если пользователь пытается сохранить продукт без указания цены, обновление отменяется, и отображается информационное сообщение. Хотя базы данных (и бизнес-логики) позволяет `NULL` `UnitPrice` s, эта конкретная страница ASP.NET — нет.


[![Пользователь нельзя оставлять пустым UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image37.png)

**Рис. 13**: пользователь не может быть передано `UnitPrice` пустым ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image39.png))


Сих пор мы рассмотрели способы использования элемента GridView `RowUpdating` событий для программного изменения значений параметров, присвоенных ObjectDataSource `UpdateParameters` коллекцию как для отмены процедуры обновления полностью. Эти концепции переносятся на элементы управления DetailsView и FormView и применимы для вставки и удаления.

Эти задачи можно также выполнить на уровне ObjectDataSource с помощью обработчиков событий для его `Inserting`, `Updating`, и `Deleting` события. Эти события возникают до вызова соответствующего метода базового объекта и последней возможность изменить коллекцию входных параметров или бесповоротно отменить операцию. Обработчики событий для этих трех событий передают объект типа [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) , имеет два представляющих интерес свойства:

- [Отмена](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), который, если значение `True`, отмена выполняемой операции
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), который представляет коллекцию `InsertParameters`, `UpdateParameters`, или `DeleteParameters`, в зависимости от того, является ли обработчик событий для `Inserting`, `Updating`, или `Deleting` событий

Чтобы продемонстрировать работу со значениями параметров на уровне ObjectDataSource, включим в элементе управления DetailsView на нашей странице, которая позволяет пользователям добавлять новый продукт. Этот DetailsView будет использоваться в качестве интерфейса для быстрого добавления нового продукта к базе данных. При добавлении нового продукта давайте разрешает пользователю вводить значения для только следует единый пользовательский интерфейс `ProductName` и `UnitPrice` поля. По умолчанию те значения, которые не предоставляются в интерфейсу вставки DetailsView будет присвоено `NULL` базы данных значение. Тем не менее, мы используем ObjectDataSource `Inserting` событие, чтобы ввести другие значения по умолчанию, как скоро можно будет увидеть.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Шаг 3: Обеспечение интерфейса для добавления новых продуктов

Перетащите инструментария в конструктор над элементом управления GridView, DetailsView очистите его `Height` и `Width` свойства и привязки его к элементу ObjectDataSource уже присутствует на странице. Это добавит BoundField или CheckBoxField для каждого из полей продукта. Так как мы хотим использовать этот DetailsView для добавления новых продуктов, необходимо установить флажок Разрешить вставку в смарт-теге; Тем не менее, нет такого параметра нет, поскольку ObjectDataSource `Insert()` метод не сопоставляется методу в `ProductsBLL` классов (Напомним, что этого сопоставления мы установили значение (None) при настройке источника данных см. рис. 3).

Чтобы настроить элемент управления ObjectDataSource, щелкните ссылку Настройка источника данных из его смарт-тег, запуск мастера. Первый экран дает возможность изменить базовый объект, к которому привязан элемент управления ObjectDataSource; Оставьте `ProductsBLL`. На следующем экране перечисляются сопоставления методов ObjectDataSource к базовому объекту. Несмотря на то, что мы явно указали, что `Insert()` и `Delete()` методы не следует сопоставлять никаким методам, если перейти на вкладках INSERT и DELETE вы увидите, что сопоставление существует. Это обусловлено `ProductsBLL` `AddProduct` и `DeleteProduct` методы используют `DataObjectMethodAttribute` атрибут, чтобы указать, что они являются методами по умолчанию для `Insert()` и `Delete()`, соответственно. Таким образом мастер ObjectDataSource выбирает эти каждый раз при запуске мастера при отсутствии явно указано другое значение.

Оставьте `Insert()` метод, указывающий на `AddProduct` метод, но снова задать раскрывающемся списке вкладки DELETE значение (None).


[![Задайте стрелку раскрывающегося списка на вкладке INSERT на метод AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image40.png)

**Рис. 14**: значение вкладку Вставка с раскрывающимся списком `AddProduct` метод ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image42.png))


[![Задайте в раскрывающемся списке вкладки DELETE значение (None)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image43.png)

**Рис. 15**: задать удалить вкладку с раскрывающимся списком (нет) ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image45.png))


После внесения этих изменений декларативный синтаксис серверного элемента управления ObjectDataSource будет расширен для включения `InsertParameters` коллекции, как показано ниже:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample8.aspx)]

Повторный запуск мастера вернулось `OldValuesParameterFormatString` свойство. Отвлекитесь и удалите это свойство при установке для него значение по умолчанию (`{0}`) либо совсем удалив его из декларативного синтаксиса.

С помощью ObjectDataSource, предоставляя возможности вставки смарт-теге DetailsView теперь включает флажок Разрешить вставку; Вернитесь в конструктор и установите этот флажок. Затем сократите элемент DetailsView, таким образом, чтобы он имеет только два поля BoundField, кроме - `ProductName` и `UnitPrice` - и CommandField. На этом этапе DetailsView должен выглядеть как:


[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample9.aspx)]

Рис. 16 показана эта страница при просмотре через браузер на этом этапе. Как вы видите, DetailsView указаны имя и цену первого продукта (Chai). Тем не менее, нам нужно, — это интерфейс для вставки, предоставляющий средства для пользователя, чтобы быстро добавить новый продукт в базе данных.


[![DetailsView не в настоящее время отрисовывается в режиме только для чтения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image46.png)

**Рис. 16**: DetailsView является в настоящее время отрисовывается в режиме только для чтения ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image48.png))


Чтобы можно было отобразить в режиме вставки необходимо задать для элемента управления DetailsView `DefaultMode` свойства `Inserting`. Это отрисовывает элемент DetailsView в режиме вставки при первом посещении и остается после вставки новой записи. Как показано на рис. 17, такой DetailsView предоставляет быстрый интерфейс для добавления новой записи.


[![DetailsView предоставляет интерфейс для быстрого добавления нового продукта](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image49.png)

**Рис. 17**: DetailsView предоставляет интерфейс для быстрого добавления нового продукта ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image51.png))


Когда пользователь вводит имя и цену продукта (например «Acme Water» и 1,99, как показано на рис. 17) и нажимает кнопку Insert, обратная передача и начинается поток операций вставки улучшалась новой записи о продукте, добавляемый базы данных. DetailsView сохраняет свой интерфейс для вставки и GridView автоматически повторно привязывается к источнику данных для включения нового продукта, как показано на рис. 18.


![Продукт](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image52.png)

**Рис. 18**: продукта «Acme Water» был добавлен в базу данных


GridView на рис. 18 он показан, полей продукта, отсутствующим в интерфейсе DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`, и так далее, назначаются `NULL` базы данных значения. Это можно увидеть, выполнив следующие действия:

1. Перейдите в проводник по серверам в Visual Studio
2. Развернув `NORTHWND.MDF` узел базы данных
3. Щелкните правой кнопкой мыши `Products` базы данных
4. Выберите Показать таблицу данных

Появится список всех записей в `Products` таблицы. Как показано на рис все столбцы нашего нового продукта не `ProductID`, `ProductName`, и `UnitPrice` имеют `NULL` значения.


[![Поля продукта не представленным в DetailsView, назначенный значения NULL](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image53.png)

**Рис. 19**: продукта поля не представленным в DetailsView назначаются `NULL` значения ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image55.png))


Необходимо предоставить значение по умолчанию, отличное от `NULL` для одного или нескольких из этих значений столбца, либо потому что `NULL` не оптимальным вариантом по умолчанию или потому что сам столбец базы данных не позволяет `NULL` s. Для выполнения этой задачи мы можно программно задать значения параметров DetailsView `InputParameters` коллекции. Это назначение можно сделать либо в случае обработчик для элемента DetailsView `ItemInserting` событий или ObjectDataSource `Inserting` событий. Так как мы уже обсуждали в с помощью событий предварительного и последующего уровней данных Web управлять уровнем, рассмотрим использование событий ObjectDataSource в настоящее время.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Шаг 4: Назначение значений для`CategoryID`и`SupplierID`параметров

В этом руководстве давайте предположим, что для нашего приложения при добавлении нового продукта посредством этого интерфейса необходимо полям `CategoryID` и `SupplierID` значение 1. Как упоминалось ранее, элемент управления ObjectDataSource имеется пара событий предварительного и последующего уровней которые запускаются во время изменения данных. При его `Insert()` вызова метода, элемент управления ObjectDataSource прежде всего создает его `Inserting` , затем вызывает метод, его `Insert()` метод был сопоставлен с и наконец создает `Inserted` событий. `Inserting` Дает нам последнюю возможность настроить входные параметры или полностью отменить операцию.

> [!NOTE]
> В реальном приложении, скорее всего вы захотите разрешить пользователю указать категорию и поставщика или имела возможность выбрать это значение для них на основе определенных критериев или бизнес логики (а не вслепую выбирать идентификатор, равный 1). Тем не менее в примере как программным способом задать значение входного параметра в событии предыдущего уровня элемента управления ObjectDataSource.


Отвлекитесь и создайте обработчик событий для элемента управления ObjectDataSource `Inserting` событий. Обратите внимание на то, что второй входной параметр обработчика события является объектом типа `ObjectDataSourceMethodEventArgs`, имеющего свойство для доступа к коллекции параметров (`InputParameters`) и свойство для отмены операции (`Cancel`).


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample10.vb)]

На этом этапе `InputParameters` свойство содержит ObjectDataSource `InsertParameters` коллекции со значениями, присвоенными из элемента управления DetailsView. Чтобы изменить значение одного из этих параметров, просто используйте: `e.InputParameters("paramName") = value`. Таким образом Чтобы задать `CategoryID` и `SupplierID` для значения 1, настроить `Inserting` обработчик событий, чтобы выглядеть следующим образом:


[!code-vb[Main](examining-the-events-associated-with-inserting-updating-and-deleting-vb/samples/sample11.vb)]

Это время при добавлении нового продукта (например Acme Soda, столбцы), `CategoryID` и `SupplierID` столбцов нового продукта устанавливаются в 1 (см. рис. 20).


[![Теперь у новых продуктов, их CategoryID и SupplierID значения, заданные для 1](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image56.png)

**Рис. 20**: новые продукты теперь имеют их `CategoryID` и `SupplierID` значения присваивается значение 1 ([Просмотр полноразмерного изображения](examining-the-events-associated-with-inserting-updating-and-deleting-vb/_static/image58.png))


## <a name="summary"></a>Сводка

Во время редактирования, вставки и удаления процесса веб-управления данными и ObjectDataSource Пройдите ряд событий предварительного и последующего уровней. В этом учебнике мы рассмотрели события предыдущего уровня и узнали, как использовать их для настройки входных параметров или отмены операции изменения данных полностью, оба из веб-управления и события элемента управления ObjectDataSource. В следующем учебном курсе мы рассмотрим создание и использование обработчиков событий для событий последующего уровня.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. (Jackie goor) и (Liz Shulok), стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](an-overview-of-inserting-updating-and-deleting-data-vb.md)
> [Вперед](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
