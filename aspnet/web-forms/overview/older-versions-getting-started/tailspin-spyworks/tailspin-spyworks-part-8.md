---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Часть 8: Последние страницы, обработка исключений и заключение | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. Часть 8 добавляет страницу контактов, о странице и исключение...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 4fb0147a5cd8621e5341e4995960adf2f00fffc0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804721"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="ce70a-104">Часть 8: Последние страницы, обработка исключений и заключение</span><span class="sxs-lookup"><span data-stu-id="ce70a-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="ce70a-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="ce70a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="ce70a-106">Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="ce70a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="ce70a-107">Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="ce70a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="ce70a-108">В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="ce70a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="ce70a-109">Часть 8 добавляет страницу контактов, о странице и обработка исключений.</span><span class="sxs-lookup"><span data-stu-id="ce70a-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="ce70a-110">Это заключение серии.</span><span class="sxs-lookup"><span data-stu-id="ce70a-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="ce70a-111">Обратитесь к странице (отправка по электронной почте из ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="ce70a-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="ce70a-112">Создать новую страницу с именем ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="ce70a-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="ce70a-113">С помощью конструктора, создайте специальный названия для включения ToolkitScriptManager и элемент управления редактора из AjaxdControlToolkit следующего вида.</span><span class="sxs-lookup"><span data-stu-id="ce70a-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="ce70a-114">.</span><span class="sxs-lookup"><span data-stu-id="ce70a-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="ce70a-115">Дважды щелкните кнопку «Отправить», чтобы создать обработчик событий щелчка в файл кода программной части и реализовать метод для отправки контактную информацию, как сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="ce70a-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="ce70a-116">Этот код требует, что ваш файл web.config содержит запись в раздел конфигурации, в котором указываются параметры SMTP-сервера для отправки почты.</span><span class="sxs-lookup"><span data-stu-id="ce70a-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="ce70a-117">О странице</span><span class="sxs-lookup"><span data-stu-id="ce70a-117">About Page</span></span>

<span data-ttu-id="ce70a-118">Создайте страницу с именем AboutUs.aspx и добавьте любым содержимым, вам нравится.</span><span class="sxs-lookup"><span data-stu-id="ce70a-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="ce70a-119">Глобального обработчика исключений</span><span class="sxs-lookup"><span data-stu-id="ce70a-119">Global Exception Handler</span></span>

<span data-ttu-id="ce70a-120">Наконец в приложении мы создала исключения при этом непредвиденных обстоятельств, "холодных" причина необработанных исключений в веб-приложением.</span><span class="sxs-lookup"><span data-stu-id="ce70a-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="ce70a-121">Мы никогда не будут необработанное исключение, которое будет отображаться для посетителей веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="ce70a-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="ce70a-122">Помимо ужасно впечатления необработанные исключения также может быть проблемой безопасности.</span><span class="sxs-lookup"><span data-stu-id="ce70a-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="ce70a-123">Чтобы решить эту проблему, мы будем реализовывать глобального обработчика исключений.</span><span class="sxs-lookup"><span data-stu-id="ce70a-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="ce70a-124">Чтобы сделать это, откройте файл Global.asax и обратите внимание, следующие предварительно созданный обработчик событий.</span><span class="sxs-lookup"><span data-stu-id="ce70a-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="ce70a-125">Добавьте код для реализации приложения\_обработчик ошибок, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="ce70a-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="ce70a-126">Затем добавьте страницу с именем Error.aspx в решение и добавьте следующий фрагмент кода разметки.</span><span class="sxs-lookup"><span data-stu-id="ce70a-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="ce70a-127">Теперь на странице\_загрузить извлечения обработчика событий, сообщения об ошибках из объекта запроса.</span><span class="sxs-lookup"><span data-stu-id="ce70a-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="ce70a-128">Заключение</span><span class="sxs-lookup"><span data-stu-id="ce70a-128">Conclusion</span></span>

<span data-ttu-id="ce70a-129">Мы уже видели, что веб-форм ASP.NET упрощает создание сложных веб-сайт с помощью доступа к базе данных, членство, AJAX, и т.д.</span><span class="sxs-lookup"><span data-stu-id="ce70a-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="ce70a-130">довольно быстро.</span><span class="sxs-lookup"><span data-stu-id="ce70a-130">pretty quickly.</span></span>

<span data-ttu-id="ce70a-131">Будем надеяться, что приведенная в этом учебнике вам средства, которые нужны для начала создания приложения собственных веб-форм ASP.NET!</span><span class="sxs-lookup"><span data-stu-id="ce70a-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ce70a-132">Назад</span><span class="sxs-lookup"><span data-stu-id="ce70a-132">Previous</span></span>](tailspin-spyworks-part-7.md)
