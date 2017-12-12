---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: "Создание взаимно исключают друг друга флажки (C#) | Документы Microsoft"
author: wenz
description: "Если можно выбрать только один набор параметров, обычно используются переключателей. То, что имеется недостаток: после выбора один переключатель в группе..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: e165c3784b246effcaeafc0ad4274bc0ca81a99c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="308a2-104">Создание взаимно исключают друг друга флажки (C#)</span><span class="sxs-lookup"><span data-stu-id="308a2-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="308a2-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="308a2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="308a2-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="308a2-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="308a2-107">Если можно выбрать только один набор параметров, обычно используются переключателей.</span><span class="sxs-lookup"><span data-stu-id="308a2-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="308a2-108">То, что имеется недостаток: после выбора один переключатель в группе не удается снять все переключатели.</span><span class="sxs-lookup"><span data-stu-id="308a2-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="308a2-109">Флажки может быть не установлен в любое время, однако не являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="308a2-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="308a2-110">Этот учебник дает преимущества обоих подходов: флажки, которые являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="308a2-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="308a2-111">Обзор</span><span class="sxs-lookup"><span data-stu-id="308a2-111">Overview</span></span>

<span data-ttu-id="308a2-112">Если можно выбрать только один набор параметров, обычно используются переключателей.</span><span class="sxs-lookup"><span data-stu-id="308a2-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="308a2-113">То, что имеется недостаток: после выбора один переключатель в группе не удается снять все переключатели.</span><span class="sxs-lookup"><span data-stu-id="308a2-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="308a2-114">Флажки может быть не установлен в любое время, однако не являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="308a2-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="308a2-115">Этот учебник дает преимущества обоих подходов: флажки, которые являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="308a2-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="308a2-116">Шаги</span><span class="sxs-lookup"><span data-stu-id="308a2-116">Steps</span></span>

<span data-ttu-id="308a2-117">Набор элементов управления ASP.NET AJAX содержит MutuallyExclusiveCheckBox расширителя.</span><span class="sxs-lookup"><span data-stu-id="308a2-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="308a2-118">Это дает возможность программистам назначить любой флажок имя группы (`Key` атрибута).</span><span class="sxs-lookup"><span data-stu-id="308a2-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="308a2-119">Из всех флажков в пределах той же группе может одновременно выбрать только один.</span><span class="sxs-lookup"><span data-stu-id="308a2-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="308a2-120">Давайте начнем с размещением два флажка на новой странице ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="308a2-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="308a2-121">Может существовать несколько, но недостаточно для демонстрации принцип, два из них:</span><span class="sxs-lookup"><span data-stu-id="308a2-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="308a2-122">Для оба флажка управления MutuallyExclusiveCheckBoxExtender могут быть помещены на странице.</span><span class="sxs-lookup"><span data-stu-id="308a2-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="308a2-123">Оба атрибута Key должны иметь такое же значение, как и значение атрибутов кнопки переключателя HTML-элементов должны быть идентичными для обозначения группы, которым они принадлежат.</span><span class="sxs-lookup"><span data-stu-id="308a2-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="308a2-124">Свойство TargetControlID расширителя указывает на ИД флажка.</span><span class="sxs-lookup"><span data-stu-id="308a2-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="308a2-125">Наконец, включить ASP.NET AJAX `ScriptManager` необходимые для всех элементов-элементов управления ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="308a2-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="308a2-126">Сохранить и запустить страницу: можно проверять и снимите оба флажка, однако во времени не могут оба флажка проверяется.</span><span class="sxs-lookup"><span data-stu-id="308a2-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="308a2-127">[![Одновременно может быть проверено только один элемент управления checkbox](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="308a2-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="308a2-128">Одновременно может быть проверено только один элемент управления checkbox ([Просмотр полноразмерное изображение](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="308a2-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="308a2-129">Вперед</span><span class="sxs-lookup"><span data-stu-id="308a2-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
