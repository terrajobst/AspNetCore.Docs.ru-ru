---
title: JsonPatch в веб-API ASP.NET Core
author: tdykstra
description: Сведения об обработке запросов JSON Patch в веб-API ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/24/2019
uid: web-api/jsonpatch
ms.openlocfilehash: 97264903d85dbb397e85fdbf7b070e2aaae74bc8
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815545"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="e7ca8-103">JsonPatch в веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7ca8-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="e7ca8-104">Автор: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="e7ca8-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e7ca8-105">В этой статье описывается обработка запросов JSON Patch в веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="e7ca8-106">Метод HTTP-запроса PATCH</span><span class="sxs-lookup"><span data-stu-id="e7ca8-106">PATCH HTTP request method</span></span>

<span data-ttu-id="e7ca8-107">Методы PUT и [PATCH](https://tools.ietf.org/html/rfc5789) используются для обновления существующего ресурса.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-107">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="e7ca8-108">Различие между ними заключается в том, что PUT заменяет весь ресурс, а PATCH указывает только изменения.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-108">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="e7ca8-109">JSON PATCH</span><span class="sxs-lookup"><span data-stu-id="e7ca8-109">JSON Patch</span></span>

<span data-ttu-id="e7ca8-110">Формат [JSON Patch](https://tools.ietf.org/html/rfc6902) используется для указания обновлений, применяемых к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="e7ca8-111">Документ JSON Patch содержит массив *операций*.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-111">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="e7ca8-112">Каждая операция обозначает определенный тип изменений, например добавление элемента массива или замену значения свойства.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-112">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="e7ca8-113">Так, следующие документы JSON описывают ресурс, сведения JSON Patch для изменения этого ресурса и результат применения операций изменения.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-113">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="e7ca8-114">Пример ресурса</span><span class="sxs-lookup"><span data-stu-id="e7ca8-114">Resource example</span></span>

[!code-csharp[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="e7ca8-115">Пример JSON Patch</span><span class="sxs-lookup"><span data-stu-id="e7ca8-115">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="e7ca8-116">В приведенном выше документе JSON:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-116">In the preceding JSON:</span></span>

* <span data-ttu-id="e7ca8-117">свойство `op` указывает тип операции;</span><span class="sxs-lookup"><span data-stu-id="e7ca8-117">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="e7ca8-118">свойство `path` указывает обновляемый элемент;</span><span class="sxs-lookup"><span data-stu-id="e7ca8-118">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="e7ca8-119">свойство `value` предоставляет новое значение.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-119">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="e7ca8-120">Ресурс после обновления</span><span class="sxs-lookup"><span data-stu-id="e7ca8-120">Resource after patch</span></span>

<span data-ttu-id="e7ca8-121">Ниже показан ресурс в том состоянии, которое он принимает после применения приведенного выше документа JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-121">Here's the resource after applying the preceding JSON Patch document:</span></span>

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

<span data-ttu-id="e7ca8-122">Изменения, внесенные при применении к ресурсу документа JSON Patch, являются атомарными, то есть ни одна из операций не будет применена, если любая из них завершится сбоем.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-122">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="e7ca8-123">Синтаксис Path</span><span class="sxs-lookup"><span data-stu-id="e7ca8-123">Path syntax</span></span>

<span data-ttu-id="e7ca8-124">Свойство [Path](https://tools.ietf.org/html/rfc6901) в объекте операции содержит косые черты между уровнями.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-124">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="e7ca8-125">Например, `"/address/zipCode"`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-125">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="e7ca8-126">Для указания элементов массива используются числовые индексы, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-126">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="e7ca8-127">Первый элемент массива `addresses` будет обозначаться как `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-127">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="e7ca8-128">Чтобы применить `add` для последнего элемента массива, укажите дефис (-) вместо номера индекса: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-128">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="e7ca8-129">Операции</span><span class="sxs-lookup"><span data-stu-id="e7ca8-129">Operations</span></span>

<span data-ttu-id="e7ca8-130">В следующей таблице перечислены поддерживаемые операции, которые определены в [спецификации JSON Patch](https://tools.ietf.org/html/rfc6902).</span><span class="sxs-lookup"><span data-stu-id="e7ca8-130">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="e7ca8-131">Операция</span><span class="sxs-lookup"><span data-stu-id="e7ca8-131">Operation</span></span>  | <span data-ttu-id="e7ca8-132">Примечания</span><span class="sxs-lookup"><span data-stu-id="e7ca8-132">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="e7ca8-133">Добавляет свойство или элемент массива.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-133">Add a property or array element.</span></span> <span data-ttu-id="e7ca8-134">Для существующего свойства устанавливает значение.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-134">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="e7ca8-135">Удаляет свойство или элемент массива.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-135">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="e7ca8-136">Действует так же, как `remove` с последующим `add` в том же расположении.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-136">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="e7ca8-137">Действует так же, как `remove` из источника с последующим `add`, в котором указаны место назначения и значение из источника.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-137">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="e7ca8-138">Действует так же, как `add`, в котором указаны место назначения и значение из источника.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-138">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="e7ca8-139">Возвращает успешный код состояния, если значение `path` совпадает с предоставленным `value`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-139">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="e7ca8-140">JsonPatch в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7ca8-140">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="e7ca8-141">Реализация ASP.NET Core для JSON Patch предоставляется в пакете NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).</span><span class="sxs-lookup"><span data-stu-id="e7ca8-141">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="e7ca8-142">Этот пакет входит в состав метапакета [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e7ca8-142">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="e7ca8-143">Код метода действия</span><span class="sxs-lookup"><span data-stu-id="e7ca8-143">Action method code</span></span>

<span data-ttu-id="e7ca8-144">В контроллере API есть метод действия для JSON Patch, который:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-144">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="e7ca8-145">помечен атрибутом `HttpPatch`;</span><span class="sxs-lookup"><span data-stu-id="e7ca8-145">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="e7ca8-146">принимает `JsonPatchDocument<T>` обычно с указанием [FromBody];</span><span class="sxs-lookup"><span data-stu-id="e7ca8-146">Accepts a `JsonPatchDocument<T>`, typically with [FromBody].</span></span>
* <span data-ttu-id="e7ca8-147">вызывает `ApplyTo` для целевого документа, чтобы применить изменения.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-147">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="e7ca8-148">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-148">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="e7ca8-149">Этот код из примера приложения работает со следующей моделью `Customer`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-149">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="e7ca8-150">Этот пример метода действия:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-150">The sample action method:</span></span>

* <span data-ttu-id="e7ca8-151">Создает документ `Customer`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-151">Constructs a `Customer`.</span></span>
* <span data-ttu-id="e7ca8-152">применяет изменения;</span><span class="sxs-lookup"><span data-stu-id="e7ca8-152">Applies the patch.</span></span>
* <span data-ttu-id="e7ca8-153">возвращает результат в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-153">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="e7ca8-154">В реальном приложении код будет извлекать данные из хранилища, например из базы данных, и обновлять эту базу данных после применения исправлений.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-154">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="e7ca8-155">Состояние модели</span><span class="sxs-lookup"><span data-stu-id="e7ca8-155">Model state</span></span>

<span data-ttu-id="e7ca8-156">Указанный выше пример метода действия вызывает перегрузку `ApplyTo`, которая принимает состояние модели в качестве одного из параметров.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-156">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="e7ca8-157">В этом случае вы получаете в ответах сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-157">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="e7ca8-158">В следующем примере показан текст ответа с кодом 400 "Неверный запрос" для операции `test`:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-158">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="e7ca8-159">Динамические объекты</span><span class="sxs-lookup"><span data-stu-id="e7ca8-159">Dynamic objects</span></span>

<span data-ttu-id="e7ca8-160">Следующий пример метода действия демонстрирует, как применить исправление к динамическому объекту.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-160">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="e7ca8-161">Операция добавления</span><span class="sxs-lookup"><span data-stu-id="e7ca8-161">The add operation</span></span>

* <span data-ttu-id="e7ca8-162">Если `path` указывает на элемент массива, новый элемент вставляется перед тем, который указан в параметре `path`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-162">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="e7ca8-163">Если `path` указывает на свойство, задается значение свойства.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-163">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="e7ca8-164">Если `path` указывает на несуществующее расположение:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-164">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="e7ca8-165">Если обновляемый ресурс является динамическим объектом, добавляется свойство.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-165">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="e7ca8-166">Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-166">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="e7ca8-167">Следующий пример документа с исправлениями устанавливает значение `CustomerName` и добавляет объект `Order` в конец массива `Orders`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-167">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="e7ca8-168">Операция удаления</span><span class="sxs-lookup"><span data-stu-id="e7ca8-168">The remove operation</span></span>

* <span data-ttu-id="e7ca8-169">Если `path` указывает на элемент массива, элемент удаляется.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-169">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="e7ca8-170">Если `path` указывает на свойство:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-170">If `path` points to a property:</span></span>
  * <span data-ttu-id="e7ca8-171">Если обновляемый ресурс является динамическим объектом, свойство удаляется.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-171">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="e7ca8-172">Если обновляемый ресурс является статическим объектом:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-172">If resource to patch is a static object:</span></span> 
    * <span data-ttu-id="e7ca8-173">Если свойство может принимать значения NULL, ему присваивается значение NULL.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-173">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="e7ca8-174">Если свойство не может принимать значения NULL, ему присваивается значение `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-174">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="e7ca8-175">Следующий пример документа с исправлениями присваивает `CustomerName` значение NULL и удаляет `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-175">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="e7ca8-176">Операция замены</span><span class="sxs-lookup"><span data-stu-id="e7ca8-176">The replace operation</span></span>

<span data-ttu-id="e7ca8-177">Эта операция функционально идентична операции `remove` с последующей `add`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-177">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="e7ca8-178">Следующий пример документа с исправлениями устанавливает значение `CustomerName` и заменяет `Orders[0]` новым объектом `Order`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-178">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="e7ca8-179">Операция перемещения</span><span class="sxs-lookup"><span data-stu-id="e7ca8-179">The move operation</span></span>

* <span data-ttu-id="e7ca8-180">Если `path` указывает на элемент массива, элемент `from` копируется в расположение элемента `path`, а затем выполняется операция `remove` для элемента `from`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-180">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="e7ca8-181">Если `path` указывает на свойство, значение свойства `from` копируется в свойство `path`, а затем выполняется операция `remove` для свойства `from`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-181">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="e7ca8-182">Если `path` указывает на несуществующее свойство:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-182">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="e7ca8-183">Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-183">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="e7ca8-184">Если исправляемый ресурс является динамическим объектом, значение `from` копируется в местоположение, указанное параметром `path`, а затем выполняется операция `remove` для свойства `from`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-184">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="e7ca8-185">Следующий пример документа исправления выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-185">The following sample patch document:</span></span>

* <span data-ttu-id="e7ca8-186">копирует значение `Orders[0].OrderName` в `CustomerName`;</span><span class="sxs-lookup"><span data-stu-id="e7ca8-186">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="e7ca8-187">присваивает `Orders[0].OrderName` значение NULL;</span><span class="sxs-lookup"><span data-stu-id="e7ca8-187">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="e7ca8-188">перемещает `Orders[1]` в расположение перед `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-188">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="e7ca8-189">Операция копирования</span><span class="sxs-lookup"><span data-stu-id="e7ca8-189">The copy operation</span></span>

<span data-ttu-id="e7ca8-190">Эта операция функционально аналогична операции `move` без последнего шага `remove`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-190">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="e7ca8-191">Следующий пример документа исправления выполняет следующие действия:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-191">The following sample patch document:</span></span>

* <span data-ttu-id="e7ca8-192">копирует значение `Orders[0].OrderName` в `CustomerName`;</span><span class="sxs-lookup"><span data-stu-id="e7ca8-192">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="e7ca8-193">вставляет копию `Orders[1]` перед `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-193">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="e7ca8-194">Операция тестирования</span><span class="sxs-lookup"><span data-stu-id="e7ca8-194">The test operation</span></span>

<span data-ttu-id="e7ca8-195">Если значение в расположении, которое задано параметром `path`, отличается от предоставленного в `value` значения, запрос завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-195">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="e7ca8-196">В этом случае сбой распространяется на весь запрос PATCH, даже если все остальные операции в документе с исправлениями могли быть выполнены успешно.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-196">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="e7ca8-197">Операция `test` традиционно используется для предотвращения обновлений при возможных конфликтах параллелизма.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-197">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="e7ca8-198">Следующий пример документа с исправлениями не изменяет прежнее значение `CustomerName` (John), так как тест завершается ошибкой:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-198">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="e7ca8-199">Получение кода</span><span class="sxs-lookup"><span data-stu-id="e7ca8-199">Get the code</span></span>

<span data-ttu-id="e7ca8-200">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="e7ca8-200">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="e7ca8-201">([Инструкция по скачиванию](xref:index#how-to-download-a-sample).)</span><span class="sxs-lookup"><span data-stu-id="e7ca8-201">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="e7ca8-202">Чтобы проверить этот пример, запустите приложение и отправьте HTTP-запросы со следующими параметрами:</span><span class="sxs-lookup"><span data-stu-id="e7ca8-202">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="e7ca8-203">URL-адрес: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="e7ca8-203">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="e7ca8-204">Метод HTTP: `PATCH`</span><span class="sxs-lookup"><span data-stu-id="e7ca8-204">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="e7ca8-205">Заголовок: `Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="e7ca8-205">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="e7ca8-206">Текст: Скопируйте и вставьте один из примеров документов JSON Patch из папки *JSON* в проекте.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-206">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7ca8-207">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e7ca8-207">Additional resources</span></span>

* [<span data-ttu-id="e7ca8-208">Спецификация IETF RFC 5789 для метода PATCH</span><span class="sxs-lookup"><span data-stu-id="e7ca8-208">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="e7ca8-209">Спецификация IETF RFC 6902 для JSON Patch</span><span class="sxs-lookup"><span data-stu-id="e7ca8-209">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="e7ca8-210">Спецификация IETF RFC 6901 для формата Path в JSON Patch</span><span class="sxs-lookup"><span data-stu-id="e7ca8-210">IETF RFC 6901 JSON Patch path format spec</span></span>](https://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="e7ca8-211">[Документация по JSON Patch](https://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="e7ca8-211">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="e7ca8-212">Содержит ссылки на ресурсы для создания документов JSON Patch.</span><span class="sxs-lookup"><span data-stu-id="e7ca8-212">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="e7ca8-213">Исходный код JSON Patch для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7ca8-213">ASP.NET Core JSON Patch source code</span></span>](https://github.com/aspnet/AspNetCore/tree/master/src/Features/JsonPatch/src)
