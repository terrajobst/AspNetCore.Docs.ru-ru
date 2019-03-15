---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665758"
---
# <a name="error-handling-sample-application"></a><span data-ttu-id="9ef3f-101">Пример приложения для обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="9ef3f-101">Error Handling Sample Application</span></span>

<span data-ttu-id="9ef3f-102">Этот пример приложения демонстрирует сценарии, описанные в статье [Обработка ошибок в ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling).</span><span class="sxs-lookup"><span data-stu-id="9ef3f-102">This sample app demonstrates the scenarios described in the [Handle errors in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) topic.</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="9ef3f-103">Настройка пользовательской страницы обработки исключений</span><span class="sxs-lookup"><span data-stu-id="9ef3f-103">Configure a custom exception handling page</span></span>

<span data-ttu-id="9ef3f-104">Если приложение не выполняется в среде разработки, ПО промежуточного слоя для обработки исключений:</span><span class="sxs-lookup"><span data-stu-id="9ef3f-104">When the app isn't running in the Development environment, Exception Handling Middleware:</span></span>

* <span data-ttu-id="9ef3f-105">Перехватывает исключения.</span><span class="sxs-lookup"><span data-stu-id="9ef3f-105">Catches exceptions.</span></span>
* <span data-ttu-id="9ef3f-106">Ведет журнал исключений.</span><span class="sxs-lookup"><span data-stu-id="9ef3f-106">Logs exceptions.</span></span>
* <span data-ttu-id="9ef3f-107">повторно выполняет запрос в альтернативном конвейере по указанному пути.</span><span class="sxs-lookup"><span data-stu-id="9ef3f-107">Re-executes the request in an alternate pipeline at the path provided.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="9ef3f-108">Настройка пользовательского кода обработки исключений</span><span class="sxs-lookup"><span data-stu-id="9ef3f-108">Configure custom exception handling code</span></span>

<span data-ttu-id="9ef3f-109">Альтернативой предоставления конечной точки для обработки ошибок с помощью пользовательской страницы обработки исключений является предоставление лямбда-функции методу `UseExceptionHandler`.</span><span class="sxs-lookup"><span data-stu-id="9ef3f-109">An alternative to serving an endpoint for errors with a custom exception handling page is to provide a lambda to `UseExceptionHandler`.</span></span> <span data-ttu-id="9ef3f-110">Использование лямбда-функции с методом `UseExceptionHandler` позволяет получить доступ к ошибке до возврата ответа.</span><span class="sxs-lookup"><span data-stu-id="9ef3f-110">Using a lambda with `UseExceptionHandler` allows access to the error before returning the response.</span></span>

<span data-ttu-id="9ef3f-111">Пример приложения демонстрирует пользовательский код обработки исключений в методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9ef3f-111">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="9ef3f-112">Следуйте инструкциям в верхней части файла *Startup.cs* (`LambdaErrorHandler`).</span><span class="sxs-lookup"><span data-stu-id="9ef3f-112">Follow the instructions at the top of the *Startup.cs* file (`LambdaErrorHandler`).</span></span> <span data-ttu-id="9ef3f-113">После запуска приложения вызовите исключение с помощью ссылки **Выдать исключение** на странице указателя.</span><span class="sxs-lookup"><span data-stu-id="9ef3f-113">After the app starts, trigger an exception with the **Throw Exception** link on the Index page.</span></span>
