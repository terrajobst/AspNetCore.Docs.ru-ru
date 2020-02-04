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
# <a name="jsonpatch-in-aspnet-core-web-api"></a>JsonPatch в веб-API ASP.NET Core

Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra) и [Kirk Larkin](https://github.com/serpent5) (Кирк Ларкин)

::: moniker range=">= aspnetcore-3.0"

В этой статье описывается обработка запросов JSON Patch в веб-API ASP.NET Core.

## <a name="package-installation"></a>Установка пакета

Поддержка для JsonPatch включается с помощью пакета `Microsoft.AspNetCore.Mvc.NewtonsoftJson`. Чтобы включить эту функцию, приложения должны сделать следующее:

* Установить пакет NuGet [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/).
* Обновите метод `Startup.ConfigureServices` в проекте, чтобы включить в него вызов к `AddNewtonsoftJson`:

  ```csharp
  services
      .AddControllersWithViews()
      .AddNewtonsoftJson();
  ```

`AddNewtonsoftJson` совместим с методами регистрации службы MVC:

  * `AddRazorPages`
  * `AddControllersWithViews`
  * `AddControllers`

## <a name="jsonpatch-addnewtonsoftjson-and-systemtextjson"></a>JsonPatch, AddNewtonsoftJson и System.Text.Json
  
`AddNewtonsoftJson` заменяет `System.Text.Json` с учетом форматировщиков ввода и вывода, используемых для форматирования **всего** содержимого JSON. Чтобы добавить поддержку `JsonPatch` с помощью `Newtonsoft.Json`, оставив другие форматировщики без изменений, измените `Startup.ConfigureServices` проекта следующим образом:

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

Для приведенного выше кода требуется ссылка на [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson) и следующие операторы using:

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a>Метод HTTP-запроса PATCH

Методы PUT и [PATCH](https://tools.ietf.org/html/rfc5789) используются для обновления существующего ресурса. Различие между ними заключается в том, что PUT заменяет весь ресурс, а PATCH указывает только изменения.

## <a name="json-patch"></a>JSON PATCH

Формат [JSON Patch](https://tools.ietf.org/html/rfc6902) используется для указания обновлений, применяемых к ресурсу. Документ JSON Patch содержит массив *операций*. Каждая операция обозначает определенный тип изменений, например добавление элемента массива или замену значения свойства.

Так, следующие документы JSON описывают ресурс, сведения JSON Patch для изменения этого ресурса и результат применения операций изменения.

### <a name="resource-example"></a>Пример ресурса

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>Пример JSON Patch

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

В приведенном выше документе JSON:

* свойство `op` указывает тип операции;
* свойство `path` указывает обновляемый элемент;
* свойство `value` предоставляет новое значение.

### <a name="resource-after-patch"></a>Ресурс после обновления

Ниже показан ресурс в том состоянии, которое он принимает после применения приведенного выше документа JSON Patch.

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

Изменения, внесенные при применении к ресурсу документа JSON Patch, являются атомарными, то есть ни одна из операций не будет применена, если любая из них завершится сбоем.

## <a name="path-syntax"></a>Синтаксис Path

Свойство [Path](https://tools.ietf.org/html/rfc6901) в объекте операции содержит косые черты между уровнями. Например, `"/address/zipCode"`.

Для указания элементов массива используются числовые индексы, начиная с нуля. Первый элемент массива `addresses` будет обозначаться как `/addresses/0`. Чтобы применить `add` для последнего элемента массива, укажите дефис (-) вместо номера индекса: `/addresses/-`.

### <a name="operations"></a>Операции

В следующей таблице перечислены поддерживаемые операции, которые определены в [спецификации JSON Patch](https://tools.ietf.org/html/rfc6902).

|Операция  | Примечания |
|-----------|--------------------------------|
| `add`     | Добавляет свойство или элемент массива. Для существующего свойства устанавливает значение.|
| `remove`  | Удаляет свойство или элемент массива. |
| `replace` | Действует так же, как `remove` с последующим `add` в том же расположении. |
| `move`    | Действует так же, как `remove` из источника с последующим `add`, в котором указаны место назначения и значение из источника. |
| `copy`    | Действует так же, как `add`, в котором указаны место назначения и значение из источника. |
| `test`    | Возвращает успешный код состояния, если значение `path` совпадает с предоставленным `value`.|

## <a name="jsonpatch-in-aspnet-core"></a>JsonPatch в ASP.NET Core

Реализация ASP.NET Core для JSON Patch предоставляется в пакете NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/).

## <a name="action-method-code"></a>Код метода действия

В контроллере API есть метод действия для JSON Patch, который:

* помечен атрибутом `HttpPatch`;
* принимает `JsonPatchDocument<T>` обычно с указанием `[FromBody]`;
* вызывает `ApplyTo` для целевого документа, чтобы применить изменения.

Ниже приведен пример:

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

Этот код из примера приложения работает со следующей моделью `Customer`.

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

Этот пример метода действия:

* Создает документ `Customer`.
* применяет изменения;
* возвращает результат в тексте ответа.

 В реальном приложении код будет извлекать данные из хранилища, например из базы данных, и обновлять эту базу данных после применения исправлений.

### <a name="model-state"></a>Состояние модели

Указанный выше пример метода действия вызывает перегрузку `ApplyTo`, которая принимает состояние модели в качестве одного из параметров. В этом случае вы получаете в ответах сообщения об ошибках. В следующем примере показан текст ответа с кодом 400 "Неверный запрос" для операции `test`:

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>Динамические объекты

Следующий пример метода действия демонстрирует, как применить исправление к динамическому объекту.

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>Операция добавления

* Если `path` указывает на элемент массива, новый элемент вставляется перед тем, который указан в параметре `path`.
* Если `path` указывает на свойство, задается значение свойства.
* Если `path` указывает на несуществующее расположение:
  * Если обновляемый ресурс является динамическим объектом, добавляется свойство.
  * Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.

Следующий пример документа с исправлениями устанавливает значение `CustomerName` и добавляет объект `Order` в конец массива `Orders`.

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>Операция удаления

* Если `path` указывает на элемент массива, элемент удаляется.
* Если `path` указывает на свойство:
  * Если обновляемый ресурс является динамическим объектом, свойство удаляется.
  * Если обновляемый ресурс является статическим объектом:
    * Если свойство может принимать значения NULL, ему присваивается значение NULL.
    * Если свойство не может принимать значения NULL, ему присваивается значение `default<T>`.

Следующий пример документа с исправлениями присваивает `CustomerName` значение NULL и удаляет `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>Операция замены

Эта операция функционально идентична операции `remove` с последующей `add`.

Следующий пример документа с исправлениями устанавливает значение `CustomerName` и заменяет `Orders[0]` новым объектом `Order`.

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>Операция перемещения

* Если `path` указывает на элемент массива, элемент `from` копируется в расположение элемента `path`, а затем выполняется операция `remove` для элемента `from`.
* Если `path` указывает на свойство, значение свойства `from` копируется в свойство `path`, а затем выполняется операция `remove` для свойства `from`.
* Если `path` указывает на несуществующее свойство:
  * Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.
  * Если исправляемый ресурс является динамическим объектом, значение `from` копируется в местоположение, указанное параметром `path`, а затем выполняется операция `remove` для свойства `from`.

Следующий пример документа исправления выполняет следующие действия:

* копирует значение `Orders[0].OrderName` в `CustomerName`;
* присваивает `Orders[0].OrderName` значение NULL;
* перемещает `Orders[1]` в расположение перед `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>Операция копирования

Эта операция функционально аналогична операции `move` без последнего шага `remove`.

Следующий пример документа исправления выполняет следующие действия:

* копирует значение `Orders[0].OrderName` в `CustomerName`;
* вставляет копию `Orders[1]` перед `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>Операция тестирования

Если значение в расположении, которое задано параметром `path`, отличается от предоставленного в `value` значения, запрос завершится ошибкой. В этом случае сбой распространяется на весь запрос PATCH, даже если все остальные операции в документе с исправлениями могли быть выполнены успешно.

Операция `test` традиционно используется для предотвращения обновлений при возможных конфликтах параллелизма.

Следующий пример документа с исправлениями не изменяет прежнее значение `CustomerName` (John), так как тест завершается ошибкой:

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>Получение кода

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2). ([Инструкция по скачиванию](xref:index#how-to-download-a-sample).)

Чтобы проверить этот пример, запустите приложение и отправьте HTTP-запросы со следующими параметрами:

* URL-адрес: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* Метод HTTP: `PATCH`
* Заголовок: `Content-Type: application/json-patch+json`
* Текст: Скопируйте и вставьте один из примеров документов JSON Patch из папки *JSON* в проекте.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Спецификация IETF RFC 5789 для метода PATCH](https://tools.ietf.org/html/rfc5789)
* [Спецификация IETF RFC 6902 для JSON Patch](https://tools.ietf.org/html/rfc6902)
* [Спецификация IETF RFC 6901 для формата Path в JSON Patch](https://tools.ietf.org/html/rfc6901)
* [Документация по JSON Patch](https://jsonpatch.com/). Содержит ссылки на ресурсы для создания документов JSON Patch.
* [Исходный код JSON Patch для ASP.NET Core](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

В этой статье описывается обработка запросов JSON Patch в веб-API ASP.NET Core.

## <a name="patch-http-request-method"></a>Метод HTTP-запроса PATCH

Методы PUT и [PATCH](https://tools.ietf.org/html/rfc5789) используются для обновления существующего ресурса. Различие между ними заключается в том, что PUT заменяет весь ресурс, а PATCH указывает только изменения.

## <a name="json-patch"></a>JSON PATCH

Формат [JSON Patch](https://tools.ietf.org/html/rfc6902) используется для указания обновлений, применяемых к ресурсу. Документ JSON Patch содержит массив *операций*. Каждая операция обозначает определенный тип изменений, например добавление элемента массива или замену значения свойства.

Так, следующие документы JSON описывают ресурс, сведения JSON Patch для изменения этого ресурса и результат применения операций изменения.

### <a name="resource-example"></a>Пример ресурса

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a>Пример JSON Patch

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

В приведенном выше документе JSON:

* свойство `op` указывает тип операции;
* свойство `path` указывает обновляемый элемент;
* свойство `value` предоставляет новое значение.

### <a name="resource-after-patch"></a>Ресурс после обновления

Ниже показан ресурс в том состоянии, которое он принимает после применения приведенного выше документа JSON Patch.

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

Изменения, внесенные при применении к ресурсу документа JSON Patch, являются атомарными, то есть ни одна из операций не будет применена, если любая из них завершится сбоем.

## <a name="path-syntax"></a>Синтаксис Path

Свойство [Path](https://tools.ietf.org/html/rfc6901) в объекте операции содержит косые черты между уровнями. Например, `"/address/zipCode"`.

Для указания элементов массива используются числовые индексы, начиная с нуля. Первый элемент массива `addresses` будет обозначаться как `/addresses/0`. Чтобы применить `add` для последнего элемента массива, укажите дефис (-) вместо номера индекса: `/addresses/-`.

### <a name="operations"></a>Операции

В следующей таблице перечислены поддерживаемые операции, которые определены в [спецификации JSON Patch](https://tools.ietf.org/html/rfc6902).

|Операция  | Примечания |
|-----------|--------------------------------|
| `add`     | Добавляет свойство или элемент массива. Для существующего свойства устанавливает значение.|
| `remove`  | Удаляет свойство или элемент массива. |
| `replace` | Действует так же, как `remove` с последующим `add` в том же расположении. |
| `move`    | Действует так же, как `remove` из источника с последующим `add`, в котором указаны место назначения и значение из источника. |
| `copy`    | Действует так же, как `add`, в котором указаны место назначения и значение из источника. |
| `test`    | Возвращает успешный код состояния, если значение `path` совпадает с предоставленным `value`.|

## <a name="jsonpatch-in-aspnet-core"></a>JsonPatch в ASP.NET Core

Реализация ASP.NET Core для JSON Patch предоставляется в пакете NuGet [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/). Этот пакет входит в состав метапакета [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

## <a name="action-method-code"></a>Код метода действия

В контроллере API есть метод действия для JSON Patch, который:

* помечен атрибутом `HttpPatch`;
* принимает `JsonPatchDocument<T>` обычно с указанием `[FromBody]`;
* вызывает `ApplyTo` для целевого документа, чтобы применить изменения.

Ниже приведен пример:

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

Этот код из примера приложения работает со следующей моделью `Customer`.

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

Этот пример метода действия:

* Создает документ `Customer`.
* применяет изменения;
* возвращает результат в тексте ответа.

 В реальном приложении код будет извлекать данные из хранилища, например из базы данных, и обновлять эту базу данных после применения исправлений.

### <a name="model-state"></a>Состояние модели

Указанный выше пример метода действия вызывает перегрузку `ApplyTo`, которая принимает состояние модели в качестве одного из параметров. В этом случае вы получаете в ответах сообщения об ошибках. В следующем примере показан текст ответа с кодом 400 "Неверный запрос" для операции `test`:

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a>Динамические объекты

Следующий пример метода действия демонстрирует, как применить исправление к динамическому объекту.

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a>Операция добавления

* Если `path` указывает на элемент массива, новый элемент вставляется перед тем, который указан в параметре `path`.
* Если `path` указывает на свойство, задается значение свойства.
* Если `path` указывает на несуществующее расположение:
  * Если обновляемый ресурс является динамическим объектом, добавляется свойство.
  * Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.

Следующий пример документа с исправлениями устанавливает значение `CustomerName` и добавляет объект `Order` в конец массива `Orders`.

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a>Операция удаления

* Если `path` указывает на элемент массива, элемент удаляется.
* Если `path` указывает на свойство:
  * Если обновляемый ресурс является динамическим объектом, свойство удаляется.
  * Если обновляемый ресурс является статическим объектом:
    * Если свойство может принимать значения NULL, ему присваивается значение NULL.
    * Если свойство не может принимать значения NULL, ему присваивается значение `default<T>`.

Следующий пример документа с исправлениями присваивает `CustomerName` значение NULL и удаляет `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a>Операция замены

Эта операция функционально идентична операции `remove` с последующей `add`.

Следующий пример документа с исправлениями устанавливает значение `CustomerName` и заменяет `Orders[0]` новым объектом `Order`.

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a>Операция перемещения

* Если `path` указывает на элемент массива, элемент `from` копируется в расположение элемента `path`, а затем выполняется операция `remove` для элемента `from`.
* Если `path` указывает на свойство, значение свойства `from` копируется в свойство `path`, а затем выполняется операция `remove` для свойства `from`.
* Если `path` указывает на несуществующее свойство:
  * Если обновляемый ресурс является статическим объектом, запрос завершается ошибкой.
  * Если исправляемый ресурс является динамическим объектом, значение `from` копируется в местоположение, указанное параметром `path`, а затем выполняется операция `remove` для свойства `from`.

Следующий пример документа исправления выполняет следующие действия:

* копирует значение `Orders[0].OrderName` в `CustomerName`;
* присваивает `Orders[0].OrderName` значение NULL;
* перемещает `Orders[1]` в расположение перед `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a>Операция копирования

Эта операция функционально аналогична операции `move` без последнего шага `remove`.

Следующий пример документа исправления выполняет следующие действия:

* копирует значение `Orders[0].OrderName` в `CustomerName`;
* вставляет копию `Orders[1]` перед `Orders[0]`.

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a>Операция тестирования

Если значение в расположении, которое задано параметром `path`, отличается от предоставленного в `value` значения, запрос завершится ошибкой. В этом случае сбой распространяется на весь запрос PATCH, даже если все остальные операции в документе с исправлениями могли быть выполнены успешно.

Операция `test` традиционно используется для предотвращения обновлений при возможных конфликтах параллелизма.

Следующий пример документа с исправлениями не изменяет прежнее значение `CustomerName` (John), так как тест завершается ошибкой:

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a>Получение кода

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2). ([Инструкция по скачиванию](xref:index#how-to-download-a-sample).)

Чтобы проверить этот пример, запустите приложение и отправьте HTTP-запросы со следующими параметрами:

* URL-адрес: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* Метод HTTP: `PATCH`
* Заголовок: `Content-Type: application/json-patch+json`
* Текст: Скопируйте и вставьте один из примеров документов JSON Patch из папки *JSON* в проекте.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Спецификация IETF RFC 5789 для метода PATCH](https://tools.ietf.org/html/rfc5789)
* [Спецификация IETF RFC 6902 для JSON Patch](https://tools.ietf.org/html/rfc6902)
* [Спецификация IETF RFC 6901 для формата Path в JSON Patch](https://tools.ietf.org/html/rfc6901)
* [Документация по JSON Patch](https://jsonpatch.com/). Содержит ссылки на ресурсы для создания документов JSON Patch.
* [Исходный код JSON Patch для ASP.NET Core](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
