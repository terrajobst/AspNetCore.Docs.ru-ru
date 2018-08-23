---
uid: webhooks/diagnostics/logging
title: Веб-перехватчики ASP.NET ведения журнала | Документация Майкрософт
author: rick-anderson
description: Как сделать ведения журналов в ASP.NET веб-перехватчики.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 2e86d519c24da102075b4da0a32787c90deb0f6b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836272"
---
# <a name="aspnet-webhooks-logging"></a>Веб-перехватчики ASP.NET ведения журнала

Веб-перехватчики Microsoft ASP.NET использует ведения журнала как способ reporting проблемах и неполадках. По умолчанию журналы записываются с помощью [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) с помощью которых они могут быть групповые [прослушиватели трассировки](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) , такие как любой другой поток журнала.

При развертывании веб-приложения как веб-приложение Azure, журналы выбираются автоматически и могут управляться вместе с любой другой [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) ведения журнала. Сведения см. в разделе [Включение ведения журнала диагностики для веб-приложений в службе приложений Azure](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Кроме того, журналы можно получить непосредственно в Visual Studio, как описано в разделе [Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Перенаправление журналов

Вместо того чтобы писать журналы, чтобы [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), можно обеспечить реализацию альтернативный ведения журнала, который может входить непосредственно к диспетчер журналов, таких как [Log4Net](http://logging.apache.org/log4net/) и [NLog ](http://nlog-project.org/). Достаточно просто указать реализацию [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) его будут учтены службой веб-перехватчики Microsoft ASP.NET и зарегистрируйте его в системе внедрения зависимостей по своему усмотрению. См. в разделе [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) подробные сведения.
