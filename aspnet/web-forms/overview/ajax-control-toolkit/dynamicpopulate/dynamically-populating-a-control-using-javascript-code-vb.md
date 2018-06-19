---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: Динамически заполнение элемента управления с помощью кода JavaScript (VB) | Документы Microsoft
author: wenz
description: Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 04bbc6fca839c2b1ed5cafd3a4411604b98e187d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869989"
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="45103-103">Динамически заполнение элемента управления с помощью кода JavaScript (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="45103-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="45103-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="45103-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="45103-105">[Загрузить код](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="45103-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="45103-106">Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="45103-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="45103-107">Можно также запустить совокупности, используя настраиваемый код JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="45103-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="45103-108">Обзор</span><span class="sxs-lookup"><span data-stu-id="45103-108">Overview</span></span>

<span data-ttu-id="45103-109">`DynamicPopulate` Элемента управления в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="45103-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="45103-110">Можно также запустить совокупности, используя настраиваемый код JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="45103-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="45103-111">Шаги</span><span class="sxs-lookup"><span data-stu-id="45103-111">Steps</span></span>

<span data-ttu-id="45103-112">Во-первых, необходимо использовать веб-службу ASP.NET, которая реализует метод, вызываемый `DynamicPopulateExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="45103-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="45103-113">Веб-служба реализует метод `getDate()` , ожидает один аргумент типа строки, которая называется `contextKey`, так как `DynamicPopulate` управления отправляет сведения о контексте один фрагмент с каждого вызова веб-службы.</span><span class="sxs-lookup"><span data-stu-id="45103-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="45103-114">Ниже приведен код (файл `DynamicPopulate.vb.asmx`) для загрузки текущей даты в одном из трех форматов:</span><span class="sxs-lookup"><span data-stu-id="45103-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="45103-115">На следующем шаге создайте новый сайт ASP.NET и начать с элементом управления ASP.NET AJAX ScriptManager:</span><span class="sxs-lookup"><span data-stu-id="45103-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="45103-116">Добавьте элемент управления label (например с помощью HTML-элемент управления с таким же именем или `<asp:Label />` веб-элемента управления) позже которого отображается результат вызова веб-службы.</span><span class="sxs-lookup"><span data-stu-id="45103-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="45103-117">Теперь включают `DynamicPopulateExtender` управление и предоставляют информации веб-службы, целевой элемент управления, но не имя элемента управления, который запускает это будет сделано позже на заполнение с использованием JavaScript пользовательские!</span><span class="sxs-lookup"><span data-stu-id="45103-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="45103-118">Теперь в компонент JavaScript.</span><span class="sxs-lookup"><span data-stu-id="45103-118">Now to the JavaScript part.</span></span> <span data-ttu-id="45103-119">`$find()` Функции, определенные библиотека ASP.NET AJAX, возвращает ссылку на объекты серверных элементов управления ASP.NET AJAX, такие как `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="45103-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="45103-120">В текущем файле `$find("dpe")` возвращает ссылку на версию `DynamicPopulateExtender` управления на странице.</span><span class="sxs-lookup"><span data-stu-id="45103-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="45103-121">Он предоставляет метод, называемый `populate()` которое запускает процесс динамического заполнения.</span><span class="sxs-lookup"><span data-stu-id="45103-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="45103-122">`populate()` Метод требует один аргумент: ключ контекста, который будет использоваться в качестве аргумента для `getDate()` веб-метода.</span><span class="sxs-lookup"><span data-stu-id="45103-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="45103-123">Например `$find("dpe").populate("format1")` заполнит метку с текущей даты в формате месяц день год.</span><span class="sxs-lookup"><span data-stu-id="45103-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="45103-124">Чтобы сделать более гибкое образца, пользователь может выбрать один из нескольких форматов даты.</span><span class="sxs-lookup"><span data-stu-id="45103-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="45103-125">Переключатель отображается для каждой из них.</span><span class="sxs-lookup"><span data-stu-id="45103-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="45103-126">Один раз когда пользователь щелкает переключатель, код JavaScript динамически заполняет метку с выбранным форматом даты.</span><span class="sxs-lookup"><span data-stu-id="45103-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="45103-127">Ниже приведены эти переключатели.</span><span class="sxs-lookup"><span data-stu-id="45103-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="45103-128">Обратите внимание, что в контексте типа "переключатель", выражение JavaScript `this.value` ссылается на значение «текущий», которая может быть точно те же данные `getDate()` метод может работать с.</span><span class="sxs-lookup"><span data-stu-id="45103-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="45103-129">[![Нажмите кнопку извлекает дату с сервера в формате, заданном](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="45103-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="45103-130">Нажмите кнопку извлекает дату с сервера в формате, заданном ([Просмотр полноразмерное изображение](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="45103-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45103-131">[Назад](dynamically-populating-a-control-vb.md)
> [Вперед](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="45103-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
