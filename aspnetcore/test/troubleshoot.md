---
title: Устранение неполадок для ASP.NET Core
author: Rick-Anderson
description: Обзор и диагностика предупреждений и ошибок в проектах ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 64a353a9bdf0753c63f676f9d07f42ba45acdcab
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217648"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Устранение неполадок в проектах ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Рекомендации по устранению неполадок по следующим ссылкам:

* [Устранение неполадок ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/troubleshoot)
* [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot)
* [Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube: диагностика проблем в приложениях ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>Пакет SDK для .NET core предупреждения

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Установлены 32-разрядную и 64-разрядные версии пакета SDK для .NET Core
В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение: 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/both32and64bit.png)

Это предупреждение появляется, когда (x86) 32-разрядных и 64-разрядный (x 64) версии [пакета SDK для .NET Core](https://www.microsoft.com/net/download/all) установлены. Распространенные причины могут быть установлены обе версии включают:

* Первоначально загрузить установщик пакета SDK для .NET Core, используя 32-разрядном компьютере, но затем копируются выполняем и он установлен на 64-разрядном компьютере. 
* 32-разрядной .NET Core SDK был установлен другим приложением.
* Неправильная версия была загружаются и устанавливаются.

Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение. Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**. Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>В нескольких местах установлен пакет SDK .NET Core
В **новый проект** диалоговое окно для ASP.NET Core может появиться следующее предупреждение: 

 В нескольких местах установлен пакет SDK .NET Core. Только шаблоны из пакетах SDK, установлена в "C:\Program Files\dotnet\sdk\' будет отображаться.

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/multiplelocations.png)

Это сообщение отображается при наличии по крайней мере один установки пакета SDK для .NET Core в каталоге за пределами * C:\Program Files\dotnet\sdk\*. Обычно это происходит после развертывания пакета SDK .NET Core на компьютере, вместо копирования и вставки установщик MSI.

Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение. Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**. Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.

### <a name="no-net-core-sdks-were-detected"></a>Обнаружены не .NET Core пакеты SDK
В **новый проект** диалоговое окно для ASP.NET Core может появиться следующее предупреждение: 

**Обнаружены не .NET Core пакеты SDK, убедитесь, что они включаются в переменной среды «PATH»**

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/NoNetCore.png)

Это предупреждение появляется, когда переменная среды `PATH` не указывает на любой пакетами SDK .NET Core на компьютере. Для решения этой проблемы:

* Установка или убедитесь, что установлен пакет SDK .NET Core.
* Проверьте `PATH` указывает переменную среды на расположение, установлен пакет SDK. Установщик обычно задает `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a>Использование IHtmlHelper.Partial может привести к взаимоблокировок приложений

В ASP.NET Core 2.1 и более поздних версиях вызов `Html.Partial` приводит к анализатора предупреждения из-за потенциальный риск взаимоблокировки. Является предупреждающее сообщение:

*Использование IHtmlHelper.Partial может привести взаимоблокировок приложений. Рассмотрите возможность использования `<partial>` вспомогательный тег или `IHtmlHelper.PartialAsync`.*

Вызовы `@Html.Partial` следует заменить на `@await Html.PartialAsync` или вспомогательных частичного тег `<partial name="_Partial" />`.

::: moniker-end
