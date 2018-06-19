---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: С помощью DynamicPopulate с пользовательский элемент управления и JavaScript (C#) | Документы Microsoft
author: wenz
description: Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: cced645733375de7ab6235efa46b8d20ed262e50
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879573"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="7068b-103">С помощью DynamicPopulate с пользовательский элемент управления и JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="7068b-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>
====================
<span data-ttu-id="7068b-104">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7068b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7068b-105">[Загрузить код](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) или [скачать PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7068b-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="7068b-106">Элемент управления DynamicPopulate в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="7068b-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="7068b-107">Можно также запустить совокупности, используя настраиваемый код JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7068b-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="7068b-108">Однако особое внимание должен быть предприняты при расширителя находится в элементе управления.</span><span class="sxs-lookup"><span data-stu-id="7068b-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="7068b-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="7068b-109">Overview</span></span>

<span data-ttu-id="7068b-110">`DynamicPopulate` Элемента управления в наборе элементов управления ASP.NET AJAX вызывает веб-службы (или метод страницы) и заполняет результирующее значение в целевой элемент управления на странице без обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="7068b-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="7068b-111">Можно также запустить совокупности, используя настраиваемый код JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7068b-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="7068b-112">Однако особое внимание должен быть предприняты при расширителя находится в элементе управления.</span><span class="sxs-lookup"><span data-stu-id="7068b-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="7068b-113">Шаги</span><span class="sxs-lookup"><span data-stu-id="7068b-113">Steps</span></span>

<span data-ttu-id="7068b-114">Во-первых, необходимо использовать веб-службу ASP.NET, которая реализует метод, вызываемый `DynamicPopulateExtender` элемента управления.</span><span class="sxs-lookup"><span data-stu-id="7068b-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="7068b-115">Веб-служба реализует метод `getDate()` , ожидает один аргумент типа строки, которая называется `contextKey`, так как `DynamicPopulate` управления отправляет сведения о контексте один фрагмент с каждого вызова веб-службы.</span><span class="sxs-lookup"><span data-stu-id="7068b-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="7068b-116">Ниже приведен код (файл `DynamicPopulate.cs.asmx`) для загрузки текущей даты в одном из трех форматов:</span><span class="sxs-lookup"><span data-stu-id="7068b-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="7068b-117">На следующем шаге необходимо создать новый пользовательский элемент управления (`.ascx` файл), обозначаемого, следующее объявление в первой строке:</span><span class="sxs-lookup"><span data-stu-id="7068b-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="7068b-118">Объект &lt; `label` &gt; элемент будет использоваться для отображения данных, поступающих с сервера.</span><span class="sxs-lookup"><span data-stu-id="7068b-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="7068b-119">Также файл пользовательского элемента управления, мы будем использовать три переключателей, каждая из которых представляет одно из трех возможных даты форматах, поддерживаемых веб-службы.</span><span class="sxs-lookup"><span data-stu-id="7068b-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="7068b-120">Когда пользователь щелкает на одном из переключателей, браузер будет выполняться код JavaScript, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="7068b-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="7068b-121">Этот код обращается к `DynamicPopulateExtender` (не волноваться по поводу необычной идентификатор пока, это будет рассматриваться позже) и запускает динамического заполнения данными.</span><span class="sxs-lookup"><span data-stu-id="7068b-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="7068b-122">В контексте текущего переключатель `this.value` ссылается на его значение, которое является `format1`, `format2` или `format3` точно веб-метода ожиданиям.</span><span class="sxs-lookup"><span data-stu-id="7068b-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="7068b-123">Единственное, что еще отсутствуют в пользовательский элемент управления является `DynamicPopulateExtender` связывает переключателей в веб-службу управления.</span><span class="sxs-lookup"><span data-stu-id="7068b-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="7068b-124">Вы можете отметить, снова необычной идентификатор, используемый в элементе управления: `mcd1$myDate` вместо `myDate`.</span><span class="sxs-lookup"><span data-stu-id="7068b-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="7068b-125">Ранее, код JavaScript, используемый `mcd1_dpe1` для доступа к `DynamicPopulateExtender` вместо `dpe1`. Эта стратегия именования имеет особые требования при использовании `DynamicPopulateExtender` в пользовательский элемент управления.</span><span class="sxs-lookup"><span data-stu-id="7068b-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="7068b-126">Кроме того необходимо встроить регулировать пользователя особенным образом для его работы.</span><span class="sxs-lookup"><span data-stu-id="7068b-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="7068b-127">Создайте новую страницу ASP.NET и зарегистрируйте префикс тега для пользовательского элемента управления, только что реализованный:</span><span class="sxs-lookup"><span data-stu-id="7068b-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="7068b-128">Затем следует включить ASP.NET AJAX `ScriptManager` управления на новой странице:</span><span class="sxs-lookup"><span data-stu-id="7068b-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="7068b-129">Наконец добавьте пользовательский элемент управления на страницу.</span><span class="sxs-lookup"><span data-stu-id="7068b-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="7068b-130">Необходимо установить его `ID` атрибута (и `runat="server"`, конечно), но у присваивается определенное имя: `mcd1` так как это префикс, используемый в пользовательский элемент управления для доступа к нему с помощью JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7068b-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="7068b-131">Вот и все!</span><span class="sxs-lookup"><span data-stu-id="7068b-131">And that's it!</span></span> <span data-ttu-id="7068b-132">Страница работает как ожидалось: пользователь выбирает один из переключателей, элемент управления в наборе средств вызывает веб-службу и отображает текущую дату в нужном формате.</span><span class="sxs-lookup"><span data-stu-id="7068b-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="7068b-133">[![Переключатели размещаются в элементе управления пользователя](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7068b-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="7068b-134">Переключатели размещаются в пользовательском элементе управления ([Просмотр полноразмерное изображение](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7068b-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7068b-135">[Назад](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Вперед](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7068b-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
