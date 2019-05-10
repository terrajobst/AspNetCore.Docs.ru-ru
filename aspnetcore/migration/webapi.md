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
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="e04e3-103">Перенос из веб-API ASP.NET в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e04e3-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="e04e3-104">По [Scott Addie](https://twitter.com/scott_addie) и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e04e3-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e04e3-105">Веб-API ASP.NET 4.x является HTTP-службу, подходящее для широкого круга клиентов, включая браузеры и мобильные устройства.</span><span class="sxs-lookup"><span data-stu-id="e04e3-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="e04e3-106">ASP.NET Core объединяет 4.x ASP.NET MVC и веб-приложение API моделирует в более простую модель программирования, известный как ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e04e3-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="e04e3-107">В этой статье показаны действия, необходимые для миграции из веб-API ASP.NET 4.x ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e04e3-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="e04e3-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e04e3-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e04e3-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e04e3-109">Prerequisites</span></span>

[!INCLUDE [net-core-prereqs-vs-2.2](../includes/net-core-prereqs-vs-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="e04e3-110">Просмотрите проект веб-API ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="e04e3-110">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="e04e3-111">В качестве отправной точки, в этой статье использует *ProductsApp* проект, созданный в [Приступая к работе с веб-API ASP.NET 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="e04e3-111">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="e04e3-112">В этом проекте простого проекта веб-API ASP.NET 4.x настраивается следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e04e3-112">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="e04e3-113">В *Global.asax.cs*, выполняется вызов для `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="e04e3-113">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="e04e3-114">`WebApiConfig` Класс находится в *App_Start* папки и имеет статическое `Register` метод:</span><span class="sxs-lookup"><span data-stu-id="e04e3-114">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="e04e3-115">Этот класс настраивает [маршрутизации с помощью атрибутов](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), несмотря на то, что он фактически не используется в проекте.</span><span class="sxs-lookup"><span data-stu-id="e04e3-115">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="e04e3-116">Он также настраивает таблицы маршрутизации, который используется в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e04e3-116">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="e04e3-117">В этом случае веб-API ASP.NET 4.x ожидает, что URL-адреса в соответствии с форматом `/api/{controller}/{id}`, с помощью `{id}` дополнительными.</span><span class="sxs-lookup"><span data-stu-id="e04e3-117">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="e04e3-118">*ProductsApp* проект включает в себя один контроллер.</span><span class="sxs-lookup"><span data-stu-id="e04e3-118">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="e04e3-119">Контроллер наследует от `ApiController` и содержит два действия:</span><span class="sxs-lookup"><span data-stu-id="e04e3-119">The controller inherits from `ApiController` and contains two actions:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

<span data-ttu-id="e04e3-120">`Product` Модели, используемой `ProductsController` — это простой класс:</span><span class="sxs-lookup"><span data-stu-id="e04e3-120">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="e04e3-121">В следующих разделах демонстрируются миграции проекта веб-API ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e04e3-121">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="e04e3-122">Создайте проект назначения</span><span class="sxs-lookup"><span data-stu-id="e04e3-122">Create destination project</span></span>

<span data-ttu-id="e04e3-123">Выполните следующие действия в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e04e3-123">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="e04e3-124">Перейдите к **файл** > **новый** > **проекта** > **других типов проектов**  >  **Решения visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="e04e3-124">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="e04e3-125">Выберите **пустое решение**и назовите решение *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="e04e3-125">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="e04e3-126">Нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="e04e3-126">Click the **OK** button.</span></span>
* <span data-ttu-id="e04e3-127">Добавить существующий *ProductsApp* проекта к решению.</span><span class="sxs-lookup"><span data-stu-id="e04e3-127">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="e04e3-128">Добавьте новый **веб-приложение ASP.NET Core** проекта к решению.</span><span class="sxs-lookup"><span data-stu-id="e04e3-128">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="e04e3-129">Выберите **.NET Core** платформу из раскрывающегося списка и выберите **API** шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="e04e3-129">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="e04e3-130">Назовите проект *ProductsCore*и нажмите кнопку **ОК** кнопки.</span><span class="sxs-lookup"><span data-stu-id="e04e3-130">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="e04e3-131">Теперь решение содержит два проекта.</span><span class="sxs-lookup"><span data-stu-id="e04e3-131">The solution now contains two projects.</span></span> <span data-ttu-id="e04e3-132">В следующих разделах объясняется миграция *ProductsApp* содержимое проекта для *ProductsCore* проекта.</span><span class="sxs-lookup"><span data-stu-id="e04e3-132">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="e04e3-133">Миграция конфигурации</span><span class="sxs-lookup"><span data-stu-id="e04e3-133">Migrate configuration</span></span>

<span data-ttu-id="e04e3-134">ASP.NET Core не использует *App_Start* папку или *Global.asax* файл и *web.config* добавляется файл во время публикации.</span><span class="sxs-lookup"><span data-stu-id="e04e3-134">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="e04e3-135">*Startup.cs* является заменой для *Global.asax* и расположен в корневом каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="e04e3-135">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="e04e3-136">`Startup` Класс обрабатывает все задачи запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="e04e3-136">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="e04e3-137">Дополнительные сведения см. в разделе <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="e04e3-137">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="e04e3-138">В ASP.NET Core MVC маршрутизация с помощью атрибутов по умолчанию включается при <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> вызывается в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-138">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="e04e3-139">Следующие `UseMvc` вызовите заменяет *ProductsApp* проекта *App_Start/WebApiConfig.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="e04e3-139">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="e04e3-140">Перенос моделей и контроллеров</span><span class="sxs-lookup"><span data-stu-id="e04e3-140">Migrate models and controllers</span></span>

<span data-ttu-id="e04e3-141">Скопировать *ProductApp* контроллер проекта и модели, он использует.</span><span class="sxs-lookup"><span data-stu-id="e04e3-141">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="e04e3-142">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="e04e3-142">Follow these steps:</span></span>

1. <span data-ttu-id="e04e3-143">Копировать *Controllers/ProductsController.cs* исходного проекта на новый.</span><span class="sxs-lookup"><span data-stu-id="e04e3-143">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="e04e3-144">Скопировать весь *моделей* папку из одного проекта в новую.</span><span class="sxs-lookup"><span data-stu-id="e04e3-144">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="e04e3-145">Изменения пространств имен скопированные файлы в соответствии с новым именем проекта (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="e04e3-145">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="e04e3-146">Настройка `using ProductsApp.Models;` инструкции в *ProductsController.cs* слишком.</span><span class="sxs-lookup"><span data-stu-id="e04e3-146">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="e04e3-147">На этом этапе создания приложения приводит ряд ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="e04e3-147">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="e04e3-148">Ошибки возникают потому, что следующие компоненты отсутствуют в ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e04e3-148">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="e04e3-149">Класс `ApiController`</span><span class="sxs-lookup"><span data-stu-id="e04e3-149">`ApiController` class</span></span>
* <span data-ttu-id="e04e3-150">Пространство имен `System.Web.Http`</span><span class="sxs-lookup"><span data-stu-id="e04e3-150">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="e04e3-151">`IHttpActionResult` Интерфейс</span><span class="sxs-lookup"><span data-stu-id="e04e3-151">`IHttpActionResult` interface</span></span>

<span data-ttu-id="e04e3-152">Исправьте ошибки следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e04e3-152">Fix the errors as follows:</span></span>

1. <span data-ttu-id="e04e3-153">Изменение `ApiController` для <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="e04e3-153">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="e04e3-154">Добавить `using Microsoft.AspNetCore.Mvc;` Разрешить `ControllerBase` ссылки.</span><span class="sxs-lookup"><span data-stu-id="e04e3-154">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="e04e3-155">Удалите `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-155">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="e04e3-156">Изменение `GetProduct` тип возвращаемого значения действия из `IHttpActionResult` для `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-156">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

<span data-ttu-id="e04e3-157">Упростите `GetProduct` действия `return` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e04e3-157">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

```csharp
return product;
```

## <a name="configure-routing"></a><span data-ttu-id="e04e3-158">Настройка маршрутизации</span><span class="sxs-lookup"><span data-stu-id="e04e3-158">Configure routing</span></span>

<span data-ttu-id="e04e3-159">Настройка маршрутизации следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e04e3-159">Configure routing as follows:</span></span>

1. <span data-ttu-id="e04e3-160">Украшение `ProductsController` класс со следующими атрибутами:</span><span class="sxs-lookup"><span data-stu-id="e04e3-160">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="e04e3-161">Предыдущий [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) атрибут настраивает шаблон маршрутизации атрибутов контроллера.</span><span class="sxs-lookup"><span data-stu-id="e04e3-161">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="e04e3-162">[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) атрибут делает Маршрутизация атрибутов является обязательным для всех действий в контроллер.</span><span class="sxs-lookup"><span data-stu-id="e04e3-162">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="e04e3-163">Маршрутизация с помощью атрибутов поддерживают только токены, такие как `[controller]` и `[action]`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-163">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="e04e3-164">Во время выполнения каждый токен заменяется именем контроллера или действия, соответственно, к которому был применен атрибут.</span><span class="sxs-lookup"><span data-stu-id="e04e3-164">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="e04e3-165">Токены сократить число соответствующих строк в проекте.</span><span class="sxs-lookup"><span data-stu-id="e04e3-165">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="e04e3-166">Маркеры также убедиться в том случае, маршруты будут синхронизированы соответствующие контроллеры и действия при автоматически переименовать рефакторинг применяются.</span><span class="sxs-lookup"><span data-stu-id="e04e3-166">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="e04e3-167">Задайте режим совместимости проекта ASP.NET Core 2.2:</span><span class="sxs-lookup"><span data-stu-id="e04e3-167">Set the project's compatibility mode to ASP.NET Core 2.2:</span></span>

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    <span data-ttu-id="e04e3-168">Предыдущего изменения:</span><span class="sxs-lookup"><span data-stu-id="e04e3-168">The preceding change:</span></span>

    * <span data-ttu-id="e04e3-169">Необходим для использования `[ApiController]` атрибут на уровне контроллера.</span><span class="sxs-lookup"><span data-stu-id="e04e3-169">Is required to use the `[ApiController]` attribute at the controller level.</span></span>
    * <span data-ttu-id="e04e3-170">Позволяет способно нарушить особенности поведения, обусловленные в ASP.NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="e04e3-170">Opts in to potentially breaking behaviors introduced in ASP.NET Core 2.2.</span></span>
1. <span data-ttu-id="e04e3-171">Включить запросы HTTP Get к `ProductController` действия:</span><span class="sxs-lookup"><span data-stu-id="e04e3-171">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="e04e3-172">Применить [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) атрибут `GetAllProducts` действие.</span><span class="sxs-lookup"><span data-stu-id="e04e3-172">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="e04e3-173">Применить `[HttpGet("{id}")]` атрибут `GetProduct` действие.</span><span class="sxs-lookup"><span data-stu-id="e04e3-173">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="e04e3-174">После указанные выше изменения и удаления неиспользуемых `using` инструкций, *ProductsController.cs* файл выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e04e3-174">After the preceding changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="e04e3-175">Запустите переноса проекта и перейдите к `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-175">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="e04e3-176">Отображается полный список три продукта.</span><span class="sxs-lookup"><span data-stu-id="e04e3-176">A full list of three products appears.</span></span> <span data-ttu-id="e04e3-177">Перейдите по адресу `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-177">Browse to `/api/products/1`.</span></span> <span data-ttu-id="e04e3-178">Отображается первым продуктом.</span><span class="sxs-lookup"><span data-stu-id="e04e3-178">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="e04e3-179">Оболочка совместимости</span><span class="sxs-lookup"><span data-stu-id="e04e3-179">Compatibility shim</span></span>

<span data-ttu-id="e04e3-180">[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) библиотека предоставляет оболочку совместимости, чтобы переместить проекты веб-API ASP.NET 4.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e04e3-180">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="e04e3-181">Оболочка совместимости расширяет ASP.NET Core поддерживать несколько соглашений из веб-API ASP.NET 4.x 2.</span><span class="sxs-lookup"><span data-stu-id="e04e3-181">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="e04e3-182">Пример, перенесена ранее в этом документе достаточно основные что оболочку совместимости был не нужен.</span><span class="sxs-lookup"><span data-stu-id="e04e3-182">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="e04e3-183">Для крупных проектов с помощью оболочек совместимости можно использовать для временно Устранение противоречий API между ASP.NET Core и ASP.NET 4.x веб-API 2.</span><span class="sxs-lookup"><span data-stu-id="e04e3-183">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="e04e3-184">Оболочку совместимости веб-API предназначен для использования в качестве временной меры для поддержки миграции крупных проектах ASP.NET 4.x веб-API в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e04e3-184">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="e04e3-185">Со временем следует обновить проекты для использования шаблонов ASP.NET Core, не полагаясь на оболочку совместимости.</span><span class="sxs-lookup"><span data-stu-id="e04e3-185">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="e04e3-186">Совместимость возможностей, включенных в `Microsoft.AspNetCore.Mvc.WebApiCompatShim` включают:</span><span class="sxs-lookup"><span data-stu-id="e04e3-186">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="e04e3-187">Добавляет `ApiController` так, чтобы контроллеров базовые типы не должны быть обновлены.</span><span class="sxs-lookup"><span data-stu-id="e04e3-187">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="e04e3-188">Включает API веб-привязки модели.</span><span class="sxs-lookup"><span data-stu-id="e04e3-188">Enables Web API-style model binding.</span></span> <span data-ttu-id="e04e3-189">ASP.NET Core MVC модели привязки работает так же, что ASP.NET 4.x MVC 5, по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e04e3-189">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="e04e3-190">Привязка более похожим на соглашения для привязки модели ASP.NET 4.x веб-API 2 модели изменения оболочек совместимости.</span><span class="sxs-lookup"><span data-stu-id="e04e3-190">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="e04e3-191">Например сложные типы автоматически привязываются из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="e04e3-191">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="e04e3-192">Расширяет привязки модели, что действия контроллера могут принимать параметры типа `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-192">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="e04e3-193">Добавляет модули форматирования сообщений разрешение действий, чтобы возвращать результаты типа `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-193">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="e04e3-194">Добавляет методы дополнительные ответа, которые воспользовались действия веб-API 2, для обслуживания ответов:</span><span class="sxs-lookup"><span data-stu-id="e04e3-194">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="e04e3-195">`HttpResponseMessage` генераторы:</span><span class="sxs-lookup"><span data-stu-id="e04e3-195">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="e04e3-196">Методы результатов действий:</span><span class="sxs-lookup"><span data-stu-id="e04e3-196">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="e04e3-197">Добавляет экземпляр `IContentNegotiator` в приложение в контейнер внедрения зависимостей и делает доступными согласования связанные типы содержимого из [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="e04e3-197">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="e04e3-198">Примеры таких типов `DefaultContentNegotiator` и `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-198">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="e04e3-199">Чтобы использовать оболочку совместимости:</span><span class="sxs-lookup"><span data-stu-id="e04e3-199">To use the compatibility shim:</span></span>

1. <span data-ttu-id="e04e3-200">Установка [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="e04e3-200">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="e04e3-201">Зарегистрируйте оболочку совместимости служб контейнера внедрения Зависимостей приложения с помощью метода `services.AddMvc().AddWebApiConventions()` в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e04e3-201">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="e04e3-202">Определение направляет конкретных API-интерфейсов с помощью web `MapWebApiRoute` на `IRouteBuilder` в приложении `IApplicationBuilder.UseMvc` вызова.</span><span class="sxs-lookup"><span data-stu-id="e04e3-202">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e04e3-203">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e04e3-203">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
