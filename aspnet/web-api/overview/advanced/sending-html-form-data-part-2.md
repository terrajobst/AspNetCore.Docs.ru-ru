---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: "Отправка данных формы HTML в веб-API ASP.NET: файл передачи и составного MIME | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="703a9-102">Отправка данных формы HTML в веб-API ASP.NET: файл передачи и составного MIME</span><span class="sxs-lookup"><span data-stu-id="703a9-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="703a9-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="703a9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="703a9-104">Часть 2: Отправка файла и составного MIME</span><span class="sxs-lookup"><span data-stu-id="703a9-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="703a9-105">Этот учебник показывает, как для передачи файлов в веб-API.</span><span class="sxs-lookup"><span data-stu-id="703a9-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="703a9-106">Также описывается обработка составных данных MIME.</span><span class="sxs-lookup"><span data-stu-id="703a9-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="703a9-107">[Загрузка завершенного проекта](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="703a9-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="703a9-108">Ниже приведен пример формы HTML для передачи файла:</span><span class="sxs-lookup"><span data-stu-id="703a9-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="703a9-109">Эта форма содержит управления для ввода текста и элемент управления входного файла.</span><span class="sxs-lookup"><span data-stu-id="703a9-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="703a9-110">Когда форма содержит элемент управления ввода файла **enctype** атрибут всегда должен иметь &quot;multipart/формы data&quot;, которое указывает, что формы будут отправляться как составного сообщения MIME.</span><span class="sxs-lookup"><span data-stu-id="703a9-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="703a9-111">Формат составного сообщения MIME можно легко понять, просмотрев пример запроса:</span><span class="sxs-lookup"><span data-stu-id="703a9-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="703a9-112">Это сообщение состоит из двух *частей*, один для каждого элемента управления.</span><span class="sxs-lookup"><span data-stu-id="703a9-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="703a9-113">Часть границы обозначаются строки, начинающиеся с тире.</span><span class="sxs-lookup"><span data-stu-id="703a9-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="703a9-114">Граница часть включает в себя случайных компонент (&quot;41184676334&quot;) для того что строка границ не отображалось случайно внутри части сообщения.</span><span class="sxs-lookup"><span data-stu-id="703a9-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="703a9-115">Каждая часть сообщения содержит один или несколько заголовков, следуют содержимое части.</span><span class="sxs-lookup"><span data-stu-id="703a9-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="703a9-116">Заголовок Content-Disposition включает в себя имя элемента управления.</span><span class="sxs-lookup"><span data-stu-id="703a9-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="703a9-117">Для файлов он также содержит имя файла.</span><span class="sxs-lookup"><span data-stu-id="703a9-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="703a9-118">Заголовок Content-Type описывает данные в части.</span><span class="sxs-lookup"><span data-stu-id="703a9-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="703a9-119">Если этот заголовок указан, значение по умолчанию — text/plain.</span><span class="sxs-lookup"><span data-stu-id="703a9-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="703a9-120">В предыдущем примере пользователь загрузить файл с именем GrandCanyon.jpg, с типом содержимого image/jpeg; значение текстового ввода &quot;отпуск&quot;.</span><span class="sxs-lookup"><span data-stu-id="703a9-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="703a9-121">Отправка файла</span><span class="sxs-lookup"><span data-stu-id="703a9-121">File Upload</span></span>

