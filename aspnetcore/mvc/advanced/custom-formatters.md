---
title: "Пользовательские модули форматирования в ASP.NET Core MVC веб-API"
author: tdykstra
description: "Узнайте, как создавать и использовать пользовательские модули форматирования для веб-API в ASP.NET Core."
keywords: "ASP.NET Core веб-api, пользовательские модули форматирования"
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 5e665abe10fd7444c3fd5f20cfeca3ef0a5f79d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="748c1-104">Пользовательские модули форматирования в ASP.NET Core MVC веб-API</span><span class="sxs-lookup"><span data-stu-id="748c1-104">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="748c1-105">По [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="748c1-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="748c1-106">ASP.NET Core MVC имеет встроенную поддержку для обмена данными в веб-API с помощью формата JSON, XML или обычного текста.</span><span class="sxs-lookup"><span data-stu-id="748c1-106">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="748c1-107">В этой статье показано, как добавить поддержку дополнительных форматов, создав пользовательские модули форматирования.</span><span class="sxs-lookup"><span data-stu-id="748c1-107">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="748c1-108">[Просмотреть или загрузить пример из GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="748c1-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="748c1-109">Когда следует использовать пользовательские модули форматирования</span><span class="sxs-lookup"><span data-stu-id="748c1-109">When to use custom formatters</span></span>

<span data-ttu-id="748c1-110">Использовать пользовательский модуль форматирования [содержимого согласования](xref:mvc/models/formatting) процесса для поддержки типа содержимого, которое не поддерживается в встроенные модули форматирования (JSON, XML и обычный текст).</span><span class="sxs-lookup"><span data-stu-id="748c1-110">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="748c1-111">Например, если некоторые из клиентов для веб-API может обрабатывать [Protobuf](https://github.com/google/protobuf) формат, может потребоваться использовать Protobuf этими клиентами, поскольку он является более эффективным.</span><span class="sxs-lookup"><span data-stu-id="748c1-111">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="748c1-112">Либо можно использовать веб-API для отправки имена и адреса в [vCard](https://wikipedia.org/wiki/VCard) формат, широко распространенный формат для обмена контактных данных.</span><span class="sxs-lookup"><span data-stu-id="748c1-112">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="748c1-113">Пример приложения, описываемых в этой статье реализует простой vCard модуль форматирования.</span><span class="sxs-lookup"><span data-stu-id="748c1-113">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="748c1-114">Общие сведения о том, как использовать пользовательский модуль форматирования</span><span class="sxs-lookup"><span data-stu-id="748c1-114">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="748c1-115">Ниже приведены шаги, чтобы создать и использовать пользовательский модуль форматирования.</span><span class="sxs-lookup"><span data-stu-id="748c1-115">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="748c1-116">Создайте класс модуля форматирования выходных данных, если необходимо сериализовать данные для отправки клиенту.</span><span class="sxs-lookup"><span data-stu-id="748c1-116">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="748c1-117">Создание класса модуль форматирования входных данных, если вы хотите выполнить десериализацию данных, полученных от клиента.</span><span class="sxs-lookup"><span data-stu-id="748c1-117">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="748c1-118">Добавить экземпляры к модули форматирования `InputFormatters` и `OutputFormatters` коллекций в [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="748c1-118">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="748c1-119">В следующих разделах руководства и примеры кода для каждого из этих действий.</span><span class="sxs-lookup"><span data-stu-id="748c1-119">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="748c1-120">Как создать класс пользовательский модуль форматирования</span><span class="sxs-lookup"><span data-stu-id="748c1-120">How to create a custom formatter class</span></span>

<span data-ttu-id="748c1-121">Чтобы создать модуль форматирования:</span><span class="sxs-lookup"><span data-stu-id="748c1-121">To create a formatter:</span></span>

* <span data-ttu-id="748c1-122">Создайте класс, производный от соответствующего базового класса.</span><span class="sxs-lookup"><span data-stu-id="748c1-122">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="748c1-123">Укажите допустимый носитель типов и кодировок в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="748c1-123">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="748c1-124">Переопределить `CanReadType` / `CanWriteType` методы</span><span class="sxs-lookup"><span data-stu-id="748c1-124">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="748c1-125">Переопределить `ReadRequestBodyAsync` / `WriteResponseBodyAsync` методы</span><span class="sxs-lookup"><span data-stu-id="748c1-125">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="748c1-126">Является производным от соответствующего базового класса</span><span class="sxs-lookup"><span data-stu-id="748c1-126">Derive from the appropriate base class</span></span>

<span data-ttu-id="748c1-127">Для текстовых типов носителей (например, vCard), являются производными от [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) или [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) базового класса.</span><span class="sxs-lookup"><span data-stu-id="748c1-127">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="748c1-128">Двоичные типы являются производными от [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) или [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) базового класса.</span><span class="sxs-lookup"><span data-stu-id="748c1-128">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="748c1-129">Укажите допустимый носитель типы и кодировки</span><span class="sxs-lookup"><span data-stu-id="748c1-129">Specify valid media types and encodings</span></span>

<span data-ttu-id="748c1-130">В конструкторе, укажите допустимый носитель типов и кодировок, добавив `SupportedMediaTypes` и `SupportedEncodings` коллекции.</span><span class="sxs-lookup"><span data-stu-id="748c1-130">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> <span data-ttu-id="748c1-131">Внедрение зависимостей конструктор в класс модуля форматирования не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="748c1-131">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="748c1-132">Например средство ведения журнала не удается получить, добавив параметр ведения журнала в конструктор.</span><span class="sxs-lookup"><span data-stu-id="748c1-132">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="748c1-133">Для доступа к службам, необходимо использовать объект context, переданного в методы.</span><span class="sxs-lookup"><span data-stu-id="748c1-133">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="748c1-134">Пример кода [ниже](#read-write) показано, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="748c1-134">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="748c1-135">Переопределить CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="748c1-135">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="748c1-136">Укажите тип можно десериализовать в или сериализации из путем переопределения `CanReadType` или `CanWriteType` методы.</span><span class="sxs-lookup"><span data-stu-id="748c1-136">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="748c1-137">Например, можно только будет создание vCard текст из `Contact` и наоборот.</span><span class="sxs-lookup"><span data-stu-id="748c1-137">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="748c1-138">Метод CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="748c1-138">The CanWriteResult method</span></span>

<span data-ttu-id="748c1-139">В некоторых сценариях необходимо переопределить `CanWriteResult` вместо `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="748c1-139">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="748c1-140">Используйте `CanWriteResult` , если выполняются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="748c1-140">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="748c1-141">Метод действия возвращает класс модели.</span><span class="sxs-lookup"><span data-stu-id="748c1-141">Your action method returns a model class.</span></span>
  * <span data-ttu-id="748c1-142">Существуют производные классы, которые могут возвращаться во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="748c1-142">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="748c1-143">Необходимо знать во время выполнения, который производным классом, возвращаемого действием.</span><span class="sxs-lookup"><span data-stu-id="748c1-143">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="748c1-144">Предположим, что подпись метода действие возвращает `Person` типа, но он может возвращать `Student` или `Instructor` тип, производный от `Person`.</span><span class="sxs-lookup"><span data-stu-id="748c1-144">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="748c1-145">Если требуется, чтобы модуль форматирования для обработки только `Student` объектов, проверьте тип [объекта](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) в контекст объект, предоставляемый для `CanWriteResult` метод.</span><span class="sxs-lookup"><span data-stu-id="748c1-145">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="748c1-146">Обратите внимание, что нет необходимости использовать `CanWriteResult` при возвращении операции `IActionResult`; в этом случае `CanWriteType` метод получает тип среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="748c1-146">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="748c1-147">Переопределить ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="748c1-147">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="748c1-148">Выполняют реальную работу десериализации или сериализации в `ReadRequestBodyAsync` или `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="748c1-148">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="748c1-149">Выделенные строки в следующем примере показано, как получить службы из контейнера внедрения зависимостей (их не удается получить из параметров конструктора).</span><span class="sxs-lookup"><span data-stu-id="748c1-149">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="748c1-150">Настройка MVC, чтобы использовать пользовательский модуль форматирования</span><span class="sxs-lookup"><span data-stu-id="748c1-150">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="748c1-151">Чтобы использовать пользовательский модуль форматирования, добавьте экземпляр класса модуля форматирования для `InputFormatters` или `OutputFormatters` коллекции.</span><span class="sxs-lookup"><span data-stu-id="748c1-151">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="748c1-152">Модули форматирования, вычисляются в порядке, в каком они добавлены.</span><span class="sxs-lookup"><span data-stu-id="748c1-152">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="748c1-153">Первый из них имеет приоритет.</span><span class="sxs-lookup"><span data-stu-id="748c1-153">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="748c1-154">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="748c1-154">Next steps</span></span>

<span data-ttu-id="748c1-155">В разделе [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), который реализует простой vCard входных данных и модули форматирования выходных данных.</span><span class="sxs-lookup"><span data-stu-id="748c1-155">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="748c1-156">Приложение считывает и записывает визитные карточки, которые выглядят как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="748c1-156">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="748c1-157">Чтобы увидеть vCard выходных данных, запустите приложение и отправка запроса Get с Accept заголовок «текст/vcard» для `http://localhost:63313/api/contacts/` (при запуске из Visual Studio) или `http://localhost:5000/api/contacts/` (при запуске из командной строки).</span><span class="sxs-lookup"><span data-stu-id="748c1-157">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="748c1-158">Чтобы добавить ее в памяти коллекцию контактов, отправьте запрос Post в URL, с заголовка Content-Type «text/vcard» и текст vCard в тексте, отформатированных как в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="748c1-158">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
