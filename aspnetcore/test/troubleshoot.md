---
title: Устранение неполадок в проектах ASP.NET Core
author: Rick-Anderson
description: Устранение неполадок при возникновении ошибок и предупреждений в проектах ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274597"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Устранение неполадок в проектах ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Рекомендации по устранению неполадок по следующим ссылкам:

* [Устранение неполадок ASP.NET Core в службе приложений Azure](xref:host-and-deploy/azure-apps/troubleshoot)
* [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot)
* [Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [Конференция NDC (Лондон, 2018): Диагностике неполадок в приложениях ASP.NET Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [Блог разработчиков ASP.NET: Устранение неполадок производительности ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>Пакет SDK для .NET core предупреждения

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Установлены 32-разрядную и 64-разрядные версии пакета SDK для .NET Core

В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:

> Установлены 32- и 64 бит версии пакета SDK для .NET Core. Только шаблоны из 64-разрядной версии, установленной на "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/both32and64bit.png)

Это предупреждение появляется, когда (x86) 32-разрядных и 64-разрядный (x 64) версии [пакета SDK для .NET Core](https://www.microsoft.com/net/download/all) установлены. Распространенных причин, которые могут быть установлены обе версии включают:

* Первоначально загрузить установщик пакета SDK для .NET Core, используя 32-разрядном компьютере, но затем скопировать выполняем и он установлен на 64-разрядном компьютере.
* 32-разрядной .NET Core SDK был установлен другим приложением.
* Неправильная версия была загружаются и устанавливаются.

Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение. Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**. Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>В нескольких местах установлен пакет SDK .NET Core

В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:

> В нескольких местах установлен пакет SDK .NET Core. Только шаблоны из пакетах SDK, установленного на "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/multiplelocations.png)

Это сообщение отображается при наличии по крайней мере один установки пакета SDK для .NET Core в каталоге за пределами *C:\\Program Files\\dotnet\\sdk\\*. Обычно это происходит, когда на компьютере, вместо копирования и вставки установщик MSI был развернут пакет SDK .NET Core.

Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение. Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**. Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.

### <a name="no-net-core-sdks-were-detected"></a>Обнаружены не .NET Core пакеты SDK

В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:

> Обнаружены не .NET Core пакеты SDK, убедитесь, что они включаются в переменной среды «PATH».

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/NoNetCore.png)

Это предупреждение появляется, когда переменная среды `PATH` не указывает на любой пакетами SDK .NET Core на компьютере. Для решения этой проблемы:

* Установка или убедитесь, что установлен пакет SDK .NET Core.
* Проверьте `PATH` указывает переменную среды на расположение, установлен пакет SDK. Установщик обычно задает `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a>Использование IHtmlHelper.Partial может привести к взаимоблокировок приложения

В ASP.NET Core 2.1 и более поздних версиях вызов `Html.Partial` приводит к анализатора предупреждения из-за потенциальный риск взаимоблокировки. Является предупреждающее сообщение:

> Использование IHtmlHelper.Partial может привести взаимоблокировок приложений. Рассмотрите возможность использования `<partial>` вспомогательный тег или `IHtmlHelper.PartialAsync`.

Вызовы `@Html.Partial` следует заменить на `@await Html.PartialAsync` или вспомогательных частичного тег `<partial name="_Partial" />`.

::: moniker-end
