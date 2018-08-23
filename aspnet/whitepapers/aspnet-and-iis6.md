---
uid: whitepapers/aspnet-and-iis6
title: Запуск ASP.NET 1.1 со службами IIS 6.0 | Документация Майкрософт
author: rick-anderson
description: Хотя Windows Server 2003 включает в себя IIS 6.0 и ASP.NET 1.1, эти компоненты отключены по умолчанию. Этот технический документ описывает способ включения IIS 6.0...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 38cd0abc1e9133b9b86cff6dd2759ce98ac5a115
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828574"
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="42a89-104">Запуск ASP.NET 1.1 со службами IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="42a89-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="42a89-105">Хотя Windows Server 2003 включает в себя IIS 6.0 и ASP.NET 1.1, эти компоненты отключены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="42a89-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="42a89-106">Этот технический документ описывает способ включения IIS 6.0 и ASP.NET 1.1 и рекомендует несколько параметров конфигурации для обеспечения оптимальной производительности из IIS и ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="42a89-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="42a89-107">Применяется к ASP.NET 1.1 и IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="42a89-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="42a89-108">В ASP.NET 1.1 поставляется с Windows Server 2003, который также включает последнюю версию из Internet Information Server (IIS) версии 6.0.</span><span class="sxs-lookup"><span data-stu-id="42a89-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="42a89-109">IIS 6.0 и ASP.NET 1.1 предназначены для тесно интегрированы и ASP.NET теперь по умолчанию для новой модели рабочих процессов IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="42a89-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="42a89-110">В ASP.NET 1.1 не устанавливается по умолчанию</span><span class="sxs-lookup"><span data-stu-id="42a89-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="42a89-111">В отличие от предыдущих версий операционных систем сервера Microsoft Internet Information Server (IIS) не включена по умолчанию. ни ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="42a89-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="42a89-112">Существует два варианта включения IIS:</span><span class="sxs-lookup"><span data-stu-id="42a89-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="42a89-113">Включение мастера настройки сервера IIS, вариант 1 #.</span><span class="sxs-lookup"><span data-stu-id="42a89-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="42a89-114">Windows Server 2003 поставляется новый «мастер настройки сервера» помогут вам правильно настроить сервер в нужном режиме.</span><span class="sxs-lookup"><span data-stu-id="42a89-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="42a89-115">Чтобы запустить мастер - примечание, чтобы запустить мастер, необходимо войти качестве администратора - перейдите к: запуск | Программы | Администрирование и выберите «настройки сервера».</span><span class="sxs-lookup"><span data-stu-id="42a89-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="42a89-116">Один раз выбранного, вы должны увидеть экран «Мастер настройки сервера»:</span><span class="sxs-lookup"><span data-stu-id="42a89-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="42a89-117">Нажмите кнопку "Далее &gt;":</span><span class="sxs-lookup"><span data-stu-id="42a89-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="42a89-118">Нажмите кнопку "Далее &gt;"</span><span class="sxs-lookup"><span data-stu-id="42a89-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="42a89-119">На этом экране необходимо выбрать "сервер приложений (IIS, ASP.NET) как параметры настройки.</span><span class="sxs-lookup"><span data-stu-id="42a89-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="42a89-120">Нажмите кнопку "Далее &gt;".</span><span class="sxs-lookup"><span data-stu-id="42a89-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="42a89-121">После выбора, чтобы настроить сервер как сервер приложений, этот экран появится запрос, должен быть установлен какие дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="42a89-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="42a89-122">Ни один из параметров установлен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="42a89-122">Neither option is selected by default.</span></span> <span data-ttu-id="42a89-123">Чтобы включить ASP.NET автоматически, необходимо выбрать "Включить ASP. NET ".</span><span class="sxs-lookup"><span data-stu-id="42a89-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="42a89-124">Нажмите кнопку "Далее &gt;".</span><span class="sxs-lookup"><span data-stu-id="42a89-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="42a89-125">На этом экране отображается, что параметры должны быть установлены.</span><span class="sxs-lookup"><span data-stu-id="42a89-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="42a89-126">Нажмите кнопку "Далее &gt;".</span><span class="sxs-lookup"><span data-stu-id="42a89-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="42a89-127">Этот экран появляется выбранные параметры используемые во время установки.</span><span class="sxs-lookup"><span data-stu-id="42a89-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="42a89-128">Это нормально для других внешний вид диалогового окна окна отображаются как установлены службы см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="42a89-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="42a89-129">Кроме того, вы может запрос местоположение установочного компакт-диска Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="42a89-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="42a89-130">Нажмите кнопку "Далее &gt;" по завершении процесса.</span><span class="sxs-lookup"><span data-stu-id="42a89-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="42a89-131">Нажмите кнопку «Готово» — Windows Server 2003 теперь настроен для поддержки IIS 6.0 и ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="42a89-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="42a89-132">Включение служб IIS, параметр #2 — Настройка вручную, IIS и ASP.NET</span><span class="sxs-lookup"><span data-stu-id="42a89-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="42a89-133">Если вы не хотите использовать «мастер настройки сервера» можно также установить IIS 6.0 и ASP.NET 1.1, с помощью «Установка и удаление программ» панели управления.</span><span class="sxs-lookup"><span data-stu-id="42a89-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="42a89-134">Сначала откройте панель управления:</span><span class="sxs-lookup"><span data-stu-id="42a89-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="42a89-135">Нажмите кнопку «Добавить/удалить Windows компоненты» откроется «Мастер компонентов Windows»:</span><span class="sxs-lookup"><span data-stu-id="42a89-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="42a89-136">Выделите и проверьте» сервера приложений"и нажмите кнопку «Сведения»?</span><span class="sxs-lookup"><span data-stu-id="42a89-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="42a89-137">Кнопка:</span><span class="sxs-lookup"><span data-stu-id="42a89-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="42a89-138">Чтобы установить ASP.NET, установите флажок "ASP. NET ".</span><span class="sxs-lookup"><span data-stu-id="42a89-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="42a89-139">Нажмите кнопку «ОК», чтобы вернуться в мастер компонентов Windows.</span><span class="sxs-lookup"><span data-stu-id="42a89-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="42a89-140">Нажмите кнопку "Далее &gt;" с помощью мастера компонентов Windows для начала установки:</span><span class="sxs-lookup"><span data-stu-id="42a89-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="42a89-141">Это нормально для других внешний вид диалогового окна окна отображаются как установлены службы см. в разделе.</span><span class="sxs-lookup"><span data-stu-id="42a89-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="42a89-142">Кроме того, вы может запрос местоположение установочного компакт-диска Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="42a89-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="42a89-143">После завершения установки вы увидите на последнем экране мастера компонентов Windows:</span><span class="sxs-lookup"><span data-stu-id="42a89-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="42a89-144">IIS 6.0 и ASP.NET 1.1 теперь настроены и доступны.</span><span class="sxs-lookup"><span data-stu-id="42a89-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="42a89-145">Рекомендуемые параметры</span><span class="sxs-lookup"><span data-stu-id="42a89-145">Recommended Settings</span></span>

