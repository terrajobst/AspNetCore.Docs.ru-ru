---
uid: web-api/overview/error-handling/exception-handling
title: Обработка исключений в ASP.NET Web API | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506963"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="ec755-102">Обработка исключений в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ec755-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="ec755-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ec755-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ec755-104">Эта статья описывает ошибки и обработка исключений в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ec755-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="ec755-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="ec755-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="ec755-106">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="ec755-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="ec755-107">Регистрация фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="ec755-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="ec755-108">Ошибка HTTP</span><span class="sxs-lookup"><span data-stu-id="ec755-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="ec755-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="ec755-109">HttpResponseException</span></span>

<span data-ttu-id="ec755-110">Что происходит, если контроллер веб-API вызывает неперехваченное исключение?</span><span class="sxs-lookup"><span data-stu-id="ec755-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="ec755-111">По умолчанию большинство исключений преобразуются в HTTP-ответа с кодом состояния 500 Внутренняя ошибка сервера.</span><span class="sxs-lookup"><span data-stu-id="ec755-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="ec755-112">**HttpResponseException** типа является особым случаем.</span><span class="sxs-lookup"><span data-stu-id="ec755-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="ec755-113">Это исключение возвращает любой код состояния HTTP, задать в конструкторе исключение.</span><span class="sxs-lookup"><span data-stu-id="ec755-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="ec755-114">Например, следующий метод возвращает 404, не найден, если *идентификатор* указан недопустимый параметр.</span><span class="sxs-lookup"><span data-stu-id="ec755-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="ec755-115">Для усиления контроля над ответ также можно создать сообщение ответа и включить его с **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="ec755-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="ec755-116">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="ec755-116">Exception Filters</span></span>

<span data-ttu-id="ec755-117">Можно настроить как веб-API обрабатывает исключения, написав *фильтра исключений*.</span><span class="sxs-lookup"><span data-stu-id="ec755-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="ec755-118">Фильтр исключений выполняется в том случае, когда метод контроллера вызывает любой необработанное исключение, которое является *не* **HttpResponseException** исключение.</span><span class="sxs-lookup"><span data-stu-id="ec755-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="ec755-119">**HttpResponseException** типа является особым случаем, поскольку он разработан специально для возврата ответа HTTP.</span><span class="sxs-lookup"><span data-stu-id="ec755-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="ec755-120">Фильтры исключений реализовать **System.Web.Http.Filters.IExceptionFilter** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="ec755-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="ec755-121">Самый простой способ записи фильтра исключений является являются производными от **System.Web.Http.Filters.ExceptionFilterAttribute** класса и переопределить **OnException** метод.</span><span class="sxs-lookup"><span data-stu-id="ec755-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="ec755-122">Фильтры исключений в веб-API ASP.NET, аналогичны в ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ec755-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="ec755-123">Тем не менее они объявлены в отдельном пространстве имен и функция отдельно.</span><span class="sxs-lookup"><span data-stu-id="ec755-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="ec755-124">В частности **HandleErrorAttribute** класс, используемый в MVC не обрабатывает исключения, вызванные контроллеров веб-API.</span><span class="sxs-lookup"><span data-stu-id="ec755-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="ec755-125">Ниже приведен фильтр, который преобразует **NotImplementedException** 501, не реализован код исключения в код состояния HTTP:</span><span class="sxs-lookup"><span data-stu-id="ec755-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="ec755-126">**Ответ** свойство **HttpActionExecutedContext** объект содержит сообщения HTTP-ответа, отправляемое клиенту.</span><span class="sxs-lookup"><span data-stu-id="ec755-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="ec755-127">Регистрация фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="ec755-127">Registering Exception Filters</span></span>

