---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: "Фильтрация на двух страницах (VB) иерархического | Документы Microsoft"
author: rick-anderson
description: "В этом учебнике мы рассмотрим, как для разделения главного и подчиненного представлений отчета на двух страницах. На странице «master» для отображения списка categ мы используем управления повторителем..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f43fa998b81800cb1a2b7796ebb3922fc1caeb8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Иерархического фильтрации на двух страницах (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) или [скачать PDF](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> В этом учебнике мы рассмотрим, как для разделения главного и подчиненного представлений отчета на двух страницах. На странице «главный» мы используем управления повторителем для подготовки к просмотру список категорий, если он выбран, чтобы начать на странице «сведения» где DataList двух столбцов показывает этих продуктов, принадлежащих выбранной категории.


## <a name="introduction"></a>Вступление

В [иерархического фильтрации по две страницы](../masterdetail/master-detail-filtering-across-two-pages-vb.md) учебнике мы рассмотрели этот шаблон, с помощью элемента управления GridView для отображения всех поставщиков в системе. Это GridView включены HyperLinkField, который подготовлен к просмотру как ссылка на второй странице, передавая `SupplierID` в строке запроса. Второй странице используется элемент управления GridView отображены продукты, предоставляемые выбранного поставщика.

Такие отчеты двухстраничный главного и подчиненного представлений можно выполнить при помощи управления DataList и повторителя. Единственное различие — что DataList ни повторителя обеспечивает поддержку управления HyperLinkField. Вместо этого необходимо добавить гиперссылку веб-элемент управления или элемент привязки HTML (`<a>`) в элементе управления `ItemTemplate`. По гиперссылке `NavigateUrl` свойство или точку привязки `href` атрибут легко настроить с помощью декларативной или программного подхода.

В этом учебнике мы рассмотрим пример, в котором перечислены категории в виде маркированного списка на одной странице с помощью элемента управления повторителем. Каждый элемент списка будет включать имя и описание, категории с именем категории отображаются в виде ссылки на вторую страницу. Щелкните эту ссылку будет whisk пользователя на вторую страницу, где DataList покажет эти продукты, которые принадлежат выбранной категории.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Шаг 1: Отображение категорий в виде маркированного списка

Первым шагом при создании любого главного и подчиненного представлений отчета является запуск по отображению записей «главный». Таким образом нашей первой задачей является отображение категории в странице «главный». Откройте `CategoryListMaster.aspx` страницы в `DataListRepeaterFiltering` , добавьте элемент управления повторителем, а, смарт-теге, необязательно, чтобы добавить новый элемент управления ObjectDataSource. Настройте новый элемент управления ObjectDataSource таким образом, чтобы он обращается к своим данным из `CategoriesBLL` класса `GetCategories` метод (см. рис. 1).


