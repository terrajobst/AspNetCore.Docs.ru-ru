---
title: Перенос из веб-API ASP.NET в ASP.NET Core
author: ardalis
description: Узнайте, как перенести реализацию веб-API из веб-API ASP.NET в ASP.NET Core MVC.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894196"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Перенос из веб-API ASP.NET в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Скотт Эдди](https://scottaddie.com) (Scott Addie)

Веб-API — это службы HTTP, которые широкого диапазона клиентов, включая браузеры и мобильные устройства. ASP.NET Core MVC включает в себя поддержку для создания веб-API, предоставляя единый согласованный способ создания веб-приложений. В этой статье мы покажем шаги, необходимые для переноса в реализации веб-API из веб-API ASP.NET в ASP.NET Core MVC.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Просмотр проекта веб-API ASP.NET

В этой статье используется пример проекта, *ProductsApp*, созданный в этой статье [Приступая к работе с веб-API ASP.NET 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) в качестве его отправной точки. В этом проекте простого проекта веб-API ASP.NET настраивается следующим образом.

В *Global.asax.cs*, выполняется вызов для `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` определяется в *App_Start*, и имеет только один статический `Register` метод:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

Этот класс настраивает [маршрутизации с помощью атрибутов](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), несмотря на то, что он фактически не используется в проекте. Он также настраивает таблицы маршрутизации, который используется в веб-API ASP.NET. В этом случае веб-API ASP.NET будет ожидать, что URL-адреса в соответствии с форматом */api/ {controller} / {id}*, с помощью *{id}* дополнительными.

*ProductsApp* проект включает в себя только один простой контроллер, который наследуется от `ApiController` и предоставляет два метода:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Наконец, модель, *продукта*, используемыми *ProductsApp*, — это простой класс:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Теперь, когда у нас есть простой проект, из которого следует начать, мы покажем как перенести этот проект веб-API в ASP.NET Core MVC.

## <a name="create-the-destination-project"></a>Создайте в проекте назначения

С помощью Visual Studio, создайте новую, пустую решение и назовите его *WebAPIMigration*. Добавить существующий *ProductsApp* проекта к нему, а затем, добавьте в решение новый проект ASP.NET Core Web Application. Назовите новый проект *ProductsCore*.

![Откройте диалоговое окно нового проекта для веб-шаблонов](webapi/_static/add-web-project.png)

Затем выберите шаблон проекта веб-API. Мы перенесем *ProductsApp* содержимое для этого проекта.

