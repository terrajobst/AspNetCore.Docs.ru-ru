---
uid: whitepapers/aspnet-and-iis6
title: "Запущена среда ASP.NET 1.1 со службами IIS 6.0 | Документы Microsoft"
author: rick-anderson
description: "Хотя Windows Server 2003 включает службы IIS 6.0 и ASP.NET версии 1.1, по умолчанию отключены следующие компоненты. В этом техническом документе описывается включение IIS 6.0..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="82af6-104">Запущена среда ASP.NET 1.1 с IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="82af6-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="82af6-105">Хотя Windows Server 2003 включает службы IIS 6.0 и ASP.NET версии 1.1, по умолчанию отключены следующие компоненты.</span><span class="sxs-lookup"><span data-stu-id="82af6-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="82af6-106">В этом техническом документе описывается включение IIS 6.0 и ASP.NET версии 1.1 и рекомендует несколько параметров конфигурации для получения оптимальной производительности из IIS и ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="82af6-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="82af6-107">Применяется к ASP.NET версии 1.1 и IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="82af6-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="82af6-108">ASP.NET версии 1.1 поставляется вместе с Windows Server 2003, который также включает последнюю версию служб Internet Information Server (IIS) версии 6.0.</span><span class="sxs-lookup"><span data-stu-id="82af6-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="82af6-109">IIS 6.0 и ASP.NET 1.1 предназначены для тесно интегрированы и теперь ASP.NET по умолчанию для новой модели рабочего процесса IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="82af6-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="82af6-110">ASP.NET версии 1.1 не устанавливается по умолчанию</span><span class="sxs-lookup"><span data-stu-id="82af6-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="82af6-111">В отличие от предыдущих версий операционных систем корпорации Майкрософт сервер Internet Information Server (IIS) не включен по умолчанию; также ASP.NET версии 1.1.</span><span class="sxs-lookup"><span data-stu-id="82af6-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="82af6-112">Существует два варианта включения IIS:</span><span class="sxs-lookup"><span data-stu-id="82af6-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="82af6-113">Включение мастера настройки сервера IIS, параметр #1.</span><span class="sxs-lookup"><span data-stu-id="82af6-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="82af6-114">Windows Server 2003 поставляется новый «мастер настройки сервера» для правильной настройки сервера в нужном режиме.</span><span class="sxs-lookup"><span data-stu-id="82af6-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="82af6-115">Чтобы запустить мастер - примечание, чтобы запустить мастер, необходимо войти как администратор - перейдите в: Пуск | Программы | Администрирование и выберите пункт «Настройка сервера».</span><span class="sxs-lookup"><span data-stu-id="82af6-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="82af6-116">Один раз выбранный пользователь видит экран открытия «Мастер настройки сервера»:</span><span class="sxs-lookup"><span data-stu-id="82af6-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="82af6-117">Нажмите кнопку "Далее &gt;":</span><span class="sxs-lookup"><span data-stu-id="82af6-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="82af6-118">Нажмите кнопку "Далее &gt;"</span><span class="sxs-lookup"><span data-stu-id="82af6-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="82af6-119">На этом экране необходимо выбрать "сервер приложений (IIS, ASP.NET) как параметры настройки.</span><span class="sxs-lookup"><span data-stu-id="82af6-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="82af6-120">Нажмите кнопку "Далее &gt;".</span><span class="sxs-lookup"><span data-stu-id="82af6-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="82af6-121">После выбора, чтобы настроить сервер как сервер приложений, этот экран отображается запрос, должны быть установлены какие дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="82af6-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="82af6-122">Ни один из параметров выбран по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="82af6-122">Neither option is selected by default.</span></span> <span data-ttu-id="82af6-123">Чтобы включить ASP.NET автоматически, необходимо выбрать "Включить ASP. NET ".</span><span class="sxs-lookup"><span data-stu-id="82af6-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="82af6-124">Нажмите кнопку "Далее &gt;".</span><span class="sxs-lookup"><span data-stu-id="82af6-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="82af6-125">На экране отображается, какие параметры должны быть установлены.</span><span class="sxs-lookup"><span data-stu-id="82af6-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="82af6-126">Нажмите кнопку "Далее &gt;".</span><span class="sxs-lookup"><span data-stu-id="82af6-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="82af6-127">Этот экран появляется во время установки выбранные параметры.</span><span class="sxs-lookup"><span data-stu-id="82af6-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="82af6-128">Он является нормальным для просмотра других диалоговое окно, поля отображаются как установлены службы.</span><span class="sxs-lookup"><span data-stu-id="82af6-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="82af6-129">Кроме того потребуется расположение установочного компакт-диска Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="82af6-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="82af6-130">Нажмите кнопку "Далее &gt;" после завершения.</span><span class="sxs-lookup"><span data-stu-id="82af6-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="82af6-131">Нажмите кнопку «Готово» — Windows Server 2003 теперь настроен для поддержки IIS 6.0 и ASP.NET версии 1.1.</span><span class="sxs-lookup"><span data-stu-id="82af6-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="82af6-132">Включение служб IIS, параметр #2 — Настройка вручную IIS и ASP.NET</span><span class="sxs-lookup"><span data-stu-id="82af6-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="82af6-133">Если вы не хотите использовать «мастер настройки сервера» можно также установить IIS 6.0 и ASP.NET 1.1, с помощью «Установка и удаление программ» панели управления.</span><span class="sxs-lookup"><span data-stu-id="82af6-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="82af6-134">Сначала откройте панель управления:</span><span class="sxs-lookup"><span data-stu-id="82af6-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="82af6-135">Нажмите кнопку «Установка и удаление компонентов Windows» чего откроется «Мастер компонентов Windows»:</span><span class="sxs-lookup"><span data-stu-id="82af6-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="82af6-136">Выделите и установите флажок «Сервер приложений» и нажмите кнопку «Состав»?</span><span class="sxs-lookup"><span data-stu-id="82af6-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="82af6-137">кнопки:</span><span class="sxs-lookup"><span data-stu-id="82af6-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="82af6-138">Чтобы установить ASP.NET, установите флажок "ASP. NET ".</span><span class="sxs-lookup"><span data-stu-id="82af6-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="82af6-139">Нажмите кнопку «ОК», чтобы вернуться в мастер компонентов Windows.</span><span class="sxs-lookup"><span data-stu-id="82af6-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="82af6-140">Нажмите кнопку "Далее &gt;" в мастере компонентов Windows, чтобы приступить к установке:</span><span class="sxs-lookup"><span data-stu-id="82af6-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="82af6-141">Он является нормальным для просмотра других диалоговое окно, поля отображаются как установлены службы.</span><span class="sxs-lookup"><span data-stu-id="82af6-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="82af6-142">Кроме того потребуется расположение установочного компакт-диска Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="82af6-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="82af6-143">После завершения установки вы увидите на последнем экране мастер компонентов Windows:</span><span class="sxs-lookup"><span data-stu-id="82af6-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="82af6-144">IIS 6.0 и ASP.NET 1.1 будут настроены и доступны.</span><span class="sxs-lookup"><span data-stu-id="82af6-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="82af6-145">Рекомендуемые параметры</span><span class="sxs-lookup"><span data-stu-id="82af6-145">Recommended Settings</span></span>

