---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Сортировка данных в элементе управления DataList или Repeater (C#) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве будет рассмотрен способ включения поддержки в элементах управления DataList и Repeater сортировки, а также как создать в элементе управления DataList или Repeater, данные которого можно...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 85b59040cce266165353fe1627ffd983473bdcb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371643"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Сортировка данных в элементе управления DataList или Repeater (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) или [скачать PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> В этом руководстве будет рассмотрен способ включения поддержки в элементах управления DataList и Repeater сортировки, а также способах создания элементов управления DataList или Repeater, данные которого может быть, разбитых на страницы и сортировки.


## <a name="introduction"></a>Вступление

В [предыдущем учебном курсе](paging-report-data-in-a-datalist-or-repeater-control-cs.md) мы рассмотрели способы добавления поддержки разбиения по страницам элементом управления DataList. Мы создали новый метод в `ProductsBLL` класс (`GetProductsAsPagedDataSource`), возвращается `PagedDataSource` объекта. При привязке элемента управления DataList или Repeater, DataList или Repeater будет отображаться запрошенной страницы данных. Этот метод похож на то, что используется внутри элементов управления GridView, DetailsView и FormView для функционирования разбиение по страницам по умолчанию встроенные.

В дополнение к предлагает техническую поддержку разбиения по страницам, GridView также включает по умолчанию, для обеспечения поддержки сортировки. Ни DataList, ни элемент управления Repeater предоставляет встроенные функциональные возможности сортировки; Тем не менее функциям сортировки могут добавляться с небольшой фрагмент кода. В этом руководстве будет рассмотрен способ включения поддержки в элементах управления DataList и Repeater сортировки, а также способах создания элементов управления DataList или Repeater, данные которого может быть, разбитых на страницы и сортировки.

## <a name="a-review-of-sorting"></a>Обзор сортировки

Как мы видели в [разбиение по страницам и сортировка данных отчета](../paging-and-sorting/paging-and-sorting-report-data-cs.md) руководстве элемент управления GridView предоставляет готовые обеспечения поддержки сортировки. Каждое поле GridView может иметь сопоставленный `SortExpression`, который указывает поле данных, по которому выполняется сортировка данных. При GridView s `AllowSorting` свойству `true`, каждого поля GridView, имеющий `SortExpression` значение свойства имеет его заголовок, отображаемой в качестве LinkButton. Когда пользователь щелкает определенного заголовка s поля GridView, происходит обратная передача, и данные отсортированы в соответствии с выбранной поле s `SortExpression`.

Элемент управления GridView имеет `SortExpression` свойство, которое хранит `SortExpression` поля GridView отсортированы по. Кроме того `SortDirection` свойство указывает, являются ли данные должны быть отсортированы в порядке возрастания или убывания (если пользователь нажимает кнопку, заменяется определенной связи заголовка поле s GridView дважды подряд, порядок сортировки).

Если GridView привязан к элементу управления источника данных, он передает его `SortExpression` и `SortDirection` свойства к данным система управления версиями. Элемент управления источником данных извлекает данные и затем он сортируется в соответствии с предоставленным `SortExpression` и `SortDirection` свойства. После сортировки данных элемента управления источником данных возвращается к GridView.

Выполнить репликацию этой функциональности с помощью элементов управления DataList или Repeater, необходимо выполнить следующее.

- Создать интерфейс сортировки
- Помните, поля данных для сортировки и необходимость отсортировать в возрастающем или убывающем порядке
- Указать элемент управления ObjectDataSource для сортировки данных по полю данных

Это будет рассмотрено эти три задачи в шагах 3 и 4. После этого мы рассмотрим, как включить разбиение по страницам и сортировка поддержки в элементе управления DataList или Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Шаг 2: Отображение продуктов в элементе управления Repeater

Прежде чем думать о реализации любого из функции, связанные с сортировки позволяют s начнем с получения списка продуктов в элементе управления Repeater. Сначала откройте `Sorting.aspx` странице в `PagingSortingDataListRepeater` папку. Добавление элемента управления Repeater на веб-страницу, установка его `ID` свойства `SortableProducts`. В смарт-теге элемента управления Repeater s, создайте новый ObjectDataSource, именуемый `ProductsDataSource` и настройте его для получения данных из `ProductsBLL` класс s `GetProducts()` метод. Выберите параметр из раскрывающегося списка на вкладках INSERT, UPDATE и DELETE (нет).


[![Создание нового ObjectDataSource и настроить его для использования метода GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Рис. 1**: Создание нового ObjectDataSource и настройте его для использования `GetProductsAsPagedDataSource()` метод ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Рис. 2**: установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет) ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


В отличие от с DataList, Visual Studio не создает автоматически `ItemTemplate` для элемента управления Repeater, после ее привязки к источнику данных. Кроме того, необходимо добавить это `ItemTemplate` декларативно, как смарт-тег элемента управления s Repeater отсутствует параметр Правка шаблонов в элементе управления DataList s. S позволяют использовать тот же `ItemTemplate` из предыдущего учебника, отображаемые s название продукта, поставщика и категории.

После добавления `ItemTemplate`, Repeater и ObjectDataSource s должна выглядеть следующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

Рис. 3 показан эту страницу при просмотре через браузер.


[![Отображается каждый продукт s имя поставщика и категории](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Рис. 3**: отображается каждый продукт — имя поставщика и категории ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Шаг 3: Указывая элемент управления ObjectDataSource для сортировки данных

Чтобы отсортировать данные, отображаемые в элементу управления Repeater, нам нужно сообщить ObjectDataSource выражения сортировки, по которому данные должны быть отсортированы. Прежде чем элемент управления ObjectDataSource извлекает свои данные, он инициирует его [ `Selecting` событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), который предоставляет возможность указать выражение сортировки. `Selecting` Обработчику события передаются в объект типа [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), который имеет свойство с именем [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) типа [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). `DataSourceSelectArguments` Класс предназначен для передачи запросов, связанных с данными из объекта-получателя данных к элементу управления источника данных и включает в себя [ `SortExpression` свойство](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Чтобы передать данные сортировки из страницы ASP.NET элемент управления ObjectDataSource, создайте обработчик событий для `Selecting` событие и используйте следующий код:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

*SortExpression* имя поля данных для сортировки данных путем (например, ProductName) должно быть присвоено значение. Нет свойства сортировки, связанных с направлением, поэтому если необходимо отсортировать данные в порядке убывания, добавьте строку DESC для *sortExpression* значение (например, ProductName DESC).

Попробуйте другие значения жестко для *sortExpression* и проверить результаты в браузере. Как показано на рис. 4, при использовании DESC ProductName как *sortExpression*, продукты сортируются по их имени в обратном алфавитном порядке.


[![Продукты сортируются по имени в обратном алфавитном порядке](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Рис. 4**: продукты сортируются по имени в обратном алфавитном порядке ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Шаг 4: Создание интерфейса сортировки и запоминание выражением сортировки и направлением

Включение поддержки в GridView сортировки преобразуется в текст заголовка каждого поля с возможностью сортировки s LinkButton, при нажатии сортирует данные соответствующим образом. Такой интерфейс сортировки имеет смысл для элементов управления GridView, где его аккуратно расположения данных в столбцах. Для элементов управления DataList и Repeater Однако другой интерфейс сортировки потребуется. Общий интерфейс сортировки список данных (в отличие от сетки данных), является список раскрывающегося списка, который содержит поля, по которым данные могут быть отсортированы. Позвольте s реализацией такого интерфейса в этом руководстве.

Добавление элемента управления DropDownList веб-элемент управления выше `SortableProducts` Repeater и задайте его `ID` свойства `SortBy`. В окне свойств нажмите кнопку с многоточием в `Items` свойство на отображение редактора коллекций ListItem. Добавить `ListItem` s, чтобы отсортировать данные по `ProductName`, `CategoryName`, и `SupplierName` поля. Кроме того, добавить `ListItem` для сортировки продуктов по имени в обратном алфавитном порядке.

`ListItem` `Text` Свойства может быть присвоено любое значение (например, имя), но `Value` свойства должно быть присвоено имя поля данных (например, ProductName). Чтобы отсортировать результаты в порядке убывания, добавьте строку DESC на имя поля данных, таких как ProductName DESC.


![Добавление ListItem для каждого из полей данных поддерживает сортировку](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Рис. 5**: добавление `ListItem` для каждого поля данных поддерживает сортировку


Наконец добавьте кнопку веб-элемент управления справа от элемента управления DropDownList. Задайте его `ID` для `RefreshRepeater` и его `Text` свойства для обновления.

После создания `ListItem` s и добавив "Обновить", DropDownList и кнопка s должен выглядеть следующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

С помощью сортировки полный DropDownList, мы теперь нужно обновить элемент управления ObjectDataSource s `Selecting` обработчик событий, что в ней используется выбранный `SortBy``ListItem` s `Value` свойство, в отличие от выражение сортировки жестко.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

На этом этапе при первом просмотре странице продукты будут сначала упорядочены по `ProductName` поля данных, так как он s `SortBy` `ListItem` выбран по умолчанию (см. рис. 6). Выбрать другой параметр, например категория сортировки, а также команда Обновить вызывает обратную передачу и повторно отсортировать данные по имени категории, как показано на рис. 7.


[![Они отсортированы сначала по имени](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Рис. 6**: продуктов, которые изначально сортируются по имени ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![Они теперь отсортированы по категории](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Рис. 7**: продуктов, которые теперь отсортированы по категориям ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Нажав кнопку "Обновить" приводит к быть автоматически переупорядочены так как состояние представления элемента управления Repeater s отключена, приводящий к элементу управления Repeater, выполнять повторную привязку к источнику данных при каждой обратной передаче данных. Если вы ve оставить Repeater s состояние представления включено, изменив порядок сортировки стрелку раскрывающегося списка, выиграл t повлияет на порядок сортировки. Чтобы исправить это, создайте обработчик событий для кнопки Обновить s `Click` событий и повторную привязку элемента управления Repeater к источнику данных (путем вызова элементу управления Repeater s `DataBind()` метод).


## <a name="remembering-the-sort-expression-and-direction"></a>Запоминание выражением сортировки и направлением

При создании сортируемого DataList или Repeater на странице, где не сортировки связанных операций обратной передачи возникает, оно императивного s, что выражением сортировки и направлением запомнить, во время обратной передачи. Например представьте, что мы изменили элементу управления Repeater в этом руководстве, чтобы включить кнопку «Удалить» в каждом из продуктов. При нажатии кнопки Delete мы d выполнять определенный код для удаления выбранного продукта и затем привяжите данные к элементу управления Repeater. Если параметры сортировки не сохраняются между обратную передачу, данные, отображаемые на экране вернется к исходный порядок сортировки.

В этом учебнике DropDownList неявно сохраняет сортировки выражения и направление в состоянии его представления для нас. Если мы использовали другой интерфейс сортировки один со, скажем, элементов управления LinkButton, различные параметры сортировки мы d нужно будет запоминать порядок сортировки во время обратной передачи. Этого можно добиться путем сохранения параметров сортировки в состоянии представления страницы s, путем включения параметра сортировки, в строке запроса или с помощью некоторых метод постоянного хранения состояний.

Будущие примеры в этом руководстве исследуется способ сохранять параметры сортировки в состоянии представления страницы s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Шаг 5: Добавление поддержки сортировки для элементов управления DataList, использующий разбиение по страницам по умолчанию

В [предыдущем учебном курсе](paging-report-data-in-a-datalist-or-repeater-control-cs.md) мы увидели, как можно реализовать разбиение по страницам по умолчанию с элементом управления DataList. Позвольте s расширить этот предыдущего примера, включив возможность сортировать разбитых на страницы данных. Сначала откройте `SortingWithDefaultPaging.aspx` и `Paging.aspx` страниц в `PagingSortingDataListRepeater` папку. Из `Paging.aspx` нажмите кнопку "источник", чтобы просмотреть декларативная разметка страницы s. Копировать выделенный текст (см. рис. 8) и вставьте его в декларативной разметке `SortingWithDefaultPaging.aspx` между `<asp:Content>` теги.


[![Репликация в декларативной разметке &lt;asp: Content&gt; теги из Paging.aspx SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Рис. 8**: репликация в декларативной разметке `<asp:Content>` тегов `Paging.aspx` для `SortingWithDefaultPaging.aspx` ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


После копирования декларативная разметка, скопируйте методы и свойства в `Paging.aspx` странице s вспомогательного класса в класс фонового кода для `SortingWithDefaultPaging.aspx`. Далее, Отвлекитесь и просмотрите `SortingWithDefaultPaging.aspx` страницу в браузере. Он должен продемонстрировать одинаковые функции и внешний вид, как `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Улучшение ProductsBLL, чтобы включить разбиение по страницам и метод сортировки по умолчанию

В предыдущем учебном курсе мы создали `GetProductsAsPagedDataSource(pageIndex, pageSize)` метод в `ProductsBLL` класс, который возвращается `PagedDataSource` объекта. Это `PagedDataSource` заполненный объект *все* продуктов (через BLL s `GetProducts()` метод), но при привязке элемента управления DataList только те записи, соответствующий указанному *pageIndex* и *pageSize* отображались входных параметров.

Ранее в этом учебнике мы добавили поддержку сортировки, указав выражение сортировки из ObjectDataSource s `Selecting` обработчик событий. Это хорошо работает при ObjectDataSource возвращается объект, который можно сортировать, например `ProductsDataTable` возвращаемые `GetProducts()` метод. Тем не менее `PagedDataSource` объект, возвращаемый `GetProductsAsPagedDataSource` метод не поддерживает сортировку его внутренний источник данных. Вместо этого нам нужно отсортировать результаты, возвращаемые `GetProducts()` метод *перед* мы поместили в `PagedDataSource`.

Для этого создайте новый метод в `ProductsBLL` класса `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Чтобы отсортировать `ProductsDataTable` возвращаемые `GetProducts()` метод, укажите `Sort` свойство по умолчанию `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

`GetProductsSortedAsPagedDataSource` Метод лишь незначительно отличается от `GetProductsAsPagedDataSource` метод, созданный в предыдущем учебном курсе. В частности `GetProductsSortedAsPagedDataSource` принимает дополнительный входной параметр `sortExpression` и присваивает это значение, чтобы `Sort` свойство `ProductDataTable` s `DefaultView`. Несколько строк кода в более поздней версии, `PagedDataSource` присваивается объект s DataSource `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Вызов метода GetProductsSortedAsPagedDataSource и указав значение для параметра SortExpression входных данных

С помощью `GetProductsSortedAsPagedDataSource` метод завершения, следующим шагом является для предоставления значения для этого параметра. ObjectDataSource в `SortingWithDefaultPaging.aspx` настроен для вызова `GetProductsAsPagedDataSource` и передает два входных параметра на его двух `QueryStringParameters`, которые указываются таким `SelectParameters` коллекции. Эти два `QueryStringParameters` указывают, что источник `GetProductsAsPagedDataSource` метод s *pageIndex* и *pageSize* параметры берутся из полей строки запроса `pageIndex` и `pageSize`.

Обновить элемент управления ObjectDataSource `SelectMethod` свойство, чтобы он вызывал новый `GetProductsSortedAsPagedDataSource` метод. Затем добавьте новый `QueryStringParameter` таким образом, чтобы *sortExpression* входного параметра осуществляется из поля строки запроса `sortExpression`. Задайте `QueryStringParameter` s `DefaultValue` к ProductName.

После внесения этих изменений декларативная разметка ObjectDataSource s должен выглядеть:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

На этом этапе `SortingWithDefaultPaging.aspx` страницы будут отсортированы результаты в алфавитном порядке по названию продукта (см. рис. 9). Это обусловлено тем, по умолчанию значение ProductName, передается в качестве `GetProductsSortedAsPagedDataSource` метод s *sortExpression* параметра.


[![По умолчанию результаты сортируются по ProductName](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Рис. 9**: по умолчанию, результаты сортируются по `ProductName` ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Если вручную добавить `sortExpression` поля строки запроса, такие как `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` результаты будут отсортированы по заданному `sortExpression`. Тем не менее это `sortExpression` параметр не включается в строке запроса при переходе на другую страницу данных. На самом деле, нажав кнопку на странице Далее или последней кнопки вернетесь нас к `Paging.aspx`! Кроме того здесь s в настоящее время сортировки интерфейс. Единственным способом, пользователь может изменить порядок сортировки разбитых на страницы данных является прямое управление строку запроса.

## <a name="creating-the-sorting-interface"></a>Создание интерфейса сортировки

Необходимо сначала обновить `RedirectUser` метод для отправки пользователю `SortingWithDefaultPaging.aspx` (а не `Paging.aspx`) и включать `sortExpression` значение в строке запроса. Мы также следует добавить только для чтения, страницам с именем `SortExpression` свойство. Это свойство, аналогичную `PageIndex` и `PageSize` свойства, созданные в предыдущем учебном курсе, возвращает значение `sortExpression` поля строки запроса, если он существует, а значение по умолчанию значение (ProductName) в противном случае.

В настоящее время `RedirectUser` метод принимает только один входной параметр индекс отображаемой страницы. Тем не менее могут возникнуть ситуации, когда необходимо перенаправить пользователя на определенную страницу данных, используя выражение сортировки, отличные от какие s, указанный в строке запроса. Сейчас мы создадим интерфейс упорядочения для этой страницы, которая будет содержать несколько кнопку веб-элементов управления для сортировки данных с помощью указанного столбца. При нажатии одной из этих кнопок, необходимо перенаправить пользователя, передавая значение выражения сортировки, соответствующие. Для обеспечения данной функциональности, создайте две версии `RedirectUser` метод. Первый из них должен принимать только индекс страницы для отображения, а второй принимает выражение страницы индекса и сортировки.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

В первом примере в этом руководстве мы создали сортировки интерфейс, с помощью элемента управления DropDownList. В этом примере s позволяют использовать три кнопки веб-элементы управления, расположенный выше элемента управления DataList, один для сортировки с помощью `ProductName`, один для `CategoryName`и один для `SupplierName`. Добавьте три кнопки веб-элементы управления, настройка их `ID` и `Text` свойства соответствующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Создайте `Click` обработчик событий для каждого. Обработчики событий должны вызывать `RedirectUser` метода, возвращает пользователя на первую страницу, с помощью соответствующих сортировки выражения.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

При первом просмотре страницы, данные отсортированы по названию продукта в алфавитном порядке (см. рис. 9). Нажмите кнопку "Далее" для перехода на второй странице данных и нажмите кнопку сортировки, кнопка категории. Это возвращает нас к первой странице данных, отсортированных по имени категории (см. рис. 10). Аналогичным образом щелкнув кнопку поставщика сортировки сортирует данные поставщиком, начиная с первой страницы данных. Выбор сортировки запоминается как страницам данные. Рис. 11 показана страница после сортировки по категориям и затем перейти на тринадцатого страницу данных.


[![Продукты упорядочены по категории](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Рис. 10**: продукты сортируются по категориям ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![Выражение сортировки данные запоминаются при разбиения на страницы через](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Рис. 11**: выражение сортировки — запоминаются при разбиения на страницы через Data ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Шаг 6: Пользовательское разбиение по страницам по записям в элементе управления Repeater

В примере DataList исследуется на шаге 5 страниц по данным с применением технологии разбиения по страницам по умолчанию неэффективным. При работе с большими объемами данных, крайне важно, что можно использовать пользовательское разбиение по страницам. Вернитесь в [эффективного разбиения на страницы через больших объемов данных](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) и [сортировки пользовательских данных, разбитых на страницы](../paging-and-sorting/sorting-custom-paged-data-cs.md) учебники, мы рассмотрели различия по умолчанию и пользовательское разбиение по страницам и созданный методы в BLL для Использование пользовательского разбиения по страницам и сортировка пользовательских разбитых на страницы данных. В частности, в этих двух предыдущих учебных курсах мы добавили следующие три метода `ProductsBLL` класса:

- `GetProductsPaged(startRowIndex, maximumRows)` Возвращает определенное подмножество записей, начиная с *startRowIndex* и не превышает *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` Возвращает определенное подмножество записей, отсортированных по указанным *sortExpression* входного параметра.
- `TotalNumberOfProducts()` Представляет общее количество записей в `Products` таблицы базы данных.

Эти методы можно использовать для эффективного страницы и сортировки данных с помощью элемента управления DataList и Repeater. Чтобы проиллюстрировать это, позвольте s начните с создания элемента управления Repeater с поддержкой пользовательского разбиения по страницам; Затем мы добавим возможности сортировки.

Откройте `SortingWithCustomPaging.aspx` странице в `PagingSortingDataListRepeater` папку и добавьте элемент управления Repeater к странице, установив его `ID` свойства `Products`. В смарт-теге элемента управления Repeater s, создайте новый ObjectDataSource, именуемый `ProductsDataSource`. Настройте его для выбора данных из `ProductsBLL` класс s `GetProductsPaged` метод.


[![Настройте элемент ObjectDataSource для использования метода GetProductsPaged класса ProductsBLL s](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Рис. 12**: Настройка ObjectDataSource для использования `ProductsBLL` класс s `GetProductsPaged` метод ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Установите раскрывающиеся списки в UPDATE, INSERT и удаление вкладок (нет) и нажмите кнопку "Далее". Мастер настройки источника данных, а теперь запрашивает источники `GetProductsPaged` метод s *startRowIndex* и *maximumRows* входных параметров. На самом деле эти параметры игнорируются. Вместо этого *startRowIndex* и *maximumRows* значения будут передаваться в `Arguments` свойства в элемент управления ObjectDataSource s `Selecting` обработчик событий, как и как мы указали *sortExpression* в этой демонстрации первый учебник s. Таким образом оставьте источника параметра раскрывающиеся списки в мастере, установленному None.


[![Оставьте набор источников параметра None](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Рис. 13**: оставьте параметр источников, задайте значение None ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Сделать *не* набора ObjectDataSource s `EnablePaging` свойства `true`. Это приведет к ObjectDataSource автоматически включать свой собственный *startRowIndex* и *maximumRows* параметров `SelectMethod` s существующий список параметров. `EnablePaging` Свойство полезно, когда пользовательские привязки, разбитых на страницы данных к элементу управления GridView, DetailsView и FormView, так как эти элементы управления ожидать определенных элементом управления ObjectDataSource s доступен только при `EnablePaging` свойство `true`. Поскольку мы имеем вручную добавить поддержку разбиения по страницам для элементов управления DataList и Repeater, оставьте это свойство установлено в `false` (по умолчанию), как мы будем позаботимся необходимую функциональность непосредственно в странице ASP.NET.


Наконец, определите Repeater s `ItemTemplate` , чтобы были показаны название продукта s, категорию и поставщика. После внесения этих изменений Repeater и ObjectDataSource s декларативный синтаксис должен выглядеть следующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Отвлекитесь и посетить страницу через обозреватель и обратите внимание на то, что записи не возвращаются. Это обусловлено тем, мы ve еще для указания *startRowIndex* и *maximumRows* значения параметров; таким образом, значение 0 передается в обоих. Чтобы задать эти значения, создайте обработчик событий для ObjectDataSource s `Selecting` событий и задать эти параметры программным образом значения для жестко запрограммированные значения от 0 до 5, соответственно:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

После этого изменения на странице при просмотре в обозревателе отображаются первые пять продуктов.


[![Отображаются первые пять записи](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Рис. 14**: первых пяти записей отображаются ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> Произойти продуктов, перечисленных на рис. 14, должны быть отсортированы по названию продукта, так как `GetProductsPaged` хранимую процедуру, которая выполняет эффективный пользовательского разбиения по страницам запрос упорядочивает результаты по `ProductName`.


Чтобы разрешить пользователям перемещаться по страницам, нам нужно отслеживать индекс начальной строки и максимальное число строк и помнить эти значения во время обратной передачи. В примере разбиение по страницам по умолчанию мы использовали поля строки запроса, чтобы сохранить эти значения; для этой демонстрации позволяют сохранять эти сведения в состоянии представления страницы s s. Создайте следующие два свойства:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Затем обновите код в обработчике событий выбор, заняв `StartRowIndex` и `MaximumRows` свойства вместо жестко запрограммированные значения от 0 до 5:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

На этом этапе на нашей странице по-прежнему отображаются только первые пять записей. Однако с этими свойствами на месте, мы будет готов, чтобы создать интерфейс разбиения по страницам.

## <a name="adding-the-paging-interface"></a>Добавление интерфейса разбиения по страницам

Использование let s, первый же "," Назад "," следующего, последнее разбиение по страницам интерфейс используется в примере разбиение по страницам по умолчанию, включая Label Web, просматривается элемент управления, отображающий какие страницы данных и существует общее количество страниц. Добавьте четыре кнопки веб-элементов управления и метку под элемент управления Repeater.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Создайте `Click` обработчики событий для четырех кнопок. При нажатии одной из этих кнопок, нам нужно обновить `StartRowIndex` и привяжите данные к элементу управления Repeater. Код для кнопок First, Previous и Next довольно проста, но для кнопки последней как определить индекс начальной строки для последней странице данных? Для вычисления этого индекса, а также вы можете определить Включение кнопки следующего и последнего нам необходимо знать, сколько записей в целом разбиваемых по страницам. Мы можете определить, вызвав `ProductsBLL` класс s `TotalNumberOfProducts()` метод. S позволяют создать свойство только для чтения, страницам с именем `TotalRowCount` , возвращающий результаты `TotalNumberOfProducts()` метод:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

С этим свойством теперь мы можем определить последний индекс начальной строки страницы s. В частности, он s целочисленный результат из `TotalRowCount` минус 1, деленное на `MaximumRows`, умноженному на `MaximumRows`. Теперь можно написать `Click` обработчики событий для четырех кнопок интерфейса разбиения на страницы:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Наконец нам нужно отключить кнопки первый» и «назад в интерфейсе разбиения по страницам, при просмотре первой страницы данных и Далее» и «последняя кнопок, при просмотре на последней странице. Для этого добавьте следующий код в элемент управления ObjectDataSource s `Selecting` обработчик событий:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

После добавления этих `Click` обработчики событий и код, чтобы включить или отключить элементы интерфейса разбиения по страницам, на основании текущий индекс начальной строки, проверка страницы в браузере. Как показано на рис. 15, при первом просмотре странице первого и кнопок будут отключены. Щелкните Далее, чтобы отобразить на второй странице данных, при щелчке последнего отображается последняя страница (см. рис. 16 и 17). При просмотре на последней странице данных кнопки "Далее" и "Дата последнего будут отключены.


[![Назад и последней кнопки будут отключены при просмотре первой страницы продуктов](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Рис. 15**: Previous и последней кнопки будут отключены при просмотре первой страницы продукты ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![Вторая страница продуктов, которые вывод](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Рис. 16**: второй странице продукты, вывод ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Щелкнув последний отображает последней странице данных](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Рис. 17**: щелкнув последнего отображает данные из окончательного страницы ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Шаг 7., Включая сортировку поддержки со специальной разбитых на страницы элемент управления Repeater

Теперь, когда используется пользовательское разбиение по страницам, мы будет готов для включения сортировки поддерживают. `ProductsBLL` Класс s `GetProductsPagedAndSorted` метод имеет те же *startRowIndex* и *maximumRows* входных параметров, как `GetProductsPaged`, но позволяет использовать дополнительный  *sortExpression* входного параметра. Чтобы использовать `GetProductsPagedAndSorted` метода из `SortingWithCustomPaging.aspx`, необходимо выполнить следующие действия:

1. Изменить элемент управления ObjectDataSource s `SelectMethod` свойства из `GetProductsPaged` для `GetProductsPagedAndSorted`.
2. Добавить *sortExpression* `Parameter` объекта для ObjectDataSource s `SelectParameters` коллекции.
3. Создайте закрытое, страницам `SortExpression` свойство, которое сохраняет его значение во время обратной передачи через состояние представления страницы s.
4. Обновить элемент управления ObjectDataSource `Selecting` обработчик событий, чтобы назначить ObjectDataSource s *sortExpression* параметр значение страницам `SortExpression` свойство.
5. Создайте интерфейс сортировки.

Начните с обновления ObjectDataSource s `SelectMethod` свойство и добавление *sortExpression* `Parameter`. Убедитесь, что *sortExpression* `Parameter` s `Type` свойству `String`. После завершения этих первых двух задач, декларативная разметка ObjectDataSource s должен выглядеть следующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Теперь нам нужны уровня страницы `SortExpression` свойство, значение которого выполняется сериализация в состояние представления. Если значение выражения не сортировки не задана, используйте ProductName по умолчанию:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Прежде чем элемент управления ObjectDataSource вызывает `GetProductsPagedAndSorted` метод, нам нужно установить *sortExpression* `Parameter` значению `SortExpression` свойство. В `Selecting` обработчик событий, добавьте следующую строку кода:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Остается только реализовать интерфейс сортировки. Как и в последнем примере, позвольте s имеют сортировки интерфейс, реализованный с помощью три кнопки веб-элементы управления разрешает пользователю сортировать результаты по имени продукта, категории или поставщику.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Создание `Click` обработчики событий для эти три элемента управления. В случае сброса обработчик, `StartRowIndex` 0, задайте `SortExpression` соответствующее значение и повторную привязку данных к элементу управления Repeater:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Все, что s — его! Хотя существуют несколько этапов, чтобы получить пользовательское разбиение по страницам и сортировка реализована, действия были очень похоже на те, которые необходимы для разбиения на страницы по умолчанию. Рис. 18 показаны продукты, при просмотре на последней странице данных при сортировке по категориям.


[![Отображаются последние данные страницы, отсортировано по категориям,](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Рис. 18**: последней странице данных, отсортировано по категориям, отображается ([Просмотр полноразмерного изображения](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> В предыдущих примерах, при сортировке по поставщику SupplierName использовался в качестве выражения сортировки. Тем не менее для реализации пользовательского разбиения по страницам, нам нужно использовать CompanyName. Это обусловлено тем, хранимая процедура отвечает за реализацию пользовательского разбиения по страницам `GetProductsPagedAndSorted` передает выражение сортировки в `ROW_NUMBER()` ключевое слово, `ROW_NUMBER()` ключевого слова требуется действительное имя столбца, а не псевдоним. Таким образом, необходимо использовать `CompanyName` (имя столбца в `Suppliers` таблицы) вместо того чтобы псевдоним, используемый в `SELECT` запроса (`SupplierName`) для выражения сортировки.


## <a name="summary"></a>Сводка

Ни элементов управления DataList и Repeater, не предоставляют встроенную поддержку сортировки, но используя фрагмент кода и пользовательский интерфейс сортировки, можно добавить такие функциональные возможности. При реализации сортировки, но не разбивается на страницы, выражение сортировки можно указать с помощью `DataSourceSelectArguments` объект, передаваемый в элемент управления ObjectDataSource s `Select` метод. Это `DataSourceSelectArguments` объект s `SortExpression` свойство может быть присвоено в элемент управления ObjectDataSource s `Selecting` обработчик событий.

Чтобы добавить возможности сортировки в элементе управления DataList или Repeater, который уже содержит поддержки разбиения по страницам, проще всего настроить уровня бизнес-логики, чтобы включить метод, который принимает выражение сортировки. Эти сведения можно затем передать в параметр в элемент управления ObjectDataSource s `SelectParameters`.

Этот учебник завершает изучение разбиение по страницам и сортировка с использованием элементов управления DataList и Repeater. В этом руководстве на последнем рассмотрено Добавление кнопки веб-элементы управления в шаблоны элементов управления DataList и Repeater-s для обеспечения функций некоторые пользовательские, инициированное пользователем на основе каждого элемента.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Основной рецензент этого учебного был Дэвид Suru. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [Вперед](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
