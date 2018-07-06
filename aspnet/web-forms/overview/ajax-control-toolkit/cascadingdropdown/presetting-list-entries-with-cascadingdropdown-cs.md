---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Предустановка записей списка с помощью с CascadingDropDown (C#) | Документация Майкрософт
author: wenz
description: Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 7797aa91452bfbed2695fd26e3b9d2a5783dd216
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825093"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a><span data-ttu-id="b0995-103">Предустановка записей списка с помощью с CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="b0995-103">Presetting List Entries with CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="b0995-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b0995-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b0995-105">[Скачать код](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b0995-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)</span></span>

> <span data-ttu-id="b0995-106">Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="b0995-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b0995-107">Используя небольшой фрагмент кода вполне возможно, что элемент списка выбран, когда данные динамически загружены.</span><span class="sxs-lookup"><span data-stu-id="b0995-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="b0995-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="b0995-108">Overview</span></span>

<span data-ttu-id="b0995-109">Элемент управления CascadingDropDown в AJAX Control Toolkit расширяет элемент управления DropDownList, что изменения в одной загрузке DropDownList соответствующих значений в другой DropDownList.</span><span class="sxs-lookup"><span data-stu-id="b0995-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="b0995-110">(К примеру, один список содержит список штатов США, и следующий список заполняется крупнейших городов в этом состоянии.) Используя небольшой фрагмент кода вполне возможно, что элемент списка выбран, когда данные динамически загружены.</span><span class="sxs-lookup"><span data-stu-id="b0995-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="b0995-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="b0995-111">Steps</span></span>

<span data-ttu-id="b0995-112">Для активации функции ASP.NET AJAX и Control Toolkit, `ScriptManager` управления необходимо поместить в любом месте на странице (но в `<form>` элемента):</span><span class="sxs-lookup"><span data-stu-id="b0995-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="b0995-113">Затем необходим элемент управления DropDownList:</span><span class="sxs-lookup"><span data-stu-id="b0995-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="b0995-114">Для этого списка добавляется расширитель CascadingDropDown, предоставляя данные веб-службы URL-адрес и метод:</span><span class="sxs-lookup"><span data-stu-id="b0995-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="b0995-115">Расширитель CascadingDropDown затем производит асинхронный вызов веб-службы со следующей сигнатурой метода:</span><span class="sxs-lookup"><span data-stu-id="b0995-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="b0995-116">Метод возвращает массив объектов типа CascadingDropDown значение.</span><span class="sxs-lookup"><span data-stu-id="b0995-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="b0995-117">Конструктор типа сначала ожидает заголовок элемента списка, а затем значение (HTML `value` атрибут).</span><span class="sxs-lookup"><span data-stu-id="b0995-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="b0995-118">Если третий аргумент имеет значение true, в списке элемент автоматически выбирается в браузере.</span><span class="sxs-lookup"><span data-stu-id="b0995-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="b0995-119">Загрузку страницы в браузере будет заполнить раскрывающийся список с тремя поставщиками второй выбраны заранее.</span><span class="sxs-lookup"><span data-stu-id="b0995-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="b0995-120">[![Список заполняется и автоматически выбраны заранее](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b0995-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="b0995-121">Список заполняется и автоматически выбраны заранее ([Просмотр полноразмерного изображения](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b0995-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b0995-122">[Назад](using-cascadingdropdown-with-a-database-cs.md)
> [Вперед](using-auto-postback-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b0995-122">[Previous](using-cascadingdropdown-with-a-database-cs.md)
[Next](using-auto-postback-with-cascadingdropdown-cs.md)</span></span>
