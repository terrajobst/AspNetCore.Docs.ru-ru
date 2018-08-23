---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Предустановка записей списка с помощью с CascadingDropDown (C#) | Документация Майкрософт
author: wenz
description: Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 2599b5015f288b2e8d02577a0865252a862574a4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836000"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Предустановка записей списка с помощью с CascadingDropDown (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList. Используя небольшой фрагмент кода вполне возможно, что элемент списка выбран, когда данные динамически загружены.


## <a name="overview"></a>Обзор

Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList. (К примеру, один список содержит список штатов США, и следующий список заполняется крупнейших городов в этом состоянии.) Используя небольшой фрагмент кода вполне возможно, что элемент списка выбран, когда данные динамически загружены.

## <a name="steps"></a>Шаги

Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

Затем необходим элемент управления DropDownList:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

Для этого списка добавляется расширитель CascadingDropDown, предоставляя данные веб-службы URL-адрес и метод:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

Расширитель CascadingDropDown затем производит асинхронный вызов веб-службы со следующей сигнатурой метода:

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

Метод возвращает массив объектов типа CascadingDropDown значение. Конструктор типа сначала ожидает заголовок элемента списка, а затем значение (HTML `value` атрибут). Если третий аргумент имеет значение true, в списке элемент автоматически выбирается в браузере.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

Загрузку страницы в браузере будет заполнить раскрывающийся список с тремя поставщиками второй выбраны заранее.


[![Список заполняется и автоматически выбраны заранее](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

Список заполняется и автоматически выбраны заранее ([Просмотр полноразмерного изображения](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](using-cascadingdropdown-with-a-database-cs.md)
> [Вперед](using-auto-postback-with-cascadingdropdown-cs.md)
