---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "Отправка данных формы HTML в веб-API ASP.NET: данные формы urlencoded | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="c0885-102">Отправка данных формы HTML в веб-API ASP.NET: данные формы urlencoded.</span><span class="sxs-lookup"><span data-stu-id="c0885-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="c0885-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c0885-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="c0885-104">Часть 1: Данные формы urlencoded</span><span class="sxs-lookup"><span data-stu-id="c0885-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="c0885-105">В этой статье показано, как для отправки данных формы urlencoded контроллер веб-API.</span><span class="sxs-lookup"><span data-stu-id="c0885-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="c0885-106">Общие сведения об HTML-формы</span><span class="sxs-lookup"><span data-stu-id="c0885-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="c0885-107">Отправка сложных типов</span><span class="sxs-lookup"><span data-stu-id="c0885-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="c0885-108">Отправка данных по AJAX</span><span class="sxs-lookup"><span data-stu-id="c0885-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="c0885-109">Отправка простых типов</span><span class="sxs-lookup"><span data-stu-id="c0885-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="c0885-110">[Загрузка завершенного проекта](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="c0885-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="c0885-111">Общие сведения об HTML-формы</span><span class="sxs-lookup"><span data-stu-id="c0885-111">Overview of HTML Forms</span></span>

<span data-ttu-id="c0885-112">Использование форм HTML GET или POST для отправки данных на сервер.</span><span class="sxs-lookup"><span data-stu-id="c0885-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="c0885-113">**Метод** атрибут **формы** элемента предоставляет метод HTTP:</span><span class="sxs-lookup"><span data-stu-id="c0885-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="c0885-114">Метод по умолчанию — GET.</span><span class="sxs-lookup"><span data-stu-id="c0885-114">The default method is GET.</span></span> <span data-ttu-id="c0885-115">Если форма использует GET, формы, в которой данные кодируются в виде строки запроса URI.</span><span class="sxs-lookup"><span data-stu-id="c0885-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="c0885-116">Если в форме используется POST, данные формы помещается в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="c0885-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="c0885-117">Для отправки данных **enctype** атрибут указывает формат текста запроса:</span><span class="sxs-lookup"><span data-stu-id="c0885-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="c0885-118">Enctype</span><span class="sxs-lookup"><span data-stu-id="c0885-118">enctype</span></span> | <span data-ttu-id="c0885-119">Описание</span><span class="sxs-lookup"><span data-stu-id="c0885-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c0885-120">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="c0885-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="c0885-121">Данные формы кодируются как пары имя значение, аналогично строку запроса URI.</span><span class="sxs-lookup"><span data-stu-id="c0885-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="c0885-122">Это формат по умолчанию для POST.</span><span class="sxs-lookup"><span data-stu-id="c0885-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="c0885-123">данные multipart/формы</span><span class="sxs-lookup"><span data-stu-id="c0885-123">multipart/form-data</span></span> | <span data-ttu-id="c0885-124">Данные формы кодируются как составного сообщения MIME.</span><span class="sxs-lookup"><span data-stu-id="c0885-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="c0885-125">Этот формат следует используйте при отправке файла на сервер.</span><span class="sxs-lookup"><span data-stu-id="c0885-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="c0885-126">Часть 1 из этой статьи рассматривается x-www-формы-urlencoded формата.</span><span class="sxs-lookup"><span data-stu-id="c0885-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="c0885-127">[Часть 2](sending-html-form-data-part-2.md) описывает составного MIME.</span><span class="sxs-lookup"><span data-stu-id="c0885-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="c0885-128">Отправка сложных типов</span><span class="sxs-lookup"><span data-stu-id="c0885-128">Sending Complex Types</span></span>

