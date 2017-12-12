---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: "Предварительная настройка записи списка с CascadingDropDown (VB) | Документы Microsoft"
author: wenz
description: "CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: c28c7893c39d9ba9f828c34da7ffdce525ee248e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Предварительная настройка записи списка с CascadingDropDown (Visual Basic)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList. С небольшого фрагмента кода существует вероятность, что элемент списка выбран, после динамически загрузить данные.


## <a name="overview"></a>Обзор

CascadingDropDown управления в наборе элементов управления AJAX расширяет элемент управления DropDownList, чтобы изменения в один загружает DropDownList соответствующих значений в другой DropDownList. (Для экземпляра один список содержит список штатов США, и далее список заполняется основных городов, в этом состоянии.) С небольшого фрагмента кода существует вероятность, что элемент списка выбран, после динамически загрузить данные.

## <a name="steps"></a>Шаги

Чтобы активировать функциональные возможности ASP.NET AJAX и набора средств управления `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемент):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Затем элемент управления DropDownList требуется:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Для этого списка добавляется расширителя CascadingDropDown, предоставляя URL-адрес и метод информации веб-службы:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

Расширитель CascadingDropDown асинхронно вызывает веб-службы со следующей сигнатурой метода:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

Метод возвращает массив объектов типа CascadingDropDown значение. Тип конструктора сначала требуется заголовок элемента списка, а затем значение (HTML `value` атрибут). Если третий аргумент имеет значение true, в списке элемент выбирается автоматически в браузере.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Загрузка страницы в браузере будут заполнены раскрывающийся список с тремя поставщиков предварительно выбран второй.


[![Список заполняется, автоматически обновляемые](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

Список заполняется, автоматически обновляемые ([Просмотр полноразмерное изображение](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

>[!div class="step-by-step"]
[Назад](using-cascadingdropdown-with-a-database-vb.md)
[Вперед](using-auto-postback-with-cascadingdropdown-vb.md)
