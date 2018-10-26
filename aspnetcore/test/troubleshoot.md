---
title: Устранение неполадок с проектов ASP.NET Core
author: Rick-Anderson
description: Устранение неполадок при возникновении ошибок и предупреждений в проектах ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: test/troubleshoot
ms.openlocfilehash: 150f2192bb4b6dd0d330fd678d9c5fa0bf31673e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090115"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="7f2c6-103">Устранение неполадок с проектов ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f2c6-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="7f2c6-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="7f2c6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7f2c6-105">Рекомендации по устранению неполадок по следующим ссылкам:</span><span class="sxs-lookup"><span data-stu-id="7f2c6-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="7f2c6-106">Представленная им ndc Прошедшей конференции (Лондон, 2018 г.): Диагностика проблем в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f2c6-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="7f2c6-107">Блог разработчиков ASP.NET: Устранение неполадок производительности ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f2c6-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="7f2c6-108">Пакет SDK для .NET core предупреждения</span><span class="sxs-lookup"><span data-stu-id="7f2c6-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="7f2c6-109">Установлены 32- и 64-разрядные версии пакета SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="7f2c6-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="7f2c6-110">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="7f2c6-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7f2c6-111">Установлены 32- и 64 разрядной версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="7f2c6-112">Только шаблоны из 64-разрядной версии, установленной на "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Снимок экрана диалогового окна OneASP.NET, отображается предупреждающее сообщение](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="7f2c6-114">Это предупреждение появляется, когда 32-(x86) и 64-разрядные (x 64) версии [пакет SDK для .NET Core](https://www.microsoft.com/net/download/all) установлены.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="7f2c6-115">Распространенные причины, которые могут быть установлены обе версии включают:</span><span class="sxs-lookup"><span data-stu-id="7f2c6-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="7f2c6-116">Изначально загрузить установщик пакета SDK для .NET Core, используя 32-разрядном компьютере, но затем скопировали его через и его установки на 64-разрядном компьютере.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="7f2c6-117">Пакет SDK для 32-разрядной .NET Core был установлен другим приложением.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="7f2c6-118">Неправильная версия была загружаются и устанавливаются.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="7f2c6-119">Удалите пакет SDK 32-разрядной .NET Core для, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="7f2c6-120">Удалить из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="7f2c6-121">Если вы понимаете, почему было создано предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="7f2c6-122">Пакет SDK для .NET Core устанавливается в нескольких местах</span><span class="sxs-lookup"><span data-stu-id="7f2c6-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="7f2c6-123">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="7f2c6-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7f2c6-124">Пакет SDK для .NET Core устанавливается в нескольких местах.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="7f2c6-125">Только шаблоны из установленных здесь: "C:\\Program Files\\dotnet\\sdk\\" будет отображаться.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Снимок экрана диалогового окна OneASP.NET, отображается предупреждающее сообщение](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="7f2c6-127">Вы видите это сообщение при наличии по крайней мере по одному разу на пакет SDK для .NET Core в каталоге за пределами *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="7f2c6-128">Обычно это происходит после развертывания пакета SDK для .NET Core на компьютере, с помощью копирования и вставки вместо установщика MSI.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="7f2c6-129">Удалите пакет SDK 32-разрядной .NET Core для, чтобы устранить это предупреждение.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="7f2c6-130">Удалить из **панели управления** > **программы и компоненты** > **удаление или изменение программы**.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="7f2c6-131">Если вы понимаете, почему было создано предупреждение и ее особенности, предупреждение можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="7f2c6-132">Нет пакетов SDK для .NET Core не обнаружены</span><span class="sxs-lookup"><span data-stu-id="7f2c6-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="7f2c6-133">В **новый проект** диалоговое окно для ASP.NET Core, может появиться следующее предупреждение:</span><span class="sxs-lookup"><span data-stu-id="7f2c6-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7f2c6-134">Нет пакетов SDK для .NET Core были обнаружены, убедитесь, что они включаются в переменной среды «PATH».</span><span class="sxs-lookup"><span data-stu-id="7f2c6-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Снимок экрана диалогового окна OneASP.NET, отображается предупреждающее сообщение](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="7f2c6-136">Это предупреждение появляется, когда переменная среды `PATH` не указывает на пакеты SDK для Core любой .NET на компьютере.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="7f2c6-137">Чтобы устранить эту проблему:</span><span class="sxs-lookup"><span data-stu-id="7f2c6-137">To resolve this problem:</span></span>

* <span data-ttu-id="7f2c6-138">Установите или убедитесь, что установлен пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="7f2c6-139">Убедитесь, что `PATH` переменной среды указывает на расположение, в котором установлен пакет SDK.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-139">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="7f2c6-140">Установщик обычно задает `PATH`.</span><span class="sxs-lookup"><span data-stu-id="7f2c6-140">The installer normally sets the `PATH`.</span></span>
