---
title: Типы возвращаемых значений действий контроллера в веб-API ASP.NET Core
author: scottaddie
description: Сведения об использовании различных типов возвращаемых значений методов действий контроллера в веб-API ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: web-api/action-return-types
ms.openlocfilehash: 84300eae4271c3ee4387be022c3576dc83e144eb
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207528"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>Типы возвращаемых значений действий контроллера в веб-API ASP.NET Core

Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([как скачивать](xref:index#how-to-download-a-sample))

ASP.NET Core предоставляет следующие параметры для типов возвращаемых значений действий контроллера веб-API:

::: moniker range=">= aspnetcore-2.1"

* [Определенный тип](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [Определенный тип](#specific-type)
* [IActionResult](#iactionresult-type)

::: moniker-end

В этом документе объясняется, когда лучше использовать каждый тип возвращаемого значения.

## <a name="specific-type"></a>Определенный тип

Простейшее действие возвращает элементарный или сложный тип данных (например, `string` или пользовательский тип объекта). Рассмотрим следующее действие, которое возвращает коллекцию пользовательских объектов `Product`:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Если не известны условия, которые необходимо соблюдать при выполнении действия, конкретного типа будет достаточно. Предыдущее действие не принимает параметры, поэтому проверка ограничений параметров не требуется.

Если в действии необходимо учитывать известные условия, используется несколько путей возврата. В этом случае рекомендуется комбинировать тип возвращаемого значения [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) с элементарным или сложным типом возвращаемого значения. Для этого типа действия требуется [IActionResult](#iactionresult-type) или [ActionResult\<T >](#actionresultt-type).

## <a name="iactionresult-type"></a>Тип IActionResult

Тип возвращаемого значения [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) можно использовать, если в действии возможно несколько типов возвращаемого значения [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult). Типы `ActionResult` представляют различные коды состояния HTTP. Некоторые наиболее распространенные типы возвращаемых значений из этой категории: [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) и [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Поскольку в действии есть несколько типов возвращаемого значения и путей, необходимо использовать атрибут [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor). Этот атрибут создает более описательные сведения об ответе для страниц справки API, создаваемых такими инструментами, как [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` указывает известные типы и коды состояния HTTP, возвращаемые действием.

### <a name="synchronous-action"></a>Синхронное действие

Рассмотрим следующее синхронное действие, в котором возможны два типа возвращаемых значений:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

В предыдущем действии возвращается код состояния 404, если продукт, представленный `id`, не существует в базовом хранилище данных. Вспомогательный метод [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) вызывается в качестве ярлыка для `return new NotFoundResult();`. Если продукт существует, объект `Product`, представляющий полезные данные, возвращается с кодом состояния 200. Вспомогательный метод [ОК](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) вызывается в качестве краткой формы `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Асинхронное действие

Рассмотрим следующее асинхронное действие, в котором возможны два типа возвращаемых значений:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

В предыдущем действии возвращается код состояния 400, если происходит сбой проверки модели и вызывается вспомогательный метод [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest). Например, следующая модель указывает, что запросы должны предоставлять свойство `Name` и значение. Таким образом, если невозможно предоставить надлежащее значение `Name` в запросе, происходит сбой проверки модели.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

Также для предыдущего действия возвращается второй известный код — 201, который создается вспомогательным методом [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). В этом пути возвращается объект `Product`.

::: moniker range=">= aspnetcore-2.1"

## <a name="actionresultt-type"></a>Тип ActionResult\<T>

ASP.NET Core 2.1 предоставляет тип возвращаемого значения [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) для действий контроллера веб-API. Он позволяет возвращать тип, производный от [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult), или [определенный тип](#specific-type). `ActionResult<T>` имеет следующие преимущества по сравнению с [типом IActionResult](#iactionresult-type):

* Свойство `Type` атрибута [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) можно исключить. Например, `[ProducesResponseType(200, Type = typeof(Product))]` упрощается до `[ProducesResponseType(200)]`. Ожидаемый тип возвращаемого значения действия вместо этого выводится из `T` в `ActionResult<T>`.
* [Операторы неявного приведения](/dotnet/csharp/language-reference/keywords/implicit) поддерживают преобразование `T` и `ActionResult` в `ActionResult<T>`. `T` преобразуется в [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), то есть `return new ObjectResult(T);` упрощается до `return T;`.

C# не поддерживает операторы неявных приведений в интерфейсах. Следовательно, для преобразования в конкретный тип необходимо использовать `ActionResult<T>`. Например, использование `IEnumerable` не работает в следующем примере:

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

Один из способов исправить приведенный выше код — возвратить `_repository.GetProducts().ToList();`.

Большинство действий имеют тип возвращаемого значения. При выполнении действия могут возникнуть непредвиденные условия, и в этом случае определенный тип не возвращается. Например, входной параметр действия может не пройти проверку модели. В этом случае обычно возвращается подходящий тип `ActionResult` вместо конкретного типа.

### <a name="synchronous-action"></a>Синхронное действие

Рассмотрим синхронное действие, в котором возможны два типа возвращаемых значений:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

В приведенном выше коде возвращается код состояния 404, если произведение не существует в базе данных. Если произведение существует, возвращается соответствующий объект `Product`. До версии ASP.NET Core 2.1 строка `return product;` имела бы вид `return Ok(product);`.

> [!TIP]
> В версии ASP.NET Core 2.1 включен вывод источника привязки параметра действия, когда класс контроллера дополнен атрибутом `[ApiController]`. Имя параметра, соответствующее имени в шаблоне маршрута, автоматически привязывается с помощью данных запроса маршрута. В результате параметр предыдущего действия `id` явным образом не помечен атрибутом [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute).

### <a name="asynchronous-action"></a>Асинхронное действие

Рассмотрим асинхронное действие, в котором возможны два типа возвращаемых значений:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Если проверка модели не пройдена, вызывается метод [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_), чтобы вернуть код состояния 400. Ему передается свойство [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), содержащее определенные ошибки проверки. Если проверка модели прошла успешно, продукт создается в базе данных. Возвращается код состояния 201.

> [!TIP]
> В версии ASP.NET Core 2.1 включен вывод источника привязки параметра действия, когда класс контроллера дополнен атрибутом `[ApiController]`. Параметры сложного типа связываются автоматически с помощью текста запроса. В результате параметр предыдущего действия `product` явным образом не помечен атрибутом [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute).

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
