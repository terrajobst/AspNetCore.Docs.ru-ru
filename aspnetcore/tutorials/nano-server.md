---
title: "ASP.NET Core в Nano Server"
author: shirhatti
description: "Узнайте, как взять существующее приложение ASP.NET Core и развернуть его в экземпляре Nano Server с IIS."
keywords: ASP.NET Core, nano server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: 39e9dea5b3cbd43f41f8a9bceb5d5f8eb6adb16d
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="8965d-104">ASP.NET Core с IIS в Nano Server</span><span class="sxs-lookup"><span data-stu-id="8965d-104">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="8965d-105">Автор [Сурабх Ширхатти](https://twitter.com/sshirhatti) (Sourabh Shirhatti)</span><span class="sxs-lookup"><span data-stu-id="8965d-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="8965d-106">В этом учебнике мы возьмем существующее приложение ASP.NET Core и развернем его в экземпляре Nano Server с IIS.</span><span class="sxs-lookup"><span data-stu-id="8965d-106">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="8965d-107">Вступление</span><span class="sxs-lookup"><span data-stu-id="8965d-107">Introduction</span></span>

<span data-ttu-id="8965d-108">Nano Server представляет собой вариант установки в Windows Server 2016, который занимает минимум памяти и отличается более высоким уровнем защиты и обслуживания, чем Server Core или полный сервер.</span><span class="sxs-lookup"><span data-stu-id="8965d-108">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="8965d-109">Дополнительные сведения и ссылки на загрузку 180-дневных ознакомительных версий см. в [официальной документации Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server).</span><span class="sxs-lookup"><span data-stu-id="8965d-109">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="8965d-110">Опробовать Nano Server можно тремя простыми способами.</span><span class="sxs-lookup"><span data-stu-id="8965d-110">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="8965d-111">В учетной записи MS</span><span class="sxs-lookup"><span data-stu-id="8965d-111">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="8965d-112">Загрузите ISO-файл Windows Server 2016 и создайте образ Nano Server.</span><span class="sxs-lookup"><span data-stu-id="8965d-112">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="8965d-113">Загрузите виртуальный жесткий диск Nano Server.</span><span class="sxs-lookup"><span data-stu-id="8965d-113">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="8965d-114">Создайте виртуальную машину в Azure, используя образ Nano Server в коллекции Azure.</span><span class="sxs-lookup"><span data-stu-id="8965d-114">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="8965d-115">Если у вас нет учетной записи Azure, вы можете получить бесплатную пробную версию на 30 дней.</span><span class="sxs-lookup"><span data-stu-id="8965d-115">If you don’t have an Azure account, you can get a free 30-day trial.</span></span>

<span data-ttu-id="8965d-116">В этом учебнике мы будем использовать второй вариант, предварительно собранный виртуальный жесткий диск Nano Server из Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="8965d-116">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="8965d-117">Перед работой с этим учебником вам потребуются [выходные данные публикации](xref:hosting/directory-structure) существующего приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8965d-117">Before proceeding with this tutorial, you will need the [published output](xref:hosting/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="8965d-118">Убедитесь в том, что приложение предназначено для выполнения в **64-разрядном** процессе.</span><span class="sxs-lookup"><span data-stu-id="8965d-118">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="8965d-119">Настройка экземпляра Nano Server</span><span class="sxs-lookup"><span data-stu-id="8965d-119">Setting up the Nano Server instance</span></span>

<span data-ttu-id="8965d-120">[Создайте виртуальную машину с помощью Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) на компьютере разработки, используя загруженный ранее виртуальный жесткий диск.</span><span class="sxs-lookup"><span data-stu-id="8965d-120">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="8965d-121">Перед входом в систему компьютер потребует задать пароль администратора.</span><span class="sxs-lookup"><span data-stu-id="8965d-121">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="8965d-122">В консоли виртуальной машины нажмите клавишу F11, чтобы установить пароль перед первым входом в систему.</span><span class="sxs-lookup"><span data-stu-id="8965d-122">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="8965d-123">Также необходимо проверить IP-адрес новой виртуальной машины либо путем проверки фиксированного IP-адреса DHCP-сервера виртуальной машины, либо по параметрам сетевого подключения агента восстановления Nano Server.</span><span class="sxs-lookup"><span data-stu-id="8965d-123">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="8965d-124">Предположим, что ваша новая виртуальная машина выполняется с локальным адресом IPv4 192.168.1.10.</span><span class="sxs-lookup"><span data-stu-id="8965d-124">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="8965d-125">Теперь вы можете им управлять с помощью удаленного взаимодействия PowerShell, которое представляет собой единственный способ полного администрирования Nano Server.</span><span class="sxs-lookup"><span data-stu-id="8965d-125">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="8965d-126">Подключение к экземпляру Nano Server, с помощью удаленного взаимодействия PowerShell</span><span class="sxs-lookup"><span data-stu-id="8965d-126">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="8965d-127">Откройте окно PowerShell с повышенными правами, чтобы добавить в список `TrustedHosts` удаленный экземпляр Nano Server.</span><span class="sxs-lookup"><span data-stu-id="8965d-127">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="8965d-128">Замените переменную `$nanoServerIpAddress` на правильный IP-адрес.</span><span class="sxs-lookup"><span data-stu-id="8965d-128">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="8965d-129">Добавив экземпляр Nano Server в `TrustedHosts`, вы сможете подключиться к нему с помощью удаленного взаимодействия PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8965d-129">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="8965d-130">После успешного подключения появится следующая строка: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`.</span><span class="sxs-lookup"><span data-stu-id="8965d-130">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="8965d-131">Создание общей папки</span><span class="sxs-lookup"><span data-stu-id="8965d-131">Creating a file share</span></span>

<span data-ttu-id="8965d-132">Создайте общую папку в Nano Server, чтобы скопировать в нее опубликованное приложение.</span><span class="sxs-lookup"><span data-stu-id="8965d-132">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="8965d-133">Выполните в удаленном сеансе следующие команды.</span><span class="sxs-lookup"><span data-stu-id="8965d-133">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="8965d-134">После выполнения указанных выше команд вы сможете получить доступ к этой общей папке, открыв `\\192.168.1.10\AspNetCoreSampleForNano` в проводнике Windows хост-компьютера.</span><span class="sxs-lookup"><span data-stu-id="8965d-134">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="8965d-135">Открытие порта в межсетевом экране</span><span class="sxs-lookup"><span data-stu-id="8965d-135">Open port in the firewall</span></span>

<span data-ttu-id="8965d-136">Выполните в удаленном сеансе следующие команды, чтобы открыть порт в межсетевом экране и позволить IIS прослушивать трафик TCP через порт TCP/8000.</span><span class="sxs-lookup"><span data-stu-id="8965d-136">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="8965d-137">Установка IIS</span><span class="sxs-lookup"><span data-stu-id="8965d-137">Installing IIS</span></span>

<span data-ttu-id="8965d-138">Добавьте поставщик `NanoServerPackage` из коллекции PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8965d-138">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="8965d-139">После того, как поставщик будет установлен и импортирован, можно установить пакеты Windows.</span><span class="sxs-lookup"><span data-stu-id="8965d-139">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="8965d-140">В созданном ранее сеансе PowerShell выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="8965d-140">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="8965d-141">Чтобы быстро проверить, правильно ли настроен IIS, откройте URL-адрес `http://192.168.1.10/` и посмотрите, откроется ли страница приветствия.</span><span class="sxs-lookup"><span data-stu-id="8965d-141">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="8965d-142">При установке IIS по умолчанию создается веб-сайт `Default Web Site`, прослушивающий порт 80.</span><span class="sxs-lookup"><span data-stu-id="8965d-142">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="8965d-143">Установка модуля ASP.NET Core (ANCM)</span><span class="sxs-lookup"><span data-stu-id="8965d-143">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="8965d-144">Модуль Core ASP.NET, IIS — это модуль 7.5 или более новой версии, отвечающий за управление процессами HTTP-прослушивателей ASP.NET Core и пропускание запросов к управляемым процессам через прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="8965d-144">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="8965d-145">На данный момент модули ASP.NET Core для IIS устанавливается вручную.</span><span class="sxs-lookup"><span data-stu-id="8965d-145">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="8965d-146">[Пакет размещения .NET Core Windows Server](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) необходимо установить на обычный компьютер (не Nano).</span><span class="sxs-lookup"><span data-stu-id="8965d-146">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="8965d-147">После установки пакета на обычный компьютер необходимо будет скопировать следующие файлы в общую папку, созданную ранее.</span><span class="sxs-lookup"><span data-stu-id="8965d-147">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="8965d-148">На обычном сервере (не Nano) с IIS выполните следующие команды копирования.</span><span class="sxs-lookup"><span data-stu-id="8965d-148">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="8965d-149">На компьютере Windows 10 замените `C:\windows\system32\inetsrv` на `C:\Program Files\IIS Express`.</span><span class="sxs-lookup"><span data-stu-id="8965d-149">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="8965d-150">На стороне Nano необходимо скопировать из созданной ранее общей папки в допустимые расположения указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="8965d-150">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="8965d-151">Для этого выполните следующие команды копирования.</span><span class="sxs-lookup"><span data-stu-id="8965d-151">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="8965d-152">В удаленном сеансе выполните следующий скрипт.</span><span class="sxs-lookup"><span data-stu-id="8965d-152">Run the following script in the remote session:</span></span>

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> <span data-ttu-id="8965d-153">После выполнения указанного выше шага удалите из общей папки файлы *aspnetcore.dll* и *aspnetcore_schema.xml*.</span><span class="sxs-lookup"><span data-stu-id="8965d-153">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="8965d-154">Установка .NET Core Framework</span><span class="sxs-lookup"><span data-stu-id="8965d-154">Installing .NET Core Framework</span></span>

<span data-ttu-id="8965d-155">При публикации приложения, которое зависит от платформы (переносимого приложения), .NET Core должен быть установлен на конечном компьютере.</span><span class="sxs-lookup"><span data-stu-id="8965d-155">If you published a Framework-dependent (portable) app, .NET Core must be installed on the target machine.</span></span> <span data-ttu-id="8965d-156">Выполните в удаленном сеансе следующий скрипт PowerShell, чтобы установить .NET Framework в Nano Server.</span><span class="sxs-lookup"><span data-stu-id="8965d-156">Execute the following PowerShell script in a remote PowerShell session to install the .NET Framework on your Nano Server.</span></span>

> [!NOTE]
> <span data-ttu-id="8965d-157">Чтобы понять различия между развертываниями, которые зависят от платформы (FDD), и самодостаточными развертываниями (SCD), обратитесь к [параметрам развертывания](https://docs.microsoft.com/dotnet/articles/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="8965d-157">To understand the differences between Framework-dependent deployments (FDD) and Self-contained deployments (SCD), see [deployment options](https://docs.microsoft.com/dotnet/articles/core/deploying/).</span></span>

<span data-ttu-id="8965d-158">[!code-powershell[Main](nano-server/Download-Dotnet.ps1)]</span><span class="sxs-lookup"><span data-stu-id="8965d-158">[!code-powershell[Main](nano-server/Download-Dotnet.ps1)]</span></span>

## <a name="publishing-the-application"></a><span data-ttu-id="8965d-159">Публикация приложения</span><span class="sxs-lookup"><span data-stu-id="8965d-159">Publishing the application</span></span>

<span data-ttu-id="8965d-160">Скопируйте опубликованные выходные данные существующего приложения в корневую общую папку.</span><span class="sxs-lookup"><span data-stu-id="8965d-160">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="8965d-161">Возможно, придется внести изменения в файл *web.config*, чтобы указать, куда был извлечен файл *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="8965d-161">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="8965d-162">Кроме того, можно добавить файл *dotnet.exe* в параметр PATH.</span><span class="sxs-lookup"><span data-stu-id="8965d-162">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="8965d-163">Пример того, как будет выглядеть *web.config*, если *dotnet.exe* **не** входит в параметр PATH:</span><span class="sxs-lookup"><span data-stu-id="8965d-163">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="8965d-164">Выполните в удаленном сеансе следующие команды, чтобы создать для опубликованного приложения сайт в IIS с иным портом, чем у веб-сайта по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8965d-164">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="8965d-165">Необходимо также открыть этот порт для доступа к Интернету.</span><span class="sxs-lookup"><span data-stu-id="8965d-165">You also need to open that port to access the web.</span></span> <span data-ttu-id="8965d-166">Для простоты в этом скрипте используется `DefaultAppPool`.</span><span class="sxs-lookup"><span data-stu-id="8965d-166">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="8965d-167">Дополнительные рекомендации по работе в пуле приложений см. в разделе [Пулы приложений](xref:publishing/iis#application-pools).</span><span class="sxs-lookup"><span data-stu-id="8965d-167">For more considerations on running under an application pool, see [Application Pools](xref:publishing/iis#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="8965d-168">Известная проблема при работе с интерфейсом командной строки .NET Core в Nano Server и ее решение</span><span class="sxs-lookup"><span data-stu-id="8965d-168">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="8965d-169">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="8965d-169">Running the application</span></span>

<span data-ttu-id="8965d-170">Доступ к опубликованному веб-приложению можно получить через браузер по адресу `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="8965d-170">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="8965d-171">Если вы настроили ведение журнала, как описано в разделе [Создание и перенаправление журналов](xref:hosting/aspnet-core-module#log-creation-and-redirection), вы можете просмотреть журналы в папке *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="8965d-171">If you've set up logging as described in [Log creation and redirection](xref:hosting/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
