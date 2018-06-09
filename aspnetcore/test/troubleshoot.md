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
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="57c38-103">Устранение неполадок в проектах ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57c38-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="57c38-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="57c38-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="57c38-105">Рекомендации по устранению неполадок по следующим ссылкам:</span><span class="sxs-lookup"><span data-stu-id="57c38-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="57c38-106">Устранение неполадок ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="57c38-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="57c38-107">Устранение неполадок ASP.NET Core в службах IIS</span><span class="sxs-lookup"><span data-stu-id="57c38-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="57c38-108">Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57c38-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="57c38-109">YouTube: диагностика проблем в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57c38-109">YouTube: Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="57c38-110">Пакет SDK для .NET core предупреждения</span><span class="sxs-lookup"><span data-stu-id="57c38-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="57c38-111">Установлены 32-разрядную и 64-разрядные версии пакета SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="57c38-111">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="57c38-112">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="57c38-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="57c38-114">Это предупреждение появляется, когда (x86) 32-разрядных и 64-разрядный (x 64) версии [пакета SDK для .NET Core](https://www.microsoft.com/net/download/all) установлены.</span><span class="sxs-lookup"><span data-stu-id="57c38-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="57c38-115">Распространенные причины могут быть установлены обе версии включают:</span><span class="sxs-lookup"><span data-stu-id="57c38-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="57c38-116">Первоначально загрузить установщик пакета SDK для .NET Core, используя 32-разрядном компьютере, но затем копируются выполняем и он установлен на 64-разрядном компьютере.</span><span class="sxs-lookup"><span data-stu-id="57c38-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="57c38-117">32-разрядной .NET Core SDK был установлен другим приложением.</span><span class="sxs-lookup"><span data-stu-id="57c38-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="57c38-118">Неправильная версия была загружаются и устанавливаются.</span><span class="sxs-lookup"><span data-stu-id="57c38-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="57c38-119">Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="57c38-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="57c38-120">Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="57c38-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="57c38-121">Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="57c38-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="57c38-122">В нескольких местах установлен пакет SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="57c38-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="57c38-123">В **новый проект** диалоговое окно для ASP.NET Core может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="57c38-123">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

 <span data-ttu-id="57c38-124">В нескольких местах установлен пакет SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="57c38-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="57c38-125">Только шаблоны из пакетах SDK, установлена в "C:\Program Files\dotnet\sdk\' будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="57c38-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="57c38-127">Это сообщение отображается при наличии по крайней мере один установки пакета SDK для .NET Core в каталоге за пределами * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="57c38-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="57c38-128">Обычно это происходит после развертывания пакета SDK .NET Core на компьютере, вместо копирования и вставки установщик MSI.</span><span class="sxs-lookup"><span data-stu-id="57c38-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="57c38-129">Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="57c38-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="57c38-130">Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="57c38-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="57c38-131">Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="57c38-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="57c38-132">Обнаружены не .NET Core пакеты SDK</span><span class="sxs-lookup"><span data-stu-id="57c38-132">No .NET Core SDKs were detected</span></span>
<span data-ttu-id="57c38-133">В **новый проект** диалоговое окно для ASP.NET Core может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="57c38-133">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

<span data-ttu-id="57c38-134">**Обнаружены не .NET Core пакеты SDK, убедитесь, что они включаются в переменной среды «PATH»**</span><span class="sxs-lookup"><span data-stu-id="57c38-134">**No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'**</span></span>

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="57c38-136">Это предупреждение появляется, когда переменная среды `PATH` не указывает на любой пакетами SDK .NET Core на компьютере.</span><span class="sxs-lookup"><span data-stu-id="57c38-136">This warning appears when the environment variable `PATH` doesn’t point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="57c38-137">Для решения этой проблемы:</span><span class="sxs-lookup"><span data-stu-id="57c38-137">To resolve this problem:</span></span>

* <span data-ttu-id="57c38-138">Установка или убедитесь, что установлен пакет SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="57c38-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="57c38-139">Проверьте `PATH` указывает переменную среды на расположение, установлен пакет SDK.</span><span class="sxs-lookup"><span data-stu-id="57c38-139">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="57c38-140">Установщик обычно задает `PATH`.</span><span class="sxs-lookup"><span data-stu-id="57c38-140">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a><span data-ttu-id="57c38-141">Использование IHtmlHelper.Partial может привести к взаимоблокировок приложений</span><span class="sxs-lookup"><span data-stu-id="57c38-141">Use of IHtmlHelper.Partial may result in application deadlocks</span></span>

<span data-ttu-id="57c38-142">В ASP.NET Core 2.1 и более поздних версиях вызов `Html.Partial` приводит к анализатора предупреждения из-за потенциальный риск взаимоблокировки.</span><span class="sxs-lookup"><span data-stu-id="57c38-142">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="57c38-143">Является предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="57c38-143">The warning message is:</span></span>

<span data-ttu-id="57c38-144">*Использование IHtmlHelper.Partial может привести взаимоблокировок приложений. Рассмотрите возможность использования `<partial>` вспомогательный тег или `IHtmlHelper.PartialAsync`.*</span><span class="sxs-lookup"><span data-stu-id="57c38-144">*Use of IHtmlHelper.Partial may result in application deadlocks. Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.*</span></span>

<span data-ttu-id="57c38-145">Вызовы `@Html.Partial` следует заменить на `@await Html.PartialAsync` или вспомогательных частичного тег `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="57c38-145">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
