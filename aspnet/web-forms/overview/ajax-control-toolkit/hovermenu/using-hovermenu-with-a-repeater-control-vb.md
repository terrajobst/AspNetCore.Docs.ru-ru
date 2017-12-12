---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: "Использование HoverMenu с элементом управления повторителем (VB) | Документы Microsoft"
author: wenz
description: "HoverMenu элемент управления в наборе элементов управления AJAX предоставляет эффекта простое всплывающее окно: при наведении указателя мыши над элементом, появится всплывающее окно с specifi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 77759afe00f341e15ee8e0f52a469d3b7c08ea89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a>Использование HoverMenu с элементом управления повторителем (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> HoverMenu элемент управления в наборе элементов управления AJAX предоставляет эффекта простое всплывающее окно: при наведении указателя мыши над элементом, появится всплывающее окно в указанной позиции. Можно также использовать этот элемент управления в пределах повторитель.


## <a name="overview"></a>Обзор

`HoverMenu` Элемент управления в наборе элементов управления AJAX предоставляет эффекта простое всплывающее окно: при наведении указателя мыши над элементом, появится всплывающее окно в указанной позиции. Можно также использовать этот элемент управления в пределах повторитель.

## <a name="steps"></a>Шаги

Во-первых источник данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна как отдельный загружаемый в разделе [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks входит в состав образцов баз данных и образцы SQL Server 2005 (загрузить в [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных будет использовать Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

Для данного образца предполагается, что экземпляр SQL Server 2005 Express Edition вызывается `SQLEXPRESS` и находится на том же компьютере, что веб-сервер; Кроме того, это настройки по умолчанию. Если настройки отличаются, необходимо адаптировать сведения о соединении для базы данных.

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

Затем добавьте источник данных на страницу. Чтобы использовать ограниченный объем данных, мы выбрать только первых пяти записей в таблице поставщиков базы данных AdventureWorks. При использовании помощника по Visual Studio для создания источника данных, помните, что ошибки в текущей версии не предшествует имени таблицы (`Vendor`) с `Purchasing`. В следующем примере показана правильный синтаксис:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

Добавьте панель, который служит в качестве модальное окно.

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

Теперь `HoverMenuExtender` вступает в действие. Так, что каждый элемент в источнике данных получит собственную всплывающее окно, расширения могут быть помещены в повторителя `<ItemTemplate>` раздела. Ниже приводится пример разметки:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

Теперь каждый элемент в источнике данных отображает всплывающее окно справа (`PopupPosition` атрибут) с задержкой 50 мс (`PopDelay` атрибута).


[![Меню при наведении указателя мыши отображается рядом с каждым элементом в повторителе](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

Меню при наведении указателя мыши отображается рядом с каждым элементом в повторителе ([Просмотр полноразмерное изображение](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](using-hovermenu-with-a-repeater-control-cs.md)
