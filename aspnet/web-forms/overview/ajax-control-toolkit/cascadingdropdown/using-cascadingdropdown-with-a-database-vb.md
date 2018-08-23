---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Использование CascadingDropDown с базой данных (Visual Basic) | Документация Майкрософт
author: wenz
description: Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 98e07764a3bd6afc8045221e9c016e57be44f5f7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835767"
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>Использование CascadingDropDown с базой данных (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList. В порядке, чтобы это работало должны создаваться специальных веб-службы.


## <a name="overview"></a>Обзор

Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList. (К примеру, один список содержит список штатов США, и следующий список заполняется крупнейших городов в этом состоянии.) В порядке, чтобы это работало должны создаваться специальных веб-службы.

## <a name="steps"></a>Шаги

Во-первых источником данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks является частью образцы SQL Server 2005 и образцов баз данных (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных является использование Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию. Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных.

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в &lt; `form` &gt; элемента):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

На следующем шаге необходимы два элемента управления DropDownList. В этом примере мы используем поставщику и контактной информации из AdventureWorks, таким образом мы создадим один список доступных поставщиков и доступных контактов:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Затем необходимо добавить два расширителей CascadingDropDown на страницу. Одно заполняет первый список (поставщики), а другой заполняет второй список (контакты). Необходимо задать следующие атрибуты:

- `ServicePath`: URL-адрес веб-службы, предоставляя элементов списка
- `ServiceMethod`: Метод веб-доставка элементов списка
- `TargetControlID`: Идентификатор из раскрывающегося списка
- `Category`: Категории сведений, переданных при вызове веб-метода
- `PromptText`: Текст, отображаемый при асинхронной загрузкой данных с сервера
- `ParentControlID`: (необязательно) родительского раскрывающемся списке, что загрузка триггеры текущего списка

В зависимости от используемого языка программирования изменяет имя веб-службы в вопросе, но все остальные значения атрибута совпадают. Ниже приведен элемент CascadingDropDown для первого раскрывающегося списка.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Нужно задать управляющих элементов-расширителей для второй список `ParentControlID` таким образом, выберите соответствующую запись в триггерах списка поставщиков загрузке связанных элементов в списке контактов.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Фактическая работа осуществляется в веб-службы, который настраивается следующим образом. Обратите внимание, что `[ScriptService]` используется атрибут, в противном случае AJAX для ASP.NET не удается создать прокси JavaScript для доступа к веб-методы из кода клиентского сценария.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

Подпись веб-методов, вызванных CascadingDropDown выглядит следующим образом:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Поэтому возвращаемое значение должно быть массивом типа `CascadingDropDownNameValue` которого определяется набор элементов управления. `GetVendors()` Метод довольно прост в реализации: код подключается к базе данных AdventureWorks и запрашивает поставщиков первые 25. Первый параметр в `CascadingDropDownNameValue` конструктор будет ее величина заголовок элемента списка, второй (значение атрибута в формате HTML &lt; `option` &gt; элемент). Ниже приведен код:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Получение связанных контактов для поставщика (имя метода: `GetContactsForVendor()`) немного сложнее. Во-первых необходимо определить поставщика, который был выбран в первом раскрывающемся списке. Набор средств управления определяет вспомогательный метод для выполнения этой задачи: `ParseKnownCategoryValuesString()` возвращает `StringDictionary` элемент, содержащий данные раскрывающегося списка:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

По соображениям безопасности необходимо сначала проверить эти данные. Таким образом, если имеется запись поставщика (поскольку `Category` первого элемента CascadingDropDown свойству `"Vendor"`), можно извлечь идентификатор выбранного поставщика:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Остальная часть метода — довольно очевидно, затем. Идентификатор поставщика используется в качестве параметра для SQL-запрос, который извлекает все связанные контакты для этого поставщика. Опять же, метод возвращает массив объектов типа `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Загрузки страницы ASP.NET, и через некоторое время короткий список поставщиков заполняется 25 записей. Выберите одну запись и обратите внимание на то, каким образом второго раскрывающегося списка заполняется данными.


[![Первый список заполняется автоматически](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

Первый список заполняется автоматически ([Просмотр полноразмерного изображения](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![Второй список заполняется согласно выбору, в первом списке](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Второй список заполняется согласно выбору, в первом списке ([Просмотр полноразмерного изображения](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Назад](filling-a-list-using-cascadingdropdown-vb.md)
> [Вперед](presetting-list-entries-with-cascadingdropdown-vb.md)
