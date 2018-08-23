---
uid: web-api/overview/error-handling/exception-handling
title: Обработка исключений в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 62e6187cd82252e7d30f21e03cc4d08418fa39ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836574"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="99de8-102">Обработка исключений в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="99de8-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="99de8-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="99de8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="99de8-104">В этой статье описывается ошибка и обработка исключений в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="99de8-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="99de8-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="99de8-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="99de8-106">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="99de8-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="99de8-107">Регистрация фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="99de8-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="99de8-108">Ошибка HTTP</span><span class="sxs-lookup"><span data-stu-id="99de8-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="99de8-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="99de8-109">HttpResponseException</span></span>

<span data-ttu-id="99de8-110">Что произойдет, если контроллер Web API создает неперехваченное исключение?</span><span class="sxs-lookup"><span data-stu-id="99de8-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="99de8-111">По умолчанию большинство исключений, преобразуются в ответ HTTP с кодом состояния 500 Внутренняя ошибка сервера.</span><span class="sxs-lookup"><span data-stu-id="99de8-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="99de8-112">**HttpResponseException** типа является особым случаем.</span><span class="sxs-lookup"><span data-stu-id="99de8-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="99de8-113">Это исключение возвращает любой код состояния HTTP, указанный в конструкторе исключения.</span><span class="sxs-lookup"><span data-stu-id="99de8-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="99de8-114">Например, следующий метод возвращает ошибку 404, не найден, если *идентификатор* недопустимый параметр.</span><span class="sxs-lookup"><span data-stu-id="99de8-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="99de8-115">Для большего контроля над ответом, можно также создать всего сообщения ответа и включить его с **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="99de8-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="99de8-116">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="99de8-116">Exception Filters</span></span>

<span data-ttu-id="99de8-117">Можно настроить как веб-API обрабатывает исключения, написав *фильтра исключений*.</span><span class="sxs-lookup"><span data-stu-id="99de8-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="99de8-118">Фильтр исключений выполняется в том случае, когда метод контроллера выдает любое необработанное исключение, которое является *не* **HttpResponseException** исключение.</span><span class="sxs-lookup"><span data-stu-id="99de8-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="99de8-119">**HttpResponseException** типа является особым случаем, поскольку он разработан специально для возврата HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="99de8-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="99de8-120">Реализовывают **System.Web.Http.Filters.IExceptionFilter** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="99de8-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="99de8-121">Самый простой способ создать фильтр исключений является наследование от **System.Web.Http.Filters.ExceptionFilterAttribute** класса и переопределить **OnException** метод.</span><span class="sxs-lookup"><span data-stu-id="99de8-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="99de8-122">Фильтры исключений в веб-API ASP.NET, аналогичны в ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="99de8-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="99de8-123">Тем не менее они объявлены в отдельном пространстве имен и функция отдельно.</span><span class="sxs-lookup"><span data-stu-id="99de8-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="99de8-124">В частности **HandleErrorAttribute** класс, используемый в MVC не обрабатывает исключения, создаваемые контроллеров веб-API.</span><span class="sxs-lookup"><span data-stu-id="99de8-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="99de8-125">Ниже приведен фильтр, который преобразует **NotImplementedException** исключения в HTTP-состояния кода 501, не реализованы:</span><span class="sxs-lookup"><span data-stu-id="99de8-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="99de8-126">**Ответа** свойство **HttpActionExecutedContext** объект содержит сообщение HTTP-ответа, отправляемого клиенту.</span><span class="sxs-lookup"><span data-stu-id="99de8-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="99de8-127">Регистрация фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="99de8-127">Registering Exception Filters</span></span>

<span data-ttu-id="99de8-128">Существует несколько способов регистрации фильтра исключений веб-API:</span><span class="sxs-lookup"><span data-stu-id="99de8-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="99de8-129">Действием</span><span class="sxs-lookup"><span data-stu-id="99de8-129">By action</span></span>
- <span data-ttu-id="99de8-130">Контроллером</span><span class="sxs-lookup"><span data-stu-id="99de8-130">By controller</span></span>
- <span data-ttu-id="99de8-131">Глобально</span><span class="sxs-lookup"><span data-stu-id="99de8-131">Globally</span></span>

