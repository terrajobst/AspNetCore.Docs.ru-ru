---
title: Устранение неполадок в проектах ASP.NET Core
author: Rick-Anderson
description: Обзор и диагностика предупреждений и ошибок в проектах ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 8ff8ddaf4a35a02db650ff429ffbbf4e76a53ecf
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613009"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="322b3-103">Устранение неполадок в проектах ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="322b3-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="322b3-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="322b3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="322b3-105">Рекомендации по устранению неполадок по следующим ссылкам:</span><span class="sxs-lookup"><span data-stu-id="322b3-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="322b3-106">Устранение неполадок ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="322b3-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="322b3-107">Устранение неполадок ASP.NET Core в службах IIS</span><span class="sxs-lookup"><span data-stu-id="322b3-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="322b3-108">Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="322b3-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="322b3-109">Конференция NDC (Лондон, 2018): Диагностике неполадок в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="322b3-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="322b3-110">Блог разработчиков ASP.NET: Устранение неполадок производительности ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="322b3-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="322b3-111">Пакет SDK для .NET core предупреждения</span><span class="sxs-lookup"><span data-stu-id="322b3-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="322b3-112">Установлены 32-разрядную и 64-разрядные версии пакета SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="322b3-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="322b3-113">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="322b3-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="322b3-114">Установлены 32- и 64 бит версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="322b3-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="322b3-115">Только шаблоны из 64-разрядной версии, установленной на "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="322b3-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="322b3-117">Это предупреждение появляется, когда (x86) 32-разрядных и 64-разрядный (x 64) версии [пакета SDK для .NET Core](https://www.microsoft.com/net/download/all) установлены.</span><span class="sxs-lookup"><span data-stu-id="322b3-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="322b3-118">Распространенных причин, которые могут быть установлены обе версии включают:</span><span class="sxs-lookup"><span data-stu-id="322b3-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="322b3-119">Первоначально загрузить установщик пакета SDK для .NET Core, используя 32-разрядном компьютере, но затем скопировать выполняем и он установлен на 64-разрядном компьютере.</span><span class="sxs-lookup"><span data-stu-id="322b3-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="322b3-120">32-разрядной .NET Core SDK был установлен другим приложением.</span><span class="sxs-lookup"><span data-stu-id="322b3-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="322b3-121">Неправильная версия была загружаются и устанавливаются.</span><span class="sxs-lookup"><span data-stu-id="322b3-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="322b3-122">Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="322b3-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="322b3-123">Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="322b3-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="322b3-124">Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="322b3-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="322b3-125">В нескольких местах установлен пакет SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="322b3-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="322b3-126">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="322b3-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="322b3-127">В нескольких местах установлен пакет SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="322b3-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="322b3-128">Только шаблоны из пакетах SDK, установленного на "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="322b3-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="322b3-130">Это сообщение отображается при наличии по крайней мере один установки пакета SDK для .NET Core в каталоге за пределами *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="322b3-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="322b3-131">Обычно это происходит, когда на компьютере, вместо копирования и вставки установщик MSI был развернут пакет SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="322b3-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="322b3-132">Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="322b3-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="322b3-133">Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="322b3-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="322b3-134">Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="322b3-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="322b3-135">Обнаружены не .NET Core пакеты SDK</span><span class="sxs-lookup"><span data-stu-id="322b3-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="322b3-136">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="322b3-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="322b3-137">Обнаружены не .NET Core пакеты SDK, убедитесь, что они включаются в переменной среды «PATH».</span><span class="sxs-lookup"><span data-stu-id="322b3-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="322b3-139">Это предупреждение появляется, когда переменная среды `PATH` не указывает на любой пакетами SDK .NET Core на компьютере.</span><span class="sxs-lookup"><span data-stu-id="322b3-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="322b3-140">Для решения этой проблемы:</span><span class="sxs-lookup"><span data-stu-id="322b3-140">To resolve this problem:</span></span>

* <span data-ttu-id="322b3-141">Установка или убедитесь, что установлен пакет SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="322b3-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="322b3-142">Проверьте `PATH` указывает переменную среды на расположение, установлен пакет SDK.</span><span class="sxs-lookup"><span data-stu-id="322b3-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="322b3-143">Установщик обычно задает `PATH`.</span><span class="sxs-lookup"><span data-stu-id="322b3-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="322b3-144">Использование IHtmlHelper.Partial может привести к взаимоблокировок приложения</span><span class="sxs-lookup"><span data-stu-id="322b3-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="322b3-145">В ASP.NET Core 2.1 и более поздних версиях вызов `Html.Partial` приводит к анализатора предупреждения из-за потенциальный риск взаимоблокировки.</span><span class="sxs-lookup"><span data-stu-id="322b3-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="322b3-146">Является предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="322b3-146">The warning message is:</span></span>

> <span data-ttu-id="322b3-147">Использование IHtmlHelper.Partial может привести взаимоблокировок приложений.</span><span class="sxs-lookup"><span data-stu-id="322b3-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="322b3-148">Рассмотрите возможность использования `<partial>` вспомогательный тег или `IHtmlHelper.PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="322b3-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="322b3-149">Вызовы `@Html.Partial` следует заменить на `@await Html.PartialAsync` или вспомогательных частичного тег `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="322b3-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