<span data-ttu-id="42a89-146">При запуске ASP.NET 1.1 в IIS 6.0 существует несколько параметров конфигурации, которые рекомендуются для обеспечения оптимальной производительности из ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="42a89-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="42a89-147">Настройка ограничения памяти рабочего процесса</span><span class="sxs-lookup"><span data-stu-id="42a89-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="42a89-148">Настройка перезапуск рабочего процесса</span><span class="sxs-lookup"><span data-stu-id="42a89-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="42a89-149">Настройка ограничения памяти рабочего процесса</span><span class="sxs-lookup"><span data-stu-id="42a89-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="42a89-150">По умолчанию IIS 6.0 не устанавливаем ограничения на объем памяти, который разрешается использовать IIS.</span><span class="sxs-lookup"><span data-stu-id="42a89-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="42a89-151">ASP. Функция кэша NET зависит ограничение памяти, поэтому кэш заранее можно удалять неиспользуемые сборки из памяти.</span><span class="sxs-lookup"><span data-stu-id="42a89-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="42a89-152">Рекомендуется настроить память, повторный запуск особенностью IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="42a89-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="42a89-153">Для настройки этого откройте диспетчер служб IIS (Пуск | Программы | Администрирование | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="42a89-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="42a89-154">Когда открыт, разверните папку «Пулы приложений»:</span><span class="sxs-lookup"><span data-stu-id="42a89-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="42a89-155">Для каждого пула приложений:</span><span class="sxs-lookup"><span data-stu-id="42a89-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="42a89-156">Щелкните правой кнопкой мыши пул приложений, например «DefaultAppPool» и выберите «Свойства»:</span><span class="sxs-lookup"><span data-stu-id="42a89-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="42a89-157">Далее следует включить, нажав одну вторичное использование памяти "Максимальный размер используемой памяти (в мегабайтах):".</span><span class="sxs-lookup"><span data-stu-id="42a89-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="42a89-158">Значение не должно быть больше, чем объем физической памяти (не виртуальном) на сервере, хорошее приближение 60% физической памяти, т. е. для сервера с объемом физической памяти выберите 310 512 МБ.</span><span class="sxs-lookup"><span data-stu-id="42a89-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="42a89-159">Кроме того, рекомендуется, максимально превышало 800 МБ при использовании 2 ГБ адресного пространства.</span><span class="sxs-lookup"><span data-stu-id="42a89-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="42a89-160">Если адресное пространство памяти сервера — 3 ГБ, ограничение максимального объема памяти для рабочего процесса может быть равна 1 800 МБ:</span><span class="sxs-lookup"><span data-stu-id="42a89-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="42a89-161">Нажмите кнопку «Применить» и «ОК», чтобы закрыть диалоговое окно свойств.</span><span class="sxs-lookup"><span data-stu-id="42a89-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="42a89-162">Повторите эти действия для всех имеющихся пулов приложений.</span><span class="sxs-lookup"><span data-stu-id="42a89-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="42a89-163">Настройка перезапуска</span><span class="sxs-lookup"><span data-stu-id="42a89-163">Configuring worker recycling</span></span>

