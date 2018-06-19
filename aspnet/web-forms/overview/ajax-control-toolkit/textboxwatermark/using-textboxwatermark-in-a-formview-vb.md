---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: С помощью TextBoxWatermark в FormView (VB) | Документы Microsoft
author: wenz
description: Элемент управления TextBoxWatermark в наборе элементов управления AJAX расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, оно я...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 5789ccc4b2c6385f3476857dce139a8b47c5e5e8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872537"
---
<a name="using-textboxwatermark-in-a-formview-vb"></a>С помощью TextBoxWatermark в FormView (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> Элемент управления TextBoxWatermark в наборе элементов управления AJAX расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, оно будет очищено. Если поле остается без ввода текста, заданными текст снова появится. Это также возможно в элементе управления FormView.


## <a name="overview"></a>Обзор

`TextBoxWatermark` В наборе элементов управления AJAX, расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, оно будет очищено. Если поле остается без ввода текста, заданными текст снова появится. Это также можно сделать в `FormView` элемента управления.

## <a name="steps"></a>Шаги

Во-первых источник данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна как отдельный загружаемый в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks входит в состав образцов баз данных и образцы SQL Server 2005 (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию. Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

Добавьте источник данных на страницу, которая поддерживает `DELETE`, `INSERT` и `UPDATE` инструкции SQL. При использовании помощника по Visual Studio для создания источника данных, помните, что ошибки в текущей версии не предшествует имени таблицы (`Vendor`) с `Purchasing`. В следующем примере показана правильный синтаксис:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

Запомните имя (`ID`) источника данных, так как он будет использоваться в `DataSourceID` свойства `FormView` элемента управления. `<InsertItemTemplate>` Раздел `FormView` содержится текстовое поле, который расширяется за счет `TextBoxWatermarkExtender` элемента управления. Убедитесь, что расширения находится внутри шаблона, но не вне его.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

Теперь при изменении пользователем в режим вставки из `FormView` управления текстового поля для нового поставщика автоматически заполняемом благодаря `TextBoxWatermarkExtender` элемента управления. Щелкните внутри текстового поля позволяет текста наполнения исчезают.


[![Водяной знак в поле поступают из расширения](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

Водяной знак в поле поступают из расширения ([Просмотр полноразмерное изображение](using-textboxwatermark-in-a-formview-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-textboxwatermark-with-validation-controls-cs.md)
> [Вперед](using-textboxwatermark-with-validation-controls-vb.md)
