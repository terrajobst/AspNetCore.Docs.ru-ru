---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Часть 6: С помощью заметок к данным для проверки модели | Документы Microsoft'
author: jongalloway
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store. Часть 6 описываются с помощью заметок к данным для модели V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="b5bba-104">Часть 6: С помощью заметок к данным для проверки модели</span><span class="sxs-lookup"><span data-stu-id="b5bba-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="b5bba-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b5bba-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b5bba-106">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="b5bba-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b5bba-107">MVC Music Store показана реализация хранилища упрощенных образец продает велосипеды через Интернет альбомы, которое реализует Администрирование сайта основные, вход пользователей и функциональные возможности корзины покупок.</span><span class="sxs-lookup"><span data-stu-id="b5bba-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="b5bba-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="b5bba-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b5bba-109">Часть 6 описываются с помощью заметок к данным для проверки модели.</span><span class="sxs-lookup"><span data-stu-id="b5bba-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="b5bba-110">У нас есть основная проблема, связанная с нашей Создание и изменение форм: они не занимаются проверку.</span><span class="sxs-lookup"><span data-stu-id="b5bba-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="b5bba-111">Нам как оставить пустым обязательные поля или типа буквы в поле «Цена» и является первой ошибки, которые мы увидим из базы данных.</span><span class="sxs-lookup"><span data-stu-id="b5bba-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="b5bba-112">Мы легко добавить проверку нашего приложения путем добавления заметок к данным для наших классов модели.</span><span class="sxs-lookup"><span data-stu-id="b5bba-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="b5bba-113">Заметок к данным позволяют описываются правила, которые требуется применить к нашей свойства модели и ASP.NET MVC берет на себя применения их и отображать соответствующие сообщения пользователям.</span><span class="sxs-lookup"><span data-stu-id="b5bba-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="b5bba-114">Добавление проверки в нашем альбом формы</span><span class="sxs-lookup"><span data-stu-id="b5bba-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="b5bba-115">Мы будем использовать следующие атрибуты заметки к данным:</span><span class="sxs-lookup"><span data-stu-id="b5bba-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="b5bba-116">**Требуется** — указывает, что свойство является обязательным полем</span><span class="sxs-lookup"><span data-stu-id="b5bba-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="b5bba-117">**DisplayName** — определяет текст, мы хотим использовать поля формы и проверка сообщений</span><span class="sxs-lookup"><span data-stu-id="b5bba-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="b5bba-118">**StringLength** — определяет максимальную длину для поля строк</span><span class="sxs-lookup"><span data-stu-id="b5bba-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="b5bba-119">**Диапазон** — обеспечивает минимальное и максимальное значение для числового поля</span><span class="sxs-lookup"><span data-stu-id="b5bba-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="b5bba-120">**Привязать** — перечисляет поля, чтобы включить или исключить при привязке значения параметра или форму для свойств модели</span><span class="sxs-lookup"><span data-stu-id="b5bba-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="b5bba-121">**ScaffoldColumn** — позволяет скрытие полей из редактор форм</span><span class="sxs-lookup"><span data-stu-id="b5bba-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="b5bba-122">*Примечание: Дополнительные сведения о проверке модели, используя атрибуты заметки к данным см. в разделе документации MSDN по адресу*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="b5bba-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="b5bba-123">Откройте класс альбом и добавьте следующие *с помощью* в начале.</span><span class="sxs-lookup"><span data-stu-id="b5bba-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="b5bba-124">Затем обновите свойства, чтобы добавить атрибуты отображения и проверки, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="b5bba-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="b5bba-125">Хотя мы существует, мы также открыли жанр и исполнитель виртуальные свойства.</span><span class="sxs-lookup"><span data-stu-id="b5bba-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="b5bba-126">Это позволяет Entity Framework отложенной загрузки их при необходимости.</span><span class="sxs-lookup"><span data-stu-id="b5bba-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="b5bba-127">После после добавления этих атрибутов для нашей модели альбома, экраном Создание и изменение немедленно начать проверку поля и отображаемые имена с помощью мы выбрали (например альбом Art URL-адрес вместо AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="b5bba-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="b5bba-128">Запустите приложение и перейдите к /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="b5bba-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="b5bba-129">Далее мы будем прерывать несколько правил проверки.</span><span class="sxs-lookup"><span data-stu-id="b5bba-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="b5bba-130">Введите цены 0 и не указывайте заголовок.</span><span class="sxs-lookup"><span data-stu-id="b5bba-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="b5bba-131">При нажатии кнопки создания, мы увидим, что поле формы проверки сообщения об ошибках, показывающая, какие поля не соответствует правилам проверки определенных.</span><span class="sxs-lookup"><span data-stu-id="b5bba-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="b5bba-132">Тестирование проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="b5bba-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="b5bba-133">Проверка на стороне сервера очень важно с точки зрения приложения, так как пользователи могут обходить проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="b5bba-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="b5bba-134">Однако веб-страницы формы, которые только реализации проверки на стороне сервера соответствуют три значительные проблемы.</span><span class="sxs-lookup"><span data-stu-id="b5bba-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="b5bba-135">Пользователь должен ожидать формы должна быть учтена и проверке на сервере, а для ответов, отправляемых в своем браузере.</span><span class="sxs-lookup"><span data-stu-id="b5bba-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="b5bba-136">Пользователь не получить оперативно реагировать при их исправить поле теперь проходить правила проверки.</span><span class="sxs-lookup"><span data-stu-id="b5bba-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="b5bba-137">Мы лишних затрат сэкономить ресурсы сервера, чтобы выполнить логику проверки, вместо использования браузер пользователя.</span><span class="sxs-lookup"><span data-stu-id="b5bba-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="b5bba-138">К счастью ASP.NET MVC 3 представление формирования шаблонов имеют клиентской проверки встроенным, требуя никаких дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="b5bba-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="b5bba-139">Введя одну букву в поле "Название" удовлетворяет требованиям проверки, поэтому сообщение проверки немедленно удаляется.</span><span class="sxs-lookup"><span data-stu-id="b5bba-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="b5bba-140">[Назад](mvc-music-store-part-5.md)
> [Вперед](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="b5bba-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
