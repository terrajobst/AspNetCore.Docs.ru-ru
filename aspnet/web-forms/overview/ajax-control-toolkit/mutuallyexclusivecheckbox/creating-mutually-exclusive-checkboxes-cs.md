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
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="407e2-104">Создание взаимоисключающих флажков (C#)</span><span class="sxs-lookup"><span data-stu-id="407e2-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="407e2-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="407e2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="407e2-106">[Скачать код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="407e2-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="407e2-107">Когда может быть выбран только один из набора параметров, обычно используются переключателей.</span><span class="sxs-lookup"><span data-stu-id="407e2-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="407e2-108">Не существует недостаток, но: после выбора переключателя в группе не удается снять все переключатели.</span><span class="sxs-lookup"><span data-stu-id="407e2-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="407e2-109">Флажки, можно снять в любой момент, но не являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="407e2-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="407e2-110">В этом руководстве приведены преимущества обоих подходов: флажки, которые являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="407e2-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="407e2-111">Обзор</span><span class="sxs-lookup"><span data-stu-id="407e2-111">Overview</span></span>

<span data-ttu-id="407e2-112">Когда может быть выбран только один из набора параметров, обычно используются переключателей.</span><span class="sxs-lookup"><span data-stu-id="407e2-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="407e2-113">Не существует недостаток, но: после выбора переключателя в группе не удается снять все переключатели.</span><span class="sxs-lookup"><span data-stu-id="407e2-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="407e2-114">Флажки, можно снять в любой момент, но не являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="407e2-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="407e2-115">В этом руководстве приведены преимущества обоих подходов: флажки, которые являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="407e2-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="407e2-116">Шаги</span><span class="sxs-lookup"><span data-stu-id="407e2-116">Steps</span></span>

<span data-ttu-id="407e2-117">ASP.NET AJAX Control Toolkit содержит расширения MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="407e2-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="407e2-118">Это дает возможность программистам назначить любой checkbox с именем группы (`Key` атрибут).</span><span class="sxs-lookup"><span data-stu-id="407e2-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="407e2-119">Все флажки в той же группе может одновременно выбрать только один.</span><span class="sxs-lookup"><span data-stu-id="407e2-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="407e2-120">Давайте начнем с размещением два флажка на новой странице ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="407e2-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="407e2-121">Может существовать несколько, но два из них достаточно для демонстрации принципа:</span><span class="sxs-lookup"><span data-stu-id="407e2-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="407e2-122">Для оба флажка элемент управления MutuallyExclusiveCheckBoxExtender должны быть размещены на странице.</span><span class="sxs-lookup"><span data-stu-id="407e2-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="407e2-123">Оба ключевые атрибуты должны иметь одинаковое значение, так же, как значение, которое атрибутов кнопку radio HTML-элементов должны совпадать для обозначения группы, которым они принадлежат.</span><span class="sxs-lookup"><span data-stu-id="407e2-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="407e2-124">Свойство TargetControlID расширителя указывает на идентификатор поля с флажком.</span><span class="sxs-lookup"><span data-stu-id="407e2-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="407e2-125">Наконец, включите ASP.NET AJAX `ScriptManager` которая необходима для всех элементов ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="407e2-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="407e2-126">Сохранить и запустить эту страницу: можно проверять и снимите оба флажка, но никогда не установлены оба флажка нельзя.</span><span class="sxs-lookup"><span data-stu-id="407e2-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="407e2-127">[![Одновременно можно проверить только один флажок](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="407e2-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="407e2-128">Одновременно можно проверить только один флажок ([Просмотр полноразмерного изображения](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="407e2-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="407e2-129">Вперед</span><span class="sxs-lookup"><span data-stu-id="407e2-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
