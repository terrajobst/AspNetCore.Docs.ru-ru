---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Создание взаимно исключают друг друга флажки (VB) | Документы Microsoft
author: wenz
description: 'Если можно выбрать только один набор параметров, обычно используются переключателей. То, что имеется недостаток: после выбора один переключатель в группе...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: cdb93a080fb62cfdc7e3ff0604a1447e2bb99071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874256"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="81d96-104">Создание взаимно исключают друг друга флажки (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="81d96-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>
====================
<span data-ttu-id="81d96-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="81d96-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="81d96-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="81d96-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="81d96-107">Если можно выбрать только один набор параметров, обычно используются переключателей.</span><span class="sxs-lookup"><span data-stu-id="81d96-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="81d96-108">То, что имеется недостаток: после выбора один переключатель в группе не удается снять все переключатели.</span><span class="sxs-lookup"><span data-stu-id="81d96-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="81d96-109">Флажки может быть не установлен в любое время, однако не являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="81d96-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="81d96-110">Этот учебник дает преимущества обоих подходов: флажки, которые являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="81d96-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="81d96-111">Обзор</span><span class="sxs-lookup"><span data-stu-id="81d96-111">Overview</span></span>

<span data-ttu-id="81d96-112">Если можно выбрать только один набор параметров, обычно используются переключателей.</span><span class="sxs-lookup"><span data-stu-id="81d96-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="81d96-113">То, что имеется недостаток: после выбора один переключатель в группе не удается снять все переключатели.</span><span class="sxs-lookup"><span data-stu-id="81d96-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="81d96-114">Флажки может быть не установлен в любое время, однако не являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="81d96-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="81d96-115">Этот учебник дает преимущества обоих подходов: флажки, которые являются взаимоисключающими.</span><span class="sxs-lookup"><span data-stu-id="81d96-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="81d96-116">Шаги</span><span class="sxs-lookup"><span data-stu-id="81d96-116">Steps</span></span>

<span data-ttu-id="81d96-117">Набор элементов управления ASP.NET AJAX содержит MutuallyExclusiveCheckBox расширителя.</span><span class="sxs-lookup"><span data-stu-id="81d96-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="81d96-118">Это дает возможность программистам назначить любой флажок имя группы (`Key` атрибута).</span><span class="sxs-lookup"><span data-stu-id="81d96-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="81d96-119">Из всех флажков в пределах той же группе может одновременно выбрать только один.</span><span class="sxs-lookup"><span data-stu-id="81d96-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="81d96-120">Давайте начнем с размещением два флажка на новой странице ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="81d96-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="81d96-121">Может существовать несколько, но недостаточно для демонстрации принцип, два из них:</span><span class="sxs-lookup"><span data-stu-id="81d96-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="81d96-122">Для оба флажка управления MutuallyExclusiveCheckBoxExtender могут быть помещены на странице.</span><span class="sxs-lookup"><span data-stu-id="81d96-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="81d96-123">Оба атрибута Key должны иметь такое же значение, как и значение атрибутов кнопки переключателя HTML-элементов должны быть идентичными для обозначения группы, которым они принадлежат.</span><span class="sxs-lookup"><span data-stu-id="81d96-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="81d96-124">Свойство TargetControlID расширителя указывает на ИД флажка.</span><span class="sxs-lookup"><span data-stu-id="81d96-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="81d96-125">Наконец, включить ASP.NET AJAX `ScriptManager` необходимые для всех элементов-элементов управления ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="81d96-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="81d96-126">Сохранить и запустить страницу: можно проверять и снимите оба флажка, однако во времени не могут оба флажка проверяется.</span><span class="sxs-lookup"><span data-stu-id="81d96-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="81d96-127">[![Одновременно может быть проверено только один элемент управления checkbox](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="81d96-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="81d96-128">Одновременно может быть проверено только один элемент управления checkbox ([Просмотр полноразмерное изображение](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="81d96-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="81d96-129">Назад</span><span class="sxs-lookup"><span data-stu-id="81d96-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