<span data-ttu-id="82af6-146">При запуске ASP.NET 1.1 в IIS 6.0 существует несколько параметров конфигурации, которые рекомендуются для получения оптимальной производительности из ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="82af6-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="82af6-147">Настройка предельных объемов памяти рабочего процесса</span><span class="sxs-lookup"><span data-stu-id="82af6-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="82af6-148">Настройка перезапуск рабочего процесса</span><span class="sxs-lookup"><span data-stu-id="82af6-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="82af6-149">Настройка предельных объемов памяти рабочего процесса</span><span class="sxs-lookup"><span data-stu-id="82af6-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="82af6-150">По умолчанию службы IIS 6.0 не задано ограничение на объем памяти, который разрешено использовать IIS.</span><span class="sxs-lookup"><span data-stu-id="82af6-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="82af6-151">ASP. Функции кэша в NET зависит ограничение памяти, поэтому кэш заранее можно удалить неиспользуемые элементы из памяти.</span><span class="sxs-lookup"><span data-stu-id="82af6-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="82af6-152">Рекомендуется настроить повторное использование компонентов IIS 6.0 памяти.</span><span class="sxs-lookup"><span data-stu-id="82af6-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="82af6-153">Для настройки этого откройте диспетчер служб IIS (Пуск | Программы | Администрирование | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="82af6-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="82af6-154">Когда открыт, откройте папку «Пулы приложений»:</span><span class="sxs-lookup"><span data-stu-id="82af6-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="82af6-155">Для каждого пула приложений:</span><span class="sxs-lookup"><span data-stu-id="82af6-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="82af6-156">Щелкните правой кнопкой мыши пул приложений, например «DefaultAppPool» и выберите «свойства».</span><span class="sxs-lookup"><span data-stu-id="82af6-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="82af6-157">Затем включите вторичное использование памяти, либо щелкнув "Максимальный размер используемой памяти (в мегабайтах):".</span><span class="sxs-lookup"><span data-stu-id="82af6-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="82af6-158">Значение не должно быть больше, чем объем физической памяти (не виртуальном) на сервере, хорошее приближение составляет 60% физической памяти, т. е. для сервера с 512 МБ физической памяти, выберите 310.</span><span class="sxs-lookup"><span data-stu-id="82af6-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="82af6-159">Также рекомендуется, максимально превышает 800 МБ при использовании 2 ГБ адресного пространства.</span><span class="sxs-lookup"><span data-stu-id="82af6-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="82af6-160">Если адресное пространство памяти сервера: 3 ГБ, ограничение максимального объема памяти для рабочего процесса может быть как 1 800 МБ:</span><span class="sxs-lookup"><span data-stu-id="82af6-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="82af6-161">Нажмите кнопку «Применить» и «ОК», чтобы закрыть диалоговое окно свойств.</span><span class="sxs-lookup"><span data-stu-id="82af6-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="82af6-162">Повторите эти действия для всех доступных пулов приложений.</span><span class="sxs-lookup"><span data-stu-id="82af6-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="82af6-163">Настройка перезапуска</span><span class="sxs-lookup"><span data-stu-id="82af6-163">Configuring worker recycling</span></span>

