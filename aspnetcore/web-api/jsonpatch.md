---
title: JsonPatch в веб-API ASP.NET Core
author: rick-anderson
description: Сведения об обработке запросов JSON Patch в веб-API ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2019
uid: web-api/jsonpatch
ms.openlocfilehash: e57556e4b3fba55c6c187092593ffab4e31ee2d9
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727025"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="c7511-103">JsonPatch в веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7511-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="c7511-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Kirk Larkin](https://github.com/serpent5) (Кирк Ларкин)</span><span class="sxs-lookup"><span data-stu-id="c7511-104">By [Tom Dykstra](https://github.com/tdykstra) and [Kirk Larkin](https://github.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c7511-105">В этой статье описывается обработка запросов JSON Patch в веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7511-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="package-installation"></a><span data-ttu-id="c7511-106">Установка пакета</span><span class="sxs-lookup"><span data-stu-id="c7511-106">Package installation</span></span>

<span data-ttu-id="c7511-107">Поддержка для JsonPatch включается с помощью пакета `Microsoft.AspNetCore.Mvc.NewtonsoftJson`.</span><span class="sxs-lookup"><span data-stu-id="c7511-107">Support for JsonPatch is enabled using the `Microsoft.AspNetCore.Mvc.NewtonsoftJson` package.</span></span> <span data-ttu-id="c7511-108">Чтобы включить эту функцию, приложения должны сделать следующее:</span><span class="sxs-lookup"><span data-stu-id="c7511-108">To enable this feature, apps must:</span></span>

* <span data-ttu-id="c7511-109">Установить пакет NuGet [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/).</span><span class="sxs-lookup"><span data-stu-id="c7511-109">Install the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package.</span></span>
* <span data-ttu-id="c7511-110">Обновите метод `Startup.ConfigureServices` в проекте, чтобы включить в него вызов к `AddNewtonsoftJson`:</span><span class="sxs-lookup"><span data-stu-id="c7511-110">Update the project's `Startup.ConfigureServices` method to include a call to `AddNewtonsoftJson`:</span></span>

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

<span data-ttu-id="c7511-111">`AddNewtonsoftJson` совместим с методами регистрации службы MVC:</span><span class="sxs-lookup"><span data-stu-id="c7511-111">`AddNewtonsoftJson` is compatible with the MVC service registration methods:</span></span>

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a><span data-ttu-id="c7511-112">JsonPatch, AddNewtonsoftJson и System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="c7511-112">JsonPatch, AddNewtonsoftJson, and System.Text.Json</span></span>
  
<span data-ttu-id="c7511-113">`AddNewtonsoftJson` заменяет `System.Text.Json` с учетом форматировщиков ввода и вывода, используемых для форматирования **всего** содержимого JSON.</span><span class="sxs-lookup"><span data-stu-id="c7511-113">`AddNewtonsoftJson` replaces the `System.Text.Json` based input and output formatters used for formatting **all** JSON content.</span></span> <span data-ttu-id="c7511-114">Чтобы добавить поддержку `JsonPatch` с помощью `Newtonsoft.Json`, оставив другие форматировщики без изменений, измените `Startup.ConfigureServices` проекта следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c7511-114">To add support for `JsonPatch` using `Newtonsoft.Json`, while leaving the other formatters unchanged, update the project's `Startup.ConfigureServices` as follows:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="c7511-115">Для приведенного выше кода требуется ссылка на [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) и следующие операторы using:</span><span class="sxs-lookup"><span data-stu-id="c7511-115">The preceding code requires a reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) and the following using statements:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a><span data-ttu-id="c7511-116">Метод HTTP-запроса PATCH</span><span class="sxs-lookup"><span data-stu-id="c7511-116">PATCH HTTP request method</span></span>

<span data-ttu-id="c7511-117">Методы PUT и [PATCH](https://tools.ietf.org/html/rfc5789) используются для обновления существующего ресурса.</span><span class="sxs-lookup"><span data-stu-id="c7511-117">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="c7511-118">Различие между ними заключается в том, что PUT заменяет весь ресурс, а PATCH указывает только изменения.</span><span class="sxs-lookup"><span data-stu-id="c7511-118">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="c7511-119">JSON PATCH</span><span class="sxs-lookup"><span data-stu-id="c7511-119">JSON Patch</span></span>

<span data-ttu-id="c7511-120">Формат [JSON Patch](https://tools.ietf.org/html/rfc6902) используется для указания обновлений, применяемых к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="c7511-120">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="c7511-121">Документ JSON Patch содержит массив *операций*.</span><span class="sxs-lookup"><span data-stu-id="c7511-121">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="c7511-122">Каждая операция обозначает определенный тип изменений, например добавление элемента массива или замену значения свойства.</span><span class="sxs-lookup"><span data-stu-id="c7511-122">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="c7511-123">Так, следующие документы JSON описывают ресурс, сведения JSON Patch для изменения этого ресурса и результат применения операций изменения.</span><span class="sxs-lookup"><span data-stu-id="c7511-123">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="c7511-124">Пример ресурса</span><span class="sxs-lookup"><span data-stu-id="c7511-124">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="c7511-125">Пример JSON Patch</span><span class="sxs-lookup"><span data-stu-id="c7511-125">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="c7511-126">В приведенном выше документе JSON:</span><span class="sxs-lookup"><span data-stu-id="c7511-126">In the preceding JSON:</span></span>

* <span data-ttu-id="c7511-127">свойство `op` указывает тип операции;</span><span class="sxs-lookup"><span data-stu-id="c7511-127">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="c7511-128">свойство `path` указывает обновляемый элемент;</span><span class="sxs-lookup"><span data-stu-id="c7511-128">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="c7511-129">свойство `value` предоставляет новое значение.</span><span class="sxs-lookup"><span data-stu-id="c7511-129">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="c7511-130">Ресурс после обновления</span><span class="sxs-lookup"><span data-stu-id="c7511-130">Resource after patch</span></span>

<span data-ttu-id="c7511-131">Ниже показан ресурс в том состоянии, которое он принимает после применения приведенного выше документа JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="c7511-131">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="c7511-132">Изменения, внесенные при применении к ресурсу документа JSON Patch, являются атомарными, то есть ни одна из операций не будет применена, если любая из них завершится сбоем.</span><span class="sxs-lookup"><span data-stu-id="c7511-132">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="c7511-133">Синтаксис Path</span><span class="sxs-lookup"><span data-stu-id="c7511-133">Path syntax</span></span>

<span data-ttu-id="c7511-134">Свойство [Path](https://tools.ietf.org/html/rfc6901) в объекте операции содержит косые черты между уровнями.</span><span class="sxs-lookup"><span data-stu-id="c7511-134">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="c7511-135">Например, `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="c7511-135">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="c7511-136">Для указания элементов массива используются числовые индексы, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="c7511-136">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="c7511-137">Первый элемент массива `addresses` будет обозначаться как `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="c7511-137">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="c7511-138">Чтобы применить `add` для последнего элемента массива, укажите дефис (-) вместо номера индекса: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="c7511-138">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="c7511-139">Операции</span><span class="sxs-lookup"><span data-stu-id="c7511-139">Operations</span></span>

<span data-ttu-id="c7511-140">В следующей таблице перечислены поддерживаемые операции, которые определены в [спецификации JSON Patch](https://tools.ietf.org/html/rfc6902).</span><span class="sxs-lookup"><span data-stu-id="c7511-140">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="c7511-141">Операция</span><span class="sxs-lookup"><span data-stu-id="c7511-141">Operation</span></span>  | <span data-ttu-id="c7511-142">Примечания</span><span class="sxs-lookup"><span data-stu-id="c7511-142">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="c7511-143">Добавляет свойство или элемент массива.</span><span class="sxs-lookup"><span data-stu-id="c7511-143">Add a property or array element.</span></span> <span data-ttu-id="c7511-144">Для существующего свойства устанавливает значение.</span><span class="sxs-lookup"><span data-stu-id="c7511-144">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="c7511-145">Удаляет свойство или элемент массива.</span><span class="sxs-lookup"><span data-stu-id="c7511-145">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="c7511-146">Действует так же, как `remove` с последующим `add` в том же расположении.</span><span class="sxs-lookup"><span data-stu-id="c7511-146">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="c7511-147">Действует так же, как `remove` из источника с последующим `add`, в котором указаны место назначения и значение из источника.</span><span class="sxs-lookup"><span data-stu-id="c7511-147">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="c7511-148">Действует так же, как `add`, в котором указаны место назначения и значение из источника.</span><span class="sxs-lookup"><span data-stu-id="c7511-148">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="c7511-149">Возвращает успешный код состояния, если значение `path` совпадает с предоставленным `value`.</span><span class="sxs-lookup"><span data-stu-id="c7511-149">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="c7511-150">JsonPatch в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7511-150">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="c7511-151">Реализация ASP.NET Core для JSON Patch предоставляется в пакете NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="c7511-151">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="c7511-152">Код метода действия</span><span class="sxs-lookup"><span data-stu-id="c7511-152">Action method code</span></span>

<span data-ttu-id="c7511-153">В контроллере API есть метод действия для JSON Patch, который:</span><span class="sxs-lookup"><span data-stu-id="c7511-153">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="c7511-154">помечен атрибутом `HttpPatch`;</span><span class="sxs-lookup"><span data-stu-id="c7511-154">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="c7511-155">принимает `JsonPatchDocument<T>` обычно с указанием `[FromBody]`;</span><span class="sxs-lookup"><span data-stu-id="c7511-155">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="c7511-156">вызывает `ApplyTo` для целевого документа, чтобы применить изменения.</span><span class="sxs-lookup"><span data-stu-id="c7511-156">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="c7511-157">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="c7511-157">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="c7511-158">Этот код из примера приложения работает со следующей моделью `Customer`.</span><span class="sxs-lookup"><span data-stu-id="c7511-158">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="c7511-159">Этот пример метода действия:</span><span class="sxs-lookup"><span data-stu-id="c7511-159">The sample action method:</span></span>

* <span data-ttu-id="c7511-160">Создает документ `Customer`.</span><span class="sxs-lookup"><span data-stu-id="c7511-160">Constructs a `Customer`.</span></span>
* <span data-ttu-id="c7511-161">применяет изменения;</span><span class="sxs-lookup"><span data-stu-id="c7511-161">Applies the patch.</span></span>
* <span data-ttu-id="c7511-162">возвращает результат в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="c7511-162">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="c7511-163">В реальном приложении код будет извлекать данные из хранилища, например из базы данных, и обновлять эту базу данных после применения исправлений.</span><span class="sxs-lookup"><span data-stu-id="c7511-163">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="c7511-164">Состояние модели</span><span class="sxs-lookup"><span data-stu-id="c7511-164">Model state</span></span>

<span data-ttu-id="c7511-165">Указанный выше пример метода действия вызывает перегрузку `ApplyTo`, которая принимает состояние модели в качестве одного из параметров.</span><span class="sxs-lookup"><span data-stu-id="c7511-165">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="c7511-166">В этом случае вы получаете в ответах сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="c7511-166">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="c7511-167">В следующем примере показан текст ответа с кодом 400 "Неверный запрос" для операции `test`:</span><span class="sxs-lookup"><span data-stu-id="c7511-167">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="c7511-168">Динамические объекты</span><span class="sxs-lookup"><span data-stu-id="c7511-168">Dynamic objects</span></span>

<span data-ttu-id="c7511-169">Следующий пример метода действия демонстрирует, как применить исправление к динамическому объекту.</span><span class="sxs-lookup"><span data-stu-id="c7511-169">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="c7511-170">Операция добавления</span><span class="sxs-lookup"><span data-stu-id="c7511-170">The add operation</span></span>

* <span data-ttu-id="c7511-171">Если `path` указывает на элемент массива, новый элемент вставляется перед тем, который указан в параметре `path`.</span><span class="sxs-lookup"><span data-stu-id="c7511-171">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="c7511-172">Если `path` указывает на свойство, задается значение свойства.</span><span class="sxs-lookup"><span data-stu-id="c7511-172">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="c7511-173">Если `path` указывает на несуществующее расположение:</span><span class="sxs-lookup"><span data-stu-id="c7511-173">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="c7511-174">Если обновляемый ресурс является динамическим объектом, добавляется свойство.</span><span class="sxs-lookup"><span data-stu-id="c7511-174">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="c7511-175">Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="c7511-175">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="c7511-176">Следующий пример документа с исправлениями устанавливает значение `CustomerName` и добавляет объект `Order` в конец массива `Orders`.</span><span class="sxs-lookup"><span data-stu-id="c7511-176">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="c7511-177">Операция удаления</span><span class="sxs-lookup"><span data-stu-id="c7511-177">The remove operation</span></span>

* <span data-ttu-id="c7511-178">Если `path` указывает на элемент массива, элемент удаляется.</span><span class="sxs-lookup"><span data-stu-id="c7511-178">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="c7511-179">Если `path` указывает на свойство:</span><span class="sxs-lookup"><span data-stu-id="c7511-179">If `path` points to a property:</span></span>
  * <span data-ttu-id="c7511-180">Если обновляемый ресурс является динамическим объектом, свойство удаляется.</span><span class="sxs-lookup"><span data-stu-id="c7511-180">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="c7511-181">Если обновляемый ресурс является статическим объектом:</span><span class="sxs-lookup"><span data-stu-id="c7511-181">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="c7511-182">Если свойство может принимать значения NULL, ему присваивается значение NULL.</span><span class="sxs-lookup"><span data-stu-id="c7511-182">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="c7511-183">Если свойство не может принимать значения NULL, ему присваивается значение `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="c7511-183">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="c7511-184">Следующий пример документа с исправлениями присваивает `CustomerName` значение NULL и удаляет `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="c7511-184">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="c7511-185">Операция замены</span><span class="sxs-lookup"><span data-stu-id="c7511-185">The replace operation</span></span>

<span data-ttu-id="c7511-186">Эта операция функционально идентична операции `remove` с последующей `add`.</span><span class="sxs-lookup"><span data-stu-id="c7511-186">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="c7511-187">Следующий пример документа с исправлениями устанавливает значение `CustomerName` и заменяет `Orders[0]` новым объектом `Order`.</span><span class="sxs-lookup"><span data-stu-id="c7511-187">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="c7511-188">Операция перемещения</span><span class="sxs-lookup"><span data-stu-id="c7511-188">The move operation</span></span>

* <span data-ttu-id="c7511-189">Если `path` указывает на элемент массива, элемент `from` копируется в расположение элемента `path`, а затем выполняется операция `remove` для элемента `from`.</span><span class="sxs-lookup"><span data-stu-id="c7511-189">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="c7511-190">Если `path` указывает на свойство, значение свойства `from` копируется в свойство `path`, а затем выполняется операция `remove` для свойства `from`.</span><span class="sxs-lookup"><span data-stu-id="c7511-190">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="c7511-191">Если `path` указывает на несуществующее свойство:</span><span class="sxs-lookup"><span data-stu-id="c7511-191">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="c7511-192">Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="c7511-192">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="c7511-193">Если исправляемый ресурс является динамическим объектом, значение `from` копируется в местоположение, указанное параметром `path`, а затем выполняется операция `remove` для свойства `from`.</span><span class="sxs-lookup"><span data-stu-id="c7511-193">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="c7511-194">Следующий пример документа исправления выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c7511-194">The following sample patch document:</span></span>

* <span data-ttu-id="c7511-195">копирует значение `Orders[0].OrderName` в `CustomerName`;</span><span class="sxs-lookup"><span data-stu-id="c7511-195">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="c7511-196">присваивает `Orders[0].OrderName` значение NULL;</span><span class="sxs-lookup"><span data-stu-id="c7511-196">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="c7511-197">перемещает `Orders[1]` в расположение перед `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="c7511-197">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="c7511-198">Операция копирования</span><span class="sxs-lookup"><span data-stu-id="c7511-198">The copy operation</span></span>

<span data-ttu-id="c7511-199">Эта операция функционально аналогична операции `move` без последнего шага `remove`.</span><span class="sxs-lookup"><span data-stu-id="c7511-199">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="c7511-200">Следующий пример документа исправления выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c7511-200">The following sample patch document:</span></span>

* <span data-ttu-id="c7511-201">копирует значение `Orders[0].OrderName` в `CustomerName`;</span><span class="sxs-lookup"><span data-stu-id="c7511-201">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="c7511-202">вставляет копию `Orders[1]` перед `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="c7511-202">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="c7511-203">Операция тестирования</span><span class="sxs-lookup"><span data-stu-id="c7511-203">The test operation</span></span>

<span data-ttu-id="c7511-204">Если значение в расположении, которое задано параметром `path`, отличается от предоставленного в `value` значения, запрос завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="c7511-204">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="c7511-205">В этом случае сбой распространяется на весь запрос PATCH, даже если все остальные операции в документе с исправлениями могли быть выполнены успешно.</span><span class="sxs-lookup"><span data-stu-id="c7511-205">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="c7511-206">Операция `test` традиционно используется для предотвращения обновлений при возможных конфликтах параллелизма.</span><span class="sxs-lookup"><span data-stu-id="c7511-206">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="c7511-207">Следующий пример документа с исправлениями не изменяет прежнее значение `CustomerName` (John), так как тест завершается ошибкой:</span><span class="sxs-lookup"><span data-stu-id="c7511-207">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="c7511-208">Получение кода</span><span class="sxs-lookup"><span data-stu-id="c7511-208">Get the code</span></span>

<span data-ttu-id="c7511-209">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="c7511-209">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="c7511-210">([Инструкция по скачиванию](xref:index#how-to-download-a-sample).)</span><span class="sxs-lookup"><span data-stu-id="c7511-210">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="c7511-211">Чтобы проверить этот пример, запустите приложение и отправьте HTTP-запросы со следующими параметрами:</span><span class="sxs-lookup"><span data-stu-id="c7511-211">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="c7511-212">URL-адрес: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="c7511-212">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="c7511-213">Метод HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="c7511-213">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="c7511-214">Заголовок: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="c7511-214">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="c7511-215">Текст: Скопируйте и вставьте один из примеров документов JSON Patch из папки *JSON* в проекте.</span><span class="sxs-lookup"><span data-stu-id="c7511-215">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7511-216">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c7511-216">Additional resources</span></span>

* [<span data-ttu-id="c7511-217">Спецификация IETF RFC 5789 для метода PATCH</span><span class="sxs-lookup"><span data-stu-id="c7511-217">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="c7511-218">Спецификация IETF RFC 6902 для JSON Patch</span><span class="sxs-lookup"><span data-stu-id="c7511-218">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="c7511-219">Спецификация IETF RFC 6901 для формата Path в JSON Patch</span><span class="sxs-lookup"><span data-stu-id="c7511-219">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="c7511-220">[Документация по JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="c7511-220">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="c7511-221">Содержит ссылки на ресурсы для создания документов JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="c7511-221">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="c7511-222">Исходный код JSON Patch для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7511-222">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c7511-223">В этой статье описывается обработка запросов JSON Patch в веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c7511-223">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="c7511-224">Метод HTTP-запроса PATCH</span><span class="sxs-lookup"><span data-stu-id="c7511-224">PATCH HTTP request method</span></span>

<span data-ttu-id="c7511-225">Методы PUT и [PATCH](https://tools.ietf.org/html/rfc5789) используются для обновления существующего ресурса.</span><span class="sxs-lookup"><span data-stu-id="c7511-225">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="c7511-226">Различие между ними заключается в том, что PUT заменяет весь ресурс, а PATCH указывает только изменения.</span><span class="sxs-lookup"><span data-stu-id="c7511-226">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="c7511-227">JSON PATCH</span><span class="sxs-lookup"><span data-stu-id="c7511-227">JSON Patch</span></span>

<span data-ttu-id="c7511-228">Формат [JSON Patch](https://tools.ietf.org/html/rfc6902) используется для указания обновлений, применяемых к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="c7511-228">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="c7511-229">Документ JSON Patch содержит массив *операций*.</span><span class="sxs-lookup"><span data-stu-id="c7511-229">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="c7511-230">Каждая операция обозначает определенный тип изменений, например добавление элемента массива или замену значения свойства.</span><span class="sxs-lookup"><span data-stu-id="c7511-230">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="c7511-231">Так, следующие документы JSON описывают ресурс, сведения JSON Patch для изменения этого ресурса и результат применения операций изменения.</span><span class="sxs-lookup"><span data-stu-id="c7511-231">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="c7511-232">Пример ресурса</span><span class="sxs-lookup"><span data-stu-id="c7511-232">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="c7511-233">Пример JSON Patch</span><span class="sxs-lookup"><span data-stu-id="c7511-233">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="c7511-234">В приведенном выше документе JSON:</span><span class="sxs-lookup"><span data-stu-id="c7511-234">In the preceding JSON:</span></span>

* <span data-ttu-id="c7511-235">свойство `op` указывает тип операции;</span><span class="sxs-lookup"><span data-stu-id="c7511-235">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="c7511-236">свойство `path` указывает обновляемый элемент;</span><span class="sxs-lookup"><span data-stu-id="c7511-236">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="c7511-237">свойство `value` предоставляет новое значение.</span><span class="sxs-lookup"><span data-stu-id="c7511-237">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="c7511-238">Ресурс после обновления</span><span class="sxs-lookup"><span data-stu-id="c7511-238">Resource after patch</span></span>

<span data-ttu-id="c7511-239">Ниже показан ресурс в том состоянии, которое он принимает после применения приведенного выше документа JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="c7511-239">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="c7511-240">Изменения, внесенные при применении к ресурсу документа JSON Patch, являются атомарными, то есть ни одна из операций не будет применена, если любая из них завершится сбоем.</span><span class="sxs-lookup"><span data-stu-id="c7511-240">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="c7511-241">Синтаксис Path</span><span class="sxs-lookup"><span data-stu-id="c7511-241">Path syntax</span></span>

<span data-ttu-id="c7511-242">Свойство [Path](https://tools.ietf.org/html/rfc6901) в объекте операции содержит косые черты между уровнями.</span><span class="sxs-lookup"><span data-stu-id="c7511-242">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="c7511-243">Например, `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="c7511-243">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="c7511-244">Для указания элементов массива используются числовые индексы, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="c7511-244">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="c7511-245">Первый элемент массива `addresses` будет обозначаться как `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="c7511-245">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="c7511-246">Чтобы применить `add` для последнего элемента массива, укажите дефис (-) вместо номера индекса: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="c7511-246">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="c7511-247">Операции</span><span class="sxs-lookup"><span data-stu-id="c7511-247">Operations</span></span>

<span data-ttu-id="c7511-248">В следующей таблице перечислены поддерживаемые операции, которые определены в [спецификации JSON Patch](https://tools.ietf.org/html/rfc6902).</span><span class="sxs-lookup"><span data-stu-id="c7511-248">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="c7511-249">Операция</span><span class="sxs-lookup"><span data-stu-id="c7511-249">Operation</span></span>  | <span data-ttu-id="c7511-250">Примечания</span><span class="sxs-lookup"><span data-stu-id="c7511-250">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="c7511-251">Добавляет свойство или элемент массива.</span><span class="sxs-lookup"><span data-stu-id="c7511-251">Add a property or array element.</span></span> <span data-ttu-id="c7511-252">Для существующего свойства устанавливает значение.</span><span class="sxs-lookup"><span data-stu-id="c7511-252">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="c7511-253">Удаляет свойство или элемент массива.</span><span class="sxs-lookup"><span data-stu-id="c7511-253">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="c7511-254">Действует так же, как `remove` с последующим `add` в том же расположении.</span><span class="sxs-lookup"><span data-stu-id="c7511-254">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="c7511-255">Действует так же, как `remove` из источника с последующим `add`, в котором указаны место назначения и значение из источника.</span><span class="sxs-lookup"><span data-stu-id="c7511-255">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="c7511-256">Действует так же, как `add`, в котором указаны место назначения и значение из источника.</span><span class="sxs-lookup"><span data-stu-id="c7511-256">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="c7511-257">Возвращает успешный код состояния, если значение `path` совпадает с предоставленным `value`.</span><span class="sxs-lookup"><span data-stu-id="c7511-257">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="c7511-258">JsonPatch в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7511-258">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="c7511-259">Реализация ASP.NET Core для JSON Patch предоставляется в пакете NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="c7511-259">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="c7511-260">Этот пакет входит в состав метапакета [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c7511-260">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="c7511-261">Код метода действия</span><span class="sxs-lookup"><span data-stu-id="c7511-261">Action method code</span></span>

<span data-ttu-id="c7511-262">В контроллере API есть метод действия для JSON Patch, который:</span><span class="sxs-lookup"><span data-stu-id="c7511-262">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="c7511-263">помечен атрибутом `HttpPatch`;</span><span class="sxs-lookup"><span data-stu-id="c7511-263">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="c7511-264">принимает `JsonPatchDocument<T>` обычно с указанием `[FromBody]`;</span><span class="sxs-lookup"><span data-stu-id="c7511-264">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="c7511-265">вызывает `ApplyTo` для целевого документа, чтобы применить изменения.</span><span class="sxs-lookup"><span data-stu-id="c7511-265">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="c7511-266">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="c7511-266">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="c7511-267">Этот код из примера приложения работает со следующей моделью `Customer`.</span><span class="sxs-lookup"><span data-stu-id="c7511-267">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="c7511-268">Этот пример метода действия:</span><span class="sxs-lookup"><span data-stu-id="c7511-268">The sample action method:</span></span>

* <span data-ttu-id="c7511-269">Создает документ `Customer`.</span><span class="sxs-lookup"><span data-stu-id="c7511-269">Constructs a `Customer`.</span></span>
* <span data-ttu-id="c7511-270">применяет изменения;</span><span class="sxs-lookup"><span data-stu-id="c7511-270">Applies the patch.</span></span>
* <span data-ttu-id="c7511-271">возвращает результат в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="c7511-271">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="c7511-272">В реальном приложении код будет извлекать данные из хранилища, например из базы данных, и обновлять эту базу данных после применения исправлений.</span><span class="sxs-lookup"><span data-stu-id="c7511-272">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="c7511-273">Состояние модели</span><span class="sxs-lookup"><span data-stu-id="c7511-273">Model state</span></span>

<span data-ttu-id="c7511-274">Указанный выше пример метода действия вызывает перегрузку `ApplyTo`, которая принимает состояние модели в качестве одного из параметров.</span><span class="sxs-lookup"><span data-stu-id="c7511-274">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="c7511-275">В этом случае вы получаете в ответах сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="c7511-275">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="c7511-276">В следующем примере показан текст ответа с кодом 400 "Неверный запрос" для операции `test`:</span><span class="sxs-lookup"><span data-stu-id="c7511-276">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="c7511-277">Динамические объекты</span><span class="sxs-lookup"><span data-stu-id="c7511-277">Dynamic objects</span></span>

<span data-ttu-id="c7511-278">Следующий пример метода действия демонстрирует, как применить исправление к динамическому объекту.</span><span class="sxs-lookup"><span data-stu-id="c7511-278">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="c7511-279">Операция добавления</span><span class="sxs-lookup"><span data-stu-id="c7511-279">The add operation</span></span>

* <span data-ttu-id="c7511-280">Если `path` указывает на элемент массива, новый элемент вставляется перед тем, который указан в параметре `path`.</span><span class="sxs-lookup"><span data-stu-id="c7511-280">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="c7511-281">Если `path` указывает на свойство, задается значение свойства.</span><span class="sxs-lookup"><span data-stu-id="c7511-281">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="c7511-282">Если `path` указывает на несуществующее расположение:</span><span class="sxs-lookup"><span data-stu-id="c7511-282">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="c7511-283">Если обновляемый ресурс является динамическим объектом, добавляется свойство.</span><span class="sxs-lookup"><span data-stu-id="c7511-283">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="c7511-284">Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="c7511-284">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="c7511-285">Следующий пример документа с исправлениями устанавливает значение `CustomerName` и добавляет объект `Order` в конец массива `Orders`.</span><span class="sxs-lookup"><span data-stu-id="c7511-285">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="c7511-286">Операция удаления</span><span class="sxs-lookup"><span data-stu-id="c7511-286">The remove operation</span></span>

* <span data-ttu-id="c7511-287">Если `path` указывает на элемент массива, элемент удаляется.</span><span class="sxs-lookup"><span data-stu-id="c7511-287">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="c7511-288">Если `path` указывает на свойство:</span><span class="sxs-lookup"><span data-stu-id="c7511-288">If `path` points to a property:</span></span>
  * <span data-ttu-id="c7511-289">Если обновляемый ресурс является динамическим объектом, свойство удаляется.</span><span class="sxs-lookup"><span data-stu-id="c7511-289">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="c7511-290">Если обновляемый ресурс является статическим объектом:</span><span class="sxs-lookup"><span data-stu-id="c7511-290">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="c7511-291">Если свойство может принимать значения NULL, ему присваивается значение NULL.</span><span class="sxs-lookup"><span data-stu-id="c7511-291">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="c7511-292">Если свойство не может принимать значения NULL, ему присваивается значение `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="c7511-292">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="c7511-293">Следующий пример документа с исправлениями присваивает `CustomerName` значение NULL и удаляет `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="c7511-293">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="c7511-294">Операция замены</span><span class="sxs-lookup"><span data-stu-id="c7511-294">The replace operation</span></span>

<span data-ttu-id="c7511-295">Эта операция функционально идентична операции `remove` с последующей `add`.</span><span class="sxs-lookup"><span data-stu-id="c7511-295">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="c7511-296">Следующий пример документа с исправлениями устанавливает значение `CustomerName` и заменяет `Orders[0]` новым объектом `Order`.</span><span class="sxs-lookup"><span data-stu-id="c7511-296">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="c7511-297">Операция перемещения</span><span class="sxs-lookup"><span data-stu-id="c7511-297">The move operation</span></span>

* <span data-ttu-id="c7511-298">Если `path` указывает на элемент массива, элемент `from` копируется в расположение элемента `path`, а затем выполняется операция `remove` для элемента `from`.</span><span class="sxs-lookup"><span data-stu-id="c7511-298">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="c7511-299">Если `path` указывает на свойство, значение свойства `from` копируется в свойство `path`, а затем выполняется операция `remove` для свойства `from`.</span><span class="sxs-lookup"><span data-stu-id="c7511-299">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="c7511-300">Если `path` указывает на несуществующее свойство:</span><span class="sxs-lookup"><span data-stu-id="c7511-300">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="c7511-301">Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="c7511-301">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="c7511-302">Если исправляемый ресурс является динамическим объектом, значение `from` копируется в местоположение, указанное параметром `path`, а затем выполняется операция `remove` для свойства `from`.</span><span class="sxs-lookup"><span data-stu-id="c7511-302">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="c7511-303">Следующий пример документа исправления выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c7511-303">The following sample patch document:</span></span>

* <span data-ttu-id="c7511-304">копирует значение `Orders[0].OrderName` в `CustomerName`;</span><span class="sxs-lookup"><span data-stu-id="c7511-304">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="c7511-305">присваивает `Orders[0].OrderName` значение NULL;</span><span class="sxs-lookup"><span data-stu-id="c7511-305">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="c7511-306">перемещает `Orders[1]` в расположение перед `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="c7511-306">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="c7511-307">Операция копирования</span><span class="sxs-lookup"><span data-stu-id="c7511-307">The copy operation</span></span>

<span data-ttu-id="c7511-308">Эта операция функционально аналогична операции `move` без последнего шага `remove`.</span><span class="sxs-lookup"><span data-stu-id="c7511-308">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="c7511-309">Следующий пример документа исправления выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c7511-309">The following sample patch document:</span></span>

* <span data-ttu-id="c7511-310">копирует значение `Orders[0].OrderName` в `CustomerName`;</span><span class="sxs-lookup"><span data-stu-id="c7511-310">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="c7511-311">вставляет копию `Orders[1]` перед `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="c7511-311">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="c7511-312">Операция тестирования</span><span class="sxs-lookup"><span data-stu-id="c7511-312">The test operation</span></span>

<span data-ttu-id="c7511-313">Если значение в расположении, которое задано параметром `path`, отличается от предоставленного в `value` значения, запрос завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="c7511-313">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="c7511-314">В этом случае сбой распространяется на весь запрос PATCH, даже если все остальные операции в документе с исправлениями могли быть выполнены успешно.</span><span class="sxs-lookup"><span data-stu-id="c7511-314">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="c7511-315">Операция `test` традиционно используется для предотвращения обновлений при возможных конфликтах параллелизма.</span><span class="sxs-lookup"><span data-stu-id="c7511-315">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="c7511-316">Следующий пример документа с исправлениями не изменяет прежнее значение `CustomerName` (John), так как тест завершается ошибкой:</span><span class="sxs-lookup"><span data-stu-id="c7511-316">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="c7511-317">Получение кода</span><span class="sxs-lookup"><span data-stu-id="c7511-317">Get the code</span></span>

<span data-ttu-id="c7511-318">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="c7511-318">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="c7511-319">([Инструкция по скачиванию](xref:index#how-to-download-a-sample).)</span><span class="sxs-lookup"><span data-stu-id="c7511-319">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="c7511-320">Чтобы проверить этот пример, запустите приложение и отправьте HTTP-запросы со следующими параметрами:</span><span class="sxs-lookup"><span data-stu-id="c7511-320">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="c7511-321">URL-адрес: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="c7511-321">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="c7511-322">Метод HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="c7511-322">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="c7511-323">Заголовок: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="c7511-323">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="c7511-324">Текст: Скопируйте и вставьте один из примеров документов JSON Patch из папки *JSON* в проекте.</span><span class="sxs-lookup"><span data-stu-id="c7511-324">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7511-325">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c7511-325">Additional resources</span></span>

* [<span data-ttu-id="c7511-326">Спецификация IETF RFC 5789 для метода PATCH</span><span class="sxs-lookup"><span data-stu-id="c7511-326">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="c7511-327">Спецификация IETF RFC 6902 для JSON Patch</span><span class="sxs-lookup"><span data-stu-id="c7511-327">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="c7511-328">Спецификация IETF RFC 6901 для формата Path в JSON Patch</span><span class="sxs-lookup"><span data-stu-id="c7511-328">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="c7511-329">[Документация по JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="c7511-329">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="c7511-330">Содержит ссылки на ресурсы для создания документов JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="c7511-330">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="c7511-331">Исходный код JSON Patch для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7511-331">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
