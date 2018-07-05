---
uid: webhooks/receiving/receivers
title: Приемники ASP.NET веб-перехватчиков | Документация Майкрософт
author: rick-anderson
description: Приемники веб-перехватчики ASP.NET
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.openlocfilehash: be1f7dcef60642231af9c593eb7329ede7c2094f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374747"
---
# <a name="aspnet-webhooks-receivers"></a>Приемники веб-перехватчики ASP.NET

Получение веб-перехватчики зависит от того, кто является отправителем. Иногда возникают дополнительные действия, регистрация веб-перехватчика, проверка того, что действительно ожидает передачи данных подписчика. Некоторые веб-перехватчики предоставляют модель push-to-pull, где запрос HTTP POST только содержит ссылку на сведения о событии, которое затем требуется извлечь независимо друг от друга. Зачастую модель безопасности довольно зависит.

Веб-перехватчики Microsoft ASP.NET предназначена для того, чтобы сделать его более простой и более согласованным для подключения к API без тратит слишком много времени, разбираясь способ обработки какой-либо определенный вариант веб-перехватчики.

Получатель веб-перехватчика несет ответственность за принятие и проверку веб-перехватчики конкретного отправителя. Получатель веб-перехватчика может поддерживать любое количество веб-перехватчики, каждый из которых свою собственную конфигурацию. Например получатель веб-перехватчика GitHub может принимать веб-перехватчиков из любое количество репозиториев GitHub.

## <a name="webhook-receiver-uris"></a>Идентификаторы URI приемника веб-перехватчика

Установка веб-перехватчики Microsoft ASP.NET обеспечивает общие контроллер веб-перехватчика, который принимает запросы веб-перехватчика из неограниченного числа служб. Когда запрос прибывает, он выбирает соответствующий получатель, который вы установили для обработки конкретного отправителя веб-перехватчика.

URI этого контроллера — URI веб-перехватчика, который можно зарегистрировать в службе и имеет вид:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

По соображениям безопасности многих приемники веб-перехватчика требуют URI *https* URI и в некоторых случаях он также должен содержать параметр дополнительный запрос, который используется для принудительного применения, что только они предназначались можно отправлять выше URI веб-перехватчиков .

<em> <receiver> </em> Компонент является имя получателя, например <em>github</em> или <em>slack</em>.

*{Id}* является необязательным идентификатором, который может использоваться для идентификации конкретной конфигурации приемника веб-перехватчика. Это можно использовать для регистрации N веб-перехватчиков с определенной получателем. Например следующие три URI можно использовать для регистрации для трех независимых веб-перехватчиков:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Установка получатель веб-перехватчика

Для получения веб-перехватчики, с помощью веб-перехватчики Microsoft ASP.NET, сначала установите пакет Nuget для поставщика веб-перехватчика или вы хотите получать веб-перехватчиков из поставщиков. Пакеты Nuget именуются [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) где последняя часть указывает службе, поддерживаемой. Пример

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) обеспечивает поддержку для получения веб-перехватчиков из GitHub и [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) обеспечивает поддержку для получения веб-перехватчиков, созданных ASP. Веб-перехватчики NET.

Без дополнительной настройки можно получить поддержку для Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello и WordPress, но можно поддерживать любое количество других поставщиков.

## <a name="configuring-a-webhook-receiver"></a>Настройки приемника веб-перехватчика

Приемники веб-перехватчика настраиваются через [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) интерфейсу и конкретной реализации этого интерфейса могут быть зарегистрированы с помощью любой модели внедрения зависимостей. Реализация по умолчанию использует параметры приложения, который может быть установлено либо в файле Web.config или, если с помощью веб-приложений Azure, можно задать с помощью [портал Azure](https://portal.azure.com/).

![Параметры приложения Azure](_static/AzureAppSettings.png)

Ниже приведен формат для параметра приложения ключей:

```
MS_WebHookReceiverSecret_<receiver>
```

Значение — разделенный запятыми список значений, соответствующих *{id}* значения, для которых веб-перехватчиков зарегистрированных, например:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Инициализация получатель веб-перехватчика

Приемники веб-перехватчика инициализируются зарегистрировав их, обычно в *WebApiConfig* статического класса, например:

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
