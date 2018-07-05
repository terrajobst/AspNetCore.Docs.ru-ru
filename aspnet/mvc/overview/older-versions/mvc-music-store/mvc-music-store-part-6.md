---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Часть 6: Использование заметок к данным для проверки модели | Документация Майкрософт'
author: jongalloway
description: В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store. Часть 6 рассматриваются с использованием заметок к данным для модели V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: ca0ac2ca909f838f1c91e6cc01b8aafa90c0b193
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397661"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="2197e-104">Часть 6: Использование заметок к данным для проверки модели</span><span class="sxs-lookup"><span data-stu-id="2197e-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="2197e-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="2197e-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="2197e-106">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые использование ASP.NET MVC и Visual Studio для разработки веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="2197e-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="2197e-107">Store музыки MVC — это реализация хранилища упрощенный пример, который продает музыкальные альбомы через Интернет и реализует базовый сайт администрирования, вход пользователя и корзины для покупок функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="2197e-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="2197e-108">В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="2197e-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="2197e-109">Часть 6 рассматривается, с помощью заметок к данным для проверки модели.</span><span class="sxs-lookup"><span data-stu-id="2197e-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="2197e-110">У нас есть основная проблема, связанная с формами наших Create и Edit: они не выполняют никакой проверки.</span><span class="sxs-lookup"><span data-stu-id="2197e-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="2197e-111">Мы можем такие вещи, как оставить обязательные поля пустым или тип буквы в поле «Цена» и является первой ошибки, мы увидим из базы данных.</span><span class="sxs-lookup"><span data-stu-id="2197e-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="2197e-112">Мы легко можно добавить проверку к нашему приложению путем добавления заметок к данным в классах модели.</span><span class="sxs-lookup"><span data-stu-id="2197e-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="2197e-113">Заметки к данным позволяют описывать правила, которые мы хотим, примененным к свойствам нашей модели и ASP.NET MVC позаботится о принудительно используя их и отображение соответствующего сообщения для наших пользователей.</span><span class="sxs-lookup"><span data-stu-id="2197e-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="2197e-114">Добавление проверки форм наших альбома</span><span class="sxs-lookup"><span data-stu-id="2197e-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="2197e-115">Мы будем использовать следующие атрибуты заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="2197e-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="2197e-116">**Требуется** — указывает, что свойство является обязательным полем</span><span class="sxs-lookup"><span data-stu-id="2197e-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="2197e-117">**DisplayName** — определяет текст, нам нужно использовать на поля формы и проверка сообщений</span><span class="sxs-lookup"><span data-stu-id="2197e-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="2197e-118">**StringLength** — определяет максимальную длину для строкового поля</span><span class="sxs-lookup"><span data-stu-id="2197e-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="2197e-119">**Диапазон** — обеспечивает максимальное и минимальное значение для числового поля</span><span class="sxs-lookup"><span data-stu-id="2197e-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="2197e-120">**Привязать** — перечислены поля, исключать или включать при привязке значения параметра или формы к свойствам модели</span><span class="sxs-lookup"><span data-stu-id="2197e-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="2197e-121">**ScaffoldColumn** — позволяет скрывать поля из редактор форм</span><span class="sxs-lookup"><span data-stu-id="2197e-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="2197e-122">*Примечание: Дополнительные сведения о проверке модели, используя атрибуты заметок к данным, см. в разделе документации MSDN по адресу*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="2197e-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="2197e-123">Откройте класс альбома и добавьте следующие *с помощью* инструкции в начало.</span><span class="sxs-lookup"><span data-stu-id="2197e-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="2197e-124">Затем обновите свойства, чтобы добавить атрибуты отображения и проверки, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="2197e-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="2197e-125">Кроме того, мы изменили также жанр и имя исполнителя на виртуальные свойства.</span><span class="sxs-lookup"><span data-stu-id="2197e-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="2197e-126">Это позволяет платформе Entity Framework отложенной загрузки их при необходимости.</span><span class="sxs-lookup"><span data-stu-id="2197e-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="2197e-127">После после добавления этих атрибутов для нашей модели альбома, наш экран Create и Edit сразу же начинает проверять поля, и с помощью отображаемые имена мы выбрали (например альбома Art URL-адрес вместо AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="2197e-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="2197e-128">Запустите приложение и перейдите к /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="2197e-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="2197e-129">Далее мы будем нарушить некоторые правила проверки.</span><span class="sxs-lookup"><span data-stu-id="2197e-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="2197e-130">Введите цены 0 и не указывайте заголовок.</span><span class="sxs-lookup"><span data-stu-id="2197e-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="2197e-131">Если щелкнуть "Создать", мы увидим форму с помощью сообщений об ошибках проверки отображаются поля, которые не соответствует правилам проверки определенных.</span><span class="sxs-lookup"><span data-stu-id="2197e-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="2197e-132">Тестирование проверки на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="2197e-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="2197e-133">Проверка на стороне сервера очень важно с точки зрения приложения, так как пользователи могут обойти проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="2197e-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="2197e-134">Однако веб-страницы формы, которые реализуют только проверки на стороне сервера соответствуют три существенные проблемы.</span><span class="sxs-lookup"><span data-stu-id="2197e-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="2197e-135">Пользователь имеет ожидания для формы должна быть учтена и проверены на сервере и для ответа на который должны отправляться свой браузер.</span><span class="sxs-lookup"><span data-stu-id="2197e-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="2197e-136">Пользователь не получать немедленные отзывы, когда он исправляет поле, так что теперь он передает правила проверки.</span><span class="sxs-lookup"><span data-stu-id="2197e-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="2197e-137">Мы тратит ресурсы сервера, чтобы выполнить логику проверки, а не используя браузер пользователя.</span><span class="sxs-lookup"><span data-stu-id="2197e-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="2197e-138">К счастью способы формирования шаблонов ASP.NET MVC 3 имеют клиентскую проверку встроенными, требующие дополнительных действий ни при каких обстоятельствах.</span><span class="sxs-lookup"><span data-stu-id="2197e-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="2197e-139">Удовлетворяет требованиям проверки, введя одну букву в поле заголовка, поэтому сообщение проверки немедленно удаляется.</span><span class="sxs-lookup"><span data-stu-id="2197e-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="2197e-140">[Назад](mvc-music-store-part-5.md)
> [Вперед](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="2197e-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
