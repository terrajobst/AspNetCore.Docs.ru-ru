---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Создание взаимно исключают друг друга флажки (C#) | Документы Microsoft
author: wenz
description: 'Если можно выбрать только один набор параметров, обычно используются переключателей. То, что имеется недостаток: после выбора один переключатель в группе...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: c3a5abe7d02ace16f4aaad8d4adfbd0cba8e84ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871809"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>Создание взаимно исключают друг друга флажки (C#)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Если можно выбрать только один набор параметров, обычно используются переключателей. То, что имеется недостаток: после выбора один переключатель в группе не удается снять все переключатели. Флажки может быть не установлен в любое время, однако не являются взаимоисключающими. Этот учебник дает преимущества обоих подходов: флажки, которые являются взаимоисключающими.


## <a name="overview"></a>Обзор

Если можно выбрать только один набор параметров, обычно используются переключателей. То, что имеется недостаток: после выбора один переключатель в группе не удается снять все переключатели. Флажки может быть не установлен в любое время, однако не являются взаимоисключающими. Этот учебник дает преимущества обоих подходов: флажки, которые являются взаимоисключающими.

## <a name="steps"></a>Шаги

Набор элементов управления ASP.NET AJAX содержит MutuallyExclusiveCheckBox расширителя. Это дает возможность программистам назначить любой флажок имя группы (`Key` атрибута). Из всех флажков в пределах той же группе может одновременно выбрать только один.

Давайте начнем с размещением два флажка на новой странице ASP.NET. Может существовать несколько, но недостаточно для демонстрации принцип, два из них:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Для оба флажка управления MutuallyExclusiveCheckBoxExtender могут быть помещены на странице. Оба атрибута Key должны иметь такое же значение, как и значение атрибутов кнопки переключателя HTML-элементов должны быть идентичными для обозначения группы, которым они принадлежат. Свойство TargetControlID расширителя указывает на ИД флажка.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Наконец, включить ASP.NET AJAX `ScriptManager` необходимые для всех элементов-элементов управления ASP.NET AJAX:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Сохранить и запустить страницу: можно проверять и снимите оба флажка, однако во времени не могут оба флажка проверяется.


[![Одновременно может быть проверено только один элемент управления checkbox](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Одновременно может быть проверено только один элемент управления checkbox ([Просмотр полноразмерное изображение](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Вперед](creating-mutually-exclusive-checkboxes-vb.md)
