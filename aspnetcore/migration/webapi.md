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
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="96c76-103">Перенос из веб-API ASP.NET в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="96c76-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="96c76-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Скотт Эдди](https://scottaddie.com) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="96c76-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="96c76-105">Веб-API — это службы HTTP, которые широкого диапазона клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="96c76-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="96c76-106">ASP.NET Core MVC включает в себя поддержку для создания веб-API, предоставляя единый согласованный способ создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="96c76-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="96c76-107">В этой статье мы покажем шаги, необходимые для переноса в реализации веб-API из веб-API ASP.NET в ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="96c76-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="96c76-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96c76-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="96c76-109">Просмотр проекта веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="96c76-109">Review ASP.NET Web API project</span></span>

<span data-ttu-id="96c76-110">В этой статье используется пример проекта, *ProductsApp*, созданный в этой статье [Приступая к работе с веб-API ASP.NET 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) в качестве его отправной точки.</span><span class="sxs-lookup"><span data-stu-id="96c76-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="96c76-111">В этом проекте простого проекта веб-API ASP.NET настраивается следующим образом.</span><span class="sxs-lookup"><span data-stu-id="96c76-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="96c76-112">В *Global.asax.cs*, выполняется вызов для `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="96c76-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="96c76-113">`WebApiConfig` определяется в *App_Start*, и имеет только один статический `Register` метод:</span><span class="sxs-lookup"><span data-stu-id="96c76-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

<span data-ttu-id="96c76-114">Этот класс настраивает [маршрутизации с помощью атрибутов](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), несмотря на то, что он фактически не используется в проекте.</span><span class="sxs-lookup"><span data-stu-id="96c76-114">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="96c76-115">Он также настраивает таблицы маршрутизации, который используется в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="96c76-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="96c76-116">В этом случае веб-API ASP.NET будет ожидать, что URL-адреса в соответствии с форматом */api/ {controller} / {id}*, с помощью *{id}* дополнительными.</span><span class="sxs-lookup"><span data-stu-id="96c76-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="96c76-117">*ProductsApp* проект включает в себя только один простой контроллер, который наследуется от `ApiController` и предоставляет два метода:</span><span class="sxs-lookup"><span data-stu-id="96c76-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="96c76-118">Наконец, модель, *продукта*, используемыми *ProductsApp*, — это простой класс:</span><span class="sxs-lookup"><span data-stu-id="96c76-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="96c76-119">Теперь, когда у нас есть простой проект, из которого следует начать, мы покажем как перенести этот проект веб-API в ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="96c76-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="96c76-120">Создайте в проекте назначения</span><span class="sxs-lookup"><span data-stu-id="96c76-120">Create the Destination Project</span></span>

<span data-ttu-id="96c76-121">С помощью Visual Studio, создайте новую, пустую решение и назовите его *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="96c76-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="96c76-122">Добавить существующий *ProductsApp* проекта к нему, а затем, добавьте в решение новый проект ASP.NET Core Web Application.</span><span class="sxs-lookup"><span data-stu-id="96c76-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="96c76-123">Назовите новый проект *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="96c76-123">Name the new project *ProductsCore*.</span></span>

![Откройте диалоговое окно нового проекта для веб-шаблонов](webapi/_static/add-web-project.png)

<span data-ttu-id="96c76-125">Затем выберите шаблон проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="96c76-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="96c76-126">Мы перенесем *ProductsApp* содержимое для этого проекта.</span><span class="sxs-lookup"><span data-stu-id="96c76-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Диалоговое окно создания веб-приложения с помощью шаблона проекта веб-API, выбранного в списке шаблонов ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="96c76-128">Удалить `Project_Readme.html` файла из нового проекта.</span><span class="sxs-lookup"><span data-stu-id="96c76-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="96c76-129">Решение теперь должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="96c76-129">Your solution should now look like this:</span></span>

![Откройте решение приложения в обозревателе решений, отображение файлов и папок проектов ProductsApp и ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="96c76-131">Миграция конфигурации</span><span class="sxs-lookup"><span data-stu-id="96c76-131">Migrate Configuration</span></span>

<span data-ttu-id="96c76-132">ASP.NET Core больше не использует *Global.asax*, *web.config*, или *App_Start* папки.</span><span class="sxs-lookup"><span data-stu-id="96c76-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="96c76-133">Вместо этого все задачи запуска выполняются в *Startup.cs* в корневом каталоге проекта (см. в разделе [запуск приложения](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="96c76-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="96c76-134">В ASP.NET Core MVC, маршрутизация на основе атрибута теперь включены по умолчанию при `UseMvc()` вызывается; и это рекомендуемый подход для настройки маршрутов веб-API (и как начальный проект веб-API обрабатывает маршрутизацию).</span><span class="sxs-lookup"><span data-stu-id="96c76-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="96c76-135">Предположим, что вы хотите использовать в проекте, в дальнейшем маршрутизации с помощью атрибутов, без дополнительных настроек не требуется.</span><span class="sxs-lookup"><span data-stu-id="96c76-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="96c76-136">Просто примените атрибуты, необходимыми для контроллеров и действий, как показано в образце `ValuesController` класс, который включен в начальный проект веб-API:</span><span class="sxs-lookup"><span data-stu-id="96c76-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="96c76-137">Обратите внимание на наличие *[controller]* в строке 8.</span><span class="sxs-lookup"><span data-stu-id="96c76-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="96c76-138">Атрибут маршрутизации на основе теперь поддерживает определенные токены, такие как *[controller]* и *[действие]*.</span><span class="sxs-lookup"><span data-stu-id="96c76-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="96c76-139">Эти токены заменяются во время выполнения с именем контроллера или действия, соответственно, к которому был применен атрибут.</span><span class="sxs-lookup"><span data-stu-id="96c76-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="96c76-140">Это помогает сократить число соответствующих строк в проекте, и это гарантирует, что маршруты будут синхронизироваться с их соответствующих контроллеров и действий при применении рефакторинг Автоматическое переименование.</span><span class="sxs-lookup"><span data-stu-id="96c76-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="96c76-141">Чтобы перенести контроллер API продуктов, нам необходимо сначала скопировать *ProductsController* в новый проект.</span><span class="sxs-lookup"><span data-stu-id="96c76-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="96c76-142">Просто включите атрибут маршрута контроллера:</span><span class="sxs-lookup"><span data-stu-id="96c76-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="96c76-143">Необходимо также добавить `[HttpGet]` атрибут два метода, поскольку они оба должны быть вызваны посредством HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="96c76-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="96c76-144">Включить в атрибут для параметра «id» исходя из предположения `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="96c76-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="96c76-145">На этом этапе маршрутизации настроен правильно; Тем не менее пока не получится протестировать его.</span><span class="sxs-lookup"><span data-stu-id="96c76-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="96c76-146">Необходимо внести дополнительные изменения перед *ProductsController* будет компилироваться.</span><span class="sxs-lookup"><span data-stu-id="96c76-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="96c76-147">Перенос моделей и контроллеров</span><span class="sxs-lookup"><span data-stu-id="96c76-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="96c76-148">Последний шаг в процесс миграции для этого простого проекта веб-API — чтобы скопировать контроллера и всех моделей, они используют.</span><span class="sxs-lookup"><span data-stu-id="96c76-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="96c76-149">В этом случае просто скопировать *Controllers/ProductsController.cs* исходного проекта на новый.</span><span class="sxs-lookup"><span data-stu-id="96c76-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="96c76-150">Скопируйте целиком папку Models исходного проекта на новый.</span><span class="sxs-lookup"><span data-stu-id="96c76-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="96c76-151">Настройка пространства имен в соответствии с новым именем проекта (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="96c76-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="96c76-152">На этом этапе можно построить приложение, и вы увидите ряд ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="96c76-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="96c76-153">Следует обычно делятся на следующие категории:</span><span class="sxs-lookup"><span data-stu-id="96c76-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="96c76-154">*ApiController* не существует</span><span class="sxs-lookup"><span data-stu-id="96c76-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="96c76-155">*System.Web.Http* пространства имен не существует</span><span class="sxs-lookup"><span data-stu-id="96c76-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="96c76-156">*IHttpActionResult* не существует</span><span class="sxs-lookup"><span data-stu-id="96c76-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="96c76-157">К счастью это все очень просто сделать:</span><span class="sxs-lookup"><span data-stu-id="96c76-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="96c76-158">Изменение *ApiController* для *контроллера* (может потребоваться добавить *с помощью Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="96c76-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="96c76-159">Удалить все с помощью инструкции, ссылающийся на *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="96c76-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="96c76-160">Измените любой метод возвращение *IHttpActionResult* для возврата *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="96c76-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="96c76-161">Когда эти изменения были сделаны и неиспользуемые операторы using удалить, перенесенный *ProductsController* класс выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="96c76-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="96c76-162">Теперь можно запустить переноса проекта и перейдите к */api/products*; и вы увидите полный список трех продуктов.</span><span class="sxs-lookup"><span data-stu-id="96c76-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="96c76-163">Перейдите к */api/products/1* и вы увидите первый продукт.</span><span class="sxs-lookup"><span data-stu-id="96c76-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a><span data-ttu-id="96c76-164">Оболочка совместимости ASP.NET 4.x веб-API 2</span><span class="sxs-lookup"><span data-stu-id="96c76-164">ASP.NET 4.x Web API 2 compatibility shim</span></span>

<span data-ttu-id="96c76-165">— Это полезное средство, при миграции веб-API ASP.NET проектов ASP.NET Core [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) библиотеки.</span><span class="sxs-lookup"><span data-stu-id="96c76-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="96c76-166">Оболочка совместимости расширяет ASP.NET Core, чтобы разрешить несколько разных веб-API 2 соглашения для использования.</span><span class="sxs-lookup"><span data-stu-id="96c76-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="96c76-167">Пример, перенесена ранее в этом документе достаточно основные оболочки совместимости не был необходим.</span><span class="sxs-lookup"><span data-stu-id="96c76-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="96c76-168">Для крупных проектов с помощью оболочек совместимости можно использовать для временно Устранение противоречий API между ASP.NET Core и ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="96c76-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="96c76-169">Оболочку совместимости веб-API предназначен для использования в качестве временной меры для упрощения переноса больших проектов веб-API в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96c76-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="96c76-170">Со временем следует обновить проекты для использования шаблонов ASP.NET Core, не полагаясь на оболочку совместимости.</span><span class="sxs-lookup"><span data-stu-id="96c76-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="96c76-171">Совместимость возможностей, включенных в `Microsoft.AspNetCore.Mvc.WebApiCompatShim` включают:</span><span class="sxs-lookup"><span data-stu-id="96c76-171">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="96c76-172">Добавляет `ApiController` так, чтобы контроллеров базовые типы не должны быть обновлены.</span><span class="sxs-lookup"><span data-stu-id="96c76-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="96c76-173">Включает API веб-привязки модели.</span><span class="sxs-lookup"><span data-stu-id="96c76-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="96c76-174">ASP.NET Core MVC модель привязки работает так же, MVC 5, по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="96c76-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="96c76-175">Привязка более похожим на соглашения для привязки модели веб-API 2 модели изменения оболочек совместимости.</span><span class="sxs-lookup"><span data-stu-id="96c76-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="96c76-176">Например сложные типы автоматически привязываются из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="96c76-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="96c76-177">Расширяет привязки модели, что действия контроллера могут принимать параметры типа `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="96c76-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="96c76-178">Добавляет модули форматирования сообщений разрешение действий, чтобы возвращать результаты типа `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="96c76-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="96c76-179">Добавляет методы дополнительные ответа, которые воспользовались действия веб-API 2, для обслуживания ответов:</span><span class="sxs-lookup"><span data-stu-id="96c76-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="96c76-180">Генераторы HttpResponseMessage:</span><span class="sxs-lookup"><span data-stu-id="96c76-180">HttpResponseMessage generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="96c76-181">Методы результатов действий:</span><span class="sxs-lookup"><span data-stu-id="96c76-181">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="96c76-182">Добавляет экземпляр `IContentNegotiator` в контейнер внедрения Зависимостей приложения и делает содержимого типы, связанные с согласования из [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) доступны.</span><span class="sxs-lookup"><span data-stu-id="96c76-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="96c76-183">Сюда входят типы, такие как `DefaultContentNegotiator`, `MediaTypeFormatter`и т. д.</span><span class="sxs-lookup"><span data-stu-id="96c76-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="96c76-184">Чтобы использовать оболочку совместимости, вам потребуется:</span><span class="sxs-lookup"><span data-stu-id="96c76-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="96c76-185">Установка [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="96c76-185">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="96c76-186">Зарегистрируйте оболочку совместимости служб контейнера внедрения Зависимостей приложения с помощью метода `services.AddMvc().AddWebApiConventions()` в приложении `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="96c76-186">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="96c76-187">Определение маршрутов API веб-задачи, с помощью `MapWebApiRoute` на `IRouteBuilder` в приложении `IApplicationBuilder.UseMvc` вызова.</span><span class="sxs-lookup"><span data-stu-id="96c76-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="96c76-188">Сводка</span><span class="sxs-lookup"><span data-stu-id="96c76-188">Summary</span></span>

<span data-ttu-id="96c76-189">Миграция простого проекта веб-API ASP.NET на ASP.NET Core MVC — достаточно проста, еще раз благодарим за встроенную поддержку веб-API в ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="96c76-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="96c76-190">Основные части, каждый проект веб-API ASP.NET, потребуется выполнить миграцию, маршрутов, контроллеров и моделей, а также обновления типов, используемые контроллерами и действиями.</span><span class="sxs-lookup"><span data-stu-id="96c76-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
