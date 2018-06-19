---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Часть 8: Последней страницы, обработки исключений и выводу | Документы Microsoft'
author: JoeStagner
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 8 добавляет страницы контактов, о странице и исключение...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: f82294aab0616012393cf3e10f932f6d1ad0cdb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886275"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="3cfb5-104">Часть 8: Последней страницы, обработки исключений и выводу</span><span class="sxs-lookup"><span data-stu-id="3cfb5-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="3cfb5-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3cfb5-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="3cfb5-106">Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="3cfb5-107">Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="3cfb5-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="3cfb5-109">Часть 8 добавляет страницы контактов, о странице и обработка исключений.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="3cfb5-110">Это заключение ряда.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="3cfb5-111">Обратитесь к странице (отправка по электронной почте из ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="3cfb5-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="3cfb5-112">Создание новой страницы с именем ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="3cfb5-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="3cfb5-113">С помощью конструктора, создайте следующую форму, выполнив особое Примечание для включения ToolkitScriptManager и элемент управления редактора, AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="3cfb5-114">.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="3cfb5-115">Дважды щелкните кнопку «Отправить», чтобы создать обработчик событий click в файле кода программной части и реализуйте метод, чтобы отправить контактные данные в виде сообщения электронной почты.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="3cfb5-116">Этот код требует, что ваш файл web.config содержит запись в раздел конфигурации, который указывает SMTP-сервера для отправки почты.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="3cfb5-117">О странице</span><span class="sxs-lookup"><span data-stu-id="3cfb5-117">About Page</span></span>

<span data-ttu-id="3cfb5-118">Создайте страницу с именем AboutUs.aspx и добавьте любое содержимое, вам нравится.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="3cfb5-119">Глобального обработчика исключений</span><span class="sxs-lookup"><span data-stu-id="3cfb5-119">Global Exception Handler</span></span>

<span data-ttu-id="3cfb5-120">Наконец в приложении возникает исключения при этом непредвиденных обстоятельств, холодного причина необработанных исключений в наш веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="3cfb5-121">Мы никогда не будут необрабатываемое исключение отображаться для посетителей веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="3cfb5-122">Помимо того, что взаимодействие с пользователем ужасных необработанные исключения также может быть проблема безопасности.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="3cfb5-123">Чтобы решить эту проблему мы реализуем глобального обработчика исключений.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="3cfb5-124">Чтобы сделать это, откройте файл Global.asax и запишите следующий обработчик событий, предварительно созданный.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="3cfb5-125">Добавьте код для реализации приложения\_обработчик ошибок, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="3cfb5-126">Затем добавьте страницу с именем Error.aspx в решение и добавьте этот фрагмент кода разметки.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="3cfb5-127">Теперь на странице\_загрузить извлечения обработчика событий, сообщения об ошибках из объекта запроса.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="3cfb5-128">Заключение</span><span class="sxs-lookup"><span data-stu-id="3cfb5-128">Conclusion</span></span>

<span data-ttu-id="3cfb5-129">Мы узнали, что веб-форм ASP.NET упрощает создание сложных веб-сайта с помощью доступа к базе данных, членство в AJAX, и т. д.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="3cfb5-130">довольно быстро.</span><span class="sxs-lookup"><span data-stu-id="3cfb5-130">pretty quickly.</span></span>

<span data-ttu-id="3cfb5-131">Надеемся, у этого учебника вы получили средства, необходимые для начала создания приложений для собственных веб-форм ASP.NET!</span><span class="sxs-lookup"><span data-stu-id="3cfb5-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3cfb5-132">Назад</span><span class="sxs-lookup"><span data-stu-id="3cfb5-132">Previous</span></span>](tailspin-spyworks-part-7.md)
