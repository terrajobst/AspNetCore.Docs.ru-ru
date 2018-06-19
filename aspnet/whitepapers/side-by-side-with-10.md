---
uid: whitepapers/side-by-side-with-10
title: Выполнение .NET Framework 1.0 и 1.1 ASP.NET Side-by-Side | Документы Microsoft
author: rick-anderson
description: В этом техническом документе описывается установка .NET версии 1.0 и .NET 1.1 на компьютере, позволяя веб-приложение ASP.NET для запуска на любой из версий обрамление...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530183"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="7a987-103">ASP.NET Side-by-Side выполнения платформы .NET Framework 1.0 и 1.1</span><span class="sxs-lookup"><span data-stu-id="7a987-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="7a987-104">В этом техническом документе описывается установка на компьютере, что позволяет веб-приложение ASP.NET для запуска на любой из версий платформы .NET версии 1.0 и .NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="7a987-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="7a987-105">Применяется к ASP.NET 1.0 и ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="7a987-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="7a987-106">В ASP.NET приложениях, называются выполняются параллельно, если они установлены на том же компьютере, но используют разные версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7a987-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="7a987-107">Ниже описана настройка приложений ASP.NET для выполнения side-by-side и приводятся подробные инструкции для:</span><span class="sxs-lookup"><span data-stu-id="7a987-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="7a987-108">Ведение сопоставления веб-приложения для .NET Framework версии 1.0, во время установки</span><span class="sxs-lookup"><span data-stu-id="7a987-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="7a987-109">Карты веб-приложения на определенную версию платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="7a987-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="7a987-110">Найти версию .NET Framework, которая использует веб-сайта</span><span class="sxs-lookup"><span data-stu-id="7a987-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="7a987-111">В большинстве случаев при обновлении компонента или приложения на компьютере старая версия удаляется и заменяется более новой версией.</span><span class="sxs-lookup"><span data-stu-id="7a987-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="7a987-112">Если новая версия не совместима с предыдущей версии, это обычно разбивает других приложений, использующих компонента или приложения.</span><span class="sxs-lookup"><span data-stu-id="7a987-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="7a987-113">Платформа .NET Framework поддерживает выполнение side-by-side, что позволяет несколько версий сборки или приложения, которое установлено на одном компьютере одновременно.</span><span class="sxs-lookup"><span data-stu-id="7a987-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="7a987-114">Поскольку одновременно может быть установлено несколько версий, управляемых приложений можно выбрать версию для использования, не влияя на приложения, использующие разные версии.</span><span class="sxs-lookup"><span data-stu-id="7a987-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="7a987-115">По умолчанию во время установки .NET Framework версии 1.1, все существующие приложения ASP.NET автоматически настраиваются для использования последней версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7a987-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="7a987-116">Если приложения ASP.NET по умолчанию для .NET Framework 1.1 не требуется, нажмите кнопку [здесь](#1) чтобы узнать, как избежать этого, во время установки.</span><span class="sxs-lookup"><span data-stu-id="7a987-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="7a987-117">Если вы обновить веб-сервер для .NET Framework 1.1 и веб-приложений для выполнения в .NET Framework версии 1.0, необходимо обновить карты скриптов Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="7a987-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="7a987-118">Сопоставление сценария — это механизм, чтобы сопоставить расширение файла .aspx для конкретного веб-приложения до версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7a987-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="7a987-119">Нажмите кнопку [здесь](#2) научиться карты веб-приложения на определенную версию платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7a987-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="7a987-120">Можно использовать диспетчер сведения Интернет или средство регистрации IIS ASP.NET (Aspnet\_regiis.exe) для поиска, какая версия .NET Framework выполняется конкретного веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="7a987-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="7a987-121">Нажмите кнопку [здесь](#3) чтобы узнать, как найти версию .NET Framework, которая использует веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="7a987-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="7a987-122">Первых импорта при миграции на .NET Framework 1.1, что для каждой версии платформы .NET Framework использует свой собственный файл Machine.config.</span><span class="sxs-lookup"><span data-stu-id="7a987-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="7a987-123">В результате Если веб-администратора внес изменения в файле Machine.config, эти изменения необходимо перенести в файл .NET Framework 1.1 Machine.config.</span><span class="sxs-lookup"><span data-stu-id="7a987-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="7a987-124">Поддержка сопоставления веб-приложения для .NET Framework 1.0 во время установки</span><span class="sxs-lookup"><span data-stu-id="7a987-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="7a987-125">По умолчанию все приложения ASP.NET автоматически настраиваются во время установки, чтобы использовать новую версию платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7a987-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="7a987-126">С помощью более новой версии платформы .NET Framework, приложения могут воспользоваться всеми преимуществами усовершенствования и новые функции, появившиеся в новой версии.</span><span class="sxs-lookup"><span data-stu-id="7a987-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="7a987-127">В то же время веб-администратора, кто может потребоваться детальный контроль над приложения, которые будут обновлены, можно предотвратить автоматическое сопоставление всех существующих приложений ASP.NET во время установки платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7a987-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="7a987-128">Чтобы предотвратить автоматическое сопоставление всего приложения ASP.NET до новой версии платформы .NET Framework, веб-администратора можно использовать с устанавливаемой Dotnetfx.exe программы установки.</span><span class="sxs-lookup"><span data-stu-id="7a987-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="7a987-129">**Чтобы предотвратить общее сопоставление приложения ASP.NET до новой версии**</span><span class="sxs-lookup"><span data-stu-id="7a987-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="7a987-130">Последовательно выберите пункты **запустить**.</span><span class="sxs-lookup"><span data-stu-id="7a987-130">Go to **Start**.</span></span>
2. <span data-ttu-id="7a987-131">Щелкните **запуска**.</span><span class="sxs-lookup"><span data-stu-id="7a987-131">Click on **run**.</span></span>
3. <span data-ttu-id="7a987-132">Наберите команду **cmd**.</span><span class="sxs-lookup"><span data-stu-id="7a987-132">Type **cmd**.</span></span>
4. <span data-ttu-id="7a987-133">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7a987-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="7a987-134">В командной строке введите следующую команду для запуска установки платформы .NET Framework: **Dotnetfx.exe/c: "Установка/noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="7a987-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="7a987-135">Нажмите кнопку **Да** в установки Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="7a987-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="7a987-136">Будет запущен процесс установки .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="7a987-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="7a987-137">Карты веб-приложения на определенную версию платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="7a987-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="7a987-138">Каждой версии платформы .NET Framework включает версию программы регистрации IIS ASP.NET (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="7a987-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="7a987-139">Это средство позволяет администраторам указать, что веб-приложение выполняться в конкретной версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7a987-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="7a987-140">Это называется сопоставление веб-приложения до версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7a987-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="7a987-141">Администраторы должны выбрать Aspnet\_regiis.exe, соответствующую версии платформы .NET Framework, который будет связан с веб-приложением.</span><span class="sxs-lookup"><span data-stu-id="7a987-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="7a987-142">Например, администратор, чтобы указать, что веб-сайт использует .NET Framework 1.1 необходимо использовать Aspnet\_regiis.exe, входящий в состав .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="7a987-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="7a987-143">Aspnet\_regiis.exe для версии 1.0 находится в каталоге:</span><span class="sxs-lookup"><span data-stu-id="7a987-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="7a987-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="7a987-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="7a987-145">Aspnet\_regiis.exe для версии 1,1 находится в каталоге:</span><span class="sxs-lookup"><span data-stu-id="7a987-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="7a987-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="7a987-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="7a987-147">Aspnet\_regiis.exe предоставляет два параметра для сценария, сопоставление веб-приложения:</span><span class="sxs-lookup"><span data-stu-id="7a987-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="7a987-148">**-s** задает карты скриптов в пути и в его дочерних каталогов.</span><span class="sxs-lookup"><span data-stu-id="7a987-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="7a987-149">**-sn** задает сопоставление только по пути.</span><span class="sxs-lookup"><span data-stu-id="7a987-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="7a987-150">Путь определяет веб-приложения IIS метаданных путь, определенный в виде W3SVC/ROOT / {WebSiteNumber} / {приложения\_имя}.</span><span class="sxs-lookup"><span data-stu-id="7a987-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="7a987-151">Например для веб-приложения, называется порталом, расположенные на веб-узле по умолчанию, путь к метабазе — W3SVC/1/КОРНЕВОЙ или портала.</span><span class="sxs-lookup"><span data-stu-id="7a987-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="7a987-152">Обратите внимание, что можно также использовать инструмент, который называется редактор метабазы получить путь к метабазе.</span><span class="sxs-lookup"><span data-stu-id="7a987-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="7a987-153">Это средство можно загрузить с сайта технической поддержки корпорации Майкрософт в [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="7a987-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="7a987-154">Запустите Aspnet\_regiis.exe -s W3SVC/1/КОРНЕВОЙ/портала для обновления на портале служб IIS сценариев карты и ее subapplication.</span><span class="sxs-lookup"><span data-stu-id="7a987-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="7a987-155">Запустите Aspnet\_regiis.exe -sn W3SVC/1/КОРНЕВОЙ/портал для портала IIS скрипт обновления сопоставления, не влияя на приложения на портале? s подкаталоги.</span><span class="sxs-lookup"><span data-stu-id="7a987-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="7a987-156">Найти версию .NET Framework, используя веб-приложения</span><span class="sxs-lookup"><span data-stu-id="7a987-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="7a987-157">Чтобы найти, какая версия .NET Framework выполняется веб-сайта, администратор может использовать диспетчер служб Интернета.</span><span class="sxs-lookup"><span data-stu-id="7a987-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="7a987-158">Версии операционной системы по-разному запустите диспетчер служб Интернета.</span><span class="sxs-lookup"><span data-stu-id="7a987-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="7a987-159">Чтобы запустить диспетчер служб, выполните приведенные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="7a987-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="7a987-160">**Чтобы запустить диспетчер служб IIS**</span><span class="sxs-lookup"><span data-stu-id="7a987-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="7a987-161">Последовательно выберите пункты **запустить**.</span><span class="sxs-lookup"><span data-stu-id="7a987-161">Go to **Start**.</span></span>
2. <span data-ttu-id="7a987-162">Щелкните **запуска**.</span><span class="sxs-lookup"><span data-stu-id="7a987-162">Click on **run**.</span></span>
3. <span data-ttu-id="7a987-163">Тип **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="7a987-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="7a987-164">В диспетчере служб Интернета, выберите веб-приложение, для которого версия платформы .NET Framework вы хотите знать.</span><span class="sxs-lookup"><span data-stu-id="7a987-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="7a987-165">Щелкните правой кнопкой мыши веб-приложение и щелкнуть **свойства.**</span><span class="sxs-lookup"><span data-stu-id="7a987-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="7a987-166">В окне свойств выберите **конфигурации.**</span><span class="sxs-lookup"><span data-stu-id="7a987-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="7a987-167">В таблице сопоставлений приложений выберите **.aspx**и нажмите кнопку **изменить**.</span><span class="sxs-lookup"><span data-stu-id="7a987-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="7a987-168">Из **исполняемый файл** текстовое поле, рассмотрим каталога версии с помощью прокрутки.</span><span class="sxs-lookup"><span data-stu-id="7a987-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="7a987-169">Если каталог версии v.1.1.4322, приложение сопоставляется .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="7a987-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="7a987-170">И наоборот Если каталог версии v1.0.3705, приложение сопоставляется .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="7a987-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