<span data-ttu-id="ec755-128">Существует несколько способов для регистрации в фильтре исключений веб-API:</span><span class="sxs-lookup"><span data-stu-id="ec755-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="ec755-129">Действия</span><span class="sxs-lookup"><span data-stu-id="ec755-129">By action</span></span>
- <span data-ttu-id="ec755-130">Контроллер</span><span class="sxs-lookup"><span data-stu-id="ec755-130">By controller</span></span>
- <span data-ttu-id="ec755-131">Глобально</span><span class="sxs-lookup"><span data-stu-id="ec755-131">Globally</span></span>

<span data-ttu-id="ec755-132">Чтобы применить фильтр для определенных действий, добавьте фильтр как атрибут действие:</span><span class="sxs-lookup"><span data-stu-id="ec755-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="ec755-133">Чтобы применить фильтр к все действия на контроллере, добавьте фильтр как атрибут в класс контроллера:</span><span class="sxs-lookup"><span data-stu-id="ec755-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="ec755-134">Чтобы применить фильтр глобально для всех контроллеров веб-API, нужно добавить экземпляр фильтра, который будет **GlobalConfiguration.Configuration.Filters** коллекции.</span><span class="sxs-lookup"><span data-stu-id="ec755-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="ec755-135">Фильтры сведения в этой коллекции применяются к любому действию контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="ec755-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="ec755-136">Если используется шаблон проекта «ASP.NET MVC 4 веб-приложения» для создания проекта, поместите код конфигурации веб-API в `WebApiConfig` класс, который находится в приложении\_Начальная папка:</span><span class="sxs-lookup"><span data-stu-id="ec755-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="ec755-137">Ошибка HTTP</span><span class="sxs-lookup"><span data-stu-id="ec755-137">HttpError</span></span>

<span data-ttu-id="ec755-138">**HttpError** объект предоставляет согласованный способ возвращает сведения об ошибке в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="ec755-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="ec755-139">Приведенный ниже показано, как вернуть код состояния HTTP 404 (не найдено) с **HttpError** в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="ec755-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="ec755-140">**CreateErrorResponse** метод расширения определен в **System.Net.Http.HttpRequestMessageExtensions** класса.</span><span class="sxs-lookup"><span data-stu-id="ec755-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="ec755-141">На внутреннем уровне **CreateErrorResponse** создает **HttpError** экземпляра, а затем создает **HttpResponseMessage** , содержащий **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="ec755-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="ec755-142">В этом примере если метод выполнен успешно, возвращает произведение в HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="ec755-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="ec755-143">Но если запрошенный продукт не найден, HTTP-ответа содержит **HttpError** в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="ec755-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="ec755-144">Ответ может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ec755-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="ec755-145">Обратите внимание, что **HttpError** в этом примере был сериализован в JSON.</span><span class="sxs-lookup"><span data-stu-id="ec755-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="ec755-146">Одно из преимуществ использования **HttpError** — что они проходят через же [согласования содержимого](../formats-and-model-binding/content-negotiation.md) и процесса сериализации, как и любые другие модели, строго типизированными.</span><span class="sxs-lookup"><span data-stu-id="ec755-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="ec755-147">Ошибка HTTP и проверка модели</span><span class="sxs-lookup"><span data-stu-id="ec755-147">HttpError and Model Validation</span></span>

<span data-ttu-id="ec755-148">Для проверки модели, можно передать состояние модели для **CreateErrorResponse**, чтобы включить ошибки проверки в ответе:</span><span class="sxs-lookup"><span data-stu-id="ec755-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="ec755-149">В этом примере может возвратить следующий ответ:</span><span class="sxs-lookup"><span data-stu-id="ec755-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="ec755-150">Дополнительные сведения о проверке модели см. в разделе [проверки модели в ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ec755-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="ec755-151">Ошибка HTTP при помощи HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="ec755-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="ec755-152">В предыдущих примерах возвращаются **HttpResponseMessage** сообщение от действия контроллера, но можно также использовать **HttpResponseException** для возврата **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="ec755-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="ec755-153">Это позволит вернуть строго типизированных модели в случае обычной успех при возврате по-прежнему **HttpError** Если возникает ошибка:</span><span class="sxs-lookup"><span data-stu-id="ec755-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
