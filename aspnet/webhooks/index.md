---
uid: webhooks/index
title: Общие сведения о веб-перехватчиков ASP.NET | Документы Microsoft
author: rick-anderson
description: Введение в ASP.NET веб-привязок.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530053"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="4189a-103">Общие сведения о веб-перехватчиков ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4189a-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="4189a-104">Веб-перехватчиков это шаблон упрощенных HTTP, обеспечивая простой pub/sub модель для подключения друг с другом служб веб-API и SaaS.</span><span class="sxs-lookup"><span data-stu-id="4189a-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="4189a-105">Когда событие происходит в службе, уведомление отправляется в форме запроса HTTP POST зарегистрированных подписчиков.</span><span class="sxs-lookup"><span data-stu-id="4189a-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="4189a-106">Запрос POST содержит сведения о событии, что делает возможным для получателя выполнить соответствующие действия.</span><span class="sxs-lookup"><span data-stu-id="4189a-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="4189a-107">Из-за их простоты веб-перехватчиков уже предоставляемых большое количество служб в том числе [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp ](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/)и многое другое.</span><span class="sxs-lookup"><span data-stu-id="4189a-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="4189a-108">Например, веб-перехватчик может указывать, что файл был изменен в [Dropbox](http://dropbox.com/), изменение кода зафиксировано в GitHub или платеж был инициализирован в [PayPal](http://www.paypal.com/), или карту было создано в [ Trello](http://www.trello.com/).</span><span class="sxs-lookup"><span data-stu-id="4189a-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="4189a-109">Возможности бесконечны!</span><span class="sxs-lookup"><span data-stu-id="4189a-109">The possibilities are endless!</span></span>

