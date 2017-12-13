---
uid: webhooks/source
title: "Веб-перехватчиков ASP.NET исходный код и пакеты NuGet | Документы Microsoft"
author: rick-anderson
description: "Ссылки на веб-перехватчиков ASP.NET исходный код и пакеты NuGet"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="b7e0f-103">Веб-перехватчиков ASP.NET исходный код и пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="b7e0f-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="b7e0f-104">Веб-перехватчиков Microsoft ASP.NET является частью семейства Microsoft ASP.NET модулей и размещается в виде [Открытие исходного проекта на GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="b7e0f-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="b7e0f-105">Это означает, что мы принимаем работы, но см. в [вклад рекомендации](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) перед отправкой запроса на включение.</span><span class="sxs-lookup"><span data-stu-id="b7e0f-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="b7e0f-106">В следующей документации по документации, которую вы читаете теперь также размещается в виде [открытым исходным кодом на сайте GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , а также принимает публикаций.</span><span class="sxs-lookup"><span data-stu-id="b7e0f-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="b7e0f-107">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="b7e0f-107">Nuget Packages</span></span>

<span data-ttu-id="b7e0f-108">Веб-перехватчиков Microsoft ASP.NET можно получить, как просмотреть пакеты Nuget, это означает, что необходимо выбрать флаг предварительного просмотра в Visual Studio для отображения этих.</span><span class="sxs-lookup"><span data-stu-id="b7e0f-108">Microsoft ASP.NET WebHooks is also available as preview Nuget packages which means that you have to select the Preview flag in Visual Studio in order to see them.</span></span>

<span data-ttu-id="b7e0f-109">[Пакетов Nuget](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) devided на три части:</span><span class="sxs-lookup"><span data-stu-id="b7e0f-109">The [Nuget packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are devided into three parts:</span></span>

* <span data-ttu-id="b7e0f-110">[Общие](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): общие пакета, который является общим для отправителей и получателей.</span><span class="sxs-lookup"><span data-stu-id="b7e0f-110">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="b7e0f-111">[Отправитель](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): набор пакетов, поддержка отправки собственных веб-перехватчиков для других пользователей.</span><span class="sxs-lookup"><span data-stu-id="b7e0f-111">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="b7e0f-112">Данная функциональная возможность для отправки веб-перехватчиков описан более подробно в [отправки веб-перехватчиков](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="b7e0f-112">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="b7e0f-113">[Приемники](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): набор пакетов, поддерживающий получение веб-перехватчиков от других.</span><span class="sxs-lookup"><span data-stu-id="b7e0f-113">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="b7e0f-114">Более подробно описана функциональность для получения веб-перехватчиков [получения веб-перехватчиков](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="b7e0f-114">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