<span data-ttu-id="82af6-164">По умолчанию службы IIS 6.0 настраивается для перезапуска рабочего процесса каждые 29 часов.</span><span class="sxs-lookup"><span data-stu-id="82af6-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="82af6-165">Это немного Агрессивный приложения выполняется ASP.NET и рекомендуется отключить перезапуск автоматического рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="82af6-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="82af6-166">Чтобы отключить перезапуск автоматического рабочего процесса, сначала откройте диспетчер служб IIS (Пуск | Программы | Администрирование | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="82af6-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="82af6-167">Когда открыт, откройте папку «Пулы приложений»:</span><span class="sxs-lookup"><span data-stu-id="82af6-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="82af6-168">Для каждого пула приложений:</span><span class="sxs-lookup"><span data-stu-id="82af6-168">For each application pool:</span></span>

1. <span data-ttu-id="82af6-169">Щелкните правой кнопкой мыши пул приложений, например «DefaultAppPool» и выберите «свойства».</span><span class="sxs-lookup"><span data-stu-id="82af6-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="82af6-170">Снимите флажок "Очистка рабочих процессов (в минутах):":</span><span class="sxs-lookup"><span data-stu-id="82af6-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="82af6-171">Нажмите кнопку «Применить» и «ОК», чтобы закрыть диалоговое окно свойств.</span><span class="sxs-lookup"><span data-stu-id="82af6-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="82af6-172">Повторите эти действия для всех доступных пулов приложений.</span><span class="sxs-lookup"><span data-stu-id="82af6-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="82af6-173">Предоставляя доступ на запись в файловой системе</span><span class="sxs-lookup"><span data-stu-id="82af6-173">Granting write access to the file system</span></span>

<span data-ttu-id="82af6-174">Если приложению требуется доступ к записи в файловой системе и использовании NTFS необходимо управления список управления Доступом к файлу или папке для предоставления ASP.NET прав на изменение.</span><span class="sxs-lookup"><span data-stu-id="82af6-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="82af6-175">Например для предоставления ASP.NET доступ на запись к c:\inetpub\wwwroot сначала откройте проводник и перейдите в каталог:</span><span class="sxs-lookup"><span data-stu-id="82af6-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="82af6-176">После этого щелкните правой кнопкой мыши каталог, например «wwwroot» и выберите пункт Свойства.</span><span class="sxs-lookup"><span data-stu-id="82af6-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="82af6-177">После открытия диалогового окна свойств перейдите на вкладку «Безопасность»:</span><span class="sxs-lookup"><span data-stu-id="82af6-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="82af6-178">Каталог c:\inetpub\wwwroot\ является специальные в том, что специальная группа IIS 6.0 "IIS\_WPG" уже имеет разрешения на чтение &amp; разрешения Execute, список содержимого папки и чтения.</span><span class="sxs-lookup"><span data-stu-id="82af6-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="82af6-179">Тем не менее чтобы предоставить разрешение на запись, необходимо установите флажок Разрешить для записи:</span><span class="sxs-lookup"><span data-stu-id="82af6-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="82af6-180">IIS 6.0 теперь имеет разрешение на запись в эту папку.</span><span class="sxs-lookup"><span data-stu-id="82af6-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="82af6-181">Чтобы предоставить разрешения на запись в другие папки, необходимо выполнить следующие действия — Обратите внимание, может потребоваться добавить в службах IIS\_группы WPG, если он еще не существует.</span><span class="sxs-lookup"><span data-stu-id="82af6-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="82af6-182">Предоставление разрешение на запись IIS\_WPG позволит любое приложение ASP.NET для записи в этот каталог.</span><span class="sxs-lookup"><span data-stu-id="82af6-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="82af6-183">Поддерживающие встроенную проверку подлинности с помощью SQL Server</span><span class="sxs-lookup"><span data-stu-id="82af6-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="82af6-184">Встроенная проверка подлинности позволяет SQL Server использовать проверку подлинности Windows NT для проверки учетных записей входа SQL Server.</span><span class="sxs-lookup"><span data-stu-id="82af6-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="82af6-185">Это позволяет пользователю обойти стандартный процесс входа в систему SQL Server.</span><span class="sxs-lookup"><span data-stu-id="82af6-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="82af6-186">При таком подходе сетевой доступ пользователь базы данных SQL Server без указания отдельный вход код или пароль, так как SQL Server получает пользователя и пароль из процесс обеспечения безопасности сети Windows NT.</span><span class="sxs-lookup"><span data-stu-id="82af6-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="82af6-187">Выбор встроенной проверки подлинности для приложения ASP.NET очень хорошо подходит, потому что когда-либо учетные данные не хранятся в строке подключения для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="82af6-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="82af6-188">А строка подключения, используемая для подключения к SQL будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="82af6-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="82af6-189">Эта строка подключения указывает SQL Server для использования учетных данных Windows, приложения, пытающихся получить доступ к SQL Server.</span><span class="sxs-lookup"><span data-stu-id="82af6-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="82af6-190">В случае ASP.NET/IIS 6 это будет учетная запись в IIS\_группы WPG.</span><span class="sxs-lookup"><span data-stu-id="82af6-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="82af6-191">Чтобы включить встроенную проверку подлинности SQL Server и ASP.NET, необходимо сначала убедитесь, что SQL Server настроен для встроенной проверки подлинности или проверки подлинности в смешанном режиме - обратитесь к администратору базы данных для определения этого.</span><span class="sxs-lookup"><span data-stu-id="82af6-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="82af6-192">Если SQL Server находится в одном из следующих двух режимов, можно использовать встроенную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="82af6-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="82af6-193">Откройте SQL Server Enterprise Manager (Пуск | Программы | Microsoft SQL Server | Enterprise Manager), выберите подходящий сервер и разверните папку «Безопасность».</span><span class="sxs-lookup"><span data-stu-id="82af6-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="82af6-194">Если "BUILTINT\IIS\_WPG" группа не указана, щелкните правой кнопкой мыши имена входа и выберите «Новое имя входа»:</span><span class="sxs-lookup"><span data-stu-id="82af6-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="82af6-195">В "имя:" текстовое поле, либо ввести "[имя сервера или домена] \IIS\_WPG" или щелкнуть многоточия, чтобы открыть средство выбора пользователя или группы Windows NT:</span><span class="sxs-lookup"><span data-stu-id="82af6-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="82af6-196">Установите IIS на текущем компьютере\_WPG группы и нажмите кнопку «Добавить» и ОК, чтобы закрыть средство выбора.</span><span class="sxs-lookup"><span data-stu-id="82af6-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="82af6-197">Необходимо также задать разрешения для доступа к базе данных и базу данных по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="82af6-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="82af6-198">Чтобы настроить базу данных по умолчанию выберите из раскрывающегося списка, например ниже Northwind установлен:</span><span class="sxs-lookup"><span data-stu-id="82af6-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="82af6-199">Далее щелкните вкладку доступ к базе данных.</span><span class="sxs-lookup"><span data-stu-id="82af6-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="82af6-200">Установите флажок для каждой базы данных, который вы хотите разрешить доступ к разрешения.</span><span class="sxs-lookup"><span data-stu-id="82af6-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="82af6-201">Также необходимо будет выбрать роли базы данных, проверка db\_владельца обеспечит ваше имя входа имеет все необходимые разрешения для управления и использование выбранной базы данных.</span><span class="sxs-lookup"><span data-stu-id="82af6-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="82af6-202">Нажмите кнопку ОК, чтобы закрыть диалоговое окно свойств.</span><span class="sxs-lookup"><span data-stu-id="82af6-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="82af6-203">Приложение ASP.NET теперь настроен для поддержки проверки подлинности SQL Server.</span><span class="sxs-lookup"><span data-stu-id="82af6-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="82af6-204">Не запускайте ASP.NET 1.0 в собственном режиме IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="82af6-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="82af6-205">ASP.NET 1.0 в службах IIS 6.0 поддерживается только в режиме совместимости с IIS 5.</span><span class="sxs-lookup"><span data-stu-id="82af6-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="82af6-206">Чтобы настроить ASP.NET 1.0 для запуска в режиме совместимости с IIS 5.0, откройте диспетчер служб Интернета и щелкните правой кнопкой мыши веб-сайтов и выберите Свойства:</span><span class="sxs-lookup"><span data-stu-id="82af6-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="82af6-207">Перейдите на вкладку "службы" и проверьте? Запускать веб-службу в режиме изоляции IIS 5.0?:</span><span class="sxs-lookup"><span data-stu-id="82af6-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
