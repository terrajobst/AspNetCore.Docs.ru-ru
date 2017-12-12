---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: "Проверяющие элементы управления (Visual Basic) при помощи TextBoxWatermark | Документы Microsoft"
author: wenz
description: "Элемент управления TextBoxWatermark в наборе элементов управления AJAX расширяет текстовое поле для отображения текста в поле. Когда пользователь щелкает в поле, оно я..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 09236481b6e51cc22a4034aa22e7c491ce27a510
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="82935-104">Использование TextBoxWatermark с проверяющие элементы управления (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="82935-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>
====================
<span data-ttu-id="82935-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="82935-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="82935-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="82935-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="82935-107">Элемент управления TextBoxWatermark в наборе элементов управления AJAX расширяет текстовое поле для отображения текста в поле.</span><span class="sxs-lookup"><span data-stu-id="82935-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="82935-108">Когда пользователь щелкает в поле, оно будет очищено.</span><span class="sxs-lookup"><span data-stu-id="82935-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="82935-109">Если поле остается без ввода текста, заданными текст снова появится.</span><span class="sxs-lookup"><span data-stu-id="82935-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="82935-110">Это может конфликтовать с элементов управления проверкой ASP.NET на одной странице, но может преодолеть эти проблемы.</span><span class="sxs-lookup"><span data-stu-id="82935-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="82935-111">Обзор</span><span class="sxs-lookup"><span data-stu-id="82935-111">Overview</span></span>

<span data-ttu-id="82935-112">`TextBoxWatermark` В наборе элементов управления AJAX, расширяет текстовое поле для отображения текста в поле.</span><span class="sxs-lookup"><span data-stu-id="82935-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="82935-113">Когда пользователь щелкает в поле, оно будет очищено.</span><span class="sxs-lookup"><span data-stu-id="82935-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="82935-114">Если поле остается без ввода текста, заданными текст снова появится.</span><span class="sxs-lookup"><span data-stu-id="82935-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="82935-115">Это может конфликтовать с элементов управления проверкой ASP.NET на одной странице, но может преодолеть эти проблемы.</span><span class="sxs-lookup"><span data-stu-id="82935-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="82935-116">Шаги</span><span class="sxs-lookup"><span data-stu-id="82935-116">Steps</span></span>

<span data-ttu-id="82935-117">Базовая настройка образца является следующее: `TextBox` водяные знаки управления с помощью `TextBoxWatermarkExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="82935-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="82935-118">Кнопка запускает обратной передачи и позже будет использоваться для запуска проверяющие элементы управления на странице.</span><span class="sxs-lookup"><span data-stu-id="82935-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="82935-119">Кроме того `ScriptManager` управления необходим для инициализации ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="82935-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="82935-120">Теперь добавьте `RequiredFieldValidator` элемент управления, который проверяет, существует ли текст в поле при отправке формы.</span><span class="sxs-lookup"><span data-stu-id="82935-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="82935-121">`InitialValue` Свойства проверяющего элемента управления должно быть присвоено то же значение, которое используется в `TextBoxWatermarkExtender` управления: при отправке формы без изменений текстовое значение является значением водяного знака в ней:</span><span class="sxs-lookup"><span data-stu-id="82935-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="82935-122">Однако есть одна проблема этого подхода: Если клиент отключает JavaScript, текстовое поле не автоматически заполняемом с текста водяного знака, поэтому `RequiredFieldValidator` не вызывает сообщение об ошибке.</span><span class="sxs-lookup"><span data-stu-id="82935-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="82935-123">Таким образом, второй `RequiredFieldValidator` управления требуется, который проверяет наличие пустые текстовые поля (пропуск `InitialValue` атрибута).</span><span class="sxs-lookup"><span data-stu-id="82935-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="82935-124">Поскольку оба проверяющие элементы управления используют `Display` = `"Dynamic"`, конечный пользователь не может отличить от внешнего вида, какой из двух проверяющие элементы управления было запущено; вместо этого выглядит так, будто был только один из них.</span><span class="sxs-lookup"><span data-stu-id="82935-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="82935-125">Наконец добавьте некоторые серверный код для вывода текста в поле, если проверяющий элемент управления выдаваться сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="82935-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


<span data-ttu-id="82935-126">[![Проверяющий элемент управления сообщает, что в поле отсутствует текст](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="82935-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="82935-127">Проверяющий элемент управления сообщает, что в поле отсутствует текст ([Просмотр полноразмерное изображение](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="82935-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="82935-128">Назад</span><span class="sxs-lookup"><span data-stu-id="82935-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
