---
ms.openlocfilehash: 41021e30ae85dd0ae42cbe6f1606727e21bd7707
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705527"
---
# <a name="aspnet-core-middleware-extensibility-sample"></a><span data-ttu-id="f5adb-101">Пример расширяемости ПО промежуточного слоя ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f5adb-101">ASP.NET Core Middleware Extensibility Sample</span></span>

<span data-ttu-id="f5adb-102">Пример демонстрирует сценарии, описанные в статье [Фабричный метод активации ПО промежуточного слоя в ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).</span><span class="sxs-lookup"><span data-stu-id="f5adb-102">This sample demonstrates the scenarios described in [Factory-based middleware activation in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).</span></span>

<span data-ttu-id="f5adb-103">В примере приложения показана активация ПО промежуточного слоя следующими способами:</span><span class="sxs-lookup"><span data-stu-id="f5adb-103">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="f5adb-104">Стандартный.</span><span class="sxs-lookup"><span data-stu-id="f5adb-104">Convention.</span></span> <span data-ttu-id="f5adb-105">Дополнительные сведения о стандартной активации ПО промежуточного слоя см. [здесь](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/).</span><span class="sxs-lookup"><span data-stu-id="f5adb-105">For more information on conventional middleware activation, see the [Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/) topic.</span></span>
* <span data-ttu-id="f5adb-106">Реализация [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware).</span><span class="sxs-lookup"><span data-stu-id="f5adb-106">An [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="f5adb-107">ПО промежуточного слоя активируется классом [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f5adb-107">The default [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) class activates the middleware.</span></span>

<span data-ttu-id="f5adb-108">Реализации ПО промежуточного слоя функционируют идентичным образом и записывают значение, предоставленное в параметре строки запроса (`key`).</span><span class="sxs-lookup"><span data-stu-id="f5adb-108">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="f5adb-109">ПО промежуточного слоя использует внедренный контекст базы данных (ограниченная служба) для записи значения строки запроса в базу данных, выполняющуюся в памяти.</span><span class="sxs-lookup"><span data-stu-id="f5adb-109">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>
