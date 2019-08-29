---
title: Пользовательские модули форматирования для веб-API в ASP.NET Core
author: rick-anderson
description: Узнайте, как создавать и использовать пользовательские модули форматирования для веб-интерфейсов API в ASP.NET Core.
ms.author: riande
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: 6fb7e192bf3e943eb9018b08fb87a833d3643208
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975673"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="aaca3-103">Пользовательские модули форматирования для веб-API в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aaca3-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="aaca3-104">Автор: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="aaca3-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="aaca3-105">В ASP.NET Core MVC имеется встроенная поддержка обмена данными в веб-интерфейсах API с помощью форматов JSON или XML.</span><span class="sxs-lookup"><span data-stu-id="aaca3-105">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON or XML.</span></span> <span data-ttu-id="aaca3-106">В этой статье показано, как добавить поддержку дополнительных форматов, создав пользовательские модули форматирования.</span><span class="sxs-lookup"><span data-stu-id="aaca3-106">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="aaca3-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aaca3-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="aaca3-108">Когда следует использовать пользовательские модули форматирования</span><span class="sxs-lookup"><span data-stu-id="aaca3-108">When to use custom formatters</span></span>

<span data-ttu-id="aaca3-109">Используйте пользовательский модуль форматирования, если необходимо, чтобы процесс [согласования содержимого](xref:web-api/advanced/formatting#content-negotiation) поддерживал тип содержимого, который не поддерживается встроенными форматировщиками (JSON и XML).</span><span class="sxs-lookup"><span data-stu-id="aaca3-109">Use a custom formatter when you want the [content negotiation](xref:web-api/advanced/formatting#content-negotiation) process to support a content type that isn't supported by the built-in formatters (JSON and XML).</span></span>

<span data-ttu-id="aaca3-110">Например, если некоторые из клиентов вашего веб-интерфейса API могут работать с форматом [Protobuf](https://github.com/google/protobuf), для обмена данными с ними может быть желательно использовать этот формат, так как он более эффективен.</span><span class="sxs-lookup"><span data-stu-id="aaca3-110">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span> <span data-ttu-id="aaca3-111">Вам может также потребоваться реализовать отправку имен и адресов веб-интерфейсом API в формате [vCard](https://wikipedia.org/wiki/VCard), распространенном формате для передачи контактных данных.</span><span class="sxs-lookup"><span data-stu-id="aaca3-111">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="aaca3-112">В образце приложения, представленном в этой статье, реализуется простой модуль форматирования vCard.</span><span class="sxs-lookup"><span data-stu-id="aaca3-112">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="aaca3-113">Общие сведения об использовании пользовательского модуля форматирования</span><span class="sxs-lookup"><span data-stu-id="aaca3-113">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="aaca3-114">Чтобы создать и использовать пользовательский модуль форматирования, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="aaca3-114">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="aaca3-115">Создайте класс модуля форматирования выходных данных, если необходимо сериализовать данные, отправляемые клиенту.</span><span class="sxs-lookup"><span data-stu-id="aaca3-115">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="aaca3-116">Создайте класс модуля форматирования входных данных, если необходимо десериализировать данные, получаемые от клиента.</span><span class="sxs-lookup"><span data-stu-id="aaca3-116">Create an input formatter class if you want to deserialize data received from the client.</span></span>
* <span data-ttu-id="aaca3-117">Добавьте экземпляры модулей форматирования в коллекции `InputFormatters` и `OutputFormatters` в [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="aaca3-117">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="aaca3-118">В следующих разделах приводятся инструкции по выполнению каждого из этих действий с примерами кода.</span><span class="sxs-lookup"><span data-stu-id="aaca3-118">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="aaca3-119">Создание пользовательского класса модуля форматирования</span><span class="sxs-lookup"><span data-stu-id="aaca3-119">How to create a custom formatter class</span></span>

<span data-ttu-id="aaca3-120">Чтобы создать модуль форматирования, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="aaca3-120">To create a formatter:</span></span>

* <span data-ttu-id="aaca3-121">Наследуйте класс от подходящего базового класса.</span><span class="sxs-lookup"><span data-stu-id="aaca3-121">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="aaca3-122">Укажите в конструкторе допустимые типы передаваемых данных и кодировки.</span><span class="sxs-lookup"><span data-stu-id="aaca3-122">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="aaca3-123">Переопределите методы `CanReadType`/`CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="aaca3-123">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="aaca3-124">Переопределите методы `ReadRequestBodyAsync`/`WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="aaca3-124">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="aaca3-125">Наследование от подходящего базового класса</span><span class="sxs-lookup"><span data-stu-id="aaca3-125">Derive from the appropriate base class</span></span>

<span data-ttu-id="aaca3-126">Для текстовых типов данных (например, vCard) произведите наследование от базового класса [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) или [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter).</span><span class="sxs-lookup"><span data-stu-id="aaca3-126">For text media types (for example, vCard), derive from the [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="aaca3-127">Пример форматировщика входных данных см. в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="aaca3-127">For an input formatter example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

<span data-ttu-id="aaca3-128">Для двоичных типов произведите наследование от базового класса [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) или [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter).</span><span class="sxs-lookup"><span data-stu-id="aaca3-128">For binary types, derive from the [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="aaca3-129">Указание допустимых типов передаваемых данных и кодировок.</span><span class="sxs-lookup"><span data-stu-id="aaca3-129">Specify valid media types and encodings</span></span>

<span data-ttu-id="aaca3-130">Укажите в конструкторе допустимые типы передаваемых данных и кодировки, добавив их в коллекции `SupportedMediaTypes` и `SupportedEncodings`.</span><span class="sxs-lookup"><span data-stu-id="aaca3-130">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

<span data-ttu-id="aaca3-131">Пример форматировщика входных данных см. в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="aaca3-131">For an input formatter example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="aaca3-132">Внедрение зависимостей конструктора в класс модуля форматирования невозможно.</span><span class="sxs-lookup"><span data-stu-id="aaca3-132">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="aaca3-133">Например, невозможно получить средство ведения журнала, добавив соответствующий параметр в конструктор.</span><span class="sxs-lookup"><span data-stu-id="aaca3-133">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="aaca3-134">Для доступа к службам необходимо использовать объект контекста, который передается в ваши методы.</span><span class="sxs-lookup"><span data-stu-id="aaca3-134">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="aaca3-135">В приведенном [ниже](#read-write) примере кода показано, как это делается.</span><span class="sxs-lookup"><span data-stu-id="aaca3-135">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="aaca3-136">Переопределение методов CanReadType и CanWriteType</span><span class="sxs-lookup"><span data-stu-id="aaca3-136">Override CanReadType/CanWriteType</span></span>

<span data-ttu-id="aaca3-137">Укажите типы, в которые может осуществляться десериализация или из которых может осуществляться сериализация, переопределив метод `CanReadType` или `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="aaca3-137">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="aaca3-138">Например, текст в формате vCard может создаваться только из типа `Contact`, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="aaca3-138">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

<span data-ttu-id="aaca3-139">Пример форматировщика входных данных см. в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="aaca3-139">For an input formatter example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="aaca3-140">Метод CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="aaca3-140">The CanWriteResult method</span></span>

<span data-ttu-id="aaca3-141">В некоторых ситуациях следует переопределять метод `CanWriteResult`, а не `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="aaca3-141">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="aaca3-142">Используйте `CanWriteResult`, если выполняются указанные ниже условия.</span><span class="sxs-lookup"><span data-stu-id="aaca3-142">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="aaca3-143">Метод действия возвращает класс модели.</span><span class="sxs-lookup"><span data-stu-id="aaca3-143">Your action method returns a model class.</span></span>
* <span data-ttu-id="aaca3-144">Существуют производные классы, которые могут возвращаться во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="aaca3-144">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="aaca3-145">Во время выполнения необходимо знать, какой производный класс был возвращен действием.</span><span class="sxs-lookup"><span data-stu-id="aaca3-145">You need to know at runtime which derived class was returned by the action.</span></span>

<span data-ttu-id="aaca3-146">Предположим, сигнатура метода действия возвращает тип `Person`, но может также возвращать типы `Student` и `Instructor`, производные от `Person`.</span><span class="sxs-lookup"><span data-stu-id="aaca3-146">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="aaca3-147">Если модуль форматирования должен обрабатывать только объекты `Student`, проверьте тип свойства [Object](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext.object#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) в объекте контекста, предоставленном методу `CanWriteResult`.</span><span class="sxs-lookup"><span data-stu-id="aaca3-147">If you want your formatter to handle only `Student` objects, check the type of [Object](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext.object#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="aaca3-148">Обратите внимание на то, что если метод действия возвращает `IActionResult`, использовать `CanWriteResult` нет необходимости. В этом случае метод `CanWriteType` получает тип во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="aaca3-148">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>

### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="aaca3-149">Переопределение методов ReadRequestBodyAsync и WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="aaca3-149">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span>

<span data-ttu-id="aaca3-150">Фактическая работа по десериализации и сериализации осуществляется в методе `ReadRequestBodyAsync` или `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="aaca3-150">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="aaca3-151">В выделенных строках ниже показано, как получить службы из контейнера внедрения зависимостей (получить их из параметров конструктора нельзя).</span><span class="sxs-lookup"><span data-stu-id="aaca3-151">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

<span data-ttu-id="aaca3-152">Пример форматировщика входных данных см. в [примере приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="aaca3-152">For an input formatter example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).</span></span>

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="aaca3-153">Настройка использования пользовательского модуля форматирования в MVC</span><span class="sxs-lookup"><span data-stu-id="aaca3-153">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="aaca3-154">Для использования пользовательского модуля форматирования добавьте экземпляр его класса в коллекцию `InputFormatters` или `OutputFormatters`.</span><span class="sxs-lookup"><span data-stu-id="aaca3-154">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="aaca3-155">Модули форматирования обрабатываются в порядке добавления.</span><span class="sxs-lookup"><span data-stu-id="aaca3-155">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="aaca3-156">Первый модуль имеет приоритет.</span><span class="sxs-lookup"><span data-stu-id="aaca3-156">The first one takes precedence.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aaca3-157">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="aaca3-157">Next steps</span></span>

* [<span data-ttu-id="aaca3-158">Пример кода форматировщика обычного текста на GitHub.</span><span class="sxs-lookup"><span data-stu-id="aaca3-158">Plain text formatter sample code on GitHub.</span></span>](https://github.com/aspnet/Entropy/tree/master/samples/Mvc.Formatters)
* <span data-ttu-id="aaca3-159">[Пример приложения для этого документа](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), в котором реализуются простые форматировщики входных и выходных данных в формате vCard.</span><span class="sxs-lookup"><span data-stu-id="aaca3-159">[Sample app for this doc](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span> <span data-ttu-id="aaca3-160">Это приложение считывает и записывает карточки vCard, которые выглядят следующим образом:</span><span class="sxs-lookup"><span data-stu-id="aaca3-160">The apps reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="aaca3-161">Чтобы увидеть выходные данные vCard, запустите приложение и отправьте запрос Get с заголовком Accept "text/vcard" на адрес `http://localhost:63313/api/contacts/` (при запуске из Visual Studio) или `http://localhost:5000/api/contacts/` (при запуске из командной строки).</span><span class="sxs-lookup"><span data-stu-id="aaca3-161">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="aaca3-162">Чтобы добавить карточку vCard в коллекцию контактов в памяти, отправьте запрос Post на тот же URL-адрес. Запрос должен иметь заголовок Content-Type "text/vcard", а в его теле должен содержаться текст карточки vCard, отформатированный так, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="aaca3-162">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>
