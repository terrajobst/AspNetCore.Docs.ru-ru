---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: "Параметр привязки веб-API ASP.NET | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ad052570fb2f168da657cd1263d8342a59d4cab0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="d7dea-102">Параметр привязки веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d7dea-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="d7dea-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d7dea-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d7dea-104">Когда веб-API вызывает метод на контроллере, его необходимо задать значения для параметров, этот процесс называется *привязки*.</span><span class="sxs-lookup"><span data-stu-id="d7dea-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="d7dea-105">В этой статье описывается, как веб-API привязывает параметры и как можно настроить процесс привязки.</span><span class="sxs-lookup"><span data-stu-id="d7dea-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="d7dea-106">По умолчанию веб-API использует следующие правила для привязки параметров:</span><span class="sxs-lookup"><span data-stu-id="d7dea-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="d7dea-107">Если параметр имеет тип «простой», веб-API пытается получить значение из URI.</span><span class="sxs-lookup"><span data-stu-id="d7dea-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="d7dea-108">Простые типы включают .NET [типы-примитивы](https://msdn.microsoft.com/en-us/library/system.type.isprimitive.aspx) (**int**, **bool**, **двойные**, и так далее), плюс **TimeSpan**, **DateTime**, **Guid**, **десятичное**, и **строка**, *, а также* все тип с преобразователь типов для преобразования из строки.</span><span class="sxs-lookup"><span data-stu-id="d7dea-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/en-us/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="d7dea-109">(Дополнительные сведения о преобразователях типов более поздней версии.)</span><span class="sxs-lookup"><span data-stu-id="d7dea-109">(More about type converters later.)</span></span>
- <span data-ttu-id="d7dea-110">Для сложных типов веб-API пытается считать значение из тела сообщения, с помощью [форматирования типа мультимедиа](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="d7dea-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="d7dea-111">Например ниже приведен типичный метод контроллера веб-API:</span><span class="sxs-lookup"><span data-stu-id="d7dea-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="d7dea-112">*Идентификатор* параметр &quot;простой&quot; тип, поэтому веб-API пытается получить значение из URI запроса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="d7dea-113">*Элемент* параметр — это сложный тип, поэтому веб-API с помощью форматирования типа мультимедиа для чтения значения из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="d7dea-114">Чтобы получить значение из URI, веб-API просматривает данные маршрута и строке запроса URI.</span><span class="sxs-lookup"><span data-stu-id="d7dea-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="d7dea-115">Данные маршрута заполняется в том случае, когда система маршрутизации анализирует URI и сопоставляет его с маршрутом.</span><span class="sxs-lookup"><span data-stu-id="d7dea-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="d7dea-116">Дополнительные сведения см. в разделе [Маршрутизация и Выбор действия](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="d7dea-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="d7dea-117">В оставшейся части этой статьи я покажу, как можно настроить привязки модели.</span><span class="sxs-lookup"><span data-stu-id="d7dea-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="d7dea-118">Для сложных типов Однако рекомендуется использовать модули форматирования типа мультимедиа, по возможности.</span><span class="sxs-lookup"><span data-stu-id="d7dea-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="d7dea-119">Основной принцип HTTP — что ресурсы отправляются в тексте сообщения, используя согласование содержимого для указания представление ресурса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="d7dea-120">Модули форматирования типа мультимедиа, разработанные для этой цели.</span><span class="sxs-lookup"><span data-stu-id="d7dea-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="d7dea-121">С помощью [FromUri]</span><span class="sxs-lookup"><span data-stu-id="d7dea-121">Using [FromUri]</span></span>

<span data-ttu-id="d7dea-122">Чтобы заставить веб-API для чтения из URI сложного типа, добавьте **[FromUri]** к параметру.</span><span class="sxs-lookup"><span data-stu-id="d7dea-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="d7dea-123">В следующем примере определяется `GeoPoint` типа, а также метод контроллера, который получает `GeoPoint` из URI.</span><span class="sxs-lookup"><span data-stu-id="d7dea-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="d7dea-124">Клиент можно поместить значения широты и долготы в строке запроса и веб-API будет использовать их для создания `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="d7dea-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="d7dea-125">Пример:</span><span class="sxs-lookup"><span data-stu-id="d7dea-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="d7dea-126">С помощью [FromBody]</span><span class="sxs-lookup"><span data-stu-id="d7dea-126">Using [FromBody]</span></span>

<span data-ttu-id="d7dea-127">Чтобы заставить веб-API для чтения простого типа в тексте запроса, добавьте **[FromBody]** к параметру:</span><span class="sxs-lookup"><span data-stu-id="d7dea-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="d7dea-128">В этом примере веб-API будет использовать модуль форматирования типа мультимедиа для чтения значение *имя* в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="d7dea-129">Ниже приведен пример запроса клиента.</span><span class="sxs-lookup"><span data-stu-id="d7dea-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="d7dea-130">Если для параметра установлено [FromBody], веб-API использует заголовок Content-Type для выбора модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="d7dea-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="d7dea-131">В этом примере — тип содержимого &quot;приложение/json&quot; и необработанные строку JSON (а не объект JSON), текст запроса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="d7dea-132">Не более одного параметра может считывать данные из текста сообщения.</span><span class="sxs-lookup"><span data-stu-id="d7dea-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="d7dea-133">Поэтому он не будет работать:</span><span class="sxs-lookup"><span data-stu-id="d7dea-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="d7dea-134">Это правило причина заключается в том, что тело запроса может храниться в потоке без использования буфера, которые могут быть прочитаны только один раз.</span><span class="sxs-lookup"><span data-stu-id="d7dea-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="d7dea-135">Преобразователи типов</span><span class="sxs-lookup"><span data-stu-id="d7dea-135">Type Converters</span></span>

<span data-ttu-id="d7dea-136">Обрабатывает класс как простой тип (при этом веб-API будет предпринята попытка привязать его из URI) веб-API можно сделать, создав **TypeConverter** и преобразования строк.</span><span class="sxs-lookup"><span data-stu-id="d7dea-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="d7dea-137">В следующем коде показано `GeoPoint` класс, представляющий точку географических плюс **TypeConverter** , преобразование из строки для `GeoPoint` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="d7dea-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="d7dea-138">`GeoPoint` Декорированных класс **[TypeConverter]** атрибут для указания преобразователь типов.</span><span class="sxs-lookup"><span data-stu-id="d7dea-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="d7dea-139">(В этом примере была вдохновлена Майк стол блога [способ привязки для пользовательских объектов в сигнатурах действия в MVC-WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="d7dea-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="d7dea-140">Теперь веб-API будет рассматривать `GeoPoint` как простой тип, то есть, он попытается привязать `GeoPoint` параметры из URI.</span><span class="sxs-lookup"><span data-stu-id="d7dea-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="d7dea-141">Не нужно включать **[FromUri]** для параметра.</span><span class="sxs-lookup"><span data-stu-id="d7dea-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="d7dea-142">Клиент может вызывать метод с URI следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d7dea-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="d7dea-143">Связыватели моделей</span><span class="sxs-lookup"><span data-stu-id="d7dea-143">Model Binders</span></span>

<span data-ttu-id="d7dea-144">Параметр более гибким, чем преобразователь типов для создания настраиваемый связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="d7dea-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="d7dea-145">С помощью связывателя модели имеется доступ к HTTP-запроса, описание действия и необработанные значения из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="d7dea-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="d7dea-146">Чтобы создать связыватель модели, реализовать **IModelBinder** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="d7dea-147">Этот интерфейс определяет единственный метод, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="d7dea-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="d7dea-148">Вот связыватель модели для `GeoPoint` объектов.</span><span class="sxs-lookup"><span data-stu-id="d7dea-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="d7dea-149">Связыватель модели Возвращает необработанные входные значения из *поставщик значений*.</span><span class="sxs-lookup"><span data-stu-id="d7dea-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="d7dea-150">Такая архитектура позволяет разделить две разные функции:</span><span class="sxs-lookup"><span data-stu-id="d7dea-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="d7dea-151">Поставщик значений принимает HTTP-запроса и заполняет словарь пар «ключ значение».</span><span class="sxs-lookup"><span data-stu-id="d7dea-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="d7dea-152">Связыватель модели использует этот словарь для заполнения модели.</span><span class="sxs-lookup"><span data-stu-id="d7dea-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="d7dea-153">Поставщик значений по умолчанию в веб-API возвращает значения из данных маршрута и строку запроса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="d7dea-154">Например, если URL-адрес является `http://localhost/api/values/1?location=48,-122`, поставщик значений создает следующие пары ключ значение:</span><span class="sxs-lookup"><span data-stu-id="d7dea-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="d7dea-155">Идентификатор = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="d7dea-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="d7dea-156">расположение = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="d7dea-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="d7dea-157">(Я буду предполагать шаблон маршрута по умолчанию, который является &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="d7dea-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="d7dea-158">Имя параметра для привязки хранится в **ModelBindingContext.ModelName** свойство.</span><span class="sxs-lookup"><span data-stu-id="d7dea-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="d7dea-159">Связыватель модели ищет ключ с таким значением в словаре.</span><span class="sxs-lookup"><span data-stu-id="d7dea-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="d7dea-160">Если значение существует и может быть преобразован в `GeoPoint`, связывателя модели присваивает значение привязки к **ModelBindingContext.Model** свойство.</span><span class="sxs-lookup"><span data-stu-id="d7dea-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="d7dea-161">Обратите внимание, что связыватель модели не ограничивается преобразования простого типа.</span><span class="sxs-lookup"><span data-stu-id="d7dea-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="d7dea-162">В этом примере связывателя модели в первую очередь в таблице известных расположений и в случае неудачи использует преобразования типов.</span><span class="sxs-lookup"><span data-stu-id="d7dea-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="d7dea-163">**Установка связывателя модели**</span><span class="sxs-lookup"><span data-stu-id="d7dea-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="d7dea-164">Существует несколько способов, чтобы задать связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="d7dea-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="d7dea-165">Во-первых, можно добавить **[ModelBinder]** к параметру.</span><span class="sxs-lookup"><span data-stu-id="d7dea-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="d7dea-166">Можно также добавить **[ModelBinder]** к типу атрибут.</span><span class="sxs-lookup"><span data-stu-id="d7dea-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="d7dea-167">Веб-API будет использоваться связыватель модели указанного все параметры этого типа.</span><span class="sxs-lookup"><span data-stu-id="d7dea-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="d7dea-168">Наконец, можно добавить поставщик связывателей модели для **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="d7dea-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="d7dea-169">Поставщик связывателей модели является просто класс фабрики, который создает связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="d7dea-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="d7dea-170">Можно создать путем наследования от поставщика [ModelBinderProvider](https://msdn.microsoft.com/en-us/library/system.web.http.modelbinding.modelbinderprovider.aspx) класса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/en-us/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="d7dea-171">Тем не менее, если ваш связыватель модели обработки одного типа, проще использовать встроенные **SimpleModelBinderProvider**, разработанный для этой цели.</span><span class="sxs-lookup"><span data-stu-id="d7dea-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="d7dea-172">В следующем примере кода показано, как это сделать:</span><span class="sxs-lookup"><span data-stu-id="d7dea-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="d7dea-173">С помощью поставщика привязки модели по-прежнему необходимо добавить **[ModelBinder]** к параметру, нужно сообщить веб-API, следует использовать связыватель модели и не модуль форматирования типа мультимедиа.</span><span class="sxs-lookup"><span data-stu-id="d7dea-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="d7dea-174">Но теперь не требуется указать тип связывателя модели в атрибуте.</span><span class="sxs-lookup"><span data-stu-id="d7dea-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="d7dea-175">Поставщики значений</span><span class="sxs-lookup"><span data-stu-id="d7dea-175">Value Providers</span></span>

<span data-ttu-id="d7dea-176">Я упоминал, что привязка модели получает значения от поставщика значений.</span><span class="sxs-lookup"><span data-stu-id="d7dea-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="d7dea-177">Чтобы создать поставщик пользовательского значения, реализовать **IValueProvider** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="d7dea-178">Ниже приведен пример, который извлекает значения из куки-файлы в запросе:</span><span class="sxs-lookup"><span data-stu-id="d7dea-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="d7dea-179">Также необходимо создать фабрику поставщика значение путем наследования от **ValueProviderFactory** класса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="d7dea-180">Добавить фабрики поставщика значение **HttpConfiguration** следующим образом.</span><span class="sxs-lookup"><span data-stu-id="d7dea-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="d7dea-181">Веб-API состоит из всех поставщиков значений, поэтому при вызове связыватель модели **ValueProvider.GetValue**, связывателя модели получает значение из первой воспроизводимых его поставщик значений.</span><span class="sxs-lookup"><span data-stu-id="d7dea-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="d7dea-182">Можно также задать фабрики поставщика значение параметра на уровне с помощью **ValueProvider** атрибута следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d7dea-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="d7dea-183">Это значение определяет веб-API для использования с указанным значением фабрика поставщика привязки модели и отказаться от использования других поставщиков зарегистрированным значением.</span><span class="sxs-lookup"><span data-stu-id="d7dea-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="d7dea-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="d7dea-184">HttpParameterBinding</span></span>

<span data-ttu-id="d7dea-185">Связыватели моделей являются более общий механизм определенного экземпляра.</span><span class="sxs-lookup"><span data-stu-id="d7dea-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="d7dea-186">Если взглянуть на **[ModelBinder]** атрибут, вы увидите, является производным от абстрактного **ParameterBindingAttribute** класса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="d7dea-187">Этот класс определяет единственный метод, **GetBinding**, который возвращает **HttpParameterBinding** объекта:</span><span class="sxs-lookup"><span data-stu-id="d7dea-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="d7dea-188">**HttpParameterBinding** отвечает за привязки в значение параметра.</span><span class="sxs-lookup"><span data-stu-id="d7dea-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="d7dea-189">В случае использования **[ModelBinder]**, возвращает атрибут **HttpParameterBinding** реализации, которую использует **IModelBinder** для выполнения фактического привязки.</span><span class="sxs-lookup"><span data-stu-id="d7dea-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="d7dea-190">Вы также можете реализовать собственные **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="d7dea-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="d7dea-191">Например, предположим, вы хотите получить теги eTag из `if-match` и `if-none-match` заголовков запроса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="d7dea-192">Мы начнем путем определения класса для представления значения eTag.</span><span class="sxs-lookup"><span data-stu-id="d7dea-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="d7dea-193">Также мы определим перечисление, указывающее, нужно ли получить ETag из `if-match` заголовок или `if-none-match` заголовок.</span><span class="sxs-lookup"><span data-stu-id="d7dea-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="d7dea-194">Вот **HttpParameterBinding** , возвращает значение ETag из нужного заголовка и привязывает его к параметру типа ETag:</span><span class="sxs-lookup"><span data-stu-id="d7dea-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="d7dea-195">**ExecuteBindingAsync** метод выполняет привязку.</span><span class="sxs-lookup"><span data-stu-id="d7dea-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="d7dea-196">В этом методе добавить значение привязанного параметра **аргумент ActionArgument** словаря в **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="d7dea-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="d7dea-197">Если ваш **ExecuteBindingAsync** метод считывает тело сообщения запроса, переопределите **WillReadBody** свойство возвращает значение true.</span><span class="sxs-lookup"><span data-stu-id="d7dea-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="d7dea-198">Текст запроса может быть небуферизованный поток, можно прочитать только один раз, поэтому веб-API принудительно применяет правила, не более одной привязки можно прочитать тело сообщения.</span><span class="sxs-lookup"><span data-stu-id="d7dea-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="d7dea-199">Чтобы применить пользовательское **HttpParameterBinding**, можно определить атрибут, который является производным от **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="d7dea-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="d7dea-200">Для `ETagParameterBinding`, мы определим два атрибута, один для `if-match` заголовки и один для `if-none-match` заголовки.</span><span class="sxs-lookup"><span data-stu-id="d7dea-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="d7dea-201">Оба являются производными от абстрактного базового класса.</span><span class="sxs-lookup"><span data-stu-id="d7dea-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="d7dea-202">Ниже приведен метод контроллера, который использует `[IfNoneMatch]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="d7dea-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="d7dea-203">Помимо **ParameterBindingAttribute**, имеется другой обработчик для добавления настраиваемого **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="d7dea-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="d7dea-204">На **HttpConfiguration** объекта, **ParameterBindingRules** свойство — это коллекция функций anomymous типа (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="d7dea-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="d7dea-205">Например, можно добавить правило, которое использует любой параметр ETag в метод GET `ETagParameterBinding` с `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="d7dea-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="d7dea-206">Данная функция должна возвращать `null` для параметров, когда привязка не применимо.</span><span class="sxs-lookup"><span data-stu-id="d7dea-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="d7dea-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="d7dea-207">IActionValueBinder</span></span>

<span data-ttu-id="d7dea-208">Процесс привязки параметров контролируется службой подключаемые **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="d7dea-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="d7dea-209">Реализация по умолчанию **IActionValueBinder** делает следующее:</span><span class="sxs-lookup"><span data-stu-id="d7dea-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="d7dea-210">Найдите **ParameterBindingAttribute** в параметре.</span><span class="sxs-lookup"><span data-stu-id="d7dea-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="d7dea-211">Сюда входят **[FromBody]**, **[FromUri]**, и **[ModelBinder]**, или настраиваемых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="d7dea-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="d7dea-212">В противном случае поиск в **HttpConfiguration.ParameterBindingRules** для функции, которая возвращает ненулевое значение **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="d7dea-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="d7dea-213">В противном случае используйте правила по умолчанию, описанной выше.</span><span class="sxs-lookup"><span data-stu-id="d7dea-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="d7dea-214">Если тип параметра «простой» или преобразователя типов, Привязывайте из URI.</span><span class="sxs-lookup"><span data-stu-id="d7dea-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="d7dea-215">Это эквивалентно помещения **[FromUri]** атрибут параметра.</span><span class="sxs-lookup"><span data-stu-id="d7dea-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="d7dea-216">В противном случае попробуйте считать этот параметр из текста сообщения.</span><span class="sxs-lookup"><span data-stu-id="d7dea-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="d7dea-217">Это эквивалентно помещения **[FromBody]** для параметра.</span><span class="sxs-lookup"><span data-stu-id="d7dea-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="d7dea-218">Если требуется, можно заменить весь **IActionValueBinder** службы с пользовательской реализацией.</span><span class="sxs-lookup"><span data-stu-id="d7dea-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d7dea-219">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d7dea-219">Additional Resources</span></span>

[<span data-ttu-id="d7dea-220">Пример настраиваемого параметра привязки</span><span class="sxs-lookup"><span data-stu-id="d7dea-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="d7dea-221">Майк стол написал хорошо серию записи в блогах о привязки параметров веб-API:</span><span class="sxs-lookup"><span data-stu-id="d7dea-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="d7dea-222">Как веб-API осуществляется привязка параметра</span><span class="sxs-lookup"><span data-stu-id="d7dea-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="d7dea-223">Привязка параметров стиля MVC для веб-API</span><span class="sxs-lookup"><span data-stu-id="d7dea-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="d7dea-224">Привязка пользовательских объектов в сигнатурах действия в MVC или веб-API</span><span class="sxs-lookup"><span data-stu-id="d7dea-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="d7dea-225">Создание поставщика пользовательских значений в веб-API</span><span class="sxs-lookup"><span data-stu-id="d7dea-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="d7dea-226">Привязка параметров веб-API в механизме</span><span class="sxs-lookup"><span data-stu-id="d7dea-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
