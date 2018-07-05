---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Использование DynamicPopulate с пользовательского элемента управления и JavaScript (C#) | Документация Майкрософт
author: wenz
description: Элемент управления DynamicPopulate в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: cba1647e69cbaebf0accb745278590e357d8c2c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370423"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="f47de-103">Использование DynamicPopulate с пользовательского элемента управления и JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="f47de-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>
====================
<span data-ttu-id="f47de-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f47de-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f47de-105">[Скачать код](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f47de-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="f47de-106">Элемент управления DynamicPopulate в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="f47de-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="f47de-107">Можно также активировать заполнение, с помощью пользовательского кода JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="f47de-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="f47de-108">Тем не менее, выполняемый при расширителя находится в пользовательский элемент управления имеет особое внимание.</span><span class="sxs-lookup"><span data-stu-id="f47de-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="f47de-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="f47de-109">Overview</span></span>

<span data-ttu-id="f47de-110">`DynamicPopulate` Элемента управления в ASP.NET AJAX Control Toolkit вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="f47de-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="f47de-111">Можно также активировать заполнение, с помощью пользовательского кода JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="f47de-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="f47de-112">Тем не менее, выполняемый при расширителя находится в пользовательский элемент управления имеет особое внимание.</span><span class="sxs-lookup"><span data-stu-id="f47de-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="f47de-113">Шаги</span><span class="sxs-lookup"><span data-stu-id="f47de-113">Steps</span></span>

<span data-ttu-id="f47de-114">Во-первых, необходимо, чтобы к веб-службе ASP.NET, который реализует метод, вызываемый `DynamicPopulateExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="f47de-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="f47de-115">Веб-служба реализует метод `getDate()` , ожидает один аргумент типа строки, называемой `contextKey`, так как `DynamicPopulate` элемент управления отправляет понимается набор сведений о контексте с каждый вызов веб-службы.</span><span class="sxs-lookup"><span data-stu-id="f47de-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="f47de-116">Ниже приведен код (файл `DynamicPopulate.cs.asmx`) который получает текущую дату в одном из трех форматов:</span><span class="sxs-lookup"><span data-stu-id="f47de-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="f47de-117">На следующем шаге, создайте новый пользовательский элемент управления (`.ascx` файл), обозначаемого, следующее объявление в первой строке:</span><span class="sxs-lookup"><span data-stu-id="f47de-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="f47de-118">Объект &lt; `label` &gt; элемент будет использоваться для отображения данных, поступающих с сервера.</span><span class="sxs-lookup"><span data-stu-id="f47de-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="f47de-119">Также в файле параметров пользователя, мы будем использовать три переключателей, каждый из которых представляет одно из трех возможных даты форматов, поддерживаемых веб-службы.</span><span class="sxs-lookup"><span data-stu-id="f47de-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="f47de-120">Когда пользователь щелкает один из переключателей, браузер выполнит код JavaScript, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f47de-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="f47de-121">Этот код обращается к `DynamicPopulateExtender` (не волнуйтесь о странный код еще, это будет рассматриваться позже) и активирует динамического заполнения данными.</span><span class="sxs-lookup"><span data-stu-id="f47de-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="f47de-122">В контексте текущего переключатель `this.value` ссылается на его значение, являющееся `format1`, `format2` или `format3` точно что ожидает веб-метода.</span><span class="sxs-lookup"><span data-stu-id="f47de-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="f47de-123">Единственное, что пока отсутствует в пользовательский элемент управления является `DynamicPopulateExtender` элемента управления, которые связывают переключателей с веб-службы.</span><span class="sxs-lookup"><span data-stu-id="f47de-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="f47de-124">Еще раз вы также могли заметить странный код, используемый в элементе управления: `mcd1$myDate` вместо `myDate`.</span><span class="sxs-lookup"><span data-stu-id="f47de-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="f47de-125">Ранее, код JavaScript, используемый `mcd1_dpe1` для доступа к `DynamicPopulateExtender` вместо `dpe1`. Эта стратегия именования имеет особые требования при использовании `DynamicPopulateExtender` внутри пользовательского элемента управления.</span><span class="sxs-lookup"><span data-stu-id="f47de-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="f47de-126">Кроме того необходимо вставить регулировать пользователя определенным образом заставить их работать.</span><span class="sxs-lookup"><span data-stu-id="f47de-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="f47de-127">Создать новую страницу ASP.NET и зарегистрировать префикс тега для пользовательского элемента управления, который вы только что реализовали:</span><span class="sxs-lookup"><span data-stu-id="f47de-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="f47de-128">Затем включите ASP.NET AJAX `ScriptManager` управления на новой странице:</span><span class="sxs-lookup"><span data-stu-id="f47de-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="f47de-129">Наконец добавьте пользовательский элемент управления на страницу.</span><span class="sxs-lookup"><span data-stu-id="f47de-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="f47de-130">Необходимо установить его `ID` атрибут (и `runat="server"`, конечно), но необходимо также присвоено определенное имя: `mcd1` так как это префикс, используемый в пользовательский элемент управления для доступа к нему с помощью JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f47de-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="f47de-131">Вот и все!</span><span class="sxs-lookup"><span data-stu-id="f47de-131">And that's it!</span></span> <span data-ttu-id="f47de-132">Страницы ведет себя так, как ожидалось: пользователь выбирает один из переключателей, элемент управления в наборе средств вызывает веб-службы и отображает текущую дату в нужном формате.</span><span class="sxs-lookup"><span data-stu-id="f47de-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="f47de-133">[![Переключатели находятся в пользовательский элемент управления](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f47de-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="f47de-134">Переключатели находятся в пользовательский элемент управления ([Просмотр полноразмерного изображения](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f47de-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f47de-135">[Назад](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Вперед](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f47de-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