<span data-ttu-id="4189a-110">Веб-перехватчиков для Microsoft ASP.NET упрощает процесс отправки и получения веб-перехватчиков как часть приложения ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="4189a-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="4189a-111">На принимающей стороне он предоставляет общую модель для получения и обработки веб-перехватчиков из любое число поставщиков веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="4189a-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="4189a-112">Они поступают из поля с поддержкой [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[ WordPress](http://www.wordpress.com) и [Zendesk](https://www.zendesk.com/) , но можно легко добавить поддержку для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="4189a-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="4189a-113">На отправляющей стороне обеспечивает поддержку для управления и хранения подписки также, как и для отправки уведомлений о событиях для правильного набора подписчиков.</span><span class="sxs-lookup"><span data-stu-id="4189a-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="4189a-114">Это позволяет определить собственный набор событий, что подписчики можно подписаться и уведомлением, когда происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="4189a-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="4189a-115">Эти части можно использовать вместе или друг от друга в зависимости от вашего сценария.</span><span class="sxs-lookup"><span data-stu-id="4189a-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="4189a-116">Если требуется получать веб-перехватчиков от других служб, можно использовать только элемент приемника; Если требуется только для предоставления веб-перехватчиков для других пользователей для использования, затем этого просто.</span><span class="sxs-lookup"><span data-stu-id="4189a-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="4189a-117">Код предназначен для ASP.NET Web API 2 и ASP.NET MVC 5 и доступен в виде [операционные системы на сайте GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="4189a-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="4189a-118">Общие сведения о веб-перехватчиков</span><span class="sxs-lookup"><span data-stu-id="4189a-118">WebHooks Overview</span></span>

<span data-ttu-id="4189a-119">Веб-привязок представляет собой шаблон, который означает, что зависит от его использование из службы для службы, но основная идея одинакова.</span><span class="sxs-lookup"><span data-stu-id="4189a-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="4189a-120">Можно считать из веб-перехватчиков модели простого pub/sub которой пользователь может подписаться на событиях, происходящих в другом месте.</span><span class="sxs-lookup"><span data-stu-id="4189a-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="4189a-121">Уведомления о событиях передаются в виде запросов HTTP POST, содержащий сведения о самом событии.</span><span class="sxs-lookup"><span data-stu-id="4189a-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="4189a-122">Обычно запрос HTTP POST содержит объект JSON или определить с помощью веб-перехватчика отправителя, включая сведения о событии, вызывает веб-перехватчика для запуска данные HTML-формы.</span><span class="sxs-lookup"><span data-stu-id="4189a-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="4189a-123">Например, пример текста запроса веб-перехватчика POST от [GitHub](http://www.github.com/) выглядит следующим образом в результате новая проблема, открываемого в конкретный репозиторий:</span><span class="sxs-lookup"><span data-stu-id="4189a-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="4189a-124">Убедитесь, что веб-перехватчик действительно от предполагаемого отправителя, запрос POST защищены каким-либо образом и затем проверяются получателем.</span><span class="sxs-lookup"><span data-stu-id="4189a-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="4189a-125">Например [веб-перехватчиков GitHub](https://developer.github.com/webhooks/) включает *X концентратора подписи* заголовка HTTP с хэшем в тексте запроса, в которой проверяется реализацией приемника, поэтому не нужно беспокоиться о нем.</span><span class="sxs-lookup"><span data-stu-id="4189a-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="4189a-126">Веб-перехватчика потока обычно выглядит приблизительно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4189a-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="4189a-127">Веб-перехватчика Отправитель создает события, которые клиент может подписаться.</span><span class="sxs-lookup"><span data-stu-id="4189a-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="4189a-128">События описаны заметные изменения в систему, например, элемент данных был вставленных, что процесс завершен, или другое.</span><span class="sxs-lookup"><span data-stu-id="4189a-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="4189a-129">Получатель веб-перехватчика подписывается с регистрации веб-перехватчика, состоящий из четыре действия:</span><span class="sxs-lookup"><span data-stu-id="4189a-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="4189a-130">URI, для которой уведомление о событии должна быть помещена в форме запроса HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="4189a-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="4189a-131">Набор фильтров описания конкретных событий, для которых должен срабатывать веб-перехватчик;</span><span class="sxs-lookup"><span data-stu-id="4189a-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="4189a-132">Секретный ключ, который используется для подписи запроса HTTP POST;</span><span class="sxs-lookup"><span data-stu-id="4189a-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="4189a-133">Дополнительные данные, которые включены в запрос HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="4189a-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="4189a-134">Например, это может быть дополнительные поля заголовка HTTP или свойств, включенных в тексте запроса HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="4189a-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="4189a-135">Когда происходит событие, соответствующий регистрации веб-перехватчика находятся и отправляет запросы HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="4189a-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="4189a-136">Как правило Создание запросов HTTP POST выполняется повторная попытка добавления несколько раз для какой-либо причине, что получатель не отвечает или результатов запроса HTTP POST в ответ на ошибку.</span><span class="sxs-lookup"><span data-stu-id="4189a-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="4189a-137">Конвейер обработки веб-перехватчиков</span><span class="sxs-lookup"><span data-stu-id="4189a-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="4189a-138">Конвейер обработки веб-перехватчиков Microsoft ASP.NET для входящего веб-перехватчиков выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4189a-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![Конвейер обработки ASP.NET веб-перехватчиков](_static/WebHookReceivers.png)

<span data-ttu-id="4189a-140">Две основные понятия, *приемники* и *обработчики*:</span><span class="sxs-lookup"><span data-stu-id="4189a-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="4189a-141">*Приемники* ответственность за обработку определенного вида веб-перехватчика из заданного отправителя и обеспечения безопасности проверок, чтобы убедиться, что веб-перехватчика запрос на самом деле является от предполагаемого отправителя.</span><span class="sxs-lookup"><span data-stu-id="4189a-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="4189a-142">*Обработчики* , как правило, когда пользовательский код выполняется обработки определенного веб-перехватчика.</span><span class="sxs-lookup"><span data-stu-id="4189a-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="4189a-143">В следующие узлы этих концепций описаны более подробно.</span><span class="sxs-lookup"><span data-stu-id="4189a-143">In the following nodes these concepts are described in more details.</span></span>
