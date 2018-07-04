---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Привязка параметров в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7519ad92334690817ae64b994762fd68e76ebc9e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377813"
---
<a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="11241-102">Привязка параметров в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="11241-102">Parameter Binding in ASP.NET Web API</span></span>
====================
<span data-ttu-id="11241-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="11241-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="11241-104">Когда веб-API вызывает метод на контроллере, его необходимо задать значения для параметров, этот процесс называется *привязки*.</span><span class="sxs-lookup"><span data-stu-id="11241-104">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span> <span data-ttu-id="11241-105">В этой статье описывается, как веб-API привязывает параметры и настройки процесса привязки.</span><span class="sxs-lookup"><span data-stu-id="11241-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span>

<span data-ttu-id="11241-106">По умолчанию веб-API использует следующие правила для привязки параметров:</span><span class="sxs-lookup"><span data-stu-id="11241-106">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="11241-107">Если параметр имеет тип «простой», веб-API пытается получить значение из URI.</span><span class="sxs-lookup"><span data-stu-id="11241-107">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="11241-108">Простые типы включают в себя .NET [типов-примитивов](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, и т. д), а также **TimeSpan**, **DateTime**, **Guid**, **десятичное**, и **строка**, *, а также* любой тип с преобразователь типов для преобразования из строки.</span><span class="sxs-lookup"><span data-stu-id="11241-108">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="11241-109">(Дополнительные сведения о преобразователях типов более поздней версии.)</span><span class="sxs-lookup"><span data-stu-id="11241-109">(More about type converters later.)</span></span>
- <span data-ttu-id="11241-110">Для сложных типов, веб-API пытается считать значение из текста сообщения, с помощью [форматирования типа мультимедиа](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="11241-110">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="11241-111">Например ниже приведен типичный метод контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="11241-111">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="11241-112">*Идентификатор* параметр &quot;простой&quot; введите, после чего пытается получить значение из URI запроса веб-API.</span><span class="sxs-lookup"><span data-stu-id="11241-112">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="11241-113">*Элемент* параметр — сложный тип, поэтому веб-API использует модуль форматирования типа мультимедиа для чтения значения из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="11241-113">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="11241-114">Чтобы получить значение из URI, веб-API выглядит в качестве данных маршрута и строке запроса URI.</span><span class="sxs-lookup"><span data-stu-id="11241-114">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="11241-115">Данные маршрута заполняется в том случае, если система маршрутизации выполняет синтаксический анализ URI и сопоставляет его с маршрутом.</span><span class="sxs-lookup"><span data-stu-id="11241-115">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="11241-116">Дополнительные сведения см. в разделе [Маршрутизация и Выбор действия](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="11241-116">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="11241-117">В оставшейся части этой статьи я покажу, как можно настроить процесс привязки модели.</span><span class="sxs-lookup"><span data-stu-id="11241-117">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="11241-118">Сложные типы Однако рекомендуется использовать модули форматирования типа мультимедиа, по возможности.</span><span class="sxs-lookup"><span data-stu-id="11241-118">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="11241-119">Ключевой принцип HTTP является то, что ресурсы отправляются в тексте сообщения, используя согласование содержимого для указания представление ресурса.</span><span class="sxs-lookup"><span data-stu-id="11241-119">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="11241-120">Модули форматирования типа мультимедиа были разработаны для этой цели.</span><span class="sxs-lookup"><span data-stu-id="11241-120">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="11241-121">С помощью [FromUri]</span><span class="sxs-lookup"><span data-stu-id="11241-121">Using [FromUri]</span></span>

<span data-ttu-id="11241-122">Чтобы заставить веб-API для чтения из URI со сложным типом, добавьте **[FromUri]** к параметру.</span><span class="sxs-lookup"><span data-stu-id="11241-122">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="11241-123">В следующем примере определяется `GeoPoint` типа, а также метод контроллера, который получает `GeoPoint` из URI.</span><span class="sxs-lookup"><span data-stu-id="11241-123">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="11241-124">Клиент можно поместить значения широты и долготы в строке запроса и веб-API будет использовать их для создания `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="11241-124">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="11241-125">Пример:</span><span class="sxs-lookup"><span data-stu-id="11241-125">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="11241-126">С помощью [FromBody]</span><span class="sxs-lookup"><span data-stu-id="11241-126">Using [FromBody]</span></span>

<span data-ttu-id="11241-127">Чтобы заставить веб-API для чтения к простому типу из текста запроса, добавьте **[FromBody]** к параметру атрибут:</span><span class="sxs-lookup"><span data-stu-id="11241-127">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="11241-128">В этом примере веб-API будет использовать модуль форматирования типа мультимедиа для чтения значение *имя* из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="11241-128">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="11241-129">Ниже приведен пример запроса клиента.</span><span class="sxs-lookup"><span data-stu-id="11241-129">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="11241-130">Если параметр имеет [FromBody], веб-API использует заголовок Content-Type для выбора модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="11241-130">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="11241-131">В этом примере — тип содержимого &quot;application/json&quot; и тело запроса является необработанная строка JSON (а не объект JSON).</span><span class="sxs-lookup"><span data-stu-id="11241-131">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="11241-132">Не более одного параметра может считывать данные из текста сообщения.</span><span class="sxs-lookup"><span data-stu-id="11241-132">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="11241-133">Так что это не будет работать:</span><span class="sxs-lookup"><span data-stu-id="11241-133">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="11241-134">Причина для этого правила является то, что тело запроса может храниться в потоке без буферизации, которые могут быть прочитаны только один раз.</span><span class="sxs-lookup"><span data-stu-id="11241-134">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="11241-135">Преобразователи типов</span><span class="sxs-lookup"><span data-stu-id="11241-135">Type Converters</span></span>

<span data-ttu-id="11241-136">Веб-API следует считать класс простой тип (таким образом, веб-API будет пытаться привязать его из URI) можно сделать, создав **TypeConverter** и предоставляя преобразование строк.</span><span class="sxs-lookup"><span data-stu-id="11241-136">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="11241-137">В следующем коде показан `GeoPoint` класс, представляющий точку географических плюс строка **TypeConverter** , преобразующий из строк `GeoPoint` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="11241-137">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="11241-138">`GeoPoint` Класс снабжен **[TypeConverter]** атрибут для указания преобразователь типов.</span><span class="sxs-lookup"><span data-stu-id="11241-138">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="11241-139">(В этом примере впечатлил Майк стол записи блога [способ привязки для пользовательских объектов в сигнатурах действия в MVC или веб-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span><span class="sxs-lookup"><span data-stu-id="11241-139">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="11241-140">Теперь веб-API будет рассматривать `GeoPoint` как простой тип, означающее, что она попытается привязать `GeoPoint` параметров из URI.</span><span class="sxs-lookup"><span data-stu-id="11241-140">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="11241-141">Не нужно включать **[FromUri]** параметра.</span><span class="sxs-lookup"><span data-stu-id="11241-141">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="11241-142">Клиент может вызвать метод с URI следующим образом:</span><span class="sxs-lookup"><span data-stu-id="11241-142">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="11241-143">Связыватели моделей</span><span class="sxs-lookup"><span data-stu-id="11241-143">Model Binders</span></span>

<span data-ttu-id="11241-144">Более гибкий вариант, чем преобразователь типов для создания настраиваемый связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="11241-144">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="11241-145">С помощью связывателя модели иметь доступ к HTTP-запроса, описание действия и необработанных значений из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="11241-145">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="11241-146">Чтобы создать связыватель модели, реализовать **IModelBinder** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="11241-146">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="11241-147">Этот интерфейс определяет единственный метод, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="11241-147">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="11241-148">Вот связыватель модели для `GeoPoint` объектов.</span><span class="sxs-lookup"><span data-stu-id="11241-148">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="11241-149">Связыватель модели Получает необработанные входные значения из *поставщик значений*.</span><span class="sxs-lookup"><span data-stu-id="11241-149">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="11241-150">Такая архитектура позволяет разделить две разные функции:</span><span class="sxs-lookup"><span data-stu-id="11241-150">This design separates two distinct functions:</span></span>

- <span data-ttu-id="11241-151">Поставщик значений принимает HTTP-запроса и заполняет словарь пар "ключ значение".</span><span class="sxs-lookup"><span data-stu-id="11241-151">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="11241-152">Связыватель модели использует этот словарь для заполнения модели.</span><span class="sxs-lookup"><span data-stu-id="11241-152">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="11241-153">Поставщик значения по умолчанию в веб-API получает значения из данных маршрута и строку запроса.</span><span class="sxs-lookup"><span data-stu-id="11241-153">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="11241-154">Например, если URL-адрес является `http://localhost/api/values/1?location=48,-122`, поставщик значений создает следующие пары "ключ значение":</span><span class="sxs-lookup"><span data-stu-id="11241-154">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="11241-155">Идентификатор = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="11241-155">id = &quot;1&quot;</span></span>
- <span data-ttu-id="11241-156">расположение = &quot;48,122&quot;</span><span class="sxs-lookup"><span data-stu-id="11241-156">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="11241-157">(Я предполагаю, шаблон маршрута по умолчанию, который является &quot;api / {controller} / {id}&quot;.)</span><span class="sxs-lookup"><span data-stu-id="11241-157">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="11241-158">Имя параметра для привязки хранится в **ModelBindingContext.ModelName** свойство.</span><span class="sxs-lookup"><span data-stu-id="11241-158">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="11241-159">Связыватель модели ищет ключ с этим значением в словаре.</span><span class="sxs-lookup"><span data-stu-id="11241-159">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="11241-160">Если значение существует и могут быть преобразованы в `GeoPoint`, связыватель модели присваивает это значение привязанного **ModelBindingContext.Model** свойство.</span><span class="sxs-lookup"><span data-stu-id="11241-160">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="11241-161">Обратите внимание на то, что связыватель модели не ограничивается простым типом преобразования.</span><span class="sxs-lookup"><span data-stu-id="11241-161">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="11241-162">В этом примере связывателя модели в первую очередь в таблицу из известных расположений, и в случае неудачи использует преобразование типов.</span><span class="sxs-lookup"><span data-stu-id="11241-162">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="11241-163">**Параметр связывателя модели**</span><span class="sxs-lookup"><span data-stu-id="11241-163">**Setting the Model Binder**</span></span>

<span data-ttu-id="11241-164">Существует несколько способов задать связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="11241-164">There are several ways to set a model binder.</span></span> <span data-ttu-id="11241-165">Во-первых, можно добавить **[ModelBinder]** к параметру.</span><span class="sxs-lookup"><span data-stu-id="11241-165">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="11241-166">Можно также добавить **[ModelBinder]** к типу атрибута.</span><span class="sxs-lookup"><span data-stu-id="11241-166">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="11241-167">Веб-API будет использоваться связыватель модели указанного все параметры этого типа.</span><span class="sxs-lookup"><span data-stu-id="11241-167">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="11241-168">Наконец, можно добавить поставщик связывателей модели для **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="11241-168">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="11241-169">Поставщик связывателей модели — просто класс фабрики, создающий связыватель модели.</span><span class="sxs-lookup"><span data-stu-id="11241-169">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="11241-170">Поставщик можно создать путем наследования от [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) класса.</span><span class="sxs-lookup"><span data-stu-id="11241-170">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="11241-171">Тем не менее, если связывателю модели обрабатывает одного типа, проще использовать встроенное **SimpleModelBinderProvider**, разработанный для этой цели.</span><span class="sxs-lookup"><span data-stu-id="11241-171">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="11241-172">В следующем примере кода показано, как это сделать:</span><span class="sxs-lookup"><span data-stu-id="11241-172">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="11241-173">С помощью поставщика привязки модели, по-прежнему необходимо добавить **[ModelBinder]** к параметру, чтобы сообщить веб-API следует использовать связыватель модели и не модуль форматирования типа мультимедиа.</span><span class="sxs-lookup"><span data-stu-id="11241-173">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="11241-174">Но теперь не нужно указывать тип связывателя модели в атрибуте:</span><span class="sxs-lookup"><span data-stu-id="11241-174">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="11241-175">Поставщики значений</span><span class="sxs-lookup"><span data-stu-id="11241-175">Value Providers</span></span>

<span data-ttu-id="11241-176">Я упомянул, что привязка модели получает значения от поставщика значений.</span><span class="sxs-lookup"><span data-stu-id="11241-176">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="11241-177">Чтобы создать поставщик пользовательского значения, реализовать **IValueProvider** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="11241-177">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="11241-178">Ниже приведен пример, который извлекает значения из файлов cookie в запросе.</span><span class="sxs-lookup"><span data-stu-id="11241-178">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="11241-179">Необходимо также создать фабрик поставщиков значений путем наследования от **ValueProviderFactory** класса.</span><span class="sxs-lookup"><span data-stu-id="11241-179">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="11241-180">Добавление фабрик поставщиков значений для **HttpConfiguration** следующим образом.</span><span class="sxs-lookup"><span data-stu-id="11241-180">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="11241-181">Веб-API объединяет все поставщики значений, поэтому при вызове метода связыватель модели **ValueProvider.GetValue**, связыватель модели получает значение из первого поставщика значений, которое сможет его создания.</span><span class="sxs-lookup"><span data-stu-id="11241-181">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="11241-182">Кроме того, можно задать фабрик поставщиков значений параметра на уровне с помощью **ValueProvider** атрибут, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="11241-182">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="11241-183">Это сообщает веб-API используют привязку модели с помощью фабрики поставщика указанное значение, а не использовать любой из других поставщиков зарегистрированным значением.</span><span class="sxs-lookup"><span data-stu-id="11241-183">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="11241-184">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="11241-184">HttpParameterBinding</span></span>

<span data-ttu-id="11241-185">Связыватели моделей — это определенный экземпляр более общий механизм.</span><span class="sxs-lookup"><span data-stu-id="11241-185">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="11241-186">Если взглянуть на **[ModelBinder]** атрибут, вы увидите, что он является производным от абстрактного **ParameterBindingAttribute** класса.</span><span class="sxs-lookup"><span data-stu-id="11241-186">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="11241-187">Этот класс определяет единственный метод, **GetBinding**, который возвращает **HttpParameterBinding** объекта:</span><span class="sxs-lookup"><span data-stu-id="11241-187">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="11241-188">**HttpParameterBinding** отвечает за привязывая параметр к значению.</span><span class="sxs-lookup"><span data-stu-id="11241-188">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="11241-189">В случае использования **[ModelBinder]**, возвращает атрибут **HttpParameterBinding** реализация, которая использует **IModelBinder** для выполнения фактического привязки.</span><span class="sxs-lookup"><span data-stu-id="11241-189">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="11241-190">Вы также можете реализовать собственные **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="11241-190">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="11241-191">Предположим, например, чтобы просмотреть теги eTag из `if-match` и `if-none-match` заголовков запроса.</span><span class="sxs-lookup"><span data-stu-id="11241-191">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="11241-192">Мы начнем с определения класса для представления теги eTag.</span><span class="sxs-lookup"><span data-stu-id="11241-192">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="11241-193">Также мы определим перечисление, чтобы указать, следует ли получить ETag из `if-match` заголовок или `if-none-match` заголовка.</span><span class="sxs-lookup"><span data-stu-id="11241-193">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="11241-194">Вот **HttpParameterBinding** , возвращает ETag из нужного заголовка и привязывает его к параметру типа ETag:</span><span class="sxs-lookup"><span data-stu-id="11241-194">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="11241-195">**ExecuteBindingAsync** метод выполняет привязку.</span><span class="sxs-lookup"><span data-stu-id="11241-195">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="11241-196">В этом методе, добавить значение привязанного параметра **аргумент ActionArgument** словарь в **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="11241-196">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="11241-197">Если ваш **ExecuteBindingAsync** метод считывает текст сообщения запроса, переопределить **WillReadBody** свойство возвращает значение true.</span><span class="sxs-lookup"><span data-stu-id="11241-197">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="11241-198">Текст запроса могут быть небуферизованный поток, можно прочитать только один раз, поэтому веб-API применяет правило, не более одной привязки можно прочитать тело сообщения.</span><span class="sxs-lookup"><span data-stu-id="11241-198">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>


<span data-ttu-id="11241-199">Чтобы применить пользовательский **HttpParameterBinding**, можно определить атрибут, который является производным от **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="11241-199">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="11241-200">Для `ETagParameterBinding`, мы определим два атрибута, один для `if-match` заголовки и один для `if-none-match` заголовки.</span><span class="sxs-lookup"><span data-stu-id="11241-200">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="11241-201">Оба являются производными от абстрактного базового класса.</span><span class="sxs-lookup"><span data-stu-id="11241-201">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="11241-202">Ниже приведен метод контроллера, который использует `[IfNoneMatch]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="11241-202">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="11241-203">Помимо **ParameterBindingAttribute**, имеется другой обработчик для добавления настраиваемого **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="11241-203">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="11241-204">На **HttpConfiguration** объекта, **ParameterBindingRules** свойство — это коллекция функций anomymous типа (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="11241-204">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anomymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="11241-205">Например, можно добавить правила, использующего любой параметр ETag в метод GET `ETagParameterBinding` с `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="11241-205">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="11241-206">Данная функция должна возвращать `null` для параметров, где привязка не применимо.</span><span class="sxs-lookup"><span data-stu-id="11241-206">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="11241-207">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="11241-207">IActionValueBinder</span></span>

<span data-ttu-id="11241-208">Весь процесс привязки параметров контролируется службой подключаемые **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="11241-208">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="11241-209">Реализация по умолчанию **IActionValueBinder** делает следующее:</span><span class="sxs-lookup"><span data-stu-id="11241-209">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="11241-210">Найдите **ParameterBindingAttribute** в параметре.</span><span class="sxs-lookup"><span data-stu-id="11241-210">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="11241-211">Сюда входят **[FromBody]**, **[FromUri]**, и **[ModelBinder]**, или настраиваемых атрибутов.</span><span class="sxs-lookup"><span data-stu-id="11241-211">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="11241-212">В противном случае папка **HttpConfiguration.ParameterBindingRules** для функции, которая возвращает ненулевой **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="11241-212">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="11241-213">В противном случае используйте правила по умолчанию, которые я описал ранее.</span><span class="sxs-lookup"><span data-stu-id="11241-213">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="11241-214">Если тип параметра «простой» или преобразователь типов, привязки из URI.</span><span class="sxs-lookup"><span data-stu-id="11241-214">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="11241-215">Это эквивалентно помещения **[FromUri]** атрибута параметра.</span><span class="sxs-lookup"><span data-stu-id="11241-215">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="11241-216">В противном случае попробуйте считывать параметр из тела сообщения.</span><span class="sxs-lookup"><span data-stu-id="11241-216">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="11241-217">Это эквивалентно помещения **[FromBody]** параметра.</span><span class="sxs-lookup"><span data-stu-id="11241-217">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="11241-218">При желании, можно заменить весь **IActionValueBinder** с пользовательской реализацией.</span><span class="sxs-lookup"><span data-stu-id="11241-218">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11241-219">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="11241-219">Additional Resources</span></span>

[<span data-ttu-id="11241-220">Пример привязки пользовательского параметра</span><span class="sxs-lookup"><span data-stu-id="11241-220">Custom Parameter Binding Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

<span data-ttu-id="11241-221">Майк стол написал хороший ряд записей блога о привязке параметра веб-API:</span><span class="sxs-lookup"><span data-stu-id="11241-221">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="11241-222">Как веб-API работает привязка параметра</span><span class="sxs-lookup"><span data-stu-id="11241-222">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="11241-223">Привязки параметров стиля MVC для веб-API</span><span class="sxs-lookup"><span data-stu-id="11241-223">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="11241-224">Как выполнить привязку к пользовательских объектов в сигнатурах действия в MVC или веб-API</span><span class="sxs-lookup"><span data-stu-id="11241-224">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="11241-225">Создание поставщика пользовательских значений в веб-API</span><span class="sxs-lookup"><span data-stu-id="11241-225">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="11241-226">Привязка параметров веб-API взгляд изнутри</span><span class="sxs-lookup"><span data-stu-id="11241-226">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
