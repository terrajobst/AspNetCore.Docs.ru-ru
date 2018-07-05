---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Разрешение только некоторых символов в текстовом поле (C#) | Документация Майкрософт
author: wenz
description: Проверяющие элементы управления ASP.NET гарантирует, что вводимые пользователем данные разрешены только некоторых символов. Тем не менее это по-прежнему не позволяет пользователям вводить недопустимые...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: dd7203d7f367f275d2d80c86119edc9645c9d24c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400734"
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="53752-104">Разрешение только некоторых символов в текстовом поле (C#)</span><span class="sxs-lookup"><span data-stu-id="53752-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="53752-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="53752-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="53752-106">[Скачать код](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="53752-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="53752-107">Проверяющие элементы управления ASP.NET гарантирует, что вводимые пользователем данные разрешены только некоторых символов.</span><span class="sxs-lookup"><span data-stu-id="53752-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="53752-108">Тем не менее это по-прежнему не запрещает пользователям вводить недопустимые символы и при попытке отправить форму.</span><span class="sxs-lookup"><span data-stu-id="53752-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="53752-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="53752-109">Overview</span></span>

<span data-ttu-id="53752-110">Проверяющие элементы управления ASP.NET гарантирует, что вводимые пользователем данные разрешены только некоторых символов.</span><span class="sxs-lookup"><span data-stu-id="53752-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="53752-111">Тем не менее это по-прежнему не запрещает пользователям вводить недопустимые символы и при попытке отправить форму.</span><span class="sxs-lookup"><span data-stu-id="53752-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="53752-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="53752-112">Steps</span></span>

<span data-ttu-id="53752-113">ASP.NET AJAX Control Toolkit содержит `FilteredTextBox` элемента управления, который расширяет текстового поля.</span><span class="sxs-lookup"><span data-stu-id="53752-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="53752-114">После активации, можно вводить только определенный набор символов в поле.</span><span class="sxs-lookup"><span data-stu-id="53752-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="53752-115">Чтобы это работало, необходимо сначала обычным образом ASP.NET AJAX `ScriptManager` которого загружала библиотеки JavaScript, которые также используется ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="53752-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="53752-116">Затем нам нужно текстового поля:</span><span class="sxs-lookup"><span data-stu-id="53752-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="53752-117">Наконец `FilteredTextBoxExtender` берет на себя ограничение символов, пользователь может ввести элемент управления.</span><span class="sxs-lookup"><span data-stu-id="53752-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="53752-118">Сначала задайте `TargetControlID` атрибут `ID` из `TextBox` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="53752-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="53752-119">Выберите один из доступных `FilterType` значений:</span><span class="sxs-lookup"><span data-stu-id="53752-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="53752-120">`Custom` по умолчанию; Вы должны предоставить список допустимых знаков</span><span class="sxs-lookup"><span data-stu-id="53752-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="53752-121">`LowercaseLetters` только строчные буквы</span><span class="sxs-lookup"><span data-stu-id="53752-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="53752-122">`Numbers` только цифры</span><span class="sxs-lookup"><span data-stu-id="53752-122">`Numbers` digits only</span></span>
- <span data-ttu-id="53752-123">`UppercaseLetters` только прописные буквы</span><span class="sxs-lookup"><span data-stu-id="53752-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="53752-124">Если `Custom FilterType` используется, `ValidChars` свойство должно быть задано и укажите список символов, которые может ввести.</span><span class="sxs-lookup"><span data-stu-id="53752-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="53752-125">Между прочим: при попытке вставить текст в текстовое поле, будут удалены все недопустимые символы.</span><span class="sxs-lookup"><span data-stu-id="53752-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="53752-126">Далее приведена разметка для `FilteredTextBoxExtender` элемент управления, который допускает только цифры (то, что также было бы возможным с `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="53752-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="53752-127">Запустите страницу и попробуйте ввести буквы в том случае, если включить JavaScript, он не будет работать; Тем не менее, на странице отображаются цифры.</span><span class="sxs-lookup"><span data-stu-id="53752-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="53752-128">Тем не менее Обратите внимание, что защита `FilteredTextBox` предоставляет не пуленепробиваема: если JavaScript включен, все данные могут вводиться в текстовом поле, поэтому необходимо использовать средства дополнительной проверки, т. е. ASP. NET-элементов управления проверки.</span><span class="sxs-lookup"><span data-stu-id="53752-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="53752-129">[![Можно вводить только цифры](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="53752-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="53752-130">Можно вводить только цифры ([Просмотр полноразмерного изображения](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="53752-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="53752-131">Вперед</span><span class="sxs-lookup"><span data-stu-id="53752-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
