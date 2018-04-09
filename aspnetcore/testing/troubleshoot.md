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
uid: testing/troubleshoot
ms.openlocfilehash: 98077081409949db14b19c7934bc162990ffc302
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="154dd-103">Устранение неполадок в проектах ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="154dd-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="154dd-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="154dd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="154dd-105">Рекомендации по устранению неполадок по следующим ссылкам:</span><span class="sxs-lookup"><span data-stu-id="154dd-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="154dd-106">Устранение неполадок ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="154dd-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="154dd-107">Устранение неполадок ASP.NET Core в службах IIS</span><span class="sxs-lookup"><span data-stu-id="154dd-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="154dd-108">Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="154dd-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="154dd-109">Пакет SDK для .NET core предупреждения</span><span class="sxs-lookup"><span data-stu-id="154dd-109">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="154dd-110">32- и 64 бит установлены обе версии пакета SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="154dd-110">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="154dd-111">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение отображается в верхней:</span><span class="sxs-lookup"><span data-stu-id="154dd-111">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="154dd-113">Это предупреждение появляется, когда (x86) 32-разрядных и 64-разрядный (x 64) версии [пакета SDK для .NET Core](https://www.microsoft.com/net/download/all) установлены.</span><span class="sxs-lookup"><span data-stu-id="154dd-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="154dd-114">Распространенные причины могут быть установлены обе версии включают:</span><span class="sxs-lookup"><span data-stu-id="154dd-114">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="154dd-115">Первоначально загрузить установщик пакета SDK для .NET Core, используя 32-разрядном компьютере, но затем копируются выполняем и он установлен на 64-разрядном компьютере.</span><span class="sxs-lookup"><span data-stu-id="154dd-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="154dd-116">32-разрядной .NET Core SDK был установлен другим приложением.</span><span class="sxs-lookup"><span data-stu-id="154dd-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="154dd-117">Неправильная версия была загружаются и устанавливаются.</span><span class="sxs-lookup"><span data-stu-id="154dd-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="154dd-118">Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="154dd-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="154dd-119">Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="154dd-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="154dd-120">Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="154dd-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="154dd-121">В нескольких местах установлен пакет SDK .NET Core</span><span class="sxs-lookup"><span data-stu-id="154dd-121">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="154dd-122">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение отображается в верхней:</span><span class="sxs-lookup"><span data-stu-id="154dd-122">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="154dd-123">В нескольких местах установлен пакет SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="154dd-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="154dd-124">Только шаблоны из пакетах SDK, установлена в "C:\Program Files\dotnet\sdk\' будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="154dd-124">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Снимок экрана OneASP.NET диалоговое окно, показывающее предупреждающее сообщение](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="154dd-126">Вы видите это сообщение, так как имеется по крайней мере один экземпляр .NET Core SDK в каталоге за пределами * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="154dd-126">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="154dd-127">Обычно это происходит после развертывания пакета SDK .NET Core на компьютере, вместо копирования и вставки установщик MSI.</span><span class="sxs-lookup"><span data-stu-id="154dd-127">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="154dd-128">Удаление 32-разрядных .NET Core SDK, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="154dd-128">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="154dd-129">Удалите из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="154dd-129">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="154dd-130">Если понять, почему возникло предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="154dd-130">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>
