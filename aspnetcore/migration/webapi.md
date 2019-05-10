---
title: Перенос из веб-API ASP.NET в ASP.NET Core
author: ardalis
description: Узнайте, как перенести реализацию веб-API из веб-API ASP.NET 4.x в ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/10/2018
uid: migration/webapi
ms.openlocfilehash: ea14b7128582a21b36c70041d59fb638eebdbef0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895011"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Перенос из веб-API ASP.NET в ASP.NET Core

По [Scott Addie](https://twitter.com/scott_addie) и [Стив Смит](https://ardalis.com/)

Веб-API ASP.NET 4.x является HTTP-службу, подходящее для широкого круга клиентов, включая браузеры и мобильные устройства. ASP.NET Core объединяет 4.x ASP.NET MVC и веб-приложение API моделирует в более простую модель программирования, известный как ASP.NET Core MVC. В этой статье показаны действия, необходимые для миграции из веб-API ASP.NET 4.x ASP.NET Core MVC.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [net-core-prereqs-vs-2.2](../includes/net-core-prereqs-vs-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>Просмотрите проект веб-API ASP.NET 4.x

В качестве отправной точки, в этой статье использует *ProductsApp* проект, созданный в [Приступая к работе с веб-API ASP.NET 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api). В этом проекте простого проекта веб-API ASP.NET 4.x настраивается следующим образом.

В *Global.asax.cs*, выполняется вызов для `WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` Класс находится в *App_Start* папки и имеет статическое `Register` метод:

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

Этот класс настраивает [маршрутизации с помощью атрибутов](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), несмотря на то, что он фактически не используется в проекте. Он также настраивает таблицы маршрутизации, который используется в веб-API ASP.NET. В этом случае веб-API ASP.NET 4.x ожидает, что URL-адреса в соответствии с форматом `/api/{controller}/{id}`, с помощью `{id}` дополнительными.

*ProductsApp* проект включает в себя один контроллер. Контроллер наследует от `ApiController` и содержит два действия:

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

`Product` Модели, используемой `ProductsController` — это простой класс:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

В следующих разделах демонстрируются миграции проекта веб-API ASP.NET Core MVC.

## <a name="create-destination-project"></a>Создайте проект назначения

Выполните следующие действия в Visual Studio.

* Перейдите к **файл** > **новый** > **проекта** > **других типов проектов**  >  **Решения visual Studio**. Выберите **пустое решение**и назовите решение *WebAPIMigration*. Нажмите кнопку **ОК** кнопки.
* Добавить существующий *ProductsApp* проекта к решению.
* Добавьте новый **веб-приложение ASP.NET Core** проекта к решению. Выберите **.NET Core** платформу из раскрывающегося списка и выберите **API** шаблона проекта. Назовите проект *ProductsCore*и нажмите кнопку **ОК** кнопки.

Теперь решение содержит два проекта. В следующих разделах объясняется миграция *ProductsApp* содержимое проекта для *ProductsCore* проекта.

## <a name="migrate-configuration"></a>Миграция конфигурации

ASP.NET Core не использует *App_Start* папку или *Global.asax* файл и *web.config* добавляется файл во время публикации. *Startup.cs* является заменой для *Global.asax* и расположен в корневом каталоге проекта. `Startup` Класс обрабатывает все задачи запуска приложения. Дополнительные сведения см. в разделе <xref:fundamentals/startup>.

В ASP.NET Core MVC маршрутизация с помощью атрибутов по умолчанию включается при <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> вызывается в `Startup.Configure`. Следующие `UseMvc` вызовите заменяет *ProductsApp* проекта *App_Start/WebApiConfig.cs* файла:

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>Перенос моделей и контроллеров

Скопировать *ProductApp* контроллер проекта и модели, он использует. Выполните следующие действия.

1. Копировать *Controllers/ProductsController.cs* исходного проекта на новый.
1. Скопировать весь *моделей* папку из одного проекта в новую.
1. Изменения пространств имен скопированные файлы в соответствии с новым именем проекта (*ProductsCore*). Настройка `using ProductsApp.Models;` инструкции в *ProductsController.cs* слишком.

На этом этапе создания приложения приводит ряд ошибок компиляции. Ошибки возникают потому, что следующие компоненты отсутствуют в ASP.NET Core:

* Класс `ApiController`
* Пространство имен `System.Web.Http`
* `IHttpActionResult` Интерфейс

Исправьте ошибки следующим образом:

1. Изменение `ApiController` для <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Добавить `using Microsoft.AspNetCore.Mvc;` Разрешить `ControllerBase` ссылки.
1. Удалите `using System.Web.Http;`.
1. Изменение `GetProduct` тип возвращаемого значения действия из `IHttpActionResult` для `ActionResult<Product>`.

Упростите `GetProduct` действия `return` следующим образом:

```csharp
return product;
```

## <a name="configure-routing"></a>Настройка маршрутизации

Настройка маршрутизации следующим образом:

1. Украшение `ProductsController` класс со следующими атрибутами:

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    Предыдущий [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) атрибут настраивает шаблон маршрутизации атрибутов контроллера. [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) атрибут делает Маршрутизация атрибутов является обязательным для всех действий в контроллер.

    Маршрутизация с помощью атрибутов поддерживают только токены, такие как `[controller]` и `[action]`. Во время выполнения каждый токен заменяется именем контроллера или действия, соответственно, к которому был применен атрибут. Токены сократить число соответствующих строк в проекте. Маркеры также убедиться в том случае, маршруты будут синхронизированы соответствующие контроллеры и действия при автоматически переименовать рефакторинг применяются.
1. Задайте режим совместимости проекта ASP.NET Core 2.2:

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    Предыдущего изменения:

    * Необходим для использования `[ApiController]` атрибут на уровне контроллера.
    * Позволяет способно нарушить особенности поведения, обусловленные в ASP.NET Core 2.2.
1. Включить запросы HTTP Get к `ProductController` действия:
    * Применить [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) атрибут `GetAllProducts` действие.
    * Применить `[HttpGet("{id}")]` атрибут `GetProduct` действие.

После указанные выше изменения и удаления неиспользуемых `using` инструкций, *ProductsController.cs* файл выглядит следующим образом:

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

Запустите переноса проекта и перейдите к `/api/products`. Отображается полный список три продукта. Перейдите по адресу `/api/products/1`. Отображается первым продуктом.

## <a name="compatibility-shim"></a>Оболочка совместимости

[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) библиотека предоставляет оболочку совместимости, чтобы переместить проекты веб-API ASP.NET 4.x ASP.NET Core. Оболочка совместимости расширяет ASP.NET Core поддерживать несколько соглашений из веб-API ASP.NET 4.x 2. Пример, перенесена ранее в этом документе достаточно основные что оболочку совместимости был не нужен. Для крупных проектов с помощью оболочек совместимости можно использовать для временно Устранение противоречий API между ASP.NET Core и ASP.NET 4.x веб-API 2.

Оболочку совместимости веб-API предназначен для использования в качестве временной меры для поддержки миграции крупных проектах ASP.NET 4.x веб-API в ASP.NET Core. Со временем следует обновить проекты для использования шаблонов ASP.NET Core, не полагаясь на оболочку совместимости.

Совместимость возможностей, включенных в `Microsoft.AspNetCore.Mvc.WebApiCompatShim` включают:

* Добавляет `ApiController` так, чтобы контроллеров базовые типы не должны быть обновлены.
* Включает API веб-привязки модели. ASP.NET Core MVC модели привязки работает так же, что ASP.NET 4.x MVC 5, по умолчанию. Привязка более похожим на соглашения для привязки модели ASP.NET 4.x веб-API 2 модели изменения оболочек совместимости. Например сложные типы автоматически привязываются из текста запроса.
* Расширяет привязки модели, что действия контроллера могут принимать параметры типа `HttpRequestMessage`.
* Добавляет модули форматирования сообщений разрешение действий, чтобы возвращать результаты типа `HttpResponseMessage`.
* Добавляет методы дополнительные ответа, которые воспользовались действия веб-API 2, для обслуживания ответов:
  * `HttpResponseMessage` генераторы:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Методы результатов действий:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Добавляет экземпляр `IContentNegotiator` в приложение в контейнер внедрения зависимостей и делает доступными согласования связанные типы содержимого из [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/). Примеры таких типов `DefaultContentNegotiator` и `MediaTypeFormatter`.

Чтобы использовать оболочку совместимости:

1. Установка [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) пакет NuGet.
1. Зарегистрируйте оболочку совместимости служб контейнера внедрения Зависимостей приложения с помощью метода `services.AddMvc().AddWebApiConventions()` в `Startup.ConfigureServices`.
1. Определение направляет конкретных API-интерфейсов с помощью web `MapWebApiRoute` на `IRouteBuilder` в приложении `IApplicationBuilder.UseMvc` вызова.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