<span data-ttu-id="c0885-129">Как правило будет отправлять сложный тип, состоящий из значениями, взятыми из нескольких элементов управления формы.</span><span class="sxs-lookup"><span data-stu-id="c0885-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="c0885-130">Рассмотрим следующую модель, представляющий обновление состояния:</span><span class="sxs-lookup"><span data-stu-id="c0885-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="c0885-131">Вот контроллер веб-API, который принимает `Update` объекта с помощью POST.</span><span class="sxs-lookup"><span data-stu-id="c0885-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="c0885-132">Использует этот контроллер [маршрутизацию на основе действия](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), поэтому шаблон маршрута &quot;api / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="c0885-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="c0885-133">Клиент будет отправлять данные в &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="c0885-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="c0885-134">Теперь напишем HTML-формы для пользователям отправлять обновления состояния.</span><span class="sxs-lookup"><span data-stu-id="c0885-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="c0885-135">Обратите внимание, что **действия** атрибута формы представляет собой URI наши действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="c0885-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="c0885-136">Вот некоторые значения, введенные в форму.</span><span class="sxs-lookup"><span data-stu-id="c0885-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="c0885-137">Когда пользователь щелкает отправки, браузер отправляет запрос HTTP следующего вида:</span><span class="sxs-lookup"><span data-stu-id="c0885-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="c0885-138">Обратите внимание, что текст запроса содержит данные формы, в формате пар «имя значение».</span><span class="sxs-lookup"><span data-stu-id="c0885-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="c0885-139">Веб-API автоматически преобразует пары «имя значение» в экземпляре `Update` класса.</span><span class="sxs-lookup"><span data-stu-id="c0885-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="c0885-140">Отправка данных по AJAX</span><span class="sxs-lookup"><span data-stu-id="c0885-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="c0885-141">Когда пользователь отправляет форму, браузер выходит из текущей страницы и отображает текст сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="c0885-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="c0885-142">Это нормально, когда ответ является HTML-страницы.</span><span class="sxs-lookup"><span data-stu-id="c0885-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="c0885-143">С веб-API, однако текст ответа обычно является либо пуст или содержит структурированные данные, такие как JSON.</span><span class="sxs-lookup"><span data-stu-id="c0885-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="c0885-144">В этом случае смысл для отправки данных формы, с помощью AJAX для запросов, чтобы страницу можно обработать ответ.</span><span class="sxs-lookup"><span data-stu-id="c0885-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="c0885-145">Ниже показано, как при отправке данных формы с использованием библиотеки jQuery.</span><span class="sxs-lookup"><span data-stu-id="c0885-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="c0885-146">JQuery **отправить** функция заменяет действий формы с помощью новой функции.</span><span class="sxs-lookup"><span data-stu-id="c0885-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="c0885-147">Это переопределяет поведение по умолчанию для кнопки «Отправить».</span><span class="sxs-lookup"><span data-stu-id="c0885-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="c0885-148">**Сериализации** функция сериализует данные формы в пары "имя значение".</span><span class="sxs-lookup"><span data-stu-id="c0885-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="c0885-149">Чтобы отправить данные формы на сервер, вызовите `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="c0885-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="c0885-150">По завершении выполнения запроса `.success()` или `.error()` обработчик отображается соответствующее сообщение для пользователя.</span><span class="sxs-lookup"><span data-stu-id="c0885-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="c0885-151">Отправка простых типов</span><span class="sxs-lookup"><span data-stu-id="c0885-151">Sending Simple Types</span></span>

<span data-ttu-id="c0885-152">В предыдущих разделах мы отправили сложный тип, который десериализуется экземпляром класса модели веб-API.</span><span class="sxs-lookup"><span data-stu-id="c0885-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="c0885-153">Можно также отправлять простые типы, такие как строка.</span><span class="sxs-lookup"><span data-stu-id="c0885-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="c0885-154">Перед отправкой простого типа, рассмотрите возможность вместо упаковки значение в сложном типе.</span><span class="sxs-lookup"><span data-stu-id="c0885-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="c0885-155">Это дает преимущества проверки модели на стороне сервера и упрощает для расширения модели при необходимости.</span><span class="sxs-lookup"><span data-stu-id="c0885-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="c0885-156">Основные шаги для отправки простого типа одинаковы, но два незначительных отличий.</span><span class="sxs-lookup"><span data-stu-id="c0885-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="c0885-157">Во-первых, в контроллере, необходимо дополнить имя параметра с **FromBody** атрибута.</span><span class="sxs-lookup"><span data-stu-id="c0885-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="c0885-158">По умолчанию веб-API предпринимает попытку получения простых типов от URI запроса.</span><span class="sxs-lookup"><span data-stu-id="c0885-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="c0885-159">**FromBody** атрибут сообщает веб-API для чтения значения из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="c0885-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="c0885-160">Веб-API считывает текст ответа и не более одного раза, только один параметр действия могут поступать из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="c0885-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="c0885-161">Если вам нужно получить несколько значений из текста запроса, определите сложный тип.</span><span class="sxs-lookup"><span data-stu-id="c0885-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="c0885-162">Во-вторых клиенту необходимо отправить значение в следующем формате:</span><span class="sxs-lookup"><span data-stu-id="c0885-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="c0885-163">В частности часть пары имя/значение должно быть пустым для простого типа.</span><span class="sxs-lookup"><span data-stu-id="c0885-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="c0885-164">Не все браузеры поддерживают это для HTML-форм, но создать этот формат в скрипте следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c0885-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="c0885-165">Ниже приведен пример формы.</span><span class="sxs-lookup"><span data-stu-id="c0885-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="c0885-166">А вот сценарий, чтобы отправить значение формы.</span><span class="sxs-lookup"><span data-stu-id="c0885-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="c0885-167">Единственное отличие от предыдущего сценария — это аргумент, переданный в **учет** функции.</span><span class="sxs-lookup"><span data-stu-id="c0885-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="c0885-168">Тот же подход можно использовать для отправки массив простых типов:</span><span class="sxs-lookup"><span data-stu-id="c0885-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="c0885-169">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c0885-169">Additional Resources</span></span>

[<span data-ttu-id="c0885-170">Часть 2: Отправка файла и составного MIME</span><span class="sxs-lookup"><span data-stu-id="c0885-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
