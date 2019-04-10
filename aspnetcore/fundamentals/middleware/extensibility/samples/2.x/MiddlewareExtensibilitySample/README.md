---
ms.openlocfilehash: 41021e30ae85dd0ae42cbe6f1606727e21bd7707
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809395"
---
# <a name="aspnet-core-middleware-extensibility-sample"></a>Пример расширяемости ПО промежуточного слоя ASP.NET Core

Пример демонстрирует сценарии, описанные в статье [Фабричный метод активации ПО промежуточного слоя в ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).

В примере приложения показана активация ПО промежуточного слоя следующими способами:

* Стандартный. Дополнительные сведения о стандартной активации ПО промежуточного слоя см. [здесь](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/).
* Реализация [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware). ПО промежуточного слоя активируется классом [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) по умолчанию.

Реализации ПО промежуточного слоя функционируют идентичным образом и записывают значение, предоставленное в параметре строки запроса (`key`). ПО промежуточного слоя использует внедренный контекст базы данных (ограниченная служба) для записи значения строки запроса в базу данных, выполняющуюся в памяти.
