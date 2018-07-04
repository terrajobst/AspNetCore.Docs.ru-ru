---
uid: webhooks/source
title: Исходный код ASP.NET веб-перехватчики и пакетов NuGet | Документация Майкрософт
author: rick-anderson
description: Ссылки на исходный код ASP.NET веб-перехватчики и пакеты NuGet
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: ''
ms.openlocfilehash: 49a6d3e92e8d6365bea6594a616922aff9d0b4ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375853"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="812d0-103">Исходный код ASP.NET веб-перехватчики и пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="812d0-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="812d0-104">Веб-перехватчики Microsoft ASP.NET является частью семейства Microsoft ASP.NET, модулей и размещен как управляющий [открытым проектом источник на GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="812d0-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="812d0-105">Это означает, что мы принимаем работы, но см. в [рекомендациями по участию](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) перед отправкой запроса на включение внесенных изменений.</span><span class="sxs-lookup"><span data-stu-id="812d0-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="812d0-106">Теперь этот электронной документации, который вы читаете также размещен как управляющий [открытым исходным кодом на GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) , а также принимает вклад.</span><span class="sxs-lookup"><span data-stu-id="812d0-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="812d0-107">Пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="812d0-107">NuGet packages</span></span>

<span data-ttu-id="812d0-108">[Пакеты NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) можно разделить на три части:</span><span class="sxs-lookup"><span data-stu-id="812d0-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="812d0-109">[Распространенные](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): общие пакета, который разделяется между отправителями и получателями.</span><span class="sxs-lookup"><span data-stu-id="812d0-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="812d0-110">[Отправитель](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): набор пакетов, поддержкой собственных веб-перехватчики пересылки другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="812d0-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="812d0-111">Функциональные возможности для отправки веб-перехватчики описан более подробно в [отправки веб-перехватчики](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="812d0-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="812d0-112">[Приемники](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): набор пакетов, поддерживающих получение веб-перехватчики от других.</span><span class="sxs-lookup"><span data-stu-id="812d0-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="812d0-113">Функциональность для получения веб-перехватчики описан более подробно в [получения веб-перехватчики](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="812d0-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
