---
title: Страницы справки по веб-API ASP.NET Core с использованием Swagger/Open API
author: rsuter
description: В этом учебнике приводится пошаговое руководство по добавлению Swagger для составления документации и страниц справки к приложению веб-API.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: e44b491fd5265e12646efa42f12eb0662e287f04
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077340"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a><span data-ttu-id="27293-103">Страницы справки по веб-API ASP.NET Core с использованием Swagger/Open API</span><span class="sxs-lookup"><span data-stu-id="27293-103">ASP.NET Core Web API help pages with Swagger / Open API</span></span>

<span data-ttu-id="27293-104">Авторы: [Кристоф Ниенабер (Christoph Nienaber)](https://twitter.com/zuckerthoben) и [Рико Сутер (Rico Suter)](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="27293-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="27293-105">При использовании веб-API разработчику бывает сложно разобраться в различных методах.</span><span class="sxs-lookup"><span data-stu-id="27293-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="27293-106">[Swagger](https://swagger.io/), также известный как Open API, позволяет решить проблему создания полезной документации и страниц справки для веб-API.</span><span class="sxs-lookup"><span data-stu-id="27293-106">[Swagger](https://swagger.io/), also known as Open API, solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="27293-107">Он имеет такие преимущества, как интерактивная документация, создание пакета SDK для клиента и возможность обнаружения API.</span><span class="sxs-lookup"><span data-stu-id="27293-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="27293-108">В этой статье демонстрируется реализация [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) и [NSwag](https://github.com/RSuter/NSwag) в .NET Swagger:</span><span class="sxs-lookup"><span data-stu-id="27293-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="27293-109">**Swashbuckle.AspNetCore** — это проект с открытым исходным кодом, предназначенный для создания документов Swagger по веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="27293-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="27293-110">**NSwag** — это еще один проект с открытым исходным кодом для интеграции [пользовательского интерфейса Swagger](https://swagger.io/swagger-ui/) или [ReDoc](https://github.com/Rebilly/ReDoc) в веб-API ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="27293-110">**NSwag** is another open source project for integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core Web APIs.</span></span> <span data-ttu-id="27293-111">Он предлагает подходы для создания C# и клиентского кода TypeScript для API.</span><span class="sxs-lookup"><span data-stu-id="27293-111">It offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--open-api"></a><span data-ttu-id="27293-112">Что такое Swagger/Open API?</span><span class="sxs-lookup"><span data-stu-id="27293-112">What is Swagger / Open API?</span></span>

<span data-ttu-id="27293-113">Swagger — это не зависящая от языка спецификация для описания [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API.</span><span class="sxs-lookup"><span data-stu-id="27293-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="27293-114">Проект Swagger был передан [OpenAPI Initiative](https://www.openapis.org/), где он теперь называется Open API.</span><span class="sxs-lookup"><span data-stu-id="27293-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as Open API.</span></span> <span data-ttu-id="27293-115">Оба названия равнозначны, но рекомендуется использовать Open API.</span><span class="sxs-lookup"><span data-stu-id="27293-115">Both names are used interchangeably; however, Open API is preferred.</span></span> <span data-ttu-id="27293-116">Он позволяет компьютерам и пользователям лучше понять возможности службы без прямого доступа к реализации (исходный код, доступ к сети, документация).</span><span class="sxs-lookup"><span data-stu-id="27293-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="27293-117">Одна из его задач — свести к минимуму объем работ, необходимых для соединения отдельных служб.</span><span class="sxs-lookup"><span data-stu-id="27293-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="27293-118">Кроме того, он позволяет сократить время, необходимое для точного документирования службы.</span><span class="sxs-lookup"><span data-stu-id="27293-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="27293-119">Спецификация Swagger (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="27293-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="27293-120">В основе потока Swagger лежит спецификация Swagger &mdash; по умолчанию это документ с именем *swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="27293-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="27293-121">Она создается цепочкой инструментов Swagger (или их сторонней реализацией) на основе вашей службы.</span><span class="sxs-lookup"><span data-stu-id="27293-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="27293-122">Он описывает возможности API и способы доступа к нему через HTTP.</span><span class="sxs-lookup"><span data-stu-id="27293-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="27293-123">Он управляет пользовательским интерфейсом Swagger и используется цепочкой инструментов, чтобы включить обнаружение и создание клиентского кода.</span><span class="sxs-lookup"><span data-stu-id="27293-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="27293-124">Ниже приведен сокращенный пример спецификации Swagger:</span><span class="sxs-lookup"><span data-stu-id="27293-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

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

## <a name="swagger-ui"></a><span data-ttu-id="27293-125">Пользовательский интерфейс Swagger</span><span class="sxs-lookup"><span data-stu-id="27293-125">Swagger UI</span></span>

<span data-ttu-id="27293-126">[Пользовательский интерфейс Swagger](https://swagger.io/swagger-ui/) обеспечивает пользовательский веб-интерфейс, предоставляющий сведения о службе с использованием созданной спецификации Swagger.</span><span class="sxs-lookup"><span data-stu-id="27293-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="27293-127">Swashbuckle и NSwag включают встроенную версию пользовательского интерфейса Swagger, чтобы его можно было разместить в приложении ASP.NET Core, используя вызов регистрации ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="27293-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="27293-128">Пользовательский веб-интерфейс выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="27293-128">The web UI looks like this:</span></span>

![Пользовательский интерфейс Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="27293-130">Каждый метод открытого действия в ваших контроллерах можно протестировать в пользовательском интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="27293-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="27293-131">Щелкните имя метода, чтобы развернуть соответствующий раздел.</span><span class="sxs-lookup"><span data-stu-id="27293-131">Click a method name to expand the section.</span></span> <span data-ttu-id="27293-132">Добавьте все необходимые параметры и нажмите кнопку **Попробуйте!**.</span><span class="sxs-lookup"><span data-stu-id="27293-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Пример тестирования в Swagger метода GET](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="27293-134">На снимках экрана используется пользовательский интерфейс Swagger версии 2.</span><span class="sxs-lookup"><span data-stu-id="27293-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="27293-135">Пример версии 3 см. в разделе [Пример Petstore](http://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="27293-135">For a version 3 example, see [Petstore example](http://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="27293-136">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="27293-136">Next steps</span></span>

* [<span data-ttu-id="27293-137">Начало работы с Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="27293-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="27293-138">Начало работы с NSwag</span><span class="sxs-lookup"><span data-stu-id="27293-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
