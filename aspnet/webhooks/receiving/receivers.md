---
uid: webhooks/receiving/receivers
title: "Приемники ASP.NET веб-перехватчиков | Документы Microsoft"
author: rick-anderson
description: "Приемники веб-перехватчиков ASP.NET"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 8c42db4056dd7a6ef77c7bcbc0eca3b5bf7c87e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receivers"></a>Приемники веб-перехватчиков ASP.NET

Получение веб-перехватчиков зависит от того, кто является отправителем. Иногда возникают дополнительные действия, регистрация веб-перехватчика, проверка действительно прослушивает подписчика. Некоторые веб-перехватчиков, обеспечивают модель push запросу, где запрос HTTP POST только содержит ссылку на сведения о событии, которое затем быть получены отдельно. Зачастую модель безопасности немного меняется.

Microsoft ASP.NET веб-перехватчиков предназначено для более простой и более согласованным для подключения к API тратить время на определение способов обработки какой-либо определенный вариант веб-привязок.

Получатель веб-перехватчика отвечает за принятие и проверка веб-перехватчиков конкретного отправителя. Веб-перехватчика получатель может поддерживать любое количество веб-привязок, каждый из которых свою собственную конфигурацию. Например веб-перехватчика GitHub получатель может принимать веб-перехватчиков от любого числа репозиториях GitHub.

## <a name="webhook-receiver-uris"></a>Идентификаторы URI приемника веб-перехватчика

Установка веб-перехватчиков Microsoft ASP.NET получить общие контроллера веб-перехватчика, которая принимает запросы веб-перехватчика из неограниченного числа служб. При получении запроса, он выбирает соответствующие приемника, установленного для обработки конкретного веб-перехватчика отправителя.

URI этого контроллера веб-перехватчика URI, который зарегистрирован в службе и имеет следующую форму:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

По соображениям безопасности нескольких получателей веб-перехватчика требуют URI *https* URI и в некоторых случаях он также должен содержать параметр дополнительный запрос, который используется для принудительного применения, только они предназначались можно отправлять выше URI веб-перехватчиков .

 *<receiver>*  Компонента — это имя получателя, например *github* или *slack*.

*{Id}* является необязательным идентификатором, который может использоваться для идентификации конкретной конфигурации приемника веб-перехватчика. Это можно использовать для регистрации веб-N привязок с определенной получателем. Например следующие три URI можно использовать для регистрации три независимых веб-привязок:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Установка веб-перехватчика получателя

Для получения веб-привязок с помощью веб-перехватчиков Microsoft ASP.NET, сначала установить пакет Nuget для веб-перехватчика поставщика или вы хотите получать веб-перехватчиков из поставщиков. Пакеты Nuget именуются [Microsoft.AspNet.WebHooks.Receivers.*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) где указывает службе, поддерживаемой в последней части. Пример

[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) обеспечивает поддержку для получения веб-перехватчиков из GitHub и [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) обеспечивает поддержку для получения веб-перехватчиков созданные ASP. Веб-привязок NET.

Без дополнительной настройки можно найти поддержку общего банка данных, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, чередующиеся, Trello и WordPress, однако существует возможность поддержки любого количества других поставщиков.

## <a name="configuring-a-webhook-receiver"></a>Настройка веб-перехватчика получателя

Приемники веб-перехватчика настраиваются через [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) интерфейсом и конкретной реализации этого интерфейса могут быть зарегистрированы с помощью любой модели внедрения зависимостей. Реализация по умолчанию использует параметры приложения, которое может быть установлено либо в файле Web.config, или при использовании веб-приложения Azure можно задать с помощью [портала Azure](https://portal.azure.com/).

![Параметры приложения Azure](_static/AzureAppSettings.png)

Для параметра приложения ключей имеет следующий формат:

```
MS_WebHookReceiverSecret_<receiver>
```

Значение является список с разделенными запятыми значениями, соответствующими *{id}* значения, для которых веб-перехватчиков зарегистрированных, например:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Инициализация приемника веб-перехватчика

Приемники веб-перехватчика инициализируются, зарегистрируйте их, обычно в *WebApiConfig* статического класса, например:

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
