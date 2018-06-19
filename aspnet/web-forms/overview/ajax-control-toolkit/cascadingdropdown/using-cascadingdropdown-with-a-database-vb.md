---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: С помощью CascadingDropDown с базой данных (Visual Basic) | Документы Microsoft
author: wenz
description: CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 4fd5d2b28540f6e2f751da9fb9d0df5f9995c20b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873590"
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>С помощью CascadingDropDown с базой данных (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList. В порядке, чтобы это работало должны создаваться специальные веб-службы.


## <a name="overview"></a>Обзор

CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList. (Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) В порядке, чтобы это работало должны создаваться специальные веб-службы.

## <a name="steps"></a>Шаги

Во-первых источник данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна как отдельный загружаемый в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks входит в состав образцов баз данных и образцы SQL Server 2005 (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию. Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в &lt; `form` &gt; элемент):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

На следующем шаге необходимы два элемента управления DropDownList. В этом примере мы используем поставщику и контактной информации из AdventureWorks, таким образом создается один список доступных поставщиков и доступных контактов:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Затем необходимо добавить два расширителей CascadingDropDown на страницу. Одно заполняет в первом списке (поставщики), а другой заполняет список второй (контактов). Необходимо задать следующие атрибуты:

- `ServicePath`: URL-адрес веб-службы доставки элементов списка
- `ServiceMethod`: Веб-метода доставки элементов списка
- `TargetControlID`: Идентификатор из раскрывающегося списка
- `Category`: Категории сведений, переданных при вызове веб-метода
- `PromptText`: Текст, отображаемый при асинхронной загрузке данных списка с сервера
- `ParentControlID`: (необязательно) родительского раскрывающегося списка, загрузкой триггеры текущего списка

В зависимости от того, используется язык программирования имя веб-службы в вопросе изменяется, но все значения атрибутов совпадают. Вот CascadingDropDown элемент для первого раскрывающегося списка.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Расширения элемента управления для второго, необходимо указать `ParentControlID` таким образом, чтобы при выборе записи в триггерах список поставщиков загрузке связанных элементов в списке контактов.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Фактические трудозатраты, затем выполняется в веб-службы, который настраивается следующим образом. Обратите внимание, что `[ScriptService]` используется атрибут, в противном случае ASP.NET AJAX не удается создать прокси-сервера JavaScript для доступа к веб-методов из кода клиентского скрипта.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

Подпись веб-методов, вызываемых CascadingDropDown выглядит следующим образом:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Поэтому возвращаемое значение должно быть массивом типа `CascadingDropDownNameValue` определяется набором средств управления. `GetVendors()` Метод довольно просто реализовать: код подключается к базе данных AdventureWorks и запрашивает первые 25 поставщиков. Первый параметр в `CascadingDropDownNameValue` конструктор имеет заголовок в списке записи второго его значение (значение атрибута в формате HTML &lt; `option` &gt; элемент). Ниже приведен код:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Получение связанного контактов для поставщика (имя метода: `GetContactsForVendor()`) немного сложнее. Во-первых необходимо определить поставщика, который был выбран в первом раскрывающемся списке. Определяет набор элементов управления вспомогательный метод для этой задачи: `ParseKnownCategoryValuesString()` возвращает `StringDictionary` элемент, содержащий данные раскрывающегося списка:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

По соображениям безопасности необходимо сначала проверить эти данные. Таким образом, если имеется запись поставщика (поскольку `Category` первого элемента CascadingDropDown свойству `"Vendor"`), идентификатор выбранного поставщика может быть получен:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Остальная часть метода — довольно прост, затем. Идентификатор поставщика используется в качестве параметра для SQL-запрос, который получает все связанные контакты для данного поставщика. Опять же, метод возвращает массив объектов типа `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Загрузить страницу ASP.NET и через короткое время заполнения списка поставщиков с 25 записей. Выберите одну запись и обратите внимание на то, как второй раскрывающийся список заполняется данными.


[![Первый список заполняется автоматически](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

Первый список заполняется автоматически ([Просмотр полноразмерное изображение](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![Второй список заполняется согласно выбору, в первом списке](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Второй список заполняется согласно выбору, в первом списке ([Просмотр полноразмерное изображение](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Назад](filling-a-list-using-cascadingdropdown-vb.md)
> [Вперед](presetting-list-entries-with-cascadingdropdown-vb.md)