[![Настройка ObjectDataSource использовать метод GetCategories класса CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Рис. 1**: Настройка ObjectDataSource для использования `CategoriesBLL` класса `GetCategories` метод ([Просмотр полноразмерное изображение](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


Затем определите шаблоны повторителя таким образом, что каждый название и описание категории отображается как элемент в виде маркированного списка. Давайте еще не беспокоиться о получении каждой категории ссылка на страницу сведений. Ниже показана декларативная разметка для повторителя и ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

С этой разметкой завершения мгновение просмотрите ход работы через браузер. Как показано на рисунке 2, повторителя выводится в виде маркированного списка отображаются имя и описание каждой категории.


[![Каждая категория будет отображена как элемент маркированного списка](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**На рисунке 2**: каждый категория будет отображена как элемент маркированного списка ([Просмотр полноразмерное изображение](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Шаг 2: Имя категории превращается в ссылку на страницу сведений

Чтобы предоставить пользователю для отображения информации «подробности» для данной категории, необходимо добавить ссылку к каждому маркированный список элементом, к которому, если он выбран, чтобы начать на вторую страницу (`ProductsForCategoryDetails.aspx`). Второй страницы, будет выведено продуктов для выбранной категории с помощью DataList. Чтобы определить категории, в которой связь была нажата, нам нужно передать выбранной категории `CategoryID` на вторую страницу через некоторый механизм. Простой и самый простой способ передачи скалярные данные из одной страницы в другой — через строку запроса, — параметром, который будет использоваться в этом учебнике. В частности `ProductsForCategoryDetails.aspx` страницы будет ожидать, что выбранный  *`categoryID`*  значение должен передаваться querystring поле с именем `CategoryID`. Например, чтобы просмотреть продукты категории напитков, который имеет `CategoryID` 1, пользователю будет посетить `ProductsForCategoryDetails.aspx?CategoryID=1`.

Создание гиперссылки для каждого элемента маркированного списка в Повторителе, то необходимо либо добавить гиперссылку веб-элемент управления или элемент привязки HTML (`<a>`) для `ItemTemplate`. В сценарии, где гиперссылки отображается одинаково для каждой строки, подойдет любой из этих подходов. Знаки повторения я предпочитаю, с помощью элемента привязки. Чтобы использовать элемент привязки, обновите ItemTemplate повторителя:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Обратите внимание, что `CategoryID` могут быть добавлены непосредственно в элемент привязки `href` по умолчанию однако для поэтому обязательно разделения `href` значение атрибута в апострофы (и Примечание кавычки) с момента `Eval` метод в пределах `href` атрибут разделяет его строки (`"CategoryID"`) в кавычки. Кроме того гиперссылку веб-элемент управления может использоваться вместо:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Обратите внимание как статические часть URL-адрес — `ProductsForCategoryDetails.aspx?CategoryID` — добавляется к результату `Eval("CategoryID")` непосредственно в синтаксис привязки данных, с помощью объединения строк.

Одно из преимуществ использования HyperLink — что его можно программным образом доступ повторителя `ItemDataBound` обработчика событий, при необходимости. Например можно отображать имя категории, как текст, а не как ссылки на категории с нет соответствующих продуктов. Такой контроль может осуществляться программными средствами в `ItemDataBound` обработчика события; для категорий нет связанных продуктов, гиперссылки `NavigateUrl` свойство может принимать значение содержит пустую строку, тем самым в результате чего это имя определенной категории Подготовка к просмотру как обычный текст (а не как ссылки). Обращаться к [форматирование DataList и повторителя после данных](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) учебника Дополнительные сведения о форматировании содержимого DataList и повторителя элемента на основании программная логика через `ItemDataBound` обработчика событий.

Если вы следуете, вы можете использовать элемент привязки или подход элемента управления HyperLink на странице. Независимо от подхода, при просмотре страницы через браузер, имена категорий должны отображаться как ссылка на `ProductsForCategoryDetails.aspx`, передавая применимый `CategoryID` значение (см. рис. 3).


[![Имена категорий связывать с ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Рис. 3**: категория имена теперь ссылку, чтобы `ProductsForCategoryDetails.aspx` ([Просмотр полноразмерное изображение](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Шаг 3: Список продуктов, которые принадлежат выбранной категории

С `CategoryListMaster.aspx` страница полностью, мы готовы к можем обратиться к реализации странице «Подробности» `ProductsForCategoryDetails.aspx`. Открыть эту страницу, перетащите DataList из области элементов в конструктор и задайте его `ID` свойства `ProductsInCategory`. Затем выберите элемент управления DataList смарт-теге для добавления новых ObjectDataSource на страницу, присвоив ему имя `ProductsInCategoryDataSource`. Настройте его таким образом, что она вызывает `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод; набор, перечислены в раскрывающемся списке на вкладках INSERT, UPDATE и DELETE (нет).


[![Настройка ObjectDataSource использовать метод GetProductsByCategoryID(categoryID) класса ProductsBLL](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Рис. 4**: Настройка ObjectDataSource для использования `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерное изображение](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


Поскольку `GetProductsByCategoryID(categoryID)` метод принимает входной параметр (*`categoryID`*), мастер источников данных выберите дает нам возможность указать источник параметров. Задайте источник параметра строки запроса с помощью QueryStringField `CategoryID`.


[![Использовать CategoryID поля строки запроса в качестве источника параметра](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Рис. 5**: используйте поле Querystring `CategoryID` как параметр источника ([Просмотр полноразмерное изображение](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Как видно из предыдущих занятий, после завершения работы мастера выберите источник данных, Visual Studio автоматически создает `ItemTemplate` для DataList списком каждого имени поля данных и значение. Замените этот шаблон, содержащий только имя, поставщик и цену продукта. Кроме того, задать элемент управления DataList `RepeatColumns` равным 2. После внесения этих изменений DataList и ObjectDataSource должна выглядеть следующим образом:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Чтобы просмотреть эту страницу в действии, запустите `CategoryListMaster.aspx` страницы, затем щелкните ссылку в маркированный список категорий. Таким образом вы перейдете на `ProductsForCategoryDetails.aspx`, передав вдоль `CategoryID` через строку запроса. `ProductsInCategoryDataSource` ObjectDataSource в `ProductsForCategoryDetails.aspx` затем получить только те продукты, для указанной категории и их отображения в DataList, который отображает два продукта для каждой строки. Рис. 6 показан снимок экрана `ProductsForCategoryDetails.aspx` при просмотре напитков.


[![Отображаются напитков, два каждой строки](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Рис. 6**: отображаются напитков, два каждой строки ([Просмотр полноразмерное изображение](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Шаг 4: Отображение сведений о категории на ProductsForCategoryDetails.aspx

Когда пользователь выбирает категорию в `CategoryListMaster.aspx`, они направляются `ProductsForCategoryDetails.aspx` и отображаются продукты, которые принадлежат выбранной категории. Однако в `ProductsForCategoryDetails.aspx` существуют не визуальные подсказки о том, какая категория выбрана. Пользователь, который предназначен для щелкните напитков, но случайно выбранная приправы никак не реализации свои ошибки после попадания `ProductsForCategoryDetails.aspx`. Чтобы устранить эту проблему, потенциальные, можно отобразить сведения о выбранной категории, его имя и описание — в верхней части `ProductsForCategoryDetails.aspx` страницы.

Для этого добавьте FormView над элементом управления повторителем в `ProductsForCategoryDetails.aspx`. Добавьте новый элемент управления ObjectDataSource страницы FormView смарт-теге с именем `CategoryDataSource` и настройте его для использования `CategoriesBLL` класса `GetCategoryByCategoryID(categoryID)` метод.


[![Доступ к сведениям о категории через метод GetCategoryByCategoryID(categoryID) класса CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Рис. 7**: доступ к сведениям о категории через `CategoriesBLL` класса `GetCategoryByCategoryID(categoryID)` метод ([Просмотр полноразмерное изображение](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


Как и в `ProductsInCategoryDataSource` ObjectDataSource добавлен на шаге 3 `CategoryDataSource`в мастер настройки источника данных запрашивает источник для `GetCategoryByCategoryID(categoryID)` метод входных параметров. Использовать одинаковые параметры как до, источника параметра установите значение строки запроса и значение QueryStringField `CategoryID` (указывают назад на рис. 5).

По завершении работы мастера Visual Studio автоматически создает `ItemTemplate`, `EditItemTemplate`, и `InsertItemTemplate` для FormView. Так как мы предлагаем интерфейс только для чтения, вы можете удалить `EditItemTemplate` и `InsertItemTemplate`. Кроме того, вы можете настроить FormView `ItemTemplate`. После удаления неиспользуемых шаблоны и настройка ItemTemplate, ваши FormView и ObjectDataSource должна выглядеть следующим образом:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

На рисунке 8 показан пример при просмотре эту страницу через браузер.

> [!NOTE]
> В дополнение к FormView, были также добавлены элемента управления HyperLink выше FormView, которое будет перенаправлять пользователя обратно в список категорий (`CategoryListMaster.aspx`). Вы можете разместить эту ссылку в другом месте или удалите его вообще.


[![Сведения о категории — теперь отображаются в верхней части страницы](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Рис. 8**: сведения о категории — теперь отображаются в верхней части страницы ([Просмотр полноразмерное изображение](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Шаг 5: Отображение сообщения, если продукты не принадлежат к выбранной категории

`CategoryListMaster.aspx` Странице перечислены все категории в системе, независимо от них связанных продуктов. Если пользователь щелкает категорию с нет соответствующих продуктов, элемент управления DataList в `ProductsForCategoryDetails.aspx` не отображается, как источника данных не будет иметь элементов. Как видно в предыдущих учебных GridView предоставляет `EmptyDataText` свойство, которое может использоваться для указания текстовое сообщение для отображения, если нет записей в источник данных. К сожалению ни DataList, ни повторителя не имеют такого свойства.

Для отображения сообщения, информирующего пользователя о том, что нет ни одного сопоставления продукта для выбранной категории, необходимо добавить метку элемент управления на страницу `Text` присваивается сообщение, отображаемое в случае, если нет ни одного соответствующего продукта. Затем необходимо программно задать его `Visible` свойство зависимости от того, является ли элемент управления DataList содержит какие-либо элементы.

Чтобы сделать это, начните с добавления метку под DataList. Задайте его `ID` свойства `NoProductsMessage` и его `Text` значение «Нет ни одного продукта для выбранной категории...» Теперь нам нужны программно задать эту метку `Visible` свойство зависимости ли все данные были привязаны к `ProductsInCategory` DataList. Это назначение должно выполняться после привязки данных к элементу управления DataList. GridView, DetailsView и FormView удалось создать обработчик событий для элемента управления `DataBound` события, который вызывается после завершения привязки данных. Однако элемент управления DataList ни повторителя имеет `DataBound` доступные события.

В данном конкретном примере можно назначить метку `Visible` свойство в `Page_Load` обработчика событий, так как данные будут назначены DataList до страницы `Load` событий. Однако такой подход не будет работать в общем случае, как данные из ObjectDataSource может быть привязан к элементу управления DataList позднее в жизненном цикле страницы. Например, если отображаемых данных основана на значение другого элемента управления, такие как если бы отображение главного и подчиненного представлений отчетов при помощи DropDownList для хранения записей «главный», данные могут не привязывается веб-управления данными до `PreRender` этап в жизненного цикла страницы.

Одно решение, которое будет работать для всех вариантов, — присвоить `Visible` свойства `False` в элемент управления DataList `ItemDataBound` (или `ItemCreated`) обработчик событий при привязке элемент `Item` или `AlternatingItem`. В этом случае мы знаем, что имеется по крайней мере один данные элемента в источнике данных и таким образом можно скрыть `NoProductsMessage` метки. Помимо этого обработчика событий, заключаются в обработчик событий для DataList `DataBinding` событий, где инициализации метки `Visible` свойства `True`. С момента `DataBinding` событие возникает перед `ItemDataBound` события, метки `Visible` начальные значения свойств `True`; Если все элементы данных, тем не менее, оно будет иметь значение `False`. Следующий код реализует следующую логику:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Все категории в базе данных Northwind связаны с одного или нескольких продуктов. Для тестирования этого компонента, я изменен вручную, базы данных Northwind в этом учебнике переназначение все продукты, связанные с категорией фруктов (`CategoryID` = 7) в категорию Морепродукты (`CategoryID` = 8). Это можно сделать из обозревателя серверов, выбрав новый запрос и с помощью следующих `UPDATE` инструкции:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

После обновления базы данных соответствующим образом, вернуться на `CategoryListMaster.aspx` и щелкните ссылку, создают. Так как больше не существуют какие-либо продукты, принадлежащие к категории фруктов, вы увидите «Нет ни одного продукта для выбранной категории...» сообщение, как показано на рис. 9.


[![Сообщение отображается, если нет продукты, принадлежащие к категории выбранного](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Рис. 9**: отображается сообщение, если нет продукты, принадлежащие к категории выбран ([Просмотр полноразмерное изображение](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>Сводка

Хотя отчеты главного и подчиненного представлений можно отобразить основные и подробные записи на одной странице, на многих веб-сайтах они размещены на двух веб-страниц. В этом учебнике мы рассмотрели способы реализации такого отчета иерархического наличием категорий, перечисленных в виде маркированного списка, используя повторитель «главный» веб-странице и соответствующих продуктов, перечисленные на странице «Подробности». Каждый элемент списка на основной веб-странице содержится ссылка на страницу сведений, который передается строка `CategoryID` значение.

На странице сведений о получении этих продуктов для указанного поставщика выполнялось через `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод.  *`categoryID`*  Было указано значение параметра декларативно с помощью `CategoryID` значение строки запроса в виде источника параметра. Также мы рассмотрели способы отображения сведений о категории в странице сведений с помощью FormView и отображать сообщение, если бы не принадлежащие к выбранной категории продуктов.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Зак Джонс и (Liz Shulok). Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
[Вперед](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
