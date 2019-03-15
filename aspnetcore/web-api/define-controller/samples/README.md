---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665513"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="6151b-101">Пример контроллера веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6151b-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="6151b-102">Этот пример приложения состоит из следующих проектов:</span><span class="sxs-lookup"><span data-stu-id="6151b-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="6151b-103">**WebApiSample.Api.22**: проект ASP.NET Core 2.2, предназначенный для .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="6151b-103">**WebApiSample.Api.22**: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="6151b-104">**WebApiSample.Api.21**: проект ASP.NET Core 2.1, предназначенный для .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="6151b-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="6151b-105">**WebApiSample.Api.Pre21**: проект ASP.NET Core 2.0, предназначенный для .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6151b-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="6151b-106">**WebApiSample.DataAccess**: библиотека классов .NET Standard 2.0, которая служит в качестве уровня доступа к данным для двух проектов веб-API.</span><span class="sxs-lookup"><span data-stu-id="6151b-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="6151b-107">В этом примере показаны различные способы создания контроллера веб-API:</span><span class="sxs-lookup"><span data-stu-id="6151b-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="6151b-108">Создание класса, производного от ControllerBase</span><span class="sxs-lookup"><span data-stu-id="6151b-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="6151b-109">Аннотирование класса атрибутом ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="6151b-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
