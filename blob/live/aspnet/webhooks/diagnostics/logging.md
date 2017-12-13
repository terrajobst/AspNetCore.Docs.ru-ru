---
uid: webhooks/diagnostics/logging
title: "Ведение журнала веб-ASP.NET и привязок | Документы Microsoft"
author: rick-anderson
description: "Как сделать ведения журналов в ASP.NET веб-привязок."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 5691d5c394b4e606282ba302953e8a06e7d73860
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="f3e4a-103">Веб-перехватчиков ASP.NET ведения журнала</span><span class="sxs-lookup"><span data-stu-id="f3e4a-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="f3e4a-104">Веб-перехватчиков Microsoft ASP.NET использует ведения журнала как сообщение о проблемах и неполадках.</span><span class="sxs-lookup"><span data-stu-id="f3e4a-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="f3e4a-105">По умолчанию журналы записываются с помощью [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) где они могут быть управляемых с помощью [прослушиватели трассировки](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) как и любой другой поток журнала.</span><span class="sxs-lookup"><span data-stu-id="f3e4a-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="f3e4a-106">При развертывании веб-приложения как веб-приложение Azure, журналы автоматически выбираться и могут управляться вместе с любым другим [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="f3e4a-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="f3e4a-107">Дополнительные сведения см. в разделе [включить ведение журнала диагностики для веб-приложений в службе приложений Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="f3e4a-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="f3e4a-108">Кроме того, журналы можно получить непосредственно в среде Visual Studio, как описано в [Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="f3e4a-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="f3e4a-109">Перенаправление журналы</span><span class="sxs-lookup"><span data-stu-id="f3e4a-109">Redirecting Logs</span></span>

<span data-ttu-id="f3e4a-110">Вместо записи журналов для [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), можно предоставить реализацию альтернативный ведения журнала, такие как войти непосредственно на диспетчер журналов [Log4Net](http://logging.apache.org/log4net/) и [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="f3e4a-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="f3e4a-111">Достаточно просто указать реализацию [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) его будут извлекаться по веб-перехватчиков Microsoft ASP.NET и зарегистрируйте его в подсистему внедрения зависимостей по своему усмотрению.</span><span class="sxs-lookup"><span data-stu-id="f3e4a-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="f3e4a-112">См. в разделе [внедрение зависимостей в ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="f3e4a-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
