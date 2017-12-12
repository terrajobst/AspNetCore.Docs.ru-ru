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
# <a name="aspnet-webhooks-logging"></a>Веб-перехватчиков ASP.NET ведения журнала

Веб-перехватчиков Microsoft ASP.NET использует ведения журнала как сообщение о проблемах и неполадках. По умолчанию журналы записываются с помощью [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) где они могут быть управляемых с помощью [прослушиватели трассировки](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) как и любой другой поток журнала.

При развертывании веб-приложения как веб-приложение Azure, журналы автоматически выбираться и могут управляться вместе с любым другим [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) ведения журнала. Дополнительные сведения см. в разделе [включить ведение журнала диагностики для веб-приложений в службе приложений Azure](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)

Кроме того, журналы можно получить непосредственно в среде Visual Studio, как описано в [Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Перенаправление журналы

Вместо записи журналов для [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), можно предоставить реализацию альтернативный ведения журнала, такие как войти непосредственно на диспетчер журналов [Log4Net](http://logging.apache.org/log4net/) и [NLog ](http://nlog-project.org/). Достаточно просто указать реализацию [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) его будут извлекаться по веб-перехватчиков Microsoft ASP.NET и зарегистрируйте его в подсистему внедрения зависимостей по своему усмотрению. См. в разделе [внедрение зависимостей в ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) подробные сведения.
