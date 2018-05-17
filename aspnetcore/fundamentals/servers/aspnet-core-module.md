---
title: Модуль ASP.NET Core
author: rick-anderson
description: Сведения о том, как модуль ASP.NET Core позволяет веб-серверу Kestrel использовать IIS или IIS Express в качестве обратного прокси-сервера.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-module"></a><span data-ttu-id="43662-103">Модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43662-103">ASP.NET Core Module</span></span>

<span data-ttu-id="43662-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Рик Штраль](https://github.com/RickStrahl) (Rick Strahl) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="43662-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="43662-105">Модуль Core ASP.NET позволяет приложениям ASP.NET Core выполняться за IIS при использовании обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="43662-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="43662-106">Служба IIS предоставляет расширенные функции безопасности и управляемости веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="43662-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="43662-107">Поддерживаемые версии Windows:</span><span class="sxs-lookup"><span data-stu-id="43662-107">Supported Windows versions:</span></span>

* <span data-ttu-id="43662-108">Windows 7 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="43662-108">Windows 7 or later</span></span>
* <span data-ttu-id="43662-109">Windows Server 2008 R2 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="43662-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="43662-110">Модуль ASP.NET Core работает только с Kestrel.</span><span class="sxs-lookup"><span data-stu-id="43662-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="43662-111">Модуль несовместим с [HTTP.sys](xref:fundamentals/servers/httpsys) (ранее — [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="43662-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="43662-112">Описание модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43662-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="43662-113">Модуль ASP.NET Core имеет собственный модуль IIS, который подключается к конвейеру IIS для перенаправления веб-запросов во внутренние приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43662-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="43662-114">Многие собственные модули, такие как проверка подлинности Windows, остаются активными.</span><span class="sxs-lookup"><span data-stu-id="43662-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="43662-115">Дополнительные сведения о модулях IIS, активных в этом модуле, см. в разделе [Модули IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="43662-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="43662-116">Так как приложения ASP.NET Core выполняются в процессе, отделенном от рабочего процесса IIS, этот модуль также обрабатывает управление процессами.</span><span class="sxs-lookup"><span data-stu-id="43662-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="43662-117">Модуль запускает процесс для приложения ASP.NET Core при поступлении первого запроса и перезапускает приложение при сбое.</span><span class="sxs-lookup"><span data-stu-id="43662-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="43662-118">Это, по сути, совпадает с поведением приложений ASP.NET 4.x, выполняемых внутрипроцессно в IIS и управляемых [службой активации процессов Windows (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="43662-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="43662-119">На следующей схеме показана связь между IIS, модулем ASP.NET Core и приложениями ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="43662-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![Модуль ASP.NET Core](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="43662-121">Запросы поступают из Интернета в драйвер HTTP.sys в режиме ядра.</span><span class="sxs-lookup"><span data-stu-id="43662-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="43662-122">Драйвер направляет запросы к службам IIS на настроенный порт веб-сайта — обычно 80 (HTTP) или 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="43662-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="43662-123">Модуль перенаправляет запросы Kestrel на случайный порт для приложения, который не является портом 80 или 443.</span><span class="sxs-lookup"><span data-stu-id="43662-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="43662-124">Модуль задает порт с помощью переменной среды во время запуска, а ПО промежуточного слоя для интеграции IIS настраивает сервер для прослушивания `http://localhost:{port}`.</span><span class="sxs-lookup"><span data-stu-id="43662-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="43662-125">Выполняются дополнительные проверки, и запросы не из модуля отклоняются.</span><span class="sxs-lookup"><span data-stu-id="43662-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="43662-126">Модуль не поддерживает переадресацию по HTTPS, поэтому запросы переадресовываются по протоколу HTTP, даже если были получены IIS по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="43662-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="43662-127">После того как Kestrel забирает запрос из модуля, запрос передается в конвейер ПО промежуточного слоя ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43662-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="43662-128">Конвейер ПО промежуточного слоя обрабатывает запрос и передает его в качестве экземпляра `HttpContext` в логику приложения.</span><span class="sxs-lookup"><span data-stu-id="43662-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="43662-129">Отклик приложения передается обратно в службу IIS, которая отправляет его обратно в HTTP-клиент, инициировавший запрос.</span><span class="sxs-lookup"><span data-stu-id="43662-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="43662-130">Модуль ASP.NET Core имеет несколько других функций.</span><span class="sxs-lookup"><span data-stu-id="43662-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="43662-131">Функции модуля:</span><span class="sxs-lookup"><span data-stu-id="43662-131">The module can:</span></span>

* <span data-ttu-id="43662-132">Задание переменных среды для рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="43662-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="43662-133">Внесение в журнал выходных данных stdout для хранилища файлов с целью устранения неполадок при запуске.</span><span class="sxs-lookup"><span data-stu-id="43662-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="43662-134">Переадресация токенов проверки подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="43662-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="43662-135">Как установить и использовать модуль ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43662-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="43662-136">Подробные инструкции о том, как установить и использовать модуль ASP.NET Core, см. в разделе [Размещение в Windows с помощью IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="43662-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="43662-137">Дополнительные сведения о настройке модуля см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="43662-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43662-138">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="43662-138">Additional resources</span></span>

* [<span data-ttu-id="43662-139">Размещение в Windows с помощью IIS</span><span class="sxs-lookup"><span data-stu-id="43662-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="43662-140">Справочник по конфигурации модуля ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43662-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="43662-141">Репозиторий GitHub для модуля ASP.NET Core (исходный код)</span><span class="sxs-lookup"><span data-stu-id="43662-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