<span data-ttu-id="703a9-122">Теперь давайте взглянем на контроллер веб-API, который считывает файлы из составного сообщения MIME.</span><span class="sxs-lookup"><span data-stu-id="703a9-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="703a9-123">Контроллер будет читать файлы асинхронно.</span><span class="sxs-lookup"><span data-stu-id="703a9-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="703a9-124">Веб-API поддерживает асинхронные операции с помощью [модель программирования на основе задач](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="703a9-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="703a9-125">Во-первых, вот код, если вы используете .NET Framework 4.5, который поддерживает **async** и **await** ключевые слова.</span><span class="sxs-lookup"><span data-stu-id="703a9-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="703a9-126">Обратите внимание, что действие контроллера не принимает никаких параметров.</span><span class="sxs-lookup"><span data-stu-id="703a9-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="703a9-127">Это, так как мы можем обработать тело запроса внутри действия, без вызова форматирования типа мультимедиа.</span><span class="sxs-lookup"><span data-stu-id="703a9-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="703a9-128">**IsMultipartContent** метод проверяет, содержит ли запрос составного сообщения MIME.</span><span class="sxs-lookup"><span data-stu-id="703a9-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="703a9-129">В противном случае возвращает код состояния HTTP 415 (неподдерживаемый тип носителя).</span><span class="sxs-lookup"><span data-stu-id="703a9-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="703a9-130">**MultipartFormDataStreamProvider** класс представляет вспомогательный объект, который выделяет для отправляемых файлов файлового потока.</span><span class="sxs-lookup"><span data-stu-id="703a9-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="703a9-131">Для считывания составного сообщения MIME следует вызвать **ReadAsMultipartAsync** метод.</span><span class="sxs-lookup"><span data-stu-id="703a9-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="703a9-132">Этот метод извлекает все части сообщения и записывает их в потоков, предоставляемых **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="703a9-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="703a9-133">При завершении метода, можно получить сведения о файлах из **FileData** свойство, которое представляет собой коллекцию из **MultipartFileData** объектов.</span><span class="sxs-lookup"><span data-stu-id="703a9-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="703a9-134">**MultipartFileData.FileName** — это имя локального файла на сервере, где был сохранен файл.</span><span class="sxs-lookup"><span data-stu-id="703a9-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="703a9-135">**MultipartFileData.Headers** содержит часть заголовка (*не* в заголовке запроса).</span><span class="sxs-lookup"><span data-stu-id="703a9-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="703a9-136">Это можно использовать для доступа к содержимому\_заголовки расстановки и Content-Type.</span><span class="sxs-lookup"><span data-stu-id="703a9-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="703a9-137">Как видно из имени, **ReadAsMultipartAsync** является асинхронным методом.</span><span class="sxs-lookup"><span data-stu-id="703a9-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="703a9-138">Для выполнения работы, после завершения работы метода, используйте [задача продолжения](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) или **await** ключевое слово (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="703a9-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="703a9-139">Ниже приведена версия .NET Framework 4.0 приведенного выше кода:</span><span class="sxs-lookup"><span data-stu-id="703a9-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="703a9-140">Чтение данных элемента управления формы</span><span class="sxs-lookup"><span data-stu-id="703a9-140">Reading Form Control Data</span></span>

<span data-ttu-id="703a9-141">HTML-формы, показанного выше было управления для ввода текста.</span><span class="sxs-lookup"><span data-stu-id="703a9-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="703a9-142">Можно получить значение из элемента управления **FormData** свойство **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="703a9-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="703a9-143">**FormData** — **NameValueCollection** , содержащий пары имя значение для элементов управления формы.</span><span class="sxs-lookup"><span data-stu-id="703a9-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="703a9-144">Коллекция может содержать повторяющиеся ключи.</span><span class="sxs-lookup"><span data-stu-id="703a9-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="703a9-145">Рассмотрим следующую форму:</span><span class="sxs-lookup"><span data-stu-id="703a9-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="703a9-146">Текст запроса может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="703a9-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="703a9-147">В этом случае **FormData** коллекция будет содержать следующие пары ключ значение:</span><span class="sxs-lookup"><span data-stu-id="703a9-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="703a9-148">маршрут: приема-передачи</span><span class="sxs-lookup"><span data-stu-id="703a9-148">trip: round-trip</span></span>
- <span data-ttu-id="703a9-149">параметры: nonstop</span><span class="sxs-lookup"><span data-stu-id="703a9-149">options: nonstop</span></span>
- <span data-ttu-id="703a9-150">параметры: дат</span><span class="sxs-lookup"><span data-stu-id="703a9-150">options: dates</span></span>
- <span data-ttu-id="703a9-151">рабочее место: окна</span><span class="sxs-lookup"><span data-stu-id="703a9-151">seat: window</span></span>
