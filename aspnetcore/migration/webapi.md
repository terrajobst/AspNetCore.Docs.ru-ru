---
title: "Миграция из веб-API ASP.NET"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 39224d1b2b0a54b043aa279659ff38134c5af7b7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="388bd-102">Миграция из веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="388bd-102">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="388bd-103">По [Стив Смит](https://ardalis.com/) и [Скотт Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="388bd-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="388bd-104">Веб-API — это службы HTTP, которые широкого диапазона клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="388bd-104">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="388bd-105">Основные ASP.NET MVC включает поддержку для создания веб-API, обеспечивая единый, согласованный способ создания веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="388bd-105">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="388bd-106">В этой статье мы демонстрируются шаги, необходимые для миграции реализация веб-API из веб-API ASP.NET ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="388bd-106">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="388bd-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="388bd-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="388bd-108">Просмотр ASP.NET Web API проекта</span><span class="sxs-lookup"><span data-stu-id="388bd-108">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="388bd-109">В этой статье используется пример проекта *ProductsApp*, созданный в статье [Приступая к работе с веб-API ASP.NET](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) начальной точкой.</span><span class="sxs-lookup"><span data-stu-id="388bd-109">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="388bd-110">В этом проекте простого проекта веб-API ASP.NET настраивается следующим образом.</span><span class="sxs-lookup"><span data-stu-id="388bd-110">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="388bd-111">В *Global.asax.cs*, выполняется вызов `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="388bd-111">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="388bd-112">`WebApiConfig`определен в *App_Start*, и имеет только один статический `Register` метод:</span><span class="sxs-lookup"><span data-stu-id="388bd-112">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="388bd-113">Этот класс настраивает [маршрутизацией атрибутов](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), несмотря на то, что он фактически не используется в проекте.</span><span class="sxs-lookup"><span data-stu-id="388bd-113">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="388bd-114">Он также настраивает таблицу маршрутизации, которая используется веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="388bd-114">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="388bd-115">В этом случае веб-API ASP.NET будет ожидать, что URL-адреса в соответствии с форматом */api/ {controller} / {id}*, с *{id}* дополнительными.</span><span class="sxs-lookup"><span data-stu-id="388bd-115">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="388bd-116">*ProductsApp* проект содержит только один простой контроллер, который наследует от `ApiController` и предоставляет два метода:</span><span class="sxs-lookup"><span data-stu-id="388bd-116">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="388bd-117">Наконец, модель *продукта*, используемый в *ProductsApp*, — это простой класс:</span><span class="sxs-lookup"><span data-stu-id="388bd-117">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="388bd-118">Теперь, когда у нас есть простой проект, из которого следует начать, мы показано, как перенести этот проект веб-API ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="388bd-118">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="388bd-119">Создайте проект назначения</span><span class="sxs-lookup"><span data-stu-id="388bd-119">Create the Destination Project</span></span>

<span data-ttu-id="388bd-120">С помощью Visual Studio, создайте новый, пустой решение и назовите его *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="388bd-120">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="388bd-121">Добавить существующий *ProductsApp* проекта к нему, затем добавьте в решение новый ASP.NET Core проект веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="388bd-121">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="388bd-122">Имя для нового проекта *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="388bd-122">Name the new project *ProductsCore*.</span></span>

![Откройте диалоговое окно нового проекта для веб-шаблонов](webapi/_static/add-web-project.png)

<span data-ttu-id="388bd-124">Затем выберите шаблон проекта веб-API.</span><span class="sxs-lookup"><span data-stu-id="388bd-124">Next, choose the Web API project template.</span></span> <span data-ttu-id="388bd-125">Мы перенесем *ProductsApp* содержимое в новый проект.</span><span class="sxs-lookup"><span data-stu-id="388bd-125">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Диалоговое окно создания веб-приложения с помощью шаблона проекта веб-API, выбранного в списке шаблонов ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="388bd-127">Удалить `Project_Readme.html` файла из нового проекта.</span><span class="sxs-lookup"><span data-stu-id="388bd-127">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="388bd-128">Решение теперь должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="388bd-128">Your solution should now look like this:</span></span>

![Откройте решение приложения в обозревателе решений файлы и папки проектов ProductsApp и ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="388bd-130">Перенос конфигурации</span><span class="sxs-lookup"><span data-stu-id="388bd-130">Migrate Configuration</span></span>

<span data-ttu-id="388bd-131">Больше не используется ASP.NET Core *Global.asax*, *web.config*, или *App_Start* папки.</span><span class="sxs-lookup"><span data-stu-id="388bd-131">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="388bd-132">Вместо этого все задачи запуска выполняются в *файла Startup.cs* в корневой папке проекта (в разделе [запуска приложения](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="388bd-132">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="388bd-133">В ASP.NET MVC ядра маршрутизации на основе атрибута теперь включается по умолчанию при `UseMvc()` вызове; и это рекомендуемый подход для настройки веб-API маршрутов (и как начальный проект веб-API обрабатывает маршрутизации).</span><span class="sxs-lookup"><span data-stu-id="388bd-133">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

<span data-ttu-id="388bd-134">Предположим, что вы хотите использовать в проекте, в дальнейшем маршрутизацией атрибутов, необходим без дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="388bd-134">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="388bd-135">Просто применить атрибуты, необходимыми для контроллеров и действий, как показано в образце `ValuesController` класс, который включен в начальный проект веб-API:</span><span class="sxs-lookup"><span data-stu-id="388bd-135">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="388bd-136">Обратите внимание на наличие *[контроллер]* в строке 8.</span><span class="sxs-lookup"><span data-stu-id="388bd-136">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="388bd-137">Атрибут маршрутизации на основе теперь поддерживает определенные токены, такие как *[контроллер]* и *[действие]*.</span><span class="sxs-lookup"><span data-stu-id="388bd-137">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="388bd-138">Эти токены заменяются во время выполнения имя контроллер или действие, соответственно, к которому был применен атрибут.</span><span class="sxs-lookup"><span data-stu-id="388bd-138">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="388bd-139">Это служит для уменьшения количества magic строк в проекте и гарантирует, что маршруты будут синхронизироваться с их соответствующие контроллеры и действия при применении Автоматическое переименование рефакторинга.</span><span class="sxs-lookup"><span data-stu-id="388bd-139">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="388bd-140">Чтобы перенести контроллер API продуктов, нам необходимо скопировать *ProductsController* в новый проект.</span><span class="sxs-lookup"><span data-stu-id="388bd-140">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="388bd-141">Затем просто включите атрибут маршрута на контроллере:</span><span class="sxs-lookup"><span data-stu-id="388bd-141">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="388bd-142">Необходимо также добавить `[HttpGet]` атрибут два метода, поскольку они оба должны быть вызваны посредством HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="388bd-142">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="388bd-143">Включить предположения параметра «идентификатор» в атрибуте `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="388bd-143">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="388bd-144">На этом этапе маршрутизации настроена правильно; Тем не менее мы не еще тестирования.</span><span class="sxs-lookup"><span data-stu-id="388bd-144">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="388bd-145">Дополнительные изменения должны быть выполнены до *ProductsController* будет компилироваться.</span><span class="sxs-lookup"><span data-stu-id="388bd-145">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="388bd-146">Миграция моделей и контроллеры</span><span class="sxs-lookup"><span data-stu-id="388bd-146">Migrate Models and Controllers</span></span>

<span data-ttu-id="388bd-147">Последний шаг в процессе миграции для этого простого проекта веб-API необходимо для копирования через контроллера и всех моделей, они используют.</span><span class="sxs-lookup"><span data-stu-id="388bd-147">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="388bd-148">В этом случае просто скопировать *Controllers/ProductsController.cs* исходного проекта в новый.</span><span class="sxs-lookup"><span data-stu-id="388bd-148">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="388bd-149">Затем скопируйте всю папку моделей исходного проекта в новый.</span><span class="sxs-lookup"><span data-stu-id="388bd-149">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="388bd-150">Настройка пространства имен в соответствии с новым именем проекта (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="388bd-150">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="388bd-151">На этом этапе можно построить приложение, и вы увидите число ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="388bd-151">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="388bd-152">Они должны обычно делятся на следующие категории:</span><span class="sxs-lookup"><span data-stu-id="388bd-152">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="388bd-153">*ApiController* не существует</span><span class="sxs-lookup"><span data-stu-id="388bd-153">*ApiController* does not exist</span></span>

* <span data-ttu-id="388bd-154">*System.Web.Http* пространство имен не существует.</span><span class="sxs-lookup"><span data-stu-id="388bd-154">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="388bd-155">*IHttpActionResult* не существует</span><span class="sxs-lookup"><span data-stu-id="388bd-155">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="388bd-156">К счастью это все очень легко исправить.</span><span class="sxs-lookup"><span data-stu-id="388bd-156">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="388bd-157">Изменение *ApiController* для *контроллера* (может потребоваться добавить *с помощью Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="388bd-157">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="388bd-158">Удалить все с помощью инструкции, ссылающиеся на *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="388bd-158">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="388bd-159">Изменение любого метода возврат *IHttpActionResult* для возврата *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="388bd-159">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="388bd-160">После завершения эти изменения были сделаны и неиспользуемые с помощью инструкций удаляется, перенесенная *ProductsController* класса выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="388bd-160">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="388bd-161">Теперь можно запустить перенесенного проекта и перейдите к */api/продуктов*; и следует просмотреть весь список продуктов, 3.</span><span class="sxs-lookup"><span data-stu-id="388bd-161">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="388bd-162">Перейдите к */api/products/1* и вы увидите первого продукта.</span><span class="sxs-lookup"><span data-stu-id="388bd-162">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="388bd-163">Сводка</span><span class="sxs-lookup"><span data-stu-id="388bd-163">Summary</span></span>

<span data-ttu-id="388bd-164">Миграция простого проекта веб-API ASP.NET ASP.NET Core MVC является довольно просто благодаря использованию встроенную поддержку веб-API в ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="388bd-164">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="388bd-165">Основных элементах, которые потребуется миграция каждый проект веб-API ASP.NET: маршруты, контроллеры и моделей, а также обновления для типов, используемых контроллеров и действий.</span><span class="sxs-lookup"><span data-stu-id="388bd-165">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
