---
title: Страницы справки по веб-API ASP.NET Core с использованием Swagger (OpenAPI)
author: rsuter
description: В этом учебнике приводится пошаговое руководство по добавлению Swagger для составления документации и страниц справки к приложению веб-API.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: d7a6ed158dcb464bb80c83773ed7d455b25ce44b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887729"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a>Страницы справки по веб-API ASP.NET Core с использованием Swagger (OpenAPI)

Авторы: [Кристоф Ниенабер (Christoph Nienaber)](https://twitter.com/zuckerthoben) и [Рико Сутер (Rico Suter)](http://rsuter.com)

При использовании веб-API разработчику бывает сложно разобраться в различных методах. [Swagger](https://swagger.io/) (также называется [OpenAPI](https://www.openapis.org/)) позволяет решить проблему создания полезной документации и страниц справки для веб-API. Он имеет такие преимущества, как интерактивная документация, создание пакета SDK для клиента и возможность обнаружения API.

В этой статье демонстрируется реализация [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) и [NSwag](https://github.com/RSuter/NSwag) в .NET Swagger:

* **Swashbuckle.AspNetCore** — это проект с открытым исходным кодом, предназначенный для создания документов Swagger по веб-API ASP.NET Core.

* **NSwag** — это еще один проект с открытым кодом для создания документации Swagger и интеграции [пользовательского интерфейса Swagger](https://swagger.io/swagger-ui/) или [ReDoc](https://github.com/Rebilly/ReDoc) в веб-API ASP.NET Core. NSwag также предлагает подходы для создания C# и клиентского кода TypeScript для API.

## <a name="what-is-swagger--openapi"></a>Что такое Swagger (OpenAPI)?

Swagger — это не зависящая от языка спецификация для описания [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API. Проект Swagger был передан [OpenAPI Initiative](https://www.openapis.org/), где он теперь называется OpenAPI. Оба названия равнозначны, но рекомендуется использовать OpenAPI. Он позволяет компьютерам и пользователям лучше понять возможности службы без прямого доступа к реализации (исходный код, доступ к сети, документация). Одна из его задач — свести к минимуму объем работ, необходимых для соединения отдельных служб. Кроме того, он позволяет сократить время, необходимое для точного документирования службы.

## <a name="swagger-specification-swaggerjson"></a>Спецификация Swagger (swagger.json)

В основе потока Swagger лежит спецификация Swagger &mdash; по умолчанию это документ с именем *swagger.json*. Она создается цепочкой инструментов Swagger (или их сторонней реализацией) на основе вашей службы. Он описывает возможности API и способы доступа к нему через HTTP. Он управляет пользовательским интерфейсом Swagger и используется цепочкой инструментов, чтобы включить обнаружение и создание клиентского кода. Ниже приведен сокращенный пример спецификации Swagger:

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

## <a name="swagger-ui"></a>Пользовательский интерфейс Swagger

[Пользовательский интерфейс Swagger](https://swagger.io/swagger-ui/) обеспечивает пользовательский веб-интерфейс, предоставляющий сведения о службе с использованием созданной спецификации Swagger. Swashbuckle и NSwag включают встроенную версию пользовательского интерфейса Swagger, чтобы его можно было разместить в приложении ASP.NET Core, используя вызов регистрации ПО промежуточного слоя. Пользовательский веб-интерфейс выглядит следующим образом:

![Пользовательский интерфейс Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Каждый метод открытого действия в ваших контроллерах можно протестировать в пользовательском интерфейсе. Щелкните имя метода, чтобы развернуть соответствующий раздел. Добавьте все необходимые параметры и нажмите кнопку **Попробуйте!**.

![Пример тестирования в Swagger метода GET](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> На снимках экрана используется пользовательский интерфейс Swagger версии 2. Пример версии 3 см. в разделе [Пример Petstore](http://petstore.swagger.io/).

## <a name="next-steps"></a>Следующие шаги

* [Начало работы с Swashbuckle](xref:tutorials/get-started-with-swashbuckle)
* [Начало работы с NSwag](xref:tutorials/get-started-with-nswag)
