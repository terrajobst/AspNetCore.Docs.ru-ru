---
title: Применять HTTPS в ASP.NET Core
author: rick-anderson
description: Показано, как требуется HTTPS/TLS в ASP.NET Core веб-приложения.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 0433ddb3bf1ef0074c683903ad4553cd6a0b4741
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687822"
---
# <a name="enforce-https-in-an-aspnet-core"></a>Применять HTTPS в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом документе показано, как:

- Требуйте использования протокола HTTPS для всех запросов.
- Перенаправление всех запросов HTTP на HTTPS.

> [!WARNING]
> Сделать **не** использовать `RequireHttpsAttribute` на веб-API, получать конфиденциальные сведения. `RequireHttpsAttribute` использует коды состояния HTTP для перенаправления обозревателей с HTTP на HTTPS. Клиенты API не может понять или подчиняются перенаправлений с HTTP на HTTPS. Такие клиенты могут отправлять сведения по протоколу HTTP. Веб-API должен:
>
>* Прослушивает HTTP.
>* Закрывает соединение с кодом состояния 400 (неправильный запрос) и обслуживает запрос.

<a name="require"></a>
## <a name="require-https"></a>Требовать использования протокола HTTPS

::: moniker range=">= aspnetcore-2.1"
Корпорация Майкрософт рекомендует всем ASP.NET Core веб-приложений вызов `UseHttpsRedirection` для перенаправления всех запросов HTTP на HTTPS. Если `UseHsts` вызывается в приложении, он должен быть вызван перед `UseHttpsRedirection`.

Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]


В приведенном ниже коде

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

* Наборы `RedirectStatusCode`.
* Задает 5001 HTTPS-порт.

::: moniker-end


::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) используется для обязательного использования протокола HTTPS. `[RequireHttpsAttribute]` можно дополнить контроллеров или методы или могут быть применены глобально. Чтобы применить атрибут глобально, добавьте следующий код в `ConfigureServices` в `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Предыдущий выделенный код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются. Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).

Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности. Применение `[RequireHttps]` атрибут ко всем страницам контроллеры и Razor не считается защищено настолько, насколько требования HTTPS глобально. Не может гарантировать `[RequireHttps]` атрибут при добавлении новых контроллеров и страниц Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Безопасность Strict транспортный протокол HTTP (HSTS)

На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [строгой безопасности транспорта (HSTS) HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) — расширение согласиться на использование безопасности, определяемый веб-приложения с помощью специальных заголовка. Когда этот заголовок получает поддерживаемого браузера браузера позволит обмен данными с передачей по протоколу HTTP к указанному домену и вместо этого будет отправлять все связи по протоколу HTTPS. Он также препятствует щелкните HTTPS через запросы в браузерах.

ASP.NET Core 2.1 или более поздней реализует HSTS с `UseHsts` метода расширения. Следующий код вызывает `UseHsts` при это приложение не в [режим разработки](xref:fundamentals/environments):

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` не рекомендуется при разработке решений, так как заголовок HSTS высокой может быть кэширован в браузерах. По умолчанию UseHsts исключает локальный петлевой адрес.

В приведенном ниже коде

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Задает параметр предварительной загрузки заголовка безопасности Strict-транспорта. Предварительная загрузка не частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается для веб-браузера для предварительной загрузки HSTS узлов на установку. Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).
* Включает [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS дочерних узлов. 
* Явно задает для параметра max-age заголовка безопасности для транспорта Strict до 60 дней. Если свойство не задано, по умолчанию используется значение 30 дней. В разделе [директива max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) для получения дополнительной информации.
* Добавляет `example.com` в список узлов для исключения.

`UseHsts` исключает замыкания на себя следующие узлы:

* `localhost` : IPv4-адрес замыкания на себя.
* `127.0.0.1` : IPv4-адрес замыкания на себя.
* `[::1]` : Петлевой адрес IPv6.

Предыдущий пример демонстрирует добавление дополнительных узлов.
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Отказаться от HTTPS на создания проекта

Включить ASP.NET Core 2.1 и более поздних версий шаблонов веб-приложений (из Visual Studio или в командной строке dotnet) [перенаправления HTTPS](#require) и [HSTS](#hsts). Для развертываний, не требующие HTTPS вы можете отказаться от HTTPS. Например некоторые серверных служб, где HTTPS обрабатывается извне на границе с помощью протокола HTTPS на каждом узле не требуется.

Чтобы отказаться от HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Снимите флажок **настроить для использования протокола HTTPS** флажок.

![Схема сущностей](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli) 

Использовать параметр `--no-https`. Пример

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Как можно настроить сертификат разработчика для Docker

В разделе [этой проблемы GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
