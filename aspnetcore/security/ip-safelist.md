---
title: Объединение списков надежных IP-адрес клиента для ASP.NET Core
author: damienbod
description: Узнайте, как по промежуточного слоя или действия фильтров для проверки IP-адресов на основе списка утвержденных IP-адреса записи.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040117"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Объединение списков надежных IP-адрес клиента для ASP.NET Core

По [Дэмиен Боуден](https://twitter.com/damien_bod) и [том Дайкстра](https://github.com/tdykstra)
 
В этой статье показано два способа реализации safelist IP-адрес (также известный как белый список):

* С помощью ASP.NET Core по промежуточного слоя для проверки удаленного IP-адрес каждого запроса.
* Проверить с помощью фильтров действий в ASP.NET Core удаленный IP-адрес запросов для конкретного действия методов.

Пример приложения показаны оба подхода. В каждом случае строка, содержащая утвержденных клиентских IP-адресов хранится в параметр приложения. В по промежуточного слоя или фильтр анализирует строку в список и проверяет, является ли удаленный IP-адрес в списке. В противном случае возвращается код состояния HTTP 403-запрещено.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-safelist"></a>Объединение списков надежных

Список настраивается в *appsettings.json* файла. Оно является списком разделенных точкой с запятой и может содержать адреса IPv4 и IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>ПО промежуточного слоя

`Configure` Метод добавляет по промежуточного слоя и передает ему строку safelist в качестве параметра конструктора.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

По промежуточного слоя выполняет синтаксический анализ строки в массив и ищет удаленный IP-адрес в массиве. Если удаленный IP-адрес не найден, по промежуточного слоя возвращает HTTP 401 запрещено. Этот процесс проверки пропускается для запросов HTTP Get.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Фильтр действий

Объединение списков надежных только для определенных контроллеров или методы действий, используйте фильтр действий. Ниже приведен пример: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

Фильтр действий добавляется в контейнер служб.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Фильтр можно использовать на контроллеру или методу действия.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

В примере приложения, фильтр применяется к `Get` метод. Так что при тестировании приложения, отправляя `Get` API запроса, атрибут проверки IP-адрес клиента. Если вы тестируете путем вызова API с любой другой метод HTTP, по промежуточного слоя проверки IP-адрес клиента.

## <a name="razor-pages-filter"></a>Фильтр страниц Razor 

Если требуется safelist в приложение Razor Pages, используйте фильтр Razor Pages. Ниже приведен пример: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

Этот фильтр включается путем добавления его в коллекцию фильтров MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Когда вы запустите приложение и запрос страницы Razor, Razor Pages фильтр проверки IP-адрес клиента.

## <a name="next-steps"></a>Следующие шаги

[Дополнительные сведения о по промежуточного слоя ASP.NET Core](xref:fundamentals/middleware/index).