<span data-ttu-id="42a89-164">По умолчанию IIS 6.0 настраивается для перезапуска рабочего процесса каждые 29 часов.</span><span class="sxs-lookup"><span data-stu-id="42a89-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="42a89-165">Это немного Агрессивный для приложения, работающего на ASP.NET, и рекомендуется отключить перезапуск автоматического рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="42a89-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="42a89-166">Чтобы отключить перезапуск автоматического рабочего процесса, сначала откройте диспетчер служб IIS (Пуск | Программы | Администрирование | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="42a89-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="42a89-167">Когда открыт, разверните папку «Пулы приложений»:</span><span class="sxs-lookup"><span data-stu-id="42a89-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="42a89-168">Для каждого пула приложений:</span><span class="sxs-lookup"><span data-stu-id="42a89-168">For each application pool:</span></span>

1. <span data-ttu-id="42a89-169">Щелкните правой кнопкой мыши пул приложений, например «DefaultAppPool» и выберите «Свойства»:</span><span class="sxs-lookup"><span data-stu-id="42a89-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="42a89-170">Снимите флажок "Очистка рабочих процессов (в минутах):":</span><span class="sxs-lookup"><span data-stu-id="42a89-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="42a89-171">Нажмите кнопку «Применить» и «ОК», чтобы закрыть диалоговое окно свойств.</span><span class="sxs-lookup"><span data-stu-id="42a89-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="42a89-172">Повторите эти действия для всех имеющихся пулов приложений.</span><span class="sxs-lookup"><span data-stu-id="42a89-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="42a89-173">Предоставляя доступ на запись в файловой системе</span><span class="sxs-lookup"><span data-stu-id="42a89-173">Granting write access to the file system</span></span>

<span data-ttu-id="42a89-174">Если приложению требуется доступ на запись в файловой системе и использовании NTFS, вам потребуется изменить список (Доступом) в папку или файл для предоставления доступа ASP.NET к.</span><span class="sxs-lookup"><span data-stu-id="42a89-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="42a89-175">Например для предоставления ASP.NET доступ на запись к c:\inetpub\wwwroot сначала откройте проводник и перейдите в каталог:</span><span class="sxs-lookup"><span data-stu-id="42a89-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="42a89-176">После этого щелкните правой кнопкой мыши каталог, например «wwwroot» и выберите его свойства.</span><span class="sxs-lookup"><span data-stu-id="42a89-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="42a89-177">Когда откроется диалоговое окно свойств, выберите вкладку «Безопасность»:</span><span class="sxs-lookup"><span data-stu-id="42a89-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="42a89-178">Каталог c:\inetpub\wwwroot\ является специальных тем, что специальной группы IIS 6.0 "IIS\_WPG" уже предоставлено чтения &amp; разрешения Execute, список содержимого папки и чтения.</span><span class="sxs-lookup"><span data-stu-id="42a89-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="42a89-179">Тем не менее чтобы предоставить разрешение на запись, вам потребуется установите флажок Разрешить для записи.</span><span class="sxs-lookup"><span data-stu-id="42a89-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="42a89-180">IIS 6.0 теперь имеет разрешение на запись для этой папки.</span><span class="sxs-lookup"><span data-stu-id="42a89-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="42a89-181">Чтобы предоставить разрешения на запись в другие папки, выполните приведенные ниже шаги - Обратите внимание на то, может потребоваться добавить IIS\_группы WPG, если он еще не существует.</span><span class="sxs-lookup"><span data-stu-id="42a89-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="42a89-182">Предоставление разрешение на запись IIS\_WPG позволит любое приложение ASP.NET для записи в этот каталог.</span><span class="sxs-lookup"><span data-stu-id="42a89-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="42a89-183">Поддержка встроенной проверки подлинности с помощью SQL Server</span><span class="sxs-lookup"><span data-stu-id="42a89-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="42a89-184">Встроенная проверка подлинности позволяет SQL Server, чтобы использовать проверку подлинности Windows NT для проверки учетных записей входа SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42a89-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="42a89-185">Это позволяет пользователю обойти стандартный процесс входа в систему SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42a89-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="42a89-186">При таком подходе сетевой доступ пользователь базы данных SQL Server без указания код отдельных пользователя или пароля, так как SQL Server получает пользователя и пароль из процесса обеспечения безопасности сети Windows NT.</span><span class="sxs-lookup"><span data-stu-id="42a89-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="42a89-187">Выбор встроенной проверки подлинности для приложений ASP.NET является хорошим выбором, так как когда-либо учетные данные не хранятся в строку подключения для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="42a89-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="42a89-188">А строка подключения, используемая для подключения к SQL будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="42a89-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="42a89-189">Эта строка подключения указывает SQL Server использовать учетные данные Windows, приложения, пытающихся получить доступ к SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42a89-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="42a89-190">В случае ASP.NET/IIS 6 это было бы учетной записи в службах IIS\_группы WPG.</span><span class="sxs-lookup"><span data-stu-id="42a89-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="42a89-191">Чтобы включить встроенную проверку подлинности SQL Server и ASP.NET, необходимо сначала убедитесь, что SQL Server настроен для встроенной проверки подлинности или проверки подлинности в смешанном режиме - обратитесь к Администратору, чтобы определить это.</span><span class="sxs-lookup"><span data-stu-id="42a89-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="42a89-192">Если SQL Server находится в одном из этих двух режимов, можно использовать встроенную проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="42a89-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="42a89-193">Откройте SQL Server Enterprise Manager (Пуск | Программы | Microsoft SQL Server | Enterprise Manager), выберите подходящий сервер и разверните папку «безопасность»:</span><span class="sxs-lookup"><span data-stu-id="42a89-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="42a89-194">Если "BUILTINT\IIS\_WPG" группа не указана, щелкните правой кнопкой мыши на именах входа и выберите «Новое имя входа»:</span><span class="sxs-lookup"><span data-stu-id="42a89-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="42a89-195">В "имя:" текстовое поле, либо введите "[имя сервера или домена] \IIS\_WPG" или нажмите кнопку с многоточием для открытия диалогового окна Выбор пользователя или группы Windows NT:</span><span class="sxs-lookup"><span data-stu-id="42a89-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="42a89-196">Установите IIS на текущем компьютере\_WPG группы и нажмите кнопку «Добавить» и "ОК" Чтобы закрыть средство выбора.</span><span class="sxs-lookup"><span data-stu-id="42a89-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="42a89-197">Затем необходимо также задать базу данных по умолчанию и разрешения для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="42a89-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="42a89-198">Чтобы настроить базу данных по умолчанию выберите из раскрывающегося списка, например ниже Northwind установлен:</span><span class="sxs-lookup"><span data-stu-id="42a89-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="42a89-199">Затем щелкните на вкладке "доступ к базе данных":</span><span class="sxs-lookup"><span data-stu-id="42a89-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="42a89-200">Установите флажок "Разрешить" для каждой базы данных, который вы хотите разрешить доступ к.</span><span class="sxs-lookup"><span data-stu-id="42a89-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="42a89-201">Также необходимо будет выбрать роли базы данных, проверка db\_владельца обеспечит имя входа имеет все необходимые разрешения для управления и использования выбранной базы данных.</span><span class="sxs-lookup"><span data-stu-id="42a89-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="42a89-202">Нажмите кнопку ОК, чтобы закрыть диалоговое окно свойств.</span><span class="sxs-lookup"><span data-stu-id="42a89-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="42a89-203">Приложения ASP.NET теперь настроен для поддержки проверки подлинности SQL Server.</span><span class="sxs-lookup"><span data-stu-id="42a89-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="42a89-204">Не запускайте ASP.NET 1.0 в собственном режиме IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="42a89-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="42a89-205">ASP.NET 1.0 на сервере IIS 6.0 поддерживается только в режиме совместимости с IIS 5.</span><span class="sxs-lookup"><span data-stu-id="42a89-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="42a89-206">Для настройки ASP.NET 1.0 для работы в режиме совместимости с IIS 5.0, откройте диспетчер служб Интернета и веб-сайтов щелкните правой кнопкой мыши и выберите Свойства:</span><span class="sxs-lookup"><span data-stu-id="42a89-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="42a89-207">Перейдите на вкладку "службы" и проверьте? Запускать веб-службу в режиме изоляции IIS 5.0?:</span><span class="sxs-lookup"><span data-stu-id="42a89-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
