---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Создание взаимоисключающих флажков (C#) | Документация Майкрософт
author: wenz
description: 'Когда может быть выбран только один из набора параметров, обычно используются переключателей. Не существует недостаток, но: после выбора переключателя в группе...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d80771081a7aefefa115e68827c52092c6559bb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838264"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>Создание взаимоисключающих флажков (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Когда может быть выбран только один из набора параметров, обычно используются переключателей. Не существует недостаток, но: после выбора переключателя в группе не удается снять все переключатели. Флажки, можно снять в любой момент, но не являются взаимоисключающими. В этом руководстве приведены преимущества обоих подходов: флажки, которые являются взаимоисключающими.


## <a name="overview"></a>Обзор

Когда может быть выбран только один из набора параметров, обычно используются переключателей. Не существует недостаток, но: после выбора переключателя в группе не удается снять все переключатели. Флажки, можно снять в любой момент, но не являются взаимоисключающими. В этом руководстве приведены преимущества обоих подходов: флажки, которые являются взаимоисключающими.

## <a name="steps"></a>Шаги

ASP.NET AJAX Control Toolkit содержит расширения MutuallyExclusiveCheckBox. Это дает возможность программистам назначить любой checkbox с именем группы (`Key` атрибут). Все флажки в той же группе может одновременно выбрать только один.

Давайте начнем с размещением два флажка на новой странице ASP.NET. Может существовать несколько, но два из них достаточно для демонстрации принципа:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Для оба флажка элемент управления MutuallyExclusiveCheckBoxExtender должны быть размещены на странице. Оба ключевые атрибуты должны иметь одинаковое значение, так же, как значение, которое атрибутов кнопку radio HTML-элементов должны совпадать для обозначения группы, которым они принадлежат. Свойство TargetControlID расширителя указывает на идентификатор поля с флажком.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Наконец, включите ASP.NET AJAX `ScriptManager` которая необходима для всех элементов ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Сохранить и запустить эту страницу: можно проверять и снимите оба флажка, но никогда не установлены оба флажка нельзя.


[![Одновременно можно проверить только один флажок](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Одновременно можно проверить только один флажок ([Просмотр полноразмерного изображения](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](creating-mutually-exclusive-checkboxes-vb.md)
