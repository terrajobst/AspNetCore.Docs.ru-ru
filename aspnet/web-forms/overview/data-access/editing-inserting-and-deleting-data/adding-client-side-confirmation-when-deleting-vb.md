---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: Добавление клиентского подтверждения при удалении (VB) | Документы Microsoft
author: rick-anderson
description: В интерфейсах, созданных таким образом пользователь может случайно удалить данные, нажав кнопку «Удалить» не нажмите кнопку "Изменить". В этом t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 03ab3f9974bca7c3e08b8d3fa6fd4fc786ebed4d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880145"
---
<a name="adding-client-side-confirmation-when-deleting-vb"></a>Добавление клиентского подтверждения при удалении (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить пример приложения](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe) или [скачать PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> В интерфейсах, созданных таким образом пользователь может случайно удалить данные, нажав кнопку «Удалить» не нажмите кнопку "Изменить". В этом руководстве мы добавим диалоговое окно подтверждения на стороне клиента, появляется, когда нажата кнопка "Удалить".


## <a name="introduction"></a>Вступление

За последние несколько учебников мы хранять показано, как использовать архитектуру приложения, ObjectDataSource и элементов управления в сочетании для предоставления вставку, изменение и удаление возможностей. Удаление интерфейсы мы хранять проверяется до сих были состоит из инструкции Delete кнопка, при щелчке, посылающий запрос и вызывает ObjectDataSource s `Delete()` метод. `Delete()` Вызывал настроенный метод из уровня бизнес-логики, который передавал вызов делегируется уровня доступа к данным, выдавая собственно `DELETE` инструкции в базу данных.

Хотя этот пользовательский интерфейс позволяет посетителям удалять записи с помощью элементов управления GridView, DetailsView и FormView, не имеет каких-либо подтверждения при нажатии кнопки «Удалить». Если пользователь случайно нажимает кнопку «Удалить», если они предназначены для нажмите кнопку Изменить, запись, которую они предназначены для обновления, вместо этого следует удалять. Чтобы предотвратить это, в этом учебнике мы добавим диалоговое окно подтверждения на стороне клиента, появляется, когда нажата кнопка "Удалить".

JavaScript `confirm(string)` функция отображает свой строковый входной параметр как текст внутри модальное диалоговое окно, оснащен две кнопки - OK и Отмена (см. рис. 1). `confirm(string)` Функция возвращает логическое значение в зависимости от того, какая кнопка нажата (`true`, при нажатии кнопки "ОК", и `false` при нажатии кнопки "Отмена").


![JavaScript confirm(string) метод отображает модальное окно, Messagebox на стороне клиента](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**Рис. 1**: JavaScript `confirm(string)` метод отображает модальное, клиентские Messagebox


При отправке формы, если значение `false` возвращается из обработчика событий на стороне клиента, то отменяется отправки формы. Используя эту функцию, можно заставить удалить кнопку s на стороне клиента `onclick` обработчик события возврата вызова `confirm("Are you sure you want to delete this product?")`. Если пользователь нажимает кнопку "Отмена", `confirm(string)` возвращает false, тем самым, что приводит к отмене отправки формы. При отсутствии обратной передачи продукт выиграл t была нажата кнопка "Удалить", удаляются. Если, тем не менее, пользователь нажимает кнопку OK в диалоговом окне подтверждения, продолжится в полной мере обратной передачи и продукт будет удален. Обратитесь к [s с помощью JavaScript `confirm()` способ отправки формы управления](http://www.webreference.com/programming/javascript/confirm/) Дополнительные сведения об этом приеме.

Немного отличается добавление необходимых клиентского скрипта, если с помощью шаблонов, чем при использовании CommandField. Таким образом в этом учебнике мы рассмотрим пример GridView и FormView.

> [!NOTE]
> С помощью приемов подтверждения на стороне клиента, как описано в этом учебнике предполагается, что пользователи пользуются с браузерами, поддерживающими JavaScript, и что JavaScript включен. Если одно из этих предположений не выполняются для конкретного пользователя, нажав кнопку «Удалить» немедленно вызовет обратную передачу (а не отображение messagebox подтверждение).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Шаг 1: Создание элемента FormView, который поддерживает удаление

Начните с добавления FormView для `ConfirmationOnDelete.aspx` страницы в `EditInsertDelete` папки, привязав его к новой ObjectDataSource, который извлекает сведения о продукте через `ProductsBLL` класса s `GetProducts()` метод. Также настройте элемент управления ObjectDataSource, чтобы `ProductsBLL` класса s `DeleteProduct(productID)` был сопоставлен ObjectDataSource s `Delete()` метода; убедитесь, что на вкладках INSERT и UPDATE, раскрывающиеся списки задаются (нет). И, наконец установите флажок Включить постраничный просмотр в FormView s смарт-тег.

После выполнения этих шагов новый декларативная разметка ObjectDataSource s будет выглядеть следующим образом:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

Как в наших предыдущих примерах, не использующих оптимистичного параллелизма, пора очистить ObjectDataSource s `OldValuesParameterFormatString` свойство.

Так как оно привязано к элемент управления ObjectDataSource, который поддерживает только удаление FormView s `ItemTemplate` предлагает только «удалить», отсутствием кнопки Создать и обновления. Декларативная разметка FormView s, однако включает дополнительные шаблоны `EditItemTemplate` и `InsertItemTemplate`, которые можно удалить. Занять некоторое время для настройки `ItemTemplate` , чтобы он отображает только подмножество продукта поля данных. Я настроил хранить свой на название продукта s в `<h3>` заголовок над свои имена поставщика и категории (вместе с «удалить»).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

С этими изменениями у нас есть полнофункциональную веб-страницы, позволяющий пользователю переключаться с одного продукта за раз возможность удалить продукт, просто нажав кнопку «Удалить». Рис. 2 показан снимок экрана ход работы сих при просмотре через браузер.


[![FormView отображает информацию об одном продукте](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**На рисунке 2**: FormView показывает сведения об одном продукте ([Просмотр полноразмерное изображение](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Шаг 2: Вызов confirm(string) функции из onclick удаление кнопки клиентских событий

С FormView создан, последний шаг — Настройка кнопка удаления таких что при ее щелчке s посетителем JavaScript `confirm(string)` функция, вызываемая. Добавление клиентского скрипта для кнопки, LinkButton или ImageButton s на стороне клиента `onclick` событий могут быть выполнены использования `OnClientClick property`, которое является новым для ASP.NET 2.0. Поскольку мы хотим использовать значение `confirm(string)` Функция вернула, просто присвойте этому свойству значение: `return confirm('Are you certain that you want to delete this product?');`

После этого изменения удалить LinkButton s должен выглядеть примерно так:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

Все, что s — его! На рисунке 3 показана снимок экрана подтверждения в действии. Нажав кнопку «Удалить» появится диалоговое окно подтверждения. При нажатии кнопки "Отмена", обратная передача отменяется, и продукт не удаляется. Если, однако пользователь нажимает кнопку ОК, продолжает обратной передачи и ObjectDataSource s `Delete()` метод вызван одновременной записи базы данных удаляются.

> [!NOTE]
> Строка, передаваемая в `confirm(string)` функцию JavaScript разделенное апострофы (вместо кавычек). В JavaScript строки можно выделять, используя любой символ. Мы используем здесь апострофы, чтобы передавать разделителей для строки в `confirm(string)` внести неоднозначность с разделителям, использованным для `OnClientClick` значение свойства.


[![Подтверждение — теперь отображается при нажатии кнопки Delete](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**Рис. 3**: подтверждение — теперь отображается при нажатии кнопки Delete ([Просмотр полноразмерное изображение](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Шаг 3: Настройка OnClientClick свойство «удалить» в CommandField

При работе с кнопки, LinkButton или ImageButton непосредственно в шаблоне, диалоговое окно подтверждения можно связать с ним просто настроив его `OnClientClick` свойство для возврата результатов JavaScript `confirm(string)` функции. Тем не менее, не CommandField, - которой добавляется поле кнопок Delete GridView и DetailsView - `OnClientClick` свойство, которое может быть задано декларативно. Вместо этого мы программным образом сделать ссылку «Удалить» в соответствующие s GridView и DetailsView `DataBound` обработчик событий, а затем установите его `OnClientClick` существует свойство.

> [!NOTE]
> При задании «удалить» s `OnClientClick` в соответствующем обработчике событий `DataBound` обработчика событий, у нас есть доступ к данные, привязанные к текущей записи. Это означает, что мы можем расширить сообщение подтверждения, чтобы включить сведения о конкретной записи, такие как «Вы действительно хотите удалить продукт Chai?» Такие настройки можно также в шаблонах, используя синтаксис привязки данных.


На практике параметр `OnClientClick` свойство button(s) Delete в CommandField, а s позволяют добавить GridView на страницу. Настройте этот GridView использовать тот же элемент управления ObjectDataSource, использующий FormView. Также можно ограничьте s GridView стояли включают только название продукта s, категорию и поставщика. Наконец установите флажок Разрешить удаление, смарт-теге s GridView. Это добавит CommandField GridView s `Columns` коллекции с его `ShowDeleteButton` свойство `true`.

После внесения этих изменений декларативная разметка s GridView должен выглядеть следующим образом:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

CommandField содержит единственный экземпляр LinkButton удалить, можно получить программным образом из GridView s `RowDataBound` обработчика событий. Как только ссылки, можно задать его `OnClientClick` свойство соответствующим образом. Создайте обработчик событий для `RowDataBound` событие, используя следующий код:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

Этот обработчик событий работает со строками данных (те, которые будут иметь «удалить») и начинается с программного обращения «удалить». В целом следует используете следующий шаблон:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType* тип кнопки, используемый CommandField - Button, LinkButton или ImageButton. По умолчанию CommandField использует элементов управления LinkButton, но это можно настроить через CommandField s `ButtonType property`. *CommandFieldIndex* – это порядковый индекс CommandField в GridView s `Columns` коллекции, в то время как *controlIndex* — это индекс «удалить» в CommandField s `Controls` коллекции. *ControlIndex* значение зависит от положения кнопки s относительно других кнопок в CommandField. Например если только отображаются в CommandField кнопка «Удалить», используйте индекс 0. Если, однако отсутствует s кнопка "Изменить", который предшествует «удалить» используйте индекс 2. Использовать индекс 2, так как были добавлены два элемента управления с CommandField перед «удалить»: кнопка "Изменить" и LiteralControl s используется для добавления некоторого пространства между кнопками Edit и Delete.

В нашем конкретном примере CommandField использует элементов управления LinkButton и выполняется поле слева имеет *commandFieldIndex* 0. Поскольку существует, но не другие кнопки «Удалить» в CommandField, мы используем *controlIndex* 0.

После установки ссылки на «удалить» в CommandField, мы записываем информацию о продукте, привязанном к текущей строке GridView. Наконец, мы устанавливаем кнопка "Удалить" s `OnClientClick` свойство к соответствующему JavaScript, который содержит имя продукта s. Поскольку JavaScript строка, передаваемая в `confirm(string)` функция отделена с помощью следует избегать апострофами, находящихся в имени продукта s апострофы. В частности, любые апострофы в имени продукта s предваряются символом «`\'`».

По завершении этих изменений, нажав на кнопку «Удалить» в GridView показывает настроенные диалоговое окно подтверждения (см. Рисунок 4). Как с messagebox подтверждения из FormView, при нажатии кнопки "Отмена" обратная передача отменяется, чтобы предотвратить удаление.

> [!NOTE]
> Этот метод может также использоваться для программного доступа к «удалить» в CommandField в DetailsView. Для DetailsView, однако d создать обработчик событий для `DataBound` события, поскольку отсутствует DetailsView `RowDataBound` событий.


[![При нажатии кнопки Delete s GridView отображается диалоговое окно подтверждения настраиваемые](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**Рис. 4**: щелчок s GridView кнопку «Удалить» выводит диалоговое окно настройки подтверждения ([Просмотр полноразмерное изображение](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))


## <a name="using-templatefields"></a>С помощью TemplateFields

Один из недостатков CommandField — что ее кнопок должен осуществляться через индексирование и что полученный объект должен быть приведен к типу соответствующие кнопки (Button, LinkButton или ImageButton). С помощью «магических чисел» и жестко типов приводит к проблемам, которые не могут быть обнаружены до времени выполнения. Например если вы или другой разработчик добавляет новые кнопки CommandField, на какой-то момент в будущем (например, кнопку «Изменить») или изменения `ButtonType` свойство, существующий код по-прежнему компилируется без ошибок, но посетив страницу возможно возникновение исключения или непредвиденное поведение, в зависимости от того, как ваш код был написан и какие изменения были сделаны.

Альтернативный подход — преобразование GridView и DetailsView s CommandFields в TemplateFields. Это создаст TemplateField с `ItemTemplate` имеется LinkButton (или кнопка или ImageButton) для каждой кнопки в CommandField. Эти кнопки `OnClientClick` свойств можно назначить декларативно, как мы показали FormView или может осуществляться программными средствами в соответствующую `DataBound` обработчика событий, используя следующий шаблон:


[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

Где *controlID* значение кнопки s `ID` свойство. При этом шаблоне по-прежнему требуется тип жестко для приведения, он избавляет от необходимости индексирования, позволяя макета можно изменять, не приведет к ошибке времени выполнения.

## <a name="summary"></a>Сводка

JavaScript `confirm(string)` функция является распространенным приемом управляющего потока отправки формы. Во время выполнения функция отображает модальное, клиентские диалоговым окном, которое включает в себя две кнопки OK и Отмена. При нажатии кнопки "ОК", `confirm(string)` возврата функцией `true`; нажмите кнопку "Отмена" возвращает `false`. Эта функция вместе с поведения отмены отправки форм, если обработчик события при отправке возвращает в браузер s `false`, можно использовать для отображения messagebox подтверждение при удалении записи.

`confirm(string)` Функции могут быть связаны с Web кнопки управления s на стороне клиента `onclick` обработчик событий с помощью элемента управления s `OnClientClick` свойство. При работе с кнопку «Удалить» в шаблоне — либо в один из шаблонов s FormView либо в TemplateField в DetailsView или GridView - это свойство можно задать декларативно или программно, как было показано в этом учебнике.

Программирование довольны!

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Назад](implementing-optimistic-concurrency-vb.md)
> [Вперед](limiting-data-modification-functionality-based-on-the-user-vb.md)
