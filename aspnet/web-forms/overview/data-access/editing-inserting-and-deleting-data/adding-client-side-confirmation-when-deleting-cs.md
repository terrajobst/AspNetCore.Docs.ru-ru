---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Добавление клиентского подтверждения при удалении (C#) | Документация Майкрософт
author: rick-anderson
description: В интерфейсы, которые мы создали ранее пользователь может случайно удалить данные, нажав кнопку Delete, Собираясь нажать кнопку "Изменить". В этом t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 7ad66e24198a83d32edeb9fddf7a6b648b993fcc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386566"
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>Добавление клиентского подтверждения при удалении (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачайте пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) или [скачать PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> В интерфейсы, которые мы создали ранее пользователь может случайно удалить данные, нажав кнопку Delete, Собираясь нажать кнопку "Изменить". В этом руководстве мы добавим диалоговое окно клиентского подтверждения, которое появляется при нажатии кнопки Delete.


## <a name="introduction"></a>Вступление

Через нескольких последних учебных курсах мы ve узнали, как использовать архитектуру приложения, ObjectDataSource и элементов управления в сочетании для предоставления Вставка, редактирование и удаление возможностей. Удаление интерфейсы мы ve проверены актуальная были состоит из удаления кнопка, при щелчке, вызывает обратную передачу и вызывает ObjectDataSource s `Delete()` метод. `Delete()` Вызывал настроенный метод из уровня бизнес-логики, который передавал вызов далее до уровня доступа к данным, выдавая собственно оператор `DELETE` инструкции к базе данных.

Хотя такой интерфейс пользователя позволяет посетителям удалять записи с помощью элементов управления GridView, DetailsView и FormView, отсутствует запрос на подтверждение при нажатии кнопки Delete. Если пользователь случайно нажимает кнопку «Удалить», когда они хотели щелкнуть изменение, вместо этого удаляется запись, которую он хотел обновить. Чтобы предотвратить это, в этом руководстве мы добавим диалоговое окно клиентского подтверждения, которое появляется при нажатии кнопки Delete.

JavaScript `confirm(string)` функция отображает свой строковый входной параметр как текст внутри модальное диалоговое окно, которое поставляется с двумя кнопками - OK и Отмена (см. рис. 1). `confirm(string)` Функция возвращает логическое значение в зависимости от того, какая кнопка нажата (`true`, если пользователь нажимает кнопку "ОК", и `false` при нажатии кнопки "Отмена").


![JavaScript confirm(string) метод отображает модальное окно Messagebox на стороне клиента](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Рис. 1**: JavaScript `confirm(string)` метод отображает модальное, клиентские Messagebox


При отправке формы, если значение `false` возвращается из обработчика событий на стороне клиента, а затем Отправка формы отменяется. Используя эту возможность, мы сможем удалить кнопку s клиентская `onclick` возвращать значение вызова `confirm("Are you sure you want to delete this product?")`. Если пользователь нажимает кнопку "Отмена", `confirm(string)` вернет false, тем самым вызывая к отмене отправки формы. При отсутствии обратной передачи удалить продукт, кнопки "Удалить" была нажата выиграл t. Если, тем не менее, пользователь нажимает кнопку ОК в диалоговом окне подтверждения, обратный вызов продолжится в полной мере, и продукт будет удален. Обратитесь к [s с помощью JavaScript `confirm()` метод для отправки формы элемент управления](http://www.webreference.com/programming/javascript/confirm/) Дополнительные сведения об этом приеме.

Добавление необходимых клиентский сценарий немного всего отличается в том случае, если с помощью шаблонов, чем при использовании CommandField. Таким образом в этом руководстве мы рассмотрим пример GridView и FormView.

> [!NOTE]
> Использование приемов подтверждения на стороне клиента, подобных описываемым в данном учебнике предполагается, что пользователи пользуются обозревателями, поддерживающими JavaScript, и что они имеют JavaScript включен. Если одно из этих предположений не выполняются для конкретного пользователя, нажав кнопку «Удалить» вызовет немедленную обратную передачу (а не отображение messagebox подтверждение).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Шаг 1: Создание элемента FormView, поддерживающего удаление

Начните с добавления элемента FormView к `ConfirmationOnDelete.aspx` странице в `EditInsertDelete` папки, привязав его к элементу управления ObjectDataSource, извлекает информацию о продукте через `ProductsBLL` класс s `GetProducts()` метод. Также настройте элемент ObjectDataSource, чтобы `ProductsBLL` класс s `DeleteProduct(productID)` относящимся ObjectDataSource s `Delete()` метода; убедитесь, что на вкладках INSERT и UPDATE, раскрывающиеся списки задаются (нет). Наконец установите флажок Enable Paging в смарт-тега FormView s.

После выполнения этих шагов новый декларативная разметка ObjectDataSource s будет выглядеть следующим образом:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Как и в наших предыдущих примерах, которые не использовали оптимистичный параллелизм, Отвлекитесь и очистите ObjectDataSource s `OldValuesParameterFormatString` свойство.

Так как он привязан к элемент управления ObjectDataSource, который поддерживает только удаление, FormView s `ItemTemplate` предлагает только кнопку удаления, кнопки «Создать и обновления», где отсутствует. Декларативная разметка FormView s, тем не менее, входят дополнительные шаблоны `EditItemTemplate` и `InsertItemTemplate`, которые можно удалить. Отвлекитесь и настроить `ItemTemplate` , чтобы он отображается только подмножество продукта полей данных. Я ve настроил свой на показ имя продукта s в `<h3>` над его имена поставщика и категории (вместе с «удалить»).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Благодаря этим изменениям у нас есть полнофункциональный веб-страницы, позволяющий пользователю переключаться с одного продукта за раз, с возможностью удалить продукт, просто нажав кнопку «Удалить». Рис. 2 показан снимок экрана ход работы до сих при просмотре через браузер.


[![FormView отображает информацию об одном продукте](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Рис. 2**: The FormView показывает сведения об одном продукте ([Просмотр полноразмерного изображения](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Шаг 2: Вызов функции confirm(string) из удаление кнопки клиентского события onclick

С FormView создан, последним шагом является настройка кнопки Delete таких что при его s нажатии посетителем вызывалась функция JavaScript `confirm(string)` функция вызывается. Добавление клиентского сценария в кнопку LinkButton и ImageButton s на стороне клиента `onclick` событий можно воспользоваться `OnClientClick property`, которое является новым для ASP.NET 2.0. Поскольку нам нужно возвращение значения `confirm(string)` Функция вернула, просто присвойте этому свойству значение: `return confirm('Are you certain that you want to delete this product?');`

После этого изменения декларативный синтаксис s Удалить LinkButton выглядит примерно так:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Все, что s — его! Рис. 3 показан снимок экрана для подтверждения в действии. Нажав кнопку «Удалить», появится диалоговое окно подтверждения. При нажатии кнопки "Отмена", то обратная передача отменяется, и продукт не удаляется. Если, однако пользователь нажимает кнопку ОК, обратная передача продолжается и ObjectDataSource s `Delete()` метод и завершается удалением записи базы данных удаляется.

> [!NOTE]
> Строка, переданная `confirm(string)` функцию JavaScript разделенное апострофами (вместо знаков кавычек). В JavaScript строки можно выделять, используя любой из этих знаков. Мы используем здесь апострофы, чтобы выделители строки передаваемого `confirm(string)` не создавали путаницы с выделителями, используемыми для `OnClientClick` значение свойства.


[![Подтверждение — теперь отображается при нажатии кнопки Delete](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Рис. 3**: подтверждение — теперь отображается при нажатии кнопки Delete ([Просмотр полноразмерного изображения](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Шаг 3: Настройка свойства OnClientClick для кнопки "Удалить" в поле CommandField

При работе с кнопки, LinkButton или ImageButton непосредственно в шаблоне, диалоговое окно подтверждения можно связать с ней, просто настроив его `OnClientClick` свойства для возврата результатов функции JavaScript `confirm(string)` функции. Тем не менее, не имеет CommandField, — который добавляет поле кнопок «Delete» к GridView или DetailsView - `OnClientClick` свойство, которое можно задать декларативно. Вместо этого нам нужно программно сослаться на кнопку удаления в соответствующие s GridView и DetailsView `DataBound` обработчик событий и установите его `OnClientClick` существует свойство.

> [!NOTE]
> При задании «удалить» s `OnClientClick` в соответствующем обработчике событий `DataBound` обработчик событий, у нас есть доступ к данных была привязана к текущей записи. Это означает, что мы можем расширить сообщение подтверждения для включения сведений о конкретной записи, например, «Вы действительно хотите удалить продукт Chai?» Такая настройка также возможна в шаблонах, использующих синтаксис привязки данных.


На практике параметр `OnClientClick` свойство для button(s) Delete в поле CommandField, let s Добавление GridView на страницу. Настройте этот GridView использовать тот же элемент управления ObjectDataSource, элемент FormView использует. Также можно ограничьте s GridView поля BoundField, кроме включать только s название продукта, категории и поставщика. Наконец установите флажок Разрешить удаление, в смарт-теге GridView s. Это добавит поле CommandField GridView s `Columns` коллекции с его `ShowDeleteButton` свойство значение `true`.

После внесения этих изменений декларативная разметка s GridView должен выглядеть следующим образом:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

CommandField содержит единственный экземпляр удалить LinkButton, может осуществляться программными средствами из GridView s `RowDataBound` обработчик событий. Как только ссылки, можно задать его `OnClientClick` свойство соответствующим образом. Создайте обработчик событий для `RowDataBound` событий, используя следующий код:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Этот обработчик событий работает со строками данных (те, которые будут иметь «удалить») и начинается с программного обращения «удалить». В целом используйте следующий шаблон:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* — это тип кнопки, используемый CommandField - кнопки LinkButton и ImageButton. По умолчанию CommandField использует элементов управления LinkButton, но его можно настроить с помощью CommandField s `ButtonType property`. *CommandFieldIndex* — это порядковый индекс CommandField в GridView s `Columns` коллекции, тогда как *controlIndex* — это индекс кнопки удаления в CommandField s `Controls` коллекции. *ControlIndex* значение зависит от положения кнопки s по отношению к другим кнопкам в CommandField. Например если единственной кнопкой, отображаемой в CommandField «удалить», используйте индекс 0. Если, однако здесь s кнопка «изменить», который предшествует «удалить», используйте индекс 2. Используется индекс 2, так как были добавлены два элемента управления с CommandField перед "Удалить": "Изменить" и LiteralControl s используется для добавления некоторого пространства между кнопками Edit и Delete.

В нашем конкретном примере CommandField использует элементов управления LinkButton и, что поле слева имеет *commandFieldIndex* 0. Так как других кнопок, но CommandField «удалить», мы используем *controlIndex* 0.

После установки ссылки на кнопку удаления в CommandField, мы записываем информацию о продукте, привязанном к текущей строке GridView. Наконец, мы устанавливаем «удалить» s `OnClientClick` свойству, соответствующему JavaScript, который включает название продукта s. Поскольку строка JavaScript, переданная `confirm(string)` функция отделена с помощью апострофы, следует избегать апострофами, апострофов в имени продукта s. В частности, любые апострофы в имени продукта s экранируются с помощью "`\'`«.

По завершении этих изменений, нажав кнопку «Удалить» отображает GridView, настроенное диалоговое окно подтверждения (см. рис. 4). Как с messagebox подтверждения из FormView, если пользователь нажимает кнопку "Отмена" обратная отправка отменяется, тем самым предотвращая удаление.

> [!NOTE]
> Этот метод также может использоваться для программного доступа к «удалить» CommandField в элементе управления DetailsView. Для элемента DetailsView, однако d создать обработчик событий для `DataBound` событий, так как нет DetailsView `RowDataBound` событий.


[![Нажав кнопку Delete s GridView отображает настроенное диалоговое окно подтверждения](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Рис. 4**: щелчке s GridView кнопки Delete отображается диалоговое окно подтверждения настройки ([Просмотр полноразмерного изображения](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>Использование полей TemplateField

Одним из недостатков CommandField является, что его кнопки должен осуществляться через индексирование и что получившийся объект должен быть приведен к типу соответствующие кнопки (кнопки LinkButton и ImageButton). Использование «магических чисел» и жестко закодированных типов приводит к проблемам, которые не могут быть обнаружены только во время выполнения. Например если вы или другой разработчик добавляет новые кнопки CommandField в какой-то момент в будущем (например, кнопка «Изменить») или изменения `ButtonType` свойство, существующий код по-прежнему будет компилироваться без ошибок, но посещение страницы может вызвать исключение или неожиданному поведению в зависимости от того, как был написан код и какие изменения были сделаны.

Альтернативным подходом является преобразование s CommandFields GridView и DetailsView в поля TemplateField. При этом будут созданы TemplateField с `ItemTemplate` , содержащий LinkButton (или кнопки или ImageButton) для каждой кнопки в CommandField. Эти кнопки `OnClientClick` можно назначать декларативно, как мы показали в FormView, или можно получить доступ программными средствами в соответствующий `DataBound` обработчик событий, используя следующий шаблон:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Где *controlID* является значением кнопки s `ID` свойство. Хотя этот шаблон по-прежнему требуется жестко тип для приведения, он избавляет от необходимости для индексирования, позволяя макет, чтобы изменить, не приводит к ошибке времени выполнения.

## <a name="summary"></a>Сводка

JavaScript `confirm(string)` функция является распространенным приемом для управления рабочего процесса отправки формы. При выполнении, функция отображает модальное, клиентские диалоговое окно, которое включает в себя две кнопки OK и Отмена. Если пользователь нажимает кнопку "ОК", `confirm(string)` возвращает `true`; нажмите кнопку "Отмена" возвращает `false`. Эта функция в сочетании с поведения браузера s для отмены отправки форм, если обработчик события при отправке возвращает `false`, можно использовать для отображения messagebox подтверждения при удалении записи.

`confirm(string)` Функции могут быть связаны с клиентской стороны элемента управления s кнопку Web `onclick` обработчик событий с помощью элемента управления s `OnClientClick` свойство. При работе с помощью кнопки удаления в шаблоне — либо в один из шаблонов FormView s либо в поле TemplateField в DetailsView и GridView - это свойство можно задать декларативно или программно, как мы видели в этом руководстве.

Счастливого вам программирования!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](implementing-optimistic-concurrency-cs.md)
> [Вперед](limiting-data-modification-functionality-based-on-the-user-cs.md)
