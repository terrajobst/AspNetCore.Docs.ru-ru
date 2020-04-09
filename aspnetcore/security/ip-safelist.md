---
title: Клиент IP-безопасное место для ASP.NET Core
author: damienbod
description: Узнайте, как написать промежуточное программное обеспечение или фильтры действий для проверки удаленных IP-адресов со списком утвержденных IP-адресов.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
uid: security/ip-safelist
ms.openlocfilehash: 2db879a6918245cbacff8b1a5dc15786ffab6a34
ms.sourcegitcommit: 196e4a36df5be5b04fedcff484a4261f8046ec57
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2020
ms.locfileid: "80471800"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Клиент IP-безопасное место для ASP.NET Core

[Дэмиен Боуден](https://twitter.com/damien_bod) и [Том Дайкстра](https://github.com/tdykstra)
 
В этой статье показаны три способа реализации списка safelist IP-адресов (также известный как список разрешений) в приложении ASP.NET Core. Сопроводительное приложение образца демонстрирует все три подхода. Вы можете использовать:

* Middleware, чтобы проверить удаленный IP-адрес каждого запроса.
* Фильтры действий MVC для проверки удаленного IP-адреса запросов на конкретные контроллеры или методы действий.
* Razor Pages фильтры для проверки удаленного IP-адреса запросов на страницы Razor.

В каждом случае строка, содержащая одобренные IP-адреса клиента, хранится в настройках приложения. Промежуток или фильтр:

* Разбирает строку в массив. 
* Проверка наличия удаленного IP-адреса в массиве.

Доступ разрешен, если массив содержит IP-адрес. В противном случае возвращается код статуса HTTP 403.

[Просмотр или загрузка образца кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples) [(как скачать)](xref:index#how-to-download-a-sample)

## <a name="ip-address-safelist"></a>Список безопасности IP-адресов

В примере приложения, IP-адрес safelist является:

* Определяется `AdminSafeList` свойством в файле *appsettings.json.*
* Строка, различенная за поломки, которая может содержать как [варианты 4 Протокола Интернета (IPv4),](https://wikipedia.org/wiki/IPv4) так и [адреса Протокола Интернета 6 (IPv6).](https://wikipedia.org/wiki/IPv6)

[!code-json[](ip-safelist/samples/3.x/ClientIpAspNetCore/appsettings.json?range=1-3&highlight=2)]

В предыдущем примере допускаются адреса `127.0.0.1` `192.168.1.5` IPv4 и и адрес `::1` iPv6 `0:0:0:0:0:0:0:1`loopback (сжатый формат для ) .

## <a name="middleware"></a>ПО промежуточного слоя

Метод `Startup.Configure` добавляет пользовательский `AdminSafeListMiddleware` тип промежуточного посуды в конвейер запроса приложения. Безопасный список извлекается с помощью поставщика конфигурации .NET Core и передается в качестве параметра конструктора.

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureAddMiddleware)]

Промежуточное программное обеспечение разбирает строку в массив и ищет удаленный IP-адрес в массиве. Если удаленный IP-адрес не найден, промежуточное программное обеспечение возвращает HTTP 403 Запрещено. Этот процесс проверки обойдя запросы HTTP GET.

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Middlewares/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Фильтр действия

Если требуется контроль доступа на основе безопасного доступа для определенных контроллеров MVC или методов действий, используйте фильтр действия. Пример:

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckActionFilter.cs?name=snippet_ClassOnly)]

В `Startup.ConfigureServices`введите фильтр действия в коллекцию фильтров MVC. В следующем примере `ClientIpCheckActionFilter` добавляется фильтр действия. Безопасный список и экземпляр регистратора консолей передаются в качестве параметров конструктора.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

Фильтр действия может быть применен к контроллеру или методу действия с атрибутом [«ServiceFilter»:](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute)

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_ActionFilter&highlight=1)]

В примере приложения фильтр действия применяется к `Get` методу действия контроллера. При тестировании приложения путем отправки:

* Запрос HTTP GET, `[ServiceFilter]` атрибут проверяет IP-адрес клиента. Если доступ к `Get` методу действия разрешен, то изменение следующего вывода консоли производится фильтром действия и методом действия:

    ```
    dbug: ClientIpSafelistComponents.Filters.ClientIpCheckActionFilter[0]
          Remote IpAddress: ::1
    dbug: ClientIpAspNetCore.Controllers.ValuesController[0]
          successful HTTP GET    
    ```

* Глагол запроса HTTP, `AdminSafeListMiddleware` кроме GET, посредник проверяет IP-адрес клиента.

## <a name="razor-pages-filter"></a>Фильтр Страницбрина

Если для приложения Razor Pages требуется контроль доступа, управляемый безопасным, используйте фильтр Razor Pages. Пример:

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckPageFilter.cs?name=snippet_ClassOnly)]

В, `Startup.ConfigureServices`включить фильтр Razor Pages, добавив его в коллекцию фильтров MVC. В следующем примере `ClientIpCheckPageFilter` добавляется фильтр Razor Pages. Безопасный список и экземпляр регистратора консолей передаются в качестве параметров конструктора.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

При запросе страницы *индекса* Razor образца приложения фильтр Razor Pages проверяет IP-адрес клиента. Фильтр производит вариацию следующего вывода консоли:

```
dbug: ClientIpSafelistComponents.Filters.ClientIpCheckPageFilter[0]
      Remote IpAddress: ::1
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/middleware/index>
* [Фильтры действия](xref:mvc/controllers/filters#action-filters)
* <xref:razor-pages/filter>
