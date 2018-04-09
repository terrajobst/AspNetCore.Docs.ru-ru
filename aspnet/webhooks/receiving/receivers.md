---
uid: webhooks/receiving/receivers
title: Приемники ASP.NET веб-перехватчиков | Документы Microsoft
author: rick-anderson
description: Приемники веб-перехватчиков ASP.NET
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: a8e42521f201f88b0ed433550e8786411b4487b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="02b85-103">Приемники веб-перехватчиков ASP.NET</span><span class="sxs-lookup"><span data-stu-id="02b85-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="02b85-104">Получение веб-перехватчиков зависит от того, кто является отправителем.</span><span class="sxs-lookup"><span data-stu-id="02b85-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="02b85-105">Иногда возникают дополнительные действия, регистрация веб-перехватчика, проверка действительно прослушивает подписчика.</span><span class="sxs-lookup"><span data-stu-id="02b85-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="02b85-106">Некоторые веб-перехватчиков, обеспечивают модель push запросу, где запрос HTTP POST только содержит ссылку на сведения о событии, которое затем быть получены отдельно.</span><span class="sxs-lookup"><span data-stu-id="02b85-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="02b85-107">Зачастую модель безопасности немного меняется.</span><span class="sxs-lookup"><span data-stu-id="02b85-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="02b85-108">Microsoft ASP.NET веб-перехватчиков предназначено для более простой и более согласованным для подключения к API тратить время на определение способов обработки какой-либо определенный вариант веб-привязок.</span><span class="sxs-lookup"><span data-stu-id="02b85-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="02b85-109">Получатель веб-перехватчика отвечает за принятие и проверка веб-перехватчиков конкретного отправителя.</span><span class="sxs-lookup"><span data-stu-id="02b85-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="02b85-110">Веб-перехватчика получатель может поддерживать любое количество веб-привязок, каждый из которых свою собственную конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="02b85-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="02b85-111">Например веб-перехватчика GitHub получатель может принимать веб-перехватчиков от любого числа репозиториях GitHub.</span><span class="sxs-lookup"><span data-stu-id="02b85-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="02b85-112">Идентификаторы URI приемника веб-перехватчика</span><span class="sxs-lookup"><span data-stu-id="02b85-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="02b85-113">Установка веб-перехватчиков Microsoft ASP.NET получить общие контроллера веб-перехватчика, которая принимает запросы веб-перехватчика из неограниченного числа служб.</span><span class="sxs-lookup"><span data-stu-id="02b85-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="02b85-114">При получении запроса, он выбирает соответствующие приемника, установленного для обработки конкретного веб-перехватчика отправителя.</span><span class="sxs-lookup"><span data-stu-id="02b85-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="02b85-115">URI этого контроллера веб-перехватчика URI, который зарегистрирован в службе и имеет следующую форму:</span><span class="sxs-lookup"><span data-stu-id="02b85-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="02b85-116">По соображениям безопасности нескольких получателей веб-перехватчика требуют URI *https* URI и в некоторых случаях он также должен содержать параметр дополнительный запрос, который используется для принудительного применения, только они предназначались можно отправлять выше URI веб-перехватчиков .</span><span class="sxs-lookup"><span data-stu-id="02b85-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="02b85-117"><em> <receiver> </em> Компонента — это имя получателя, например <em>github</em> или <em>slack</em>.</span><span class="sxs-lookup"><span data-stu-id="02b85-117">The <em><receiver></em> component is the name of the receiver, for example <em>github</em> or <em>slack</em>.</span></span>

<span data-ttu-id="02b85-118">*{Id}* является необязательным идентификатором, который может использоваться для идентификации конкретной конфигурации приемника веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="02b85-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="02b85-119">Это можно использовать для регистрации веб-N привязок с определенной получателем.</span><span class="sxs-lookup"><span data-stu-id="02b85-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="02b85-120">Например следующие три URI можно использовать для регистрации три независимых веб-привязок:</span><span class="sxs-lookup"><span data-stu-id="02b85-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="02b85-121">Установка веб-перехватчика получателя</span><span class="sxs-lookup"><span data-stu-id="02b85-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="02b85-122">Для получения веб-привязок с помощью веб-перехватчиков Microsoft ASP.NET, сначала установить пакет Nuget для веб-перехватчика поставщика или вы хотите получать веб-перехватчиков из поставщиков.</span><span class="sxs-lookup"><span data-stu-id="02b85-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="02b85-123">Пакеты Nuget именуются [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) где указывает службе, поддерживаемой в последней части.</span><span class="sxs-lookup"><span data-stu-id="02b85-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="02b85-124">Пример</span><span class="sxs-lookup"><span data-stu-id="02b85-124">For example</span></span>

<span data-ttu-id="02b85-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) обеспечивает поддержку для получения веб-перехватчиков из GitHub и [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) обеспечивает поддержку для получения веб-перехватчиков созданные ASP. Веб-привязок NET.</span><span class="sxs-lookup"><span data-stu-id="02b85-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="02b85-126">Без дополнительной настройки можно найти поддержку общего банка данных, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, чередующиеся, Trello и WordPress, однако существует возможность поддержки любого количества других поставщиков.</span><span class="sxs-lookup"><span data-stu-id="02b85-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="02b85-127">Настройка веб-перехватчика получателя</span><span class="sxs-lookup"><span data-stu-id="02b85-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="02b85-128">Приемники веб-перехватчика настраиваются через [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) интерфейсом и конкретной реализации этого интерфейса могут быть зарегистрированы с помощью любой модели внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="02b85-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="02b85-129">Реализация по умолчанию использует параметры приложения, которое может быть установлено либо в файле Web.config, или при использовании веб-приложения Azure можно задать с помощью [портала Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="02b85-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Параметры приложения Azure](_static/AzureAppSettings.png)

<span data-ttu-id="02b85-131">Для параметра приложения ключей имеет следующий формат:</span><span class="sxs-lookup"><span data-stu-id="02b85-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="02b85-132">Значение является список с разделенными запятыми значениями, соответствующими *{id}* значения, для которых веб-перехватчиков зарегистрированных, например:</span><span class="sxs-lookup"><span data-stu-id="02b85-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="02b85-133">Инициализация приемника веб-перехватчика</span><span class="sxs-lookup"><span data-stu-id="02b85-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="02b85-134">Приемники веб-перехватчика инициализируются, зарегистрируйте их, обычно в *WebApiConfig* статического класса, например:</span><span class="sxs-lookup"><span data-stu-id="02b85-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
