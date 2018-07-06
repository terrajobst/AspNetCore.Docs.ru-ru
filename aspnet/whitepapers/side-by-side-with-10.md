---
uid: whitepapers/side-by-side-with-10
title: ASP.NET Side-by-Side выполнения платформы .NET Framework 1.0 и 1.1 | Документация Майкрософт
author: rick-anderson
description: В этом официальном документе описывается, как установить .NET 1.0 и .NET 1.1 на компьютере, позволяя веб-приложению ASP.NET для выполнения на любую версию снижает...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1018845e3d2c6603732bfbbde78f4a9183e49d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808399"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="00006-103">ASP.NET Side-by-Side выполнения платформы .NET Framework 1.0 и 1.1</span><span class="sxs-lookup"><span data-stu-id="00006-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="00006-104">Этот технический документ описывает установку на компьютере, позволяя веб-приложение ASP.NET под управлением любой версии платформы .NET 1.0 и .NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="00006-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="00006-105">Применяется к ASP.NET 1.0 и ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="00006-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="00006-106">В ASP.NET приложения, называются выполняются параллельно, если они установлены на одном компьютере, но используют разные версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00006-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="00006-107">Следующий раздел описывает настройку приложений ASP.NET для выполнения side-by-side и приведены подробные инструкции:</span><span class="sxs-lookup"><span data-stu-id="00006-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="00006-108">Ведение сопоставления веб-приложения для .NET Framework версии 1.0, во время установки</span><span class="sxs-lookup"><span data-stu-id="00006-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="00006-109">Карты веб-приложения к определенной версии платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="00006-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="00006-110">Найти версию .NET Framework, которая использует веб-сайта</span><span class="sxs-lookup"><span data-stu-id="00006-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="00006-111">В большинстве случаев при обновлении компонента или приложения на компьютере более старой версии удаляется и заменяется более новой версии.</span><span class="sxs-lookup"><span data-stu-id="00006-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="00006-112">Если новая версия не совместим с предыдущей версией, это обычно останавливается в другие приложения, использующие компонент или приложение.</span><span class="sxs-lookup"><span data-stu-id="00006-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="00006-113">.NET Framework обеспечивает поддержку side-by-side выполнение, что позволяет несколько версий сборки или приложения, устанавливаемые на одном компьютере, в то же время.</span><span class="sxs-lookup"><span data-stu-id="00006-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="00006-114">Так как одновременно может быть установлено несколько версий, управляемых приложений можно выбрать версию для использования, не влияя на приложения, использующие разные версии.</span><span class="sxs-lookup"><span data-stu-id="00006-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="00006-115">По умолчанию во время установки платформы .NET Framework версии 1.1, все существующие приложения ASP.NET автоматически настраиваются для использования последней версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00006-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="00006-116">Если вы не хотите приложений ASP.NET по умолчанию .NET Framework 1.1, щелкните [здесь](#1) чтобы узнать, как во избежание этого во время установки.</span><span class="sxs-lookup"><span data-stu-id="00006-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="00006-117">Если вы обновите веб-сервера для .NET Framework 1.1 и один или несколько веб-приложений для запуска .NET Framework 1.0, необходимо обновить сопоставления сценария с Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="00006-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="00006-118">Сопоставление скрипта — это механизм, чтобы сопоставить расширение файла .aspx для конкретного веб-приложения, на версию .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00006-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="00006-119">Нажмите кнопку [здесь](#2) Чтобы научиться сопоставлять веб-приложения к определенной версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00006-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="00006-120">Можно использовать диспетчер сведения Интернет или средство регистрации IIS ASP.NET (Aspnet\_regiis.exe) для поиска, какая версия .NET Framework установлена определенного веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="00006-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="00006-121">Нажмите кнопку [здесь](#3) чтобы узнать, как найти версию .NET Framework, которая использует веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="00006-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="00006-122">Импорт при миграции на .NET Framework 1.1, важно, каждая версия платформы .NET Framework использует свой собственный файл Machine.config.</span><span class="sxs-lookup"><span data-stu-id="00006-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="00006-123">Таким образом Если веб-администратора внес изменения в файле Machine.config, эти изменения должны быть перенесены в файле .NET Framework 1.1 Machine.config.</span><span class="sxs-lookup"><span data-stu-id="00006-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="00006-124">Поддержка сопоставления веб-приложения для .NET Framework 1.0, во время установки</span><span class="sxs-lookup"><span data-stu-id="00006-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="00006-125">По умолчанию все существующие приложения ASP.NET автоматически настраиваются во время установки для использования новой версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00006-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="00006-126">С помощью новой версии платформы .NET Framework, приложения могут воспользоваться всеми преимуществами улучшений и новых возможностей, включенных в новом выпуске.</span><span class="sxs-lookup"><span data-stu-id="00006-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="00006-127">В то же время администратору Web, который может обеспечить точный контроль над какие приложения обновлены, можно предотвратить автоматическое сопоставление всех существующих приложений ASP.NET во время установки платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00006-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="00006-128">Чтобы предотвратить автоматическое сопоставление всего приложения ASP.NET до новой версии платформы .NET Framework, администратором можно использовать с устанавливаемой Dotnetfx.exe программы установки.</span><span class="sxs-lookup"><span data-stu-id="00006-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="00006-129">**Чтобы предотвратить общее сопоставление приложения ASP.NET до новой версии**</span><span class="sxs-lookup"><span data-stu-id="00006-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="00006-130">Перейдите к **запустить**.</span><span class="sxs-lookup"><span data-stu-id="00006-130">Go to **Start**.</span></span>
2. <span data-ttu-id="00006-131">Щелкните **запуска**.</span><span class="sxs-lookup"><span data-stu-id="00006-131">Click on **run**.</span></span>
3. <span data-ttu-id="00006-132">Наберите команду **cmd**.</span><span class="sxs-lookup"><span data-stu-id="00006-132">Type **cmd**.</span></span>
4. <span data-ttu-id="00006-133">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="00006-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="00006-134">В командной строке введите следующую команду для запуска установки платформы .NET Framework: **Dotnetfx.exe/c: "Установка/noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="00006-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="00006-135">Нажмите кнопку **Да** в программе установки Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="00006-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="00006-136">Будет запущен процесс установки .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="00006-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="00006-137">Карты веб-приложения к определенной версии платформы .NET Framework</span><span class="sxs-lookup"><span data-stu-id="00006-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="00006-138">Каждая версия платформы .NET Framework включает версию средства регистрации ASP.NET IIS (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="00006-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="00006-139">Это средство позволяет администраторам выполнять веб-приложения в определенной версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00006-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="00006-140">Это упоминается как сопоставление веб-приложения до версии платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="00006-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="00006-141">Администраторы должны выбрать Aspnet\_regiis.exe, соответствующую версии платформы .NET Framework, которая будет связана с веб-приложением.</span><span class="sxs-lookup"><span data-stu-id="00006-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="00006-142">Например, администратор, который хочет, чтобы указать, что веб-сайт использует .NET Framework 1.1 необходимо использовать Aspnet\_regiis.exe, входящий в состав .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="00006-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="00006-143">Aspnet\_regiis.exe для версии 1.0 находится в каталоге:</span><span class="sxs-lookup"><span data-stu-id="00006-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="00006-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="00006-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="00006-145">Aspnet\_regiis.exe для версии 1,1 находится в каталоге:</span><span class="sxs-lookup"><span data-stu-id="00006-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="00006-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="00006-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="00006-147">Aspnet\_regiis.exe предоставляет два варианта для скрипта, сопоставление веб-приложения:</span><span class="sxs-lookup"><span data-stu-id="00006-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="00006-148">**-s** задает сопоставление в пути и в его дочерних каталогов.</span><span class="sxs-lookup"><span data-stu-id="00006-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="00006-149">**-sn** задает сопоставление только путь.</span><span class="sxs-lookup"><span data-stu-id="00006-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="00006-150">Путь определяет веб-приложения IIS метаданных путь, который определен в виде W3SVC/ROOT / {WebSiteNumber} / {приложения\_имя}.</span><span class="sxs-lookup"><span data-stu-id="00006-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="00006-151">Например для веб-приложения, называется порталом, расположенный на веб-узле по умолчанию, путь к метабазе — W3SVC/1/КОРНЕВОЙ и портала.</span><span class="sxs-lookup"><span data-stu-id="00006-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="00006-152">Обратите внимание, что можно также использовать редактор метабазы средство получить путь к метабазе.</span><span class="sxs-lookup"><span data-stu-id="00006-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="00006-153">Это средство можно загрузить на сайте службы поддержки Майкрософт в [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="00006-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="00006-154">Запустите Aspnet\_regiis.exe -s W3SVC/1/КОРНЕВОЙ или портал, чтобы обновить портал IIS сценариев карты и ее subapplication.</span><span class="sxs-lookup"><span data-stu-id="00006-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="00006-155">Запустите Aspnet\_regiis.exe -sn W3SVC/1/КОРНЕВОЙ/портал, чтобы обновить скрипт портала IIS сопоставить, не влияя на приложения на портале? s подкаталоги.</span><span class="sxs-lookup"><span data-stu-id="00006-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="00006-156">Найти версию .NET Framework, использующего веб-приложения</span><span class="sxs-lookup"><span data-stu-id="00006-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="00006-157">Администратор может использовать диспетчер служб Интернета, чтобы найти, какая версия .NET Framework выполняется веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="00006-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="00006-158">Различных версий операционной системы по-разному запустите диспетчер служб Интернета.</span><span class="sxs-lookup"><span data-stu-id="00006-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="00006-159">Чтобы запустить диспетчер service, выполните приведенные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="00006-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="00006-160">**Чтобы запустить диспетчер служб IIS**</span><span class="sxs-lookup"><span data-stu-id="00006-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="00006-161">Перейдите к **запустить**.</span><span class="sxs-lookup"><span data-stu-id="00006-161">Go to **Start**.</span></span>
2. <span data-ttu-id="00006-162">Щелкните **запуска**.</span><span class="sxs-lookup"><span data-stu-id="00006-162">Click on **run**.</span></span>
3. <span data-ttu-id="00006-163">Тип **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="00006-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="00006-164">Из диспетчера служб Интернета, выберите веб-приложения, версия платформы .NET Framework, вы хотите узнать.</span><span class="sxs-lookup"><span data-stu-id="00006-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="00006-165">Щелкните правой кнопкой мыши на веб-приложение и щелкнуть **свойства.**</span><span class="sxs-lookup"><span data-stu-id="00006-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="00006-166">В окне «Свойства» выберите **конфигурации.**</span><span class="sxs-lookup"><span data-stu-id="00006-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="00006-167">В таблице сопоставлений приложения выберите **.aspx**и нажмите кнопку **изменить**.</span><span class="sxs-lookup"><span data-stu-id="00006-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="00006-168">Из **исполняемый файл** текстовое поле, взгляд на каталог версии с помощью прокрутки.</span><span class="sxs-lookup"><span data-stu-id="00006-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="00006-169">Если каталог версии v.1.1.4322, оно сопоставляется .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="00006-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="00006-170">И наоборот Если каталог версии v1.0.3705, оно сопоставляется .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="00006-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
