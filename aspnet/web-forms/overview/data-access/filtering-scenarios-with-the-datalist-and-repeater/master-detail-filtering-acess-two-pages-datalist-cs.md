---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: «Основной/подробности» фильтрации на двух страницах (C#) | Документация Майкрософт
author: rick-anderson
description: В этом руководстве мы рассмотрим размещение отчета «основной/подробности» на двух страницах. На странице «master» мы используем элементом управления Repeater для визуализации списка categ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 28a30fff5092cf995d267f15f608739af000e32c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377857"
---
<a name="masterdetail-filtering-across-two-pages-c"></a>«Основной/подробности» фильтрации на двух страницах (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) или [скачать PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> В этом руководстве мы рассмотрим размещение отчета «основной/подробности» на двух страницах. В «основной» странице мы используем элементом управления Repeater для визуализации списка категорий, который, если он выбран, пользователь сможет перейти к странице «сведения» где двухстолбцовый элемент управления DataList показывает продукты, принадлежащие к выбранной категории.


## <a name="introduction"></a>Вступление

В [предыдущем учебном курсе](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) был рассмотрен способ отображения отчетов «основной/подробности» на одной веб-странице, используя списки DropDownList для отображения «основных» записей и элемент управления DataList для «подробностей». Другим распространенным шаблоном таких отчетов «основной/подробности» является размещение основных записей на одной веб-странице а подробностей на другой. В более ранней ["основной/подробности" Фильтрация по две страницы](../masterdetail/master-detail-filtering-across-two-pages-cs.md) мы изучили этот шаблон, с помощью элемента управления GridView для отображения всех поставщиков в системе. Этот GridView включены поля HyperLinkField, который отображается как ссылка на вторую страницу, передавая `SupplierID` в строке запроса. Вторая страница используется GridView отображены продукты, предоставляемые выбранным поставщиком.

Такие отчеты две страницы «основной/подробности» может выполняться с помощью также элементов управления DataList и Repeater. Единственное различие в том, что ни элемент управления DataList, ни элементу управления Repeater предоставляет поддержки для поля HyperLinkField элемента управления. Вместо этого необходимо добавить гиперссылку веб-элемент управления или элемент привязки HTML (`<a>`) в элементе управления `ItemTemplate`. Гиперссылки `NavigateUrl` или атрибут привязки `href` атрибута можно затем настроить, используя декларативный или программный подходы.

В этом руководстве мы рассмотрим пример, в котором выводятся категории в маркированном списке на одну страницу, с помощью элемента управления Repeater. Каждый элемент списка будет включать имя и описание, категорию с именем категории отображаются в виде ссылки на вторую страницу. Щелчок ссылки перенесет пользователя на вторую страницу, где элемент управления DataList покажет продукты, принадлежащие выбранной категории.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Шаг 1: Отображение категорий в маркированном списке

Первым шагом в создании любого отчета «основной/подробности» является отображение «основных» записей. Таким образом наша первая задача является отображение категорий в «основной» странице. Откройте `CategoryListMaster.aspx` странице в `DataListRepeaterFiltering` папки, добавьте элемент управления Repeater и, в смарт-теге, необязательно, чтобы добавить новый элемент управления ObjectDataSource. Настройте новый элемент управления ObjectDataSource, таким образом, чтобы он обращается к своим данным из `CategoriesBLL` класса `GetCategories` метод (см. рис. 1).


[![Настройте элемент ObjectDataSource для использования метода GetCategories класса categoriesbll](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Рис. 1**: Настройка ObjectDataSource для использования `CategoriesBLL` класса `GetCategories` метод ([Просмотр полноразмерного изображения](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))


Затем определите шаблоны элемента Repeater, таким образом, чтобы он отображает каждый имя и описание категории как элемент в маркированном списке. Давайте еще не волноваться о наличии каждой категории ссылку на страницу сведений. Ниже показана декларативная разметка для элемента управления Repeater и ObjectDataSource.

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

По завершении этой разметки Отвлекитесь и просмотрите ход работы через браузер. Как показано на рис. 2, элементу управления Repeater визуализируется как маркированный список, показывая имя и описание каждой категории.


[![Каждая категория отображается как элемент маркированного списка](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Рис. 2**: Каждая категория отображается как элемент маркированного списка ([Просмотр полноразмерного изображения](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Шаг 2: Превращение имени категории в ссылку на страницу сведений

Чтобы разрешить пользователю отображать «подробности» для данной категории, нам нужно добавить ссылку к каждому маркированном списке элемент, который, если он выбран, будет переводить пользователя на вторую страницу (`ProductsForCategoryDetails.aspx`). Эта вторая страница затем отобразит продукты для выбранной категории, используя элемент управления DataList. Чтобы определить категорию, ссылка которой была щелкнута, необходимо передать выбранной категории `CategoryID` на второй странице через какой-либо механизм. Самый простой и самый простой способ передачи скалярных данных с одной страницы к другой — через строку запроса, — параметром, который будет использоваться в этом руководстве. В частности `ProductsForCategoryDetails.aspx` страницы будет ожидать передачи выбранного *`categoryID`* значение для передачи через поле строки запроса с именем `CategoryID`. Например, чтобы просмотреть продукты категории «Напитки», которая имеет `CategoryID` 1, пользователь посетит `ProductsForCategoryDetails.aspx?CategoryID=1`.

Создание гиперссылки для каждого элемента маркированного списка в элемент управления Repeater, необходимо либо добавить гиперссылку веб-элемент управления или элемент привязки HTML (`<a>`) для `ItemTemplate`. В ситуации, когда гиперссылка отображается одинаково для каждой строки, применимы оба подхода. Для знаки повторения я предпочитаю использовать элемент привязки. Чтобы использовать элемент привязки, обновите ItemTemplate элемента Repeater:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Обратите внимание, что `CategoryID` можно ввести прямо внутрь атрибута элемента привязки `href` атрибут; тем не менее, чтобы таким образом не забудьте разделять `href` значение атрибута с апострофами (и Примечание кавычки) с момента `Eval` метод в рамках `href` выделяет свою строку (`"CategoryID"`) кавычки. Кроме того гиперссылки веб-элемент управления можно использовать:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Обратите внимание как к статической части URL-адрес — `ProductsForCategoryDetails.aspx?CategoryID` — добавляется к результату `Eval("CategoryID")` непосредственно внутри синтаксиса привязки данных, использование объединения строк.

Является одним из преимуществ использования элемента управления HyperLink, что его можно получить доступ программными средствами из элемента Repeater `ItemDataBound` обработчик событий, при необходимости. Например может потребоваться отобразить имя категории как текст, а не как ссылку для категории с не связано никаких продуктов. Такая проверка может выполняться программными средствами в `ItemDataBound` обработчик событий; для категорий, не имеющий связанные продукты, гиперссылки `NavigateUrl` удалось задать свойство содержит пустую строку, таким образом, что имя определенной категории Подготовка к просмотру как обычный текст (а не как ссылку). Вернуться к [форматирование элементов управления DataList и Repeater от данных](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) руководстве Дополнительные сведения о форматировании содержимого элементов управления DataList и Repeater на основе программной логики через `ItemDataBound` обработчик событий.

Если вы следуете, вы можете использовать элемент привязки или подход элемента управления гиперссылки на странице. Независимо от подхода, при просмотре страницы через обозреватель каждое имя категории должно подготавливаться к просмотру как ссылку `ProductsForCategoryDetails.aspx`, передав в соответствующем `CategoryID` значение (см. рис. 3).


[![Имена категорий теперь ссылка на ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Рис. 3**: категория имена теперь ссылку `ProductsForCategoryDetails.aspx` ([Просмотр полноразмерного изображения](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Шаг 3: Перечисление продуктов, принадлежащих выбранной категории

С помощью `CategoryListMaster.aspx` страница полностью, мы готовы к обратим наше внимание на реализацию страницы «подробности», `ProductsForCategoryDetails.aspx`. Открыть эту страницу, перетащите элемент управления DataList из инструментария в конструктор и задайте его `ID` свойства `ProductsInCategory`. Затем выберите смарт-теге элемента управления DataList для добавления нового источника ObjectDataSource на страницу, назовите его `ProductsInCategoryDataSource`. Настройте его таким образом, чтобы он вызывал `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод; набора раскрывающихся списках на вкладках INSERT, UPDATE и DELETE (нет).


[![Настройка ObjectDataSource на использование класса ProductsBLL метод GetProductsByCategoryID(categoryID)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Рис. 4**: Настройка ObjectDataSource для использования `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод ([Просмотр полноразмерного изображения](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))


Так как `GetProductsByCategoryID(categoryID)` метод принимает входной параметр (*`categoryID`*), Выбор источника данных мастер дает возможность указать источник параметра. В качестве источника параметра строки запроса, с помощью QueryStringField `CategoryID`.


[![Поле Querystring CategoryID использовать как источник параметра](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Рис. 5**: поле строки запроса `CategoryID` как источник параметра ([Просмотр полноразмерного изображения](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))


Как мы видели в предыдущих учебных курсах, после завершения работы мастера выберите источник данных, Visual Studio автоматически создает `ItemTemplate` со списком Имя поля данных и значение каждого элемента управления DataList. Замените этот шаблон, перечисляющий только название, поставщика и цену продукта. Кроме того, значение элемента управления DataList `RepeatColumns` значение 2. После внесения этих изменений декларативная разметка элементов управления DataList и ObjectDataSource должен выглядеть следующим образом:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Чтобы просмотреть эту страницу в действии, запустите в `CategoryListMaster.aspx` страница; затем щелкните ссылку в маркированном списке категорий. Таким образом вы перейдете на `ProductsForCategoryDetails.aspx`, передав вдоль `CategoryID` через строку запроса. `ProductsInCategoryDataSource` ObjectDataSource в `ProductsForCategoryDetails.aspx` затем будет получать только те продукты для указанной категории и их отображения в элементе управления DataList, который отображает два продукта на строку. Рис. 6 показан снимок экрана `ProductsForCategoryDetails.aspx` при просмотре напитков.


[![Отображаются напитков, два каждой строки](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Рис. 6**: «Напитки», отображаются, два каждой строки ([Просмотр полноразмерного изображения](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Шаг 4: Отображение сведений о категории на ProductsForCategoryDetails.aspx

Когда пользователь выбирает категорию в `CategoryListMaster.aspx`, они перешли `ProductsForCategoryDetails.aspx` и видит продукты, принадлежащие выбранной категории. Однако в `ProductsForCategoryDetails.aspx` нет никаких визуальных подсказок о том, какая категория выбрана. Пользователь, который хотели щелкнуть Beverages, но случайно щелкнули Специи не имеет возможности реализации свою ошибку, после достижения `ProductsForCategoryDetails.aspx`. Чтобы смягчить эту потенциальную проблему, мы можем отобразить сведения о выбранной категории — его имя и описание — в верхней части `ProductsForCategoryDetails.aspx` страницы.

Для этого добавьте FormView над элементом управления Repeater в `ProductsForCategoryDetails.aspx`. Добавьте новый элемент управления ObjectDataSource на страницу из смарт-тега FormView с именем `CategoryDataSource` и настройте его для использования `CategoriesBLL` класса `GetCategoryByCategoryID(categoryID)` метод.


[![Доступ к сведениям о категории через метод GetCategoryByCategoryID(categoryID) класса categoriesbll](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Рис. 7**: доступ к сведениям о категории через `CategoriesBLL` класса `GetCategoryByCategoryID(categoryID)` метод ([Просмотр полноразмерного изображения](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))


Как и в `ProductsInCategoryDataSource` ObjectDataSource добавлен на шаге 3, `CategoryDataSource`в мастер настройки источников данных запрашивает источник для `GetCategoryByCategoryID(categoryID)` входных параметров метода. Использовать одинаковые параметры что и раньше, установив источник параметра строки запроса и значение QueryStringField `CategoryID` (см. рис. 5).

После завершения работы мастера, Visual Studio автоматически создает `ItemTemplate`, `EditItemTemplate`, и `InsertItemTemplate` для FormView. Так как мы предлагаем интерфейс только для чтения, вы можете удалить `EditItemTemplate` и `InsertItemTemplate`. Кроме того, вы можете настроить FormView `ItemTemplate`. После удаления неиспользуемых шаблоны и настройка ItemTemplate, декларативная разметка FormView и ObjectDataSource должен выглядеть следующим образом:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

Рис. 8 показан экран, при просмотре этой страницы в обозревателе.

> [!NOTE]
> В дополнение к FormView, я также добавил элемент управления "Гиперссылка" выше FormView, который возвращает пользователя обратно к списку категорий (`CategoryListMaster.aspx`). Вы можете поместить эту ссылку в другом месте или пропустить.


[![— Сведения о категориях теперь отображается в верхней части страницы](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Рис. 8**: — сведения о категориях теперь отображается в верхней части страницы ([Просмотр полноразмерного изображения](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Шаг 5: Отображение сообщения при отсутствии продуктов в выбранной категории

`CategoryListMaster.aspx` Странице перечислены все категории в системе, независимо от них связанных продуктов. Если пользователь щелкает категорию с не связано никаких продуктов, элемент управления DataList в `ProductsForCategoryDetails.aspx` не будет визуализирован, как его источник данных не будет иметь элементов. Как мы видели в предыдущих учебных курсах, элемент GridView предоставляет `EmptyDataText` свойство, которое может использоваться для указания текстового сообщения, отображаемое, если нет записей в источнике данных. К сожалению ни элемент управления DataList, ни элемент управления Repeater не имеют такого свойства.

Чтобы отобразить сообщение, информирующее пользователя, что отсутствуют соответствующие продукты для выбранной категории, нам нужно добавить метку элемент управления на страницу `Text` которого присваивается сообщение, отображаемое в случае, если отсутствуют соответствующие продукты. Затем необходимо программно задать его `Visible` значение в зависимости от того, является ли элемент управления DataList содержит какие-либо элементы.

Чтобы выполнить это, начнем с добавления метку под элемент управления DataList. Задайте его `ID` свойства `NoProductsMessage` и его `Text` значение «Нет ни одного продукта для выбранной категории...» Далее нам нужно программно задать эту метку `Visible` значение в зависимости от того, является ли все данные были привязаны к `ProductsInCategory` DataList. Это назначение должно выполняться после привязки данных к элементу управления DataList. GridView, DetailsView и FormView, мы создадим обработчик событий для элемента управления `DataBound` , запускающегося после привязки данных. Тем не менее, ни элемент управления DataList, ни Repeater имеет `DataBound` доступных событий.

В данном конкретном примере можно назначить метки `Visible` свойство в `Page_Load` обработчик событий, так как данные будут назначены DataList до того, как страница `Load` событий. Тем не менее этот подход не будет работать в общем случае, как данные из элемента управления ObjectDataSource может быть привязан к DataList позже в жизненном цикле страницы. Например, если отображаемых данных создается на основе значения в другом элементе управления, например при отображении отчета «основной/подробности» с помощью элемента управления DropDownList для размещения «основных» записей, данные могут не восстанавливаться Дата, указанная веб-управления данными до `PreRender` этап в жизненного цикла страницы.

Одно решение, которое будет работать для всех вариантов, — присвоить `Visible` свойства `False` в элементе управления DataList `ItemDataBound` (или `ItemCreated`) при привязке типа элементов `Item` или `AlternatingItem`. В этом случае мы знаем, что данные по крайней мере один элемент в источнике данных и таким образом можно скрыть `NoProductsMessage` метки. Помимо этого обработчика событий, необходимо также обработчик событий для элемента управления DataList `DataBinding` событий, где мы инициализируем метки `Visible` свойства `True`. Так как `DataBinding` возникает прежде событий `ItemDataBound` события, метки `Visible` будет изначально установлено `True`; Если существуют элементы данных, тем не менее, он будет присвоено `False`. Следующий код реализует эту логику:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Все категории в базе данных Northwind связаны с одного или нескольких продуктов. Чтобы протестировать эту функцию, я Ручная Подстройка базы данных Northwind для этого руководства переназначение всех продуктов, связанных с этой категории (`CategoryID` = 7) в категорию Seafood (`CategoryID` = 8). Это можно сделать из обозревателя серверов, выбрав новый запрос и с помощью следующих `UPDATE` инструкции:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

После обновления базы данных соответствующим образом, вернитесь к `CategoryListMaster.aspx` странице, а затем щелкните ссылку создания. Так как не все продукты, принадлежащие к этой категории, вы увидите сообщение «Нет ни одного продукта для выбранной категории...», как показано на рис. 9.


[![Сообщение отображается в том случае, если нет продукты, принадлежащие к выбранной категории](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Рис. 9**: отображается сообщение, если нет продукты, принадлежащие к выбранной категории ([Просмотр полноразмерного изображения](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))


## <a name="summary"></a>Сводка

Хотя основные и подробные записи в основных/подробных отчетах могут отображаться на одной странице, на многих веб-сайтах они размещены на двух веб-страницах. В этом учебнике мы рассмотрели способы реализация такого отчета «основной/подробности» посредством перечисления категорий в маркированном списке, с помощью элемента управления Repeater на «основной» веб-странице и соответствующих продуктов на странице «Подробности». Каждый элемент списка в главной веб-странице содержит ссылку на страницу сведений, которая передается строки `CategoryID` значение.

На странице подробностей Получение продуктов для указанного поставщика достигалось посредством `ProductsBLL` класса `GetProductsByCategoryID(categoryID)` метод. *`categoryID`* Значение параметра было указано декларативно с помощью `CategoryID` значение строки запроса, как источник параметра. Мы также рассмотрели способы отображения сведений о категории на странице подробностей с помощью элемента FormView и отображения сообщения при отсутствии продуктов выбранной категории.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Джонс (Zack Jones) и (Liz Shulok), стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [Вперед](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
