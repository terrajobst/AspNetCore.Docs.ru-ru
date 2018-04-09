---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Сортировка данных в DataList или управления повторителем (C#) | Документы Microsoft
author: rick-anderson
description: В этом учебнике мы рассмотрим, как для включения поддержки в DataList и повторителя сортировки, а также как создать DataList или повторителя, данные которого можно...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f31425a46408d6d544c6cdf2ce169b5547a2dd8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Сортировка данных в DataList или управления повторителем (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) или [скачать PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> В этом учебнике мы рассмотрим, как для включения поддержки в DataList и повторителя сортировки, а также как создать DataList или повторителя, данные можно разбиением на страницы и сортировки.


## <a name="introduction"></a>Вступление

В [с предыдущим учебником](paging-report-data-in-a-datalist-or-repeater-control-cs.md) мы рассмотрели, как добавить поддержку разбиения по страницам в DataList. Мы создали новый метод в `ProductsBLL` класса (`GetProductsAsPagedDataSource`), которые вернули `PagedDataSource` объекта. При привязке к DataList или повторителя, DataList или повторителя бы отобразить запрошенную страницу данных. Этот метод аналогичен чего используется внутренне элементами управления GridView, DetailsView и FormView для функционирования подкачки встроенного по умолчанию.

Помимо предложения поддержка разбиения на страницы, GridView также включает без дополнительной настройки, поддержки сортировки. DataList ни повторителя предоставляет встроенные функциональные возможности сортировки; Тем не менее функциям сортировки могут быть добавлены с большой объем кода. В этом учебнике мы рассмотрим, как для включения поддержки в DataList и повторителя сортировки, а также как создать DataList или повторителя, данные можно разбиением на страницы и сортировки.

## <a name="a-review-of-sorting"></a>Просмотрите сортировки

Как мы видели в [разбиение по страницам и сортировка данных отчета](../paging-and-sorting/paging-and-sorting-report-data-cs.md) , элемент управления GridView учебник без дополнительной настройки, поддержки сортировки. Каждое поле GridView имеет связанный с ним `SortExpression`, указывающая поле данных, по которому будут сортироваться данные. Когда GridView s `AllowSorting` свойству `true`, каждое поле GridView с `SortExpression` его к просмотру в виде LinkButton заголовок имеет значение свойства. Когда пользователь щелкает определенного заголовка s поле GridView, обратная передача и данных сортируется в соответствии с выбранной поле s `SortExpression`.

У элемента управления GridView `SortExpression` свойства, которое хранит `SortExpression` поля GridView данные отсортированы по. Кроме того `SortDirection` свойство указывает, является ли данные должны быть отсортированы в порядке возрастания или убывания (если пользователь нажимает кнопку, заменяется значением определенных связей заголовок поля s GridView два раза подряд, порядок сортировки).

Если GridView привязан к элементу управления источника данных, он передает его `SortExpression` и `SortDirection` свойства к данным система управления версиями. Элемент управления источником данных извлекает данные и он сортируется в соответствии с предоставленной `SortExpression` и `SortDirection` свойства. После сортировки данных элемента управления источником данных возвращается к GridView.

Чтобы реплицировать эта функциональность с элементами управления DataList или повторителя, необходимо выполнить следующее.

- Создать интерфейс сортировки
- Помните, поле данных, по которому выполняется сортировка и тип сортировки — по возрастанию или убыванию
- Указать ObjectDataSource сортировку по полю данных

Мы исследуем в этом выпуске эти три задачи в шагах 3 и 4. После этого мы изучим, как включить разбиение по страницам и сортировка в DataList и повторителя поддержки.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Шаг 2: Отображение продукты в повторитель

Прежде чем мы беспокоиться о реализации любого из функции, связанные с сортировкой, позволяют s начинается с составления списка продуктов в элементе управления повторителем. Сначала откройте `Sorting.aspx` страницы в `PagingSortingDataListRepeater` папки. Добавление элемента управления повторителем веб-страницы, установка его `ID` свойства `SortableProducts`. Смарт-тег повторителя s, создайте новый элемент управления ObjectDataSource с именем `ProductsDataSource` и настройте его для получения данных из `ProductsBLL` класса s `GetProducts()` метод. Выберите (нет) из раскрывающихся списков на вкладках INSERT, UPDATE и DELETE.


[![Создайте ObjectDataSource и настройте его с помощью метода GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Рис. 1**: ObjectDataSource создайте и настройте его для использования `GetProductsAsPagedDataSource()` метод ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**На рисунке 2**: установите раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет) ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


В отличие от с DataList, Visual Studio не создает автоматически `ItemTemplate` для управления повторителем после ее привязки к источнику данных. Кроме того, необходимо добавить это `ItemTemplate` декларативно, как смарт-тег элемента управления s повторителя отсутствует параметр редактирование шаблонов в DataList s. S позволяют использовать те же `ItemTemplate` с предыдущим учебником, который отображаться название продукта s, поставщиков и категории.

После добавления `ItemTemplate`, повторителя и ObjectDataSource s должна выглядеть следующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

На рисунке 3 показана эту страницу при просмотре через браузер.


[![Отображается каждый продукт s имя поставщика и категории](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Рис. 3**: отображается каждый продукт s имя поставщика и категории ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Шаг 3: Указание ObjectDataSource для сортировки данных

Чтобы отсортировать данные, отображаемые в повторителя, нам необходимо информировать ObjectDataSource выражения сортировки, с помощью которого данные должны быть отсортированы. Прежде, чем элемент управления ObjectDataSource извлекает свои данные, сначала происходит его [ `Selecting` событий](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), который дает возможность указать выражение сортировки. `Selecting` Обработчик событий передается объект типа [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), который имеет свойство с именем [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) типа [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). `DataSourceSelectArguments` Класс предназначен для передачи запросов, связанных с данными из объекта-получателя данных в элемент управления источником данных и включает [ `SortExpression` свойства](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Чтобы передать данные сортировки ObjectDataSource со страницы ASP.NET, создайте обработчик событий для `Selecting` событий и используйте следующий код:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

*SortExpression* имя поля данных для сортировки данных по (например ProductName) должно быть назначено значение. Нет связанных с направлением свойства сортировки, поэтому, если необходимо отсортировать данные в порядке убывания, добавить строку DESC для *sortExpression* значение (например ProductName DESC).

Попробуйте некоторые различные жестко запрограммированные значения *sortExpression* и проверить результаты в браузере. Как показано на рис. 4, при использовании DESC ProductName как *sortExpression*, продукты сортируются по их именам в обратном алфавитном порядке.


[![Продукты сортируются по имени в обратном алфавитном порядке](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Рис. 4**: продукты сортируются по имени в обратном алфавитном порядке ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Шаг 4: Создание интерфейса сортировки и запоминание выражение сортировки и направление

Включение поддержки в GridView сортировки преобразует текст заголовка каждого поля сортируемого s в LinkButton, при щелчке сортирует данные соответствующим образом. Сортировки интерфейс смысл GridView, где его аккуратно расположения данных в столбцах. Для элементов управления DataList и повторителя Однако другой интерфейс сортировки потребуется. Общий интерфейс сортировки список данных (в отличие от сетки данных), является предоставляет поля, по которым данные могут быть отсортированы в списке. Разрешить s реализовать такой интерфейс для этого учебника.

Добавить веб-управления DropDownList выше `SortableProducts` повторителя и задайте его `ID` свойства `SortBy`. В окне свойств нажмите кнопку с многоточием в `Items` свойства Отображение редактора коллекций ListItem. Добавить `ListItem` s, чтобы сортировать данные по `ProductName`, `CategoryName`, и `SupplierName` поля. Кроме того, добавить `ListItem` Чтобы отсортировать продукты по их именам в обратном алфавитном порядке.

`ListItem` `Text` Свойства можно задать любое значение (например, имя), но `Value` свойства должно быть присвоено имя поля данных (например, ProductName). Чтобы отсортировать результаты в порядке убывания, добавьте строку DESC на имя поля данных, например ProductName DESC.


![Добавьте элемент ListItem для каждого из полей сортировки данных](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Рис. 5**: добавление `ListItem` для каждого из полей сортировки данных


Наконец добавьте кнопку веб-элемент управления справа от DropDownList. Задайте его `ID` для `RefreshRepeater` и его `Text` свойства для обновления.

После создания `ListItem` s и добавление кнопки обновления DropDownList и кнопку s должен выглядеть следующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

С завершения сортировки DropDownList мы Далее необходимо обновить ObjectDataSource s `Selecting` обработчик событий, что он использует выбранный `SortBy``ListItem` s `Value` свойство, в отличие от выражение сортировки жестко.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

На этом этапе при первом просмотре странице продукты будут сначала упорядочены по `ProductName` поля данных, как оно s `SortBy` `ListItem` по умолчанию (см. рис. 6). Выбрать другой параметр, например категории сортировки, а также команда Обновить вызывает обратную передачу и повторно сортировать данные, имя категории, как показано на рис. 7.


[![Они отсортированы сначала по имени](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Рис. 6**: продуктов, которые изначально сортируются по имени ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![Они теперь отсортированы по категориям](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Рис. 7**: продуктов, которые теперь отсортированы по категориям ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Нажав кнопку "Обновить" в результате данные быть автоматически переупорядочены так как состояние просмотра s повторителя была отключена, тем самым вызывая повторителя для повторной привязки к источнику данных для каждой обратной передачи. Если можно хранить оставить повторителя s состояние представления включен, изменения сортировки раскрывающегося списка, выиграл t повлияет на порядок сортировки. Чтобы исправить это, создайте обработчик событий для кнопки Обновить s `Click` событий и повторную привязку к источнику данных повторителя (путем вызова повторителя s `DataBind()` метода).


## <a name="remembering-the-sort-expression-and-direction"></a>Запоминание выражение сортировки и направление

При создании сортируемого DataList или повторителя на странице, где не сортировки связанные обратных передач может произойти, оно императивного s, выражение сортировки и направление запоминать. Например предположим, что повторителя обновлены в этом учебнике, чтобы включить кнопку «Удалить» с каждого продукта. Когда пользователь нажимает кнопку удаления мы d выполнять определенный код для удаления выбранного продукта и затем привяжите данные к повторителя. Если параметры сортировки, не сохраняются между обратной передачи, данные, отображаемые на экране будет возвращено исходный порядок сортировки.

В этом учебнике DropDownList неявно сохраняет сортировки выражения и направление в свое состояние представления для нас. Если мы использовали другой интерфейс сортировки одно с, скажем, элементов управления LinkButton, предоставленный различные параметры сортировки d необходимо уделить особое внимание следует помнить порядка сортировки во время обратной передачи. Этого можно добиться путем сохранения параметров сортировки в состояние представления страницы s путем включения параметра сортировки в строке запроса или по какой-либо другой подход состояние сохраняемости.

Будущих примеры в этом учебнике исследовать, как сохранить сведения сортировки в состояние просмотра страницы s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Шаг 5: Добавление сортировки поддержки в DataList, использующий разбиения на страницы по умолчанию

В [предыдущего учебника](paging-report-data-in-a-datalist-or-repeater-control-cs.md) мы рассмотрели, как реализовать разбиение на страницы по умолчанию с помощью DataList. Разрешить s расширить этот предыдущий пример, чтобы включить возможность сортировать разбитых на страницы данных. Сначала откройте `SortingWithDefaultPaging.aspx` и `Paging.aspx` страниц в `PagingSortingDataListRepeater` папки. Из `Paging.aspx` щелкните «источник», чтобы просмотреть декларативная разметка страницы s. Копировать выделенный текст (см. рис. 8) и вставьте его в декларативной разметке `SortingWithDefaultPaging.aspx` между `<asp:Content>` тегов.


[![Реплицировать декларативная разметка в &lt;asp: Content&gt; тегов из Paging.aspx SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Рис. 8**: реплицировать декларативная разметка в `<asp:Content>` теги из `Paging.aspx` для `SortingWithDefaultPaging.aspx` ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


После копирования декларативная разметка, скопируйте методы и свойства в `Paging.aspx` s кода класса для класса с выделенным кодом для страницы `SortingWithDefaultPaging.aspx`. Далее занять некоторое время для просмотра `SortingWithDefaultPaging.aspx` страницы в браузере. Она должна вести себя ту же функциональность и внешний вид, как `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Улучшение ProductsBLL, чтобы включить разбиение по страницам и метод сортировки по умолчанию

В предыдущем учебнике мы создали `GetProductsAsPagedDataSource(pageIndex, pageSize)` метод в `ProductsBLL` класс, который вернул `PagedDataSource` объекта. Это `PagedDataSource` заполненный объект *все* продуктов (через уровень бизнес-ЛОГИКИ s `GetProducts()` метод), но при привязке к элементу управления DataList только те записи, соответствующий указанному *pageIndex* и *pageSize* отображались входных параметров.

Ранее в этом учебнике мы добавили поддержку сортировки, указав выражение сортировки из ObjectDataSource s `Selecting` обработчика событий. Это хорошо работает при ObjectDataSource возвращается объект, который можно сортировать, таких как `ProductsDataTable` возвращенных `GetProducts()` метод. Тем не менее `PagedDataSource` объект, возвращаемый `GetProductsAsPagedDataSource` метод не поддерживает сортировку его внутреннее данных источника. Вместо этого нужно отсортировать результаты, возвращенные `GetProducts()` метод *перед* мы поместили в `PagedDataSource`.

Для этого создайте новый метод в `ProductsBLL` класса `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Чтобы отсортировать `ProductsDataTable` возвращенных `GetProducts()` метод, укажите `Sort` свойство по умолчанию `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

`GetProductsSortedAsPagedDataSource` Метод лишь незначительно отличается от `GetProductsAsPagedDataSource` метод, созданный в предыдущем учебнике. В частности `GetProductsSortedAsPagedDataSource` принимает дополнительный параметр ввода `sortExpression` и присваивает это значение `Sort` свойство `ProductDataTable` s `DefaultView`. Несколько строк кода в более поздней версии, `PagedDataSource` присваивается объект s DataSource `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Вызов метода GetProductsSortedAsPagedDataSource и указав значение для параметра SortExpression входных данных

С `GetProductsSortedAsPagedDataSource` метод завершения, следующим шагом является для предоставления значения для этого параметра. ObjectDataSource в `SortingWithDefaultPaging.aspx` настраивается в настоящее время для вызова `GetProductsAsPagedDataSource` метод и передает два входных параметра через его `QueryStringParameters`, которые определены в `SelectParameters` коллекции. Эти два `QueryStringParameters` указывают, что источник для `GetProductsAsPagedDataSource` метод s *pageIndex* и *pageSize* параметры берутся из полей строки запроса `pageIndex` и `pageSize`.

Обновить ObjectDataSource s `SelectMethod` свойство, чтобы он вызывал новый `GetProductsSortedAsPagedDataSource` метод. Добавьте новый `QueryStringParameter` , чтобы *sortExpression* входного параметра осуществляется из поля строки запроса `sortExpression`. Задать `QueryStringParameter` s `DefaultValue` для ProductName.

После внесения этих изменений ObjectDataSource s должна выглядеть как:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

На этом этапе `SortingWithDefaultPaging.aspx` страницы будут отсортированы результаты в алфавитном порядке по названию продукта (см. рис. 9). Это происходит потому, что по умолчанию значение ProductName передается в качестве `GetProductsSortedAsPagedDataSource` метод s *sortExpression* параметра.


[![По умолчанию результаты сортируются по ProductName](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Рис. 9**: по умолчанию результаты сортируются по `ProductName` ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Если вручную добавить `sortExpression` поля строки запроса, таких как `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` результаты будут отсортированы по заданному `sortExpression`. Тем не менее это `sortExpression` параметр не включается в строке запроса при переходе на другую страницу данных. На самом деле, нажав кнопку Далее или последней страницы кнопок вернетесь нам к `Paging.aspx`! Кроме того существует s в настоящее время сортировки интерфейса. Единственным способом, пользователь может изменить порядок сортировки разбитых на страницы данных — путем изменения непосредственно в строку запроса.

## <a name="creating-the-sorting-interface"></a>Создание интерфейса сортировки

Необходимо сначала обновить `RedirectUser` метод для отправки пользователю `SortingWithDefaultPaging.aspx` (вместо `Paging.aspx`) и для включения `sortExpression` значение в строке запроса. Также следует добавить только для чтения, уровне страниц с именем `SortExpression` свойство. Это свойство аналогично `PageIndex` и `PageSize` свойства, созданные в предыдущем учебнике возвращает значение `sortExpression` поля строки запроса, если он существует и значение по умолчанию значение (ProductName) в противном случае.

В настоящее время `RedirectUser` метод принимает только один входной параметр индекс страницы для отображения. Тем не менее могут возникнуть ситуации, когда мы хотим перенаправит пользователя на определенную страницу данных с помощью выражения сортировки, отличные от какие s, указанный в строке запроса. Позже мы создадим интерфейс сортировки для этой страницы, которая будет содержать несколько кнопку веб-элементов управления для сортировки данных в указанном столбце. После нажатия одной из этих кнопок, мы хотим перенаправлять пользователя, передавая значение выражения сортировки, соответствующие. Для поддержки этой функции, создайте две версии `RedirectUser` метод. Первый должен принимать только индекс страницы для отображения, а второй принимает выражение страницы индекса и сортировки.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

В первом примере в этом учебнике мы создали сортировки интерфейс, с помощью DropDownList. В этом примере s позволяют использовать три кнопки веб-элементов управления, расположенный выше DataList один для сортировки по `ProductName`, один для `CategoryName`и один для `SupplierName`. Добавьте три кнопки веб-элементы управления, установка их `ID` и `Text` свойства соответствующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

Создайте `Click` обработчик событий для каждого. Обработчики событий должны вызывать `RedirectUser` метод, возврат на первую страницу, с помощью выражения сортировки соответствующего пользователя.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

При первом посещении страницы, данные отсортированы по названию продукта в алфавитном порядке (указывают назад на рис. 9). Нажмите кнопку Далее, чтобы перейти на вторую страницу данных, а затем нажмите кнопку сортировки, кнопка категории. Возвращает нам на первую страницу данных, отсортированных по имени категории (см. рис. 10). Аналогичным образом щелкнув кнопкой поставщиков сортировки сортирует данные по поставщикам, начиная с первой страницы данных. Выбор сортировки запоминается как страницам данных. Рис. 11 показана страница после сортировки по категориям, а затем перейдите к странице тринадцатый данных.


[![Продукты отсортированные по категориям](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Рис. 10**: продукты отсортированные по категориям ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![Выражение сортировки — сохраненных при разбивка на страницы через Data](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Рис. 11**: выражение сортировки — сохраненных при разбивка на страницы через Data ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Шаг 6: Пользовательское разбиение по страницам по записям повторитель

В примере DataList проверяются в шаге 5 страниц по данным способом подкачки неэффективным по умолчанию. При разбиении на страницы с большими объемами данных, важно использовать пользовательское разбиение по страницам. В [эффективно подкачки через большие объемы данных](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) и [сортировки пользовательских данных, разбитых на страницы](../paging-and-sorting/sorting-custom-paged-data-cs.md) учебники, мы рассмотрели различия между по умолчанию и пользовательское разбиение по страницам и созданные методы в МЕТОДА для Использование пользовательских разбиение по страницам и сортировка пользовательских разбитых на страницы данных. В частности, в этих двух предыдущих учебниках мы добавили следующие три метода `ProductsBLL` класса:

- `GetProductsPaged(startRowIndex, maximumRows)` Возвращает подмножество записей, начиная с *startRowIndex* и не превышая *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` Возвращает подмножество записей, отсортированных по указанным *sortExpression* входного параметра.
- `TotalNumberOfProducts()` предоставляет общее число записей в `Products` таблицы базы данных.

Эти методы можно использовать для эффективного страницы и сортировки данных с помощью элемента управления DataList или повторителя. Чтобы проиллюстрировать это, позволяют s начните с создания управления повторителем с настраиваемую поддержку разбиения на страницы; Затем мы добавим возможности сортировки.

Откройте `SortingWithCustomPaging.aspx` страницы в `PagingSortingDataListRepeater` папки и добавьте повторитель страницу, установив его `ID` свойства `Products`. Смарт-тег повторителя s, создайте новый элемент управления ObjectDataSource с именем `ProductsDataSource`. Настройте его для выбора данных из `ProductsBLL` класса s `GetProductsPaged` метод.


[![Настройка ObjectDataSource можно использовать метод GetProductsPaged класса ProductsBLL s](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Рис. 12**: Настройка ObjectDataSource для использования `ProductsBLL` класса s `GetProductsPaged` метод ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Задать раскрывающихся списков в UPDATE, INSERT и удаление вкладок (нет) и нажмите кнопку "Далее". Мастер настройки источника данных, а теперь запрашивает источники `GetProductsPaged` метод s *startRowIndex* и *maximumRows* входных параметров. На самом деле эти параметры игнорируются. Вместо этого *startRowIndex* и *maximumRows* значения будут передаваться в `Arguments` свойства в элемент управления ObjectDataSource s `Selecting` обработчика событий, как и как мы указали *sortExpression* в этой демонстрации первого учебника s. Таким образом оставьте параметр источника раскрывающихся списков в мастере на None.


[![Оставьте параметр набор источников None](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Рис. 13**: оставьте параметр источников, значение None ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Сделать *не* набор ObjectDataSource s `EnablePaging` свойства `true`. Это приведет к ObjectDataSource автоматически включать свой собственный *startRowIndex* и *maximumRows* параметры `SelectMethod` s существующий список параметров. `EnablePaging` Свойство полезно, когда пользовательские привязки постраничных данных к элементу управления GridView, DetailsView и FormView, поскольку эти элементы управления ожидает определенное поведение из ObjectDataSource s только при `EnablePaging` свойство `true`. Поскольку мы имеем вручную добавить поддержку разбиения на страницы DataList и повторителя, оставьте это свойство установлено в `false` (по умолчанию), как мы будем встраивает в необходимую функциональность непосредственно в нашу страницу ASP.NET.


Наконец, определите повторителя s `ItemTemplate` , чтобы название продукта s, категории и поставщик отображаются. После внесения этих изменений повторителя и ObjectDataSource s должен выглядеть следующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Теперь пора перейти на страницу с помощью браузера и обратите внимание, что записи не возвращаются. Это, поскольку мы хранять еще для указания *startRowIndex* и *maximumRows* значения параметров; таким образом, значения 0 передаются в обоих. Чтобы задать эти значения, создайте обработчик событий для ObjectDataSource s `Selecting` событий и установите эти параметры программным образом значений жестко запрограммированные значения от 0 до 5, соответственно:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Благодаря этому изменению странице при просмотре через браузер, показаны первые пять продукты.


[![Отображаются первых пяти записей](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Рис. 14**: отображаются первых пяти записей ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> Продукты, содержащиеся на рис. 14 произойти должны быть отсортированы по названию продукта, так как `GetProductsPaged` хранимую процедуру, которая выполняет эффективного пользовательского запроса подкачки упорядочивает результаты по `ProductName`.


Чтобы разрешить пользователям перемещаться по страницам, нам нужно отслеживать индекс начальной строки и максимальное число строк и запомнить эти значения во время обратной передачи. В примере разбиения на страницы по умолчанию использовался полей строки запроса для сохранения этих значений; для этой демонстрации позволяют сохранить эту информацию в состояние просмотра страницы s s. Создайте следующие два свойства:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

Затем обновите код в обработчик событий при выборе, чтобы он использовал `StartRowIndex` и `MaximumRows` свойства вместо жестко закодированные значения от 0 до 5:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

На этом этапе нашу страницу по-прежнему показывает только первые пять записей. Однако с этими свойствами на месте, мы готов для создания нашей интерфейса постраничного просмотра.

## <a name="adding-the-paging-interface"></a>Добавление интерфейса постраничного просмотра

Let s используйте интерфейс же во-первых, назад, далее, после последнего разбиение по страницам используется в примере разбиения на страницы по умолчанию, включая Интернет метки просматривается элемент управления, отображающий какие страницы данных и существует общее количество страниц. Добавьте четыре кнопки веб-элементов управления и подпись под повторителя.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

Создайте `Click` обработчики событий для четырех кнопок. После нажатия одной из этих кнопок, нам нужно обновить `StartRowIndex` и привяжите данные к повторителя. Код для кнопки первой, предыдущую и следующую довольно проста, но для кнопки последней как мы определяем индекс начальной строки для последней странице данных? Для вычисления этого индекса, а также возможность определить Включение кнопок Далее и Дата последнего нам требуется узнать, сколько записей всего страниц. Это можно определить, вызвав `ProductsBLL` класса s `TotalNumberOfProducts()` метод. S позволяют создать свойство только для чтения, уровне страниц с именем `TotalRowCount` , возвращает результаты `TotalNumberOfProducts()` метод:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Это свойство можно теперь определить индекс последней страницы s начальной строки. В частности, он s целочисленный результат из `TotalRowCount` минус 1, деленное на `MaximumRows`и умноженному `MaximumRows`. Теперь можно написать `Click` обработчики событий для четырех кнопок интерфейс разбиения на страницы:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Наконец необходимо отключить кнопки первой» и «назад в интерфейсе постраничного просмотра, при просмотре первой страницы данных и кнопок Далее и Дата последнего при просмотре на последней странице. Для этого добавьте следующий код в элемент управления ObjectDataSource s `Selecting` обработчик событий:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

После добавления этих `Click` обработчики событий и код, чтобы включить или отключить элементы интерфейса постраничного просмотра, основанные на текущий индекс начальной строки проверять страницу в браузере. Как показано на рис. 15, при первом посещении страницы первого и кнопок будут отключены. Кнопки Далее показаны на второй странице данных, щелкнув последнего отображает последней страницы (см. рисунки 16 и 17). При просмотре на последней странице данных кнопок "Далее" и "Дата последнего отключены.


[![Назад и последней кнопки отключены при просмотре первой страницы продуктов](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Рис. 15**: Previous и последней кнопки отключены при просмотре первой страницы продуктов ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![Вторая страница продуктов, которые вывод](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**На рисунке 16**: второй страницы продукты, вывод ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Нажав кнопку последней отображаются на последней странице данных](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Рисунок 17**: при щелчке последнего отображаются данные из последней страницы ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Шаг 7., Включая поддержки со специальной сортировки разбитых на страницы повторителя

Теперь, когда используется пользовательское разбиение по страницам, повторно все готово для включения сортировки поддерживаются. `ProductsBLL` Класса s `GetProductsPagedAndSorted` метод имеет те же *startRowIndex* и *maximumRows* входных параметров, как `GetProductsPaged`, но позволяет использовать дополнительный  *sortExpression* входного параметра. Для использования `GetProductsPagedAndSorted` метод `SortingWithCustomPaging.aspx`, необходимо выполнить следующие действия:

1. Изменить ObjectDataSource s `SelectMethod` свойство из `GetProductsPaged` для `GetProductsPagedAndSorted`.
2. Добавить *sortExpression* `Parameter` объекта ObjectDataSource s `SelectParameters` коллекции.
3. Создать закрытый, уровне страниц `SortExpression` свойство, которое сохраняет его значение во время обратной передачи через состояния просмотра страницы s.
4. Обновить ObjectDataSource s `Selecting` обработчик событий, чтобы назначить ObjectDataSource s *sortExpression* параметр значение уровня страницы `SortExpression` свойство.
5. Создайте интерфейс сортировки.

Начните с обновления ObjectDataSource s `SelectMethod` свойств и добавления *sortExpression* `Parameter`. Убедитесь, что *sortExpression* `Parameter` s `Type` свойству `String`. После выполнения этих задач первых двух декларативная разметка ObjectDataSource s должен выглядеть следующим образом:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

Теперь нам нужны уровне страницы `SortExpression` свойства, значение которого сериализуется в состоянии представления. Если значение выражения не сортировки используйте ProductName по умолчанию:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Перед вызывает элемент управления ObjectDataSource `GetProductsPagedAndSorted` нам нужно указать метод *sortExpression* `Parameter` значение `SortExpression` свойства. В `Selecting` обработчика событий, добавьте следующую строку кода:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Осталось реализовать интерфейс сортировки. Как это делалось в последнем примере позволяют s имеют сортировки интерфейс, реализованный с помощью три кнопки веб-элементы управления позволяют пользователю для сортировки результатов название продукта, категории или поставщика.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Создание `Click` обработчики событий для эти три элемента управления Button. В случае сброса обработчик, `StartRowIndex` задано значение 0, `SortExpression` соответствующее значение и повторную привязку данных к повторителя:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

Все, что s — его! Несмотря на наличие ряд действий, чтобы получить пользовательское разбиение по страницам и сортировка реализован, действия были очень похоже на те, которые необходимы для разбиения на страницы по умолчанию. На рисунке 18 показаны продукты, при просмотре на последней странице данных при сортировке по категориям.


[![Отображаются последние данные страницы, Sorted по категориям,](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**На рисунке 18**: последней страницы данных, Sorted по категориям, отображается ([Просмотр полноразмерное изображение](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> В предыдущих примерах, при сортировке по поставщикам, которые «название» был использован в качестве выражения сортировки. Однако для реализации пользовательского разбиения на страницы, необходимо использовать CompanyName. Это, поскольку хранимая процедура отвечает за реализацию пользовательское разбиение по страницам `GetProductsPagedAndSorted` передает выражение сортировки в `ROW_NUMBER()` ключевое слово, `ROW_NUMBER()` ключевое слово требуется действительное имя столбца, а не псевдоним. Таким образом, мы используем `CompanyName` (имя столбца в `Suppliers` таблицы) вместо псевдоним, используемый в `SELECT` запроса (`SupplierName`) для выражения сортировки.


## <a name="summary"></a>Сводка

Ни DataList ни повторителя предоставляют встроенную поддержку сортировки, но с помощью небольшой код и пользовательский интерфейс сортировки, можно добавить такие функциональные возможности. При реализации сортировки, но не разбиение по страницам, выражение сортировки можно указать с помощью `DataSourceSelectArguments` , переданное в ObjectDataSource s `Select` метод. Это `DataSourceSelectArguments` объект s `SortExpression` можно назначить свойства в элемент управления ObjectDataSource s `Selecting` обработчика событий.

Чтобы добавить возможности сортировки в DataList или повторителя, который уже обеспечивает поддержку разбиения на страницы, простой подход — это настройка уровня бизнес-логики, чтобы включить метод, который принимает выражение сортировки. Эти данные затем можно передать в через параметр в ObjectDataSource s `SelectParameters`.

Этот учебник завершает изучение разбиение по страницам и сортировка с помощью элементов управления DataList и повторителя. В этом руководстве Далее и последней проверит как добавить кнопку веб-элементов управления в DataList и повторителя s шаблоны для обеспечения функций некоторые пользовательские, инициированной пользователем отдельно для каждого элемента.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основной рецензент этого учебника было David Suru. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
> [Вперед](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
