---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Модульное тестирование контроллеров в ASP.NET Web API 2 | Документация Майкрософт
author: MikeWasson
description: В этом разделе описывается определенных приемов контроллеры модульного тестирования в веб-API 2. Перед прочтением этого раздела, может потребоваться ознакомьтесь с руководством единицы...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 1a3cfa1962a5f914fd2393088bec4424f6453d07
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389387"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="fbf3a-104">Модульное тестирование контроллеров в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="fbf3a-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="fbf3a-105">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fbf3a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="fbf3a-106">В этом разделе описывается определенных приемов контроллеры модульного тестирования в веб-API 2.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="fbf3a-107">Перед прочтением этого раздела, ознакомьтесь с руководством может потребоваться [модульного тестирования ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), который показывает, как добавить в решение проект модульного теста.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fbf3a-108">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="fbf3a-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="fbf3a-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="fbf3a-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="fbf3a-110">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="fbf3a-110">Web API 2</span></span>
> - <span data-ttu-id="fbf3a-111">[MOQ](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="fbf3a-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="fbf3a-112">Я использовал Moq, но тот же принцип применим для любой платформы макетирования.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="fbf3a-113">MOQ 4.5.30 (и более поздних версий) поддерживает Visual Studio 2017, Roslyn и .NET 4.5 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="fbf3a-114">Распространенный подход в модульных тестах — &quot;упорядочить act утверждение&quot;:</span><span class="sxs-lookup"><span data-stu-id="fbf3a-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="fbf3a-115">Упорядочить: Настройте все необходимые компоненты для выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="fbf3a-116">ACT: Выполните тест.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-116">Act: Perform the test.</span></span>
- <span data-ttu-id="fbf3a-117">Assert: Убедитесь, что тест успешно.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="fbf3a-118">На этапе компоновки будут часто использовать макет или заглушки объектов.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="fbf3a-119">Сводит к минимуму число зависимостей, чтобы тест предназначен для тестирования одну вещь.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="fbf3a-120">Вот некоторые действия, что следует подвергать модульному тестированию в контроллерах веб-API.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="fbf3a-121">Это действие возвращает правильный тип ответа.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="fbf3a-122">Недопустимые параметры возврата ответа исправить ошибку.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="fbf3a-123">Действие вызывает правильный метод на уровне репозитория или службы.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="fbf3a-124">Если ответ содержит модель предметной области, проверьте тип модели.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="fbf3a-125">Ниже перечислены некоторые общие действия для тестирования, но конкретная обработка зависит от реализации контроллера.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="fbf3a-126">В частности, она заметным отличиям ли возвращать действий контроллера **HttpResponseMessage** или **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="fbf3a-127">Дополнительные сведения об этих типах результат, см. в разделе [результатов действий в веб-Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="fbf3a-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="fbf3a-128">Тестирование действий, которые возвращают HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="fbf3a-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="fbf3a-129">Ниже приведен пример контроллера возвращаемого значения которой действия **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="fbf3a-130">Обратите внимание, что контроллер использует внедрение зависимостей для внедрения `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="fbf3a-131">В результате контроллер эффективность тестирования, так как можно внедрить макет репозитория.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="fbf3a-132">Следующий модульный тест проверяет, что `Get` метод записи `Product` в текст ответа.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="fbf3a-133">Предполагается, что `repository` является макет `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="fbf3a-134">Очень важно задать **запроса** и **конфигурации** на контроллере.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="fbf3a-135">В противном случае тест завершится ошибкой с **ArgumentNullException** или **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="fbf3a-136">Тестирование компоновки</span><span class="sxs-lookup"><span data-stu-id="fbf3a-136">Testing Link Generation</span></span>

<span data-ttu-id="fbf3a-137">`Post` Вызовы методов **UrlHelper.Link** для создания ссылок в ответе.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="fbf3a-138">Для этого требуется настройка немного сложнее в модульном тесте:</span><span class="sxs-lookup"><span data-stu-id="fbf3a-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="fbf3a-139">**UrlHelper** класс должен запроса URL-адрес и данные маршрута, поэтому тест должен задать значения для них.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="fbf3a-140">Другой вариант — макет или заглушки **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="fbf3a-141">При таком подходе вы замените значение по умолчанию [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) с версией макет или заглушки, которая возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="fbf3a-142">Давайте перепишем тестирования с помощью [Moq](https://github.com/Moq) framework.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="fbf3a-143">Установить `Moq` пакет NuGet в проекте теста.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="fbf3a-144">В этой версии не требуется настроить все данные маршрута, так как макет **UrlHelper** возвращает строковую константу.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="fbf3a-145">Тестирование действий, которые возвращают IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="fbf3a-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="fbf3a-146">В веб-API 2, может возвращать действие контроллера **IHttpActionResult**, который является аналогом **ActionResult** в ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="fbf3a-147">**IHttpActionResult** интерфейс определяет шаблон команды для создания ответов HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="fbf3a-148">Вместо непосредственно создания ответа, возвращает ли контроллер **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="fbf3a-149">Позже, конвейер вызывает **IHttpActionResult** для создания ответа.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="fbf3a-150">Такой подход упрощает написание модульных тестов, так как вы можете пропустить массу установки, которая необходима для **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="fbf3a-151">Ниже приведен пример контроллера возвращаемого значения которой действия **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="fbf3a-152">В этом примере показаны некоторые распространенные шаблоны с помощью **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="fbf3a-153">Давайте посмотрим, как модульное тестирование их.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="fbf3a-154">Действие возвращает 200 (ОК) с телом ответа</span><span class="sxs-lookup"><span data-stu-id="fbf3a-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="fbf3a-155">`Get` Вызовы методов `Ok(product)` при обнаружении продукта.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="fbf3a-156">В модульном тесте, убедитесь, что возвращаемый тип — **OkNegotiatedContentResult** и возвращаемый продукт имеет идентификатор справа.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="fbf3a-157">Обратите внимание на то, что модульный тест не выполняет результат действия.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="fbf3a-158">Можно предположить, что результат действия правильно создает HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="fbf3a-159">(Вот почему платформа веб-API имеет свой собственный модульные тесты!)</span><span class="sxs-lookup"><span data-stu-id="fbf3a-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="fbf3a-160">Действие возвращает ошибку 404 (не найдено)</span><span class="sxs-lookup"><span data-stu-id="fbf3a-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="fbf3a-161">`Get` Вызовы методов `NotFound()` Если продукт не найден.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="fbf3a-162">В этом случае модульный тест только проверки, если возвращаемый тип — **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="fbf3a-163">Действие возвращает 200 (ОК) без текста ответа</span><span class="sxs-lookup"><span data-stu-id="fbf3a-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="fbf3a-164">`Delete` Вызовы методов `Ok()` возвращать пустой ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="fbf3a-165">Как в предыдущем примере, модульный тест проверяет тип возвращаемого значения, в этом случае **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="fbf3a-166">Действие возвращает код 201 (создано), заголовок расположения</span><span class="sxs-lookup"><span data-stu-id="fbf3a-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="fbf3a-167">`Post` Вызовы методов `CreatedAtRoute` чтобы возвратить ответ HTTP 201 с URI в заголовке Location.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="fbf3a-168">В модульном тесте убедитесь, что действие задает правильные значения маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="fbf3a-169">Действие возвращает другой 2xx с телом ответа</span><span class="sxs-lookup"><span data-stu-id="fbf3a-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="fbf3a-170">`Put` Вызовы методов `Content` чтобы возвратить ответ HTTP 202 (принято) с телом ответа.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="fbf3a-171">Этот случай похож на возвращение 200 (ОК), но модульный тест следует также проверить код состояния.</span><span class="sxs-lookup"><span data-stu-id="fbf3a-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="fbf3a-172">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fbf3a-172">Additional Resources</span></span>

- [<span data-ttu-id="fbf3a-173">Макетирование Entity Framework при модульном тестировании ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="fbf3a-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="fbf3a-174">[Написание тестов для службы веб-API ASP.NET](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (запись блога по Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="fbf3a-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="fbf3a-175">Отладка ASP.NET Web API с помощью отладчика маршрута</span><span class="sxs-lookup"><span data-stu-id="fbf3a-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
