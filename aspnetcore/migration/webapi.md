---
title: Переход с веб-API ASP.NET на ASP.NET Core
author: ardalis
description: Узнайте, как перенести реализацию веб-API с веб-API ASP.NET 4. x на ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/webapi
ms.openlocfilehash: 7f61b78c589fc9d01061b50554e5a639e372c3d8
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653008"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Переход с веб-API ASP.NET на ASP.NET Core

[Скотта Эдди (](https://twitter.com/scott_addie) и [Виктор Смит](https://ardalis.com/)

Веб-API ASP.NET 4. x — это служба HTTP, которая достигает широкого спектра клиентов, включая браузеры и мобильные устройства. ASP.NET Core объединяет модели приложений MVC и веб-API ASP.NET 4. x в более простую модель программирования, известную как ASP.NET Core MVC. В этой статье описываются шаги, необходимые для перехода с веб-API ASP.NET 4. x на ASP.NET Core MVC.

[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>предварительные требования

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>Обзор проекта веб-API ASP.NET 4. x

В качестве отправной точки в этой статье используется проект *продуктсапп* , созданный в [Начало работы с веб-API ASP.NET 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api). В этом проекте простой проект веб-API ASP.NET 4. x настраивается следующим образом.

В *Global.asax.CS*выполняется вызов `WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

Класс `WebApiConfig` находится в папке *App_Start* и имеет статический метод `Register`:

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

Этот класс настраивает [маршрутизацию атрибутов](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), хотя на самом деле она не используется в проекте. Он также настраивает таблицу маршрутизации, используемую веб-API ASP.NET. В этом случае ASP.NET 4. x Web API ждет, чтобы URL-адреса совпадали с форматом `/api/{controller}/{id}`, а `{id}` быть необязательным.

Проект *продуктсапп* включает один контроллер. Контроллер наследуется от `ApiController` и содержит два действия:

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

Модель `Product`, используемая `ProductsController`, является простым классом:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

В следующих разделах демонстрируется перенос проекта веб-API в ASP.NET Core MVC.

## <a name="create-destination-project"></a>Создать целевой проект

В Visual Studio выполните следующие действия.

* Последовательно выберите **файл** > **новый** **проект** >  > **другие типы проектов** > **решения Visual Studio**. Выберите **пустое решение**и назовите решение *вебапимигратион*. Нажмите кнопку **ОК** .
* Добавьте существующий проект *продуктсапп* в решение.
* Добавьте в решение новый проект **веб-приложения ASP.NET Core** . Выберите целевую платформу **.NET Core** в раскрывающемся списке и выберите шаблон проекта **API** . Назовите проект *продуктскоре*и нажмите кнопку **ОК** .

Решение теперь содержит два проекта. В следующих разделах описывается перенос содержимого проекта *продуктсапп* в проект *продуктскоре* .

## <a name="migrate-configuration"></a>Миграция конфигурации

ASP.NET Core не использует папку *App_Start* или файл *Global. asax* , а файл *Web. config* добавляется во время публикации. *Startup.CS* является заменой для *Global. asax* и находится в корневом каталоге проекта. Класс `Startup` обрабатывает все задачи запуска приложения. Дополнительные сведения см. в разделе <xref:fundamentals/startup>.

В ASP.NET Core MVC маршрутизация атрибутов включается по умолчанию при вызове <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> в `Startup.Configure`. Следующий вызов `UseMvc` заменяет файл */вебапиконфиг.кс App_Start* проекта *продуктсапп* :

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>Перенос моделей и контроллеров

Скопируйте контроллер проекта *продуктапп* и модель, которую он использует. Выполните следующие действия.

1. Скопируйте *Controllers/продуктсконтроллер. CS* из исходного проекта в новый.
1. Скопируйте всю папку *Models* из исходного проекта в новую.
1. Измените пространства имен скопированных файлов в соответствии с новым именем проекта (*продуктскоре*). Также измените инструкцию `using ProductsApp.Models;` в *ProductsController.CS* .

На этом этапе сборка приложения приводит к ряду ошибок компиляции. Ошибки возникают из-за отсутствия в ASP.NET Core следующих компонентов:

* Класс `ApiController`
* Пространство имен `System.Web.Http`
* интерфейс `IHttpActionResult`

Исправьте ошибки следующим образом:

1. Измените `ApiController` на <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Добавьте `using Microsoft.AspNetCore.Mvc;`, чтобы разрешить ссылку на `ControllerBase`.
1. Удалите `using System.Web.Http;`.
1. Измените тип возвращаемого значения действия `GetProduct` с `IHttpActionResult` на `ActionResult<Product>`.

Упростите инструкцию `return` действия `GetProduct` следующим образом:

```csharp
return product;
```

## <a name="configure-routing"></a>Настройка маршрутизации

Настройте маршрутизацию следующим образом.

1. Пометьте `ProductsController` класс следующими атрибутами:

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    Предыдущий атрибут [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) настраивает шаблон маршрутизации атрибутов контроллера. Атрибут [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) делает маршрутизацию атрибутов требованием для всех действий в этом контроллере.

    Маршрутизация атрибутов поддерживает маркеры, такие как `[controller]` и `[action]`. Во время выполнения каждый токен заменяется именем контроллера или действия соответственно, к которому был применен атрибут. Токены уменьшают количество волшебных строк в проекте. Кроме того, токены обеспечивают синхронизацию маршрутов с соответствующими контроллерами и действиями при применении рефакторинга автоматического переименования.
1. Задайте для режима совместимости проекта значение ASP.NET Core 2,2:

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    Предыдущее изменение:

    * Требуется для использования атрибута `[ApiController]` на уровне контроллера.
    * Может привести к возможным нарушениям поведений, появившимся в ASP.NET Core 2,2.
1. Включить HTTP-запросы GET к `ProductController` действиям:
    * Примените атрибут [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) к действию `GetAllProducts`.
    * Примените атрибут `[HttpGet("{id}")]` к действию `GetProduct`.

После предыдущих изменений и удаления неиспользуемых `using`ных инструкций файл *ProductsController.CS* выглядит следующим образом:

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

Запустите перенесенный проект и перейдите к `/api/products`. Появится полный список из трех продуктов. Перейдите по адресу `/api/products/1`. Появится первый продукт.

## <a name="compatibility-shim"></a>Оболочка совместимости

Библиотека [Microsoft. AspNetCore. MVC. вебапикомпатшим](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) предоставляет оболочку совместимости для перемещения проектов веб-API ASP.NET 4. x в ASP.NET Core. Оболочка совместимости расширяет ASP.NET Core для поддержки ряда соглашений от ASP.NET 4. x Web API 2. Пример, переданный ранее в этом документе, достаточно прост в том, что оболочка совместимости не была нужна. Для более крупных проектов использование оболочки совместимости может оказаться полезным для временного разделения зазора API между ASP.NET Core и ASP.NET 4. x Web API 2.

Оболочка совместимости веб-API предназначена для использования в качестве временной меры для поддержки переноса больших проектов веб-API ASP.NET 4. x в ASP.NET Core. Со временем проекты должны быть обновлены для использования шаблонов ASP.NET Core, а не полагаться на оболочку совместимости.

Функции обеспечения совместимости, включенные в `Microsoft.AspNetCore.Mvc.WebApiCompatShim`, включают:

* Добавляет тип `ApiController`, поэтому не требуется обновлять базовые типы контроллеров.
* Включает привязку модели в стиле веб-API. ASP.NET Core функция привязки модели MVC аналогична ASP.NET 4. x MVC 5 по умолчанию. Оболочка совместимости изменяет привязку модели так, чтобы она была похожа на соглашения о привязке модели Web API 2 ASP.NET 4. x. Например, сложные типы автоматически привязываются из текста запроса.
* Расширяет привязку модели, чтобы действия контроллера могли принимать параметры типа `HttpRequestMessage`.
* Добавляет модули форматирования сообщений, позволяя действиям возвращать результаты типа `HttpResponseMessage`.
* Добавляет дополнительные методы ответа, которые могут использоваться действиями веб-API 2 для обслуживания ответов:
  * `HttpResponseMessage` генераторы:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Методы результата действия:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Добавляет экземпляр `IContentNegotiator` в контейнер внедрения зависимостей (DI) приложения и делает доступными типы, связанные с согласованием содержимого, из [Microsoft. AspNet. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/). Примерами таких типов являются `DefaultContentNegotiator` и `MediaTypeFormatter`.

Чтобы использовать оболочку совместимости, выполните следующие действия.

1. Установите пакет NuGet [Microsoft. AspNetCore. MVC. вебапикомпатшим](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) .
1. Зарегистрируйте службы оболочки совместимости с контейнером внедрения приложения, вызвав `services.AddMvc().AddWebApiConventions()` в `Startup.ConfigureServices`.
1. Определите зависящие от веб-API маршруты с помощью `MapWebApiRoute` на `IRouteBuilder` в вызове `IApplicationBuilder.UseMvc` приложения.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
