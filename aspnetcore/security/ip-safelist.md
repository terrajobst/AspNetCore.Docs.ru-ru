---
title: Адрес списка надежных IP-адресов клиента для ASP.NET Core
author: damienbod
description: Узнайте, как создавать по промежуточного слоя или фильтры действий для проверки удаленных IP-адресов по списку утвержденных IP-адресов.
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: ca5b0f8088773027f7403120247cbeca8900bcf5
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034347"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Адрес списка надежных IP-адресов клиента для ASP.NET Core

[(Damien Бауден](https://twitter.com/damien_bod) и [Tom Dykstra)](https://github.com/tdykstra)
 
В этой статье показано три способа реализации списка надежных IP-адресов (также известного как список разрешений) в приложении ASP.NET Core. Можно использовать:

* По промежуточного слоя для проверки удаленного IP-адреса каждого запроса.
* Фильтры действий для проверки удаленного IP-адреса запросов для конкретных контроллеров или методов действий.
* Razor Pages фильтры для проверки удаленного IP-адреса запросов на страницах Razor.

В каждом случае строка, содержащая утвержденные IP-адреса клиентов, сохраняется в параметре приложения. По промежуточного слоя или фильтра анализирует строку в виде списка и проверяет, входит ли удаленный IP-адрес в список. В противном случае возвращается код состояния HTTP 403 запрещено.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>Списка надежных отправителей

Список настраивается в файле *appSettings. JSON* . Это список, разделенный точкой с запятой, который может содержать адреса IPv4 и IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>ПО промежуточного слоя

Метод `Configure` добавляет по промежуточного слоя и передает ему строку списка надежных отправителей в параметре конструктора.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

По промежуточного слоя анализирует строку в массив и ищет удаленный IP-адрес в массиве. Если удаленный IP-адрес не найден, по промежуточного слоя возвращает HTTP 401 запрещено. Этот процесс проверки обходится для HTTP-запросов GET.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Фильтр действий

Если вы хотите, чтобы он был только для конкретных контроллеров или методов действий, используйте фильтр действий. Ниже приведен пример: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckFilter.cs)]

Фильтр действий добавляется в контейнер служб.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Затем фильтр можно использовать на контроллере или в методе действия.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

В примере приложения фильтр применяется к методу `Get`. Поэтому при тестировании приложения путем отправки `Get` запроса API атрибут проверяет IP-адрес клиента. При тестировании путем вызова API с любым другим методом HTTP по промежуточного слоя проверяет IP-адрес клиента.

## <a name="razor-pages-filter"></a>Фильтр Razor Pages 

Если требуется использование списка надежных отправителей для Razor Pages приложения, используйте фильтр Razor Pages. Ниже приведен пример: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckPageFilter.cs)]

Этот фильтр включается путем добавления его в коллекцию фильтров MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

При запуске приложения и запросе страницы Razor фильтр Razor Pages проверяет IP-адрес клиента.

## <a name="next-steps"></a>Следующие шаги

Дополнительные [сведения о ASP.NET Core по промежуточного слоя](xref:fundamentals/middleware/index).
