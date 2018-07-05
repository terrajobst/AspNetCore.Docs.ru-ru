---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Создание взаимоисключающих флажков (VB) | Документация Майкрософт
author: wenz
description: 'Когда может быть выбран только один из набора параметров, обычно используются переключателей. Не существует недостаток, но: после выбора переключателя в группе...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: c71ba157a52335078a01dc8419370db33a37c43e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391383"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Создание взаимоисключающих флажков (VB)
====================
по [Кристиан Wenz](https://github.com/wenz)

[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Когда может быть выбран только один из набора параметров, обычно используются переключателей. Не существует недостаток, но: после выбора переключателя в группе не удается снять все переключатели. Флажки, можно снять в любой момент, но не являются взаимоисключающими. В этом руководстве приведены преимущества обоих подходов: флажки, которые являются взаимоисключающими.


## <a name="overview"></a>Обзор

Когда может быть выбран только один из набора параметров, обычно используются переключателей. Не существует недостаток, но: после выбора переключателя в группе не удается снять все переключатели. Флажки, можно снять в любой момент, но не являются взаимоисключающими. В этом руководстве приведены преимущества обоих подходов: флажки, которые являются взаимоисключающими.

## <a name="steps"></a>Шаги

ASP.NET AJAX Control Toolkit содержит расширения MutuallyExclusiveCheckBox. Это дает возможность программистам назначить любой checkbox с именем группы (`Key` атрибут). Все флажки в той же группе может одновременно выбрать только один.

Давайте начнем с размещением два флажка на новой странице ASP.NET. Может существовать несколько, но два из них достаточно для демонстрации принципа:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Для оба флажка элемент управления MutuallyExclusiveCheckBoxExtender должны быть размещены на странице. Оба ключевые атрибуты должны иметь одинаковое значение, так же, как значение, которое атрибутов кнопку radio HTML-элементов должны совпадать для обозначения группы, которым они принадлежат. Свойство TargetControlID расширителя указывает на идентификатор поля с флажком.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Наконец, включите ASP.NET AJAX `ScriptManager` которая необходима для всех элементов ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Сохранить и запустить эту страницу: можно проверять и снимите оба флажка, но никогда не установлены оба флажка нельзя.


[![Одновременно можно проверить только один флажок](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Одновременно можно проверить только один флажок ([Просмотр полноразмерного изображения](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Назад](creating-mutually-exclusive-checkboxes-cs.md)
