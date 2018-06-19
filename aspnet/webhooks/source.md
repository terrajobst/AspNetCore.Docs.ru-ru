---
uid: webhooks/source
title: Веб-перехватчиков ASP.NET исходный код и пакеты NuGet | Документы Microsoft
author: rick-anderson
description: Ссылки на веб-перехватчиков ASP.NET исходный код и пакеты NuGet
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2018
ms.locfileid: "27709974"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="f0399-103">Веб-перехватчиков ASP.NET исходный код и пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="f0399-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="f0399-104">Веб-перехватчиков Microsoft ASP.NET является частью семейства Microsoft ASP.NET модулей и размещается в виде [Открытие исходного проекта на GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="f0399-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="f0399-105">Это означает, что мы принимаем работы, но см. в [вклад рекомендации](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) перед отправкой запроса на включение.</span><span class="sxs-lookup"><span data-stu-id="f0399-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="f0399-106">В следующей документации по документации, которую вы читаете теперь также размещается в виде [открытым исходным кодом на сайте GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , а также принимает публикаций.</span><span class="sxs-lookup"><span data-stu-id="f0399-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="f0399-107">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="f0399-107">NuGet packages</span></span>

<span data-ttu-id="f0399-108">[Пакетов NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) делятся на три части:</span><span class="sxs-lookup"><span data-stu-id="f0399-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="f0399-109">[Общие](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): общие пакета, который является общим для отправителей и получателей.</span><span class="sxs-lookup"><span data-stu-id="f0399-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="f0399-110">[Отправитель](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): набор пакетов, поддержка отправки собственных веб-перехватчиков для других пользователей.</span><span class="sxs-lookup"><span data-stu-id="f0399-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="f0399-111">Данная функциональная возможность для отправки веб-перехватчиков описан более подробно в [отправки веб-перехватчиков](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="f0399-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="f0399-112">[Приемники](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): набор пакетов, поддерживающий получение веб-перехватчиков от других.</span><span class="sxs-lookup"><span data-stu-id="f0399-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="f0399-113">Более подробно описана функциональность для получения веб-перехватчиков [получения веб-перехватчиков](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="f0399-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
