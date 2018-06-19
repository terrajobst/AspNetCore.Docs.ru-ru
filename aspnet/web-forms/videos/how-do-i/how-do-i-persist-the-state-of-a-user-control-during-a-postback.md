---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[Инструкции]: сохранить состояние пользовательского элемента управления при обратной передаче | Документы Microsoft'
author: rick-anderson
description: В этой видео пиксел Крис показывает, как для сохранения состояния одного или нескольких объектов в пользовательском элементе управления. Во-первых создается пользовательский элемент управления, представляющий abilit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525973"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Инструкции]: сохранить состояние пользовательского элемента управления во время обратной передачи
[How Do I]: Persist the State of a User Control During a Postback
====================
<span data-ttu-id="3d154-105">по [Крис пиксел](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="3d154-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="3d154-106">В этой видео пиксел Крис показывает, как для сохранения состояния одного или нескольких объектов в пользовательском элементе управления.</span><span class="sxs-lookup"><span data-stu-id="3d154-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="3d154-107">Во-первых создается пользовательский элемент управления, представляющий возможность пользователю указать критерии фильтрации для поиска.</span><span class="sxs-lookup"><span data-stu-id="3d154-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="3d154-108">Кроме того сопровождающий класс фильтра создается для хранения сведений о фильтре.</span><span class="sxs-lookup"><span data-stu-id="3d154-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="3d154-109">Несколько элементов пользовательского интерфейса добавляются к элементу управления фильтра, а также некоторые методы и свойства для хранения текущего фильтра данных в экземпляр класса фильтра.</span><span class="sxs-lookup"><span data-stu-id="3d154-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="3d154-110">Далее сохраняемости пользовательского элемента управления реализуется с помощью метода RegisterRequiresControlState и связанные методы сохранить или восстановить.</span><span class="sxs-lookup"><span data-stu-id="3d154-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="3d154-111">Эти методы хранения экземпляра класса фильтра и его данные во время обратной передачи страницы.</span><span class="sxs-lookup"><span data-stu-id="3d154-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="3d154-112">Наконец существует как для хранения нескольких объектов в реализации состояние элемента управления.</span><span class="sxs-lookup"><span data-stu-id="3d154-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="3d154-113">&#9654; Посмотрите видео (в минутах 23)</span><span class="sxs-lookup"><span data-stu-id="3d154-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