<span data-ttu-id="99de8-132">Чтобы применить фильтр для определенных действий, добавьте фильтр как атрибут действия:</span><span class="sxs-lookup"><span data-stu-id="99de8-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="99de8-133">Чтобы применить фильтр, чтобы все действия на контроллере, добавьте фильтр как атрибут в класс контроллера:</span><span class="sxs-lookup"><span data-stu-id="99de8-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="99de8-134">Чтобы применить фильтр глобально ко всем контроллерам веб-API, добавьте экземпляр фильтра, который **GlobalConfiguration.Configuration.Filters** коллекции.</span><span class="sxs-lookup"><span data-stu-id="99de8-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="99de8-135">В этой коллекции фильтры об исключении применяются к любому действию контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="99de8-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="99de8-136">Если вы используете шаблон проекта «ASP.NET MVC 4 веб-приложение» для создания проекта, поместите код конфигурации веб-API внутри `WebApiConfig` класс, который находится в приложении\_Начальная папка:</span><span class="sxs-lookup"><span data-stu-id="99de8-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="99de8-137">Ошибка HTTP</span><span class="sxs-lookup"><span data-stu-id="99de8-137">HttpError</span></span>

<span data-ttu-id="99de8-138">**HttpError** объекта обеспечивают согласованное выполнение для возврата сведений об ошибке в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="99de8-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="99de8-139">В следующем примере показано, как вернуть код состояния HTTP 404 (не найдено) с **HttpError** в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="99de8-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="99de8-140">**CreateErrorResponse** является методом расширения, определенные в **System.Net.Http.HttpRequestMessageExtensions** класса.</span><span class="sxs-lookup"><span data-stu-id="99de8-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="99de8-141">На внутреннем уровне **CreateErrorResponse** создает **HttpError** экземпляра, а затем создает **HttpResponseMessage** , содержащий **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="99de8-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="99de8-142">В этом примере если метод выполнен успешно, возвращает продукт в HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="99de8-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="99de8-143">Но если запрашиваемого продукта не найден, ответ HTTP содержит **HttpError** в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="99de8-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="99de8-144">Ответ может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="99de8-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="99de8-145">Обратите внимание, что **HttpError** был сериализован в формат JSON, в этом примере.</span><span class="sxs-lookup"><span data-stu-id="99de8-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="99de8-146">Одно из преимуществ использования **HttpError** — что он проходит через же [согласования содержимого](../formats-and-model-binding/content-negotiation.md) и все другие строго типизированную модель процесса сериализации.</span><span class="sxs-lookup"><span data-stu-id="99de8-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="99de8-147">Ошибка HTTP и проверке модели</span><span class="sxs-lookup"><span data-stu-id="99de8-147">HttpError and Model Validation</span></span>

<span data-ttu-id="99de8-148">Для проверки модели, можно передать в состояние модели, чтобы **CreateErrorResponse**, чтобы включить ошибки проверки в ответе:</span><span class="sxs-lookup"><span data-stu-id="99de8-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="99de8-149">В этом примере может вернуть следующий ответ:</span><span class="sxs-lookup"><span data-stu-id="99de8-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="99de8-150">Дополнительные сведения о проверке модели, см. в разделе [проверка модели в веб-API ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="99de8-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="99de8-151">С помощью Ошибка HTTP с HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="99de8-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="99de8-152">В предыдущих примерах возвращаются **HttpResponseMessage** сообщение от действия контроллера, но можно также использовать **HttpResponseException** для возврата **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="99de8-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="99de8-153">Это позволит вернуть строго типизированную модель в случае обычной успеха при возврате по-прежнему **HttpError** Если возникает ошибка:</span><span class="sxs-lookup"><span data-stu-id="99de8-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
