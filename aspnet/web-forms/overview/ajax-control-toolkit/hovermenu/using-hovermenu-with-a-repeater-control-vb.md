---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Использование HoverMenu с элементом управления Repeater (VB) | Документация Майкрософт
author: wenz
description: 'Элемент управления HoverMenu в AJAX Control Toolkit предоставляет простое всплывающее окно эффект: при наведении указателя мыши на элемент, появится всплывающее окно в specifi...'
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a00d2c610696672a36ebbb78b80abec6c5150101
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821347"
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a>Использование HoverMenu с элементом управления Repeater (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> Элемент управления HoverMenu в AJAX Control Toolkit предоставляет простое всплывающее окно эффект: при наведении указателя мыши на элемент, появится всплывающее окно в указанной позиции. Можно также использовать этот элемент управления в элементе управления repeater.


## <a name="overview"></a>Обзор

`HoverMenu` Элемента управления в AJAX Control Toolkit предоставляет простое всплывающее окно эффект: при наведении указателя мыши на элемент, появится всплывающее окно в указанной позиции. Можно также использовать этот элемент управления в элементе управления repeater.

## <a name="steps"></a>Шаги

Во-первых источником данных является обязательным. В этом примере используется база данных AdventureWorks и Microsoft SQL Server 2005 Express Edition. Базы данных является необязательной частью установки Visual Studio (включая экспресс-выпуск), а также доступна для загрузки в разделе [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Базы данных AdventureWorks является частью образцы SQL Server 2005 и образцов баз данных (загрузить в [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Самый простой способ настроить базы данных является использование Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) и присоединения `AdventureWorks.mdf` файл базы данных.

В этом примере мы предполагаем, что экземпляр SQL Server 2005 Express Edition называется `SQLEXPRESS` и находится на одном компьютере с веб-сервера; это также настройки по умолчанию. Если настройки отличаются, вам нужно адаптировать сведения о соединении для базы данных.

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

Затем добавьте источник данных на страницу. Чтобы использовать ограниченный объем данных, мы выбрать только первые пять записей в таблице поставщиков базы данных AdventureWorks. Если вы используете этот помощник используется Visual Studio для создания источника данных, виду, что ошибки в текущей версии не префикса имени таблицы (`Vendor`) с `Purchasing`. В следующей разметке показан правильный синтаксис:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

Добавьте панель, который служит в качестве модального всплывающего окна:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

Теперь `HoverMenuExtender` вступает в действие. Таким образом, чтобы каждый элемент в источнике данных получает свой собственный контекстного меню, расширения должны быть размещены в элементе repeater `<ItemTemplate>` раздел. Ниже приводится пример разметки:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

Теперь каждый элемент в источнике данных отображает контекстное меню справа (`PopupPosition` атрибут) с задержкой в 50 мс (`PopDelay` атрибут).


[![Меню при наведении указателя мыши отображается рядом с каждым элементом в элементу управления repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

Меню при наведении указателя мыши отображается рядом с каждым элементом в элементу управления repeater ([Просмотр полноразмерного изображения](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-hovermenu-with-a-repeater-control-cs.md)