![Диалоговое окно создания веб-приложения с помощью шаблона проекта веб-API, выбранного в списке шаблонов ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

Удалить `Project_Readme.html` файла из нового проекта. Решение теперь должен выглядеть следующим образом:

![Откройте решение приложения в обозревателе решений, отображение файлов и папок проектов ProductsApp и ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Миграция конфигурации

ASP.NET Core больше не использует *Global.asax*, *web.config*, или *App_Start* папки. Вместо этого все задачи запуска выполняются в *Startup.cs* в корневом каталоге проекта (см. в разделе [запуск приложения](../fundamentals/startup.md)). В ASP.NET Core MVC, маршрутизация на основе атрибута теперь включены по умолчанию при `UseMvc()` вызывается; и это рекомендуемый подход для настройки маршрутов веб-API (и как начальный проект веб-API обрабатывает маршрутизацию).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Предположим, что вы хотите использовать в проекте, в дальнейшем маршрутизации с помощью атрибутов, без дополнительных настроек не требуется. Просто примените атрибуты, необходимыми для контроллеров и действий, как показано в образце `ValuesController` класс, который включен в начальный проект веб-API:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Обратите внимание на наличие *[controller]* в строке 8. Атрибут маршрутизации на основе теперь поддерживает определенные токены, такие как *[controller]* и *[действие]*. Эти токены заменяются во время выполнения с именем контроллера или действия, соответственно, к которому был применен атрибут. Это помогает сократить число соответствующих строк в проекте, и это гарантирует, что маршруты будут синхронизироваться с их соответствующих контроллеров и действий при применении рефакторинг Автоматическое переименование.

Чтобы перенести контроллер API продуктов, нам необходимо сначала скопировать *ProductsController* в новый проект. Просто включите атрибут маршрута контроллера:

```csharp
[Route("api/[controller]")]
```

Необходимо также добавить `[HttpGet]` атрибут два метода, поскольку они оба должны быть вызваны посредством HTTP Get. Включить в атрибут для параметра «id» исходя из предположения `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

На этом этапе маршрутизации настроен правильно; Тем не менее пока не получится протестировать его. Необходимо внести дополнительные изменения перед *ProductsController* будет компилироваться.

## <a name="migrate-models-and-controllers"></a>Перенос моделей и контроллеров

Последний шаг в процесс миграции для этого простого проекта веб-API — чтобы скопировать контроллера и всех моделей, они используют. В этом случае просто скопировать *Controllers/ProductsController.cs* исходного проекта на новый. Скопируйте целиком папку Models исходного проекта на новый. Настройка пространства имен в соответствии с новым именем проекта (*ProductsCore*).  На этом этапе можно построить приложение, и вы увидите ряд ошибок компиляции. Следует обычно делятся на следующие категории:

* *ApiController* не существует

* *System.Web.Http* пространства имен не существует

* *IHttpActionResult* не существует

К счастью это все очень просто сделать:

* Изменение *ApiController* для *контроллера* (может потребоваться добавить *с помощью Microsoft.AspNetCore.Mvc*)

* Удалить все с помощью инструкции, ссылающийся на *System.Web.Http*

* Измените любой метод возвращение *IHttpActionResult* для возврата *IActionResult*

Когда эти изменения были сделаны и неиспользуемые операторы using удалить, перенесенный *ProductsController* класс выглядит следующим образом:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Теперь можно запустить переноса проекта и перейдите к */api/products*; и вы увидите полный список трех продуктов. Перейдите к */api/products/1* и вы увидите первый продукт.

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>Оболочка совместимости ASP.NET 4.x веб-API 2

— Это полезное средство, при миграции веб-API ASP.NET проектов ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) библиотеки. Оболочка совместимости расширяет ASP.NET Core, чтобы разрешить несколько разных веб-API 2 соглашения для использования. Пример, перенесена ранее в этом документе достаточно основные оболочки совместимости не был необходим. Для крупных проектов с помощью оболочек совместимости можно использовать для временно Устранение противоречий API между ASP.NET Core и ASP.NET Web API 2.

Оболочку совместимости веб-API предназначен для использования в качестве временной меры для упрощения переноса больших проектов веб-API в ASP.NET Core. Со временем следует обновить проекты для использования шаблонов ASP.NET Core, не полагаясь на оболочку совместимости.

Совместимость возможностей, включенных в `Microsoft.AspNetCore.Mvc.WebApiCompatShim` включают:

* Добавляет `ApiController` так, чтобы контроллеров базовые типы не должны быть обновлены.
* Включает API веб-привязки модели. ASP.NET Core MVC модель привязки работает так же, MVC 5, по умолчанию. Привязка более похожим на соглашения для привязки модели веб-API 2 модели изменения оболочек совместимости. Например сложные типы автоматически привязываются из текста запроса.
* Расширяет привязки модели, что действия контроллера могут принимать параметры типа `HttpRequestMessage`.
* Добавляет модули форматирования сообщений разрешение действий, чтобы возвращать результаты типа `HttpResponseMessage`.
* Добавляет методы дополнительные ответа, которые воспользовались действия веб-API 2, для обслуживания ответов:
  * Генераторы HttpResponseMessage:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Методы результатов действий:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Добавляет экземпляр `IContentNegotiator` в контейнер внедрения Зависимостей приложения и делает содержимого типы, связанные с согласования из [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) доступны. Сюда входят типы, такие как `DefaultContentNegotiator`, `MediaTypeFormatter`и т. д.

Чтобы использовать оболочку совместимости, вам потребуется:

* Установка [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) пакет NuGet.
* Зарегистрируйте оболочку совместимости служб контейнера внедрения Зависимостей приложения с помощью метода `services.AddMvc().AddWebApiConventions()` в приложении `Startup.ConfigureServices` метод.
* Определение маршрутов API веб-задачи, с помощью `MapWebApiRoute` на `IRouteBuilder` в приложении `IApplicationBuilder.UseMvc` вызова.

## <a name="summary"></a>Сводка

Миграция простого проекта веб-API ASP.NET на ASP.NET Core MVC — достаточно проста, еще раз благодарим за встроенную поддержку веб-API в ASP.NET Core MVC. Основные части, каждый проект веб-API ASP.NET, потребуется выполнить миграцию, маршрутов, контроллеров и моделей, а также обновления типов, используемые контроллерами и действиями.
