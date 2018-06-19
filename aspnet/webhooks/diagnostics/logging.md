---
uid: webhooks/diagnostics/logging
title: Ведение журнала веб-ASP.NET и привязок | Документы Microsoft
author: rick-anderson
description: Как сделать ведения журналов в ASP.NET веб-привязок.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044829"
---
# <a name="aspnet-webhooks-logging"></a>Веб-перехватчиков ASP.NET ведения журнала

Веб-перехватчиков Microsoft ASP.NET использует ведения журнала как сообщение о проблемах и неполадках. По умолчанию журналы записываются с помощью [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) где они могут быть управляемых с помощью [прослушиватели трассировки](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) как и любой другой поток журнала.

При развертывании веб-приложения как веб-приложение Azure, журналы автоматически выбираться и могут управляться вместе с любым другим [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) ведения журнала. Дополнительные сведения см. в разделе [включить ведение журнала диагностики для веб-приложений в службе приложений Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Кроме того, журналы можно получить непосредственно в среде Visual Studio, как описано в [Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Перенаправление журналы

Вместо записи журналов для [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), можно предоставить реализацию альтернативный ведения журнала, такие как войти непосредственно на диспетчер журналов [Log4Net](http://logging.apache.org/log4net/) и [NLog ](http://nlog-project.org/). Достаточно просто указать реализацию [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) его будут извлекаться по веб-перехватчиков Microsoft ASP.NET и зарегистрируйте его в подсистему внедрения зависимостей по своему усмотрению. См. в разделе [внедрение зависимостей в ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) подробные сведения.
