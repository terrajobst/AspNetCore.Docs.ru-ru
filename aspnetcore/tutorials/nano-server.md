---
title: "ASP.NET Core в Nano Server"
author: shirhatti
description: "Узнайте, как взять существующее приложение ASP.NET Core и развернуть его в экземпляре Nano Server с IIS."
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: d9b55fb42088b447451326b7ee573d9bfa5f5941
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>ASP.NET Core с IIS в Nano Server

Автор [Сурабх Ширхатти](https://twitter.com/sshirhatti) (Sourabh Shirhatti)

В этом учебнике мы возьмем существующее приложение ASP.NET Core и развернем его в экземпляре Nano Server с IIS.

## <a name="introduction"></a>Вступление

Nano Server представляет собой вариант установки в Windows Server 2016, который занимает минимум памяти и отличается более высоким уровнем защиты и обслуживания, чем Server Core или полный сервер. Дополнительные сведения и ссылки на загрузку 180-дневных ознакомительных версий см. в [официальной документации Nano Server](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server). 

Опробовать Nano Server можно тремя простыми способами. В учетной записи MS

1. Загрузите ISO-файл Windows Server 2016 и создайте образ Nano Server.

2. Загрузите виртуальный жесткий диск Nano Server.

3. Создайте виртуальную машину в Azure, используя образ Nano Server в коллекции Azure. Если у вас нет учетной записи Azure, вы можете получить бесплатную пробную версию на 30 дней.

В этом учебнике мы будем использовать второй вариант, предварительно собранный виртуальный жесткий диск Nano Server из Windows Server 2016.

Перед работой с этим учебником вам потребуются [выходные данные публикации](xref:host-and-deploy/directory-structure) существующего приложения ASP.NET Core. Убедитесь в том, что приложение предназначено для выполнения в **64-разрядном** процессе.

## <a name="setting-up-the-nano-server-instance"></a>Настройка экземпляра Nano Server

[Создайте виртуальную машину с помощью Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) на компьютере разработки, используя загруженный ранее виртуальный жесткий диск. Перед входом в систему компьютер потребует задать пароль администратора. В консоли виртуальной машины нажмите клавишу F11, чтобы установить пароль перед первым входом в систему. Также необходимо проверить IP-адрес новой виртуальной машины либо путем проверки фиксированного IP-адреса DHCP-сервера виртуальной машины, либо по параметрам сетевого подключения агента восстановления Nano Server.

> [!NOTE]
> Предположим, что ваша новая виртуальная машина выполняется с локальным адресом IPv4 192.168.1.10.

Теперь вы можете им управлять с помощью удаленного взаимодействия PowerShell, которое представляет собой единственный способ полного администрирования Nano Server.

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>Подключение к экземпляру Nano Server, с помощью удаленного взаимодействия PowerShell

Откройте окно PowerShell с повышенными правами, чтобы добавить в список `TrustedHosts` удаленный экземпляр Nano Server.

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> Замените переменную `$nanoServerIpAddress` на правильный IP-адрес.

Добавив экземпляр Nano Server в `TrustedHosts`, вы сможете подключиться к нему с помощью удаленного взаимодействия PowerShell.

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

После успешного подключения появится следующая строка: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`.

## <a name="creating-a-file-share"></a>Создание общей папки

Создайте общую папку в Nano Server, чтобы скопировать в нее опубликованное приложение. Выполните в удаленном сеансе следующие команды.

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

После выполнения указанных выше команд вы сможете получить доступ к этой общей папке, открыв `\\192.168.1.10\AspNetCoreSampleForNano` в проводнике Windows хост-компьютера.

## <a name="open-port-in-the-firewall"></a>Открытие порта в межсетевом экране

Выполните в удаленном сеансе следующие команды, чтобы открыть порт в межсетевом экране и позволить IIS прослушивать трафик TCP через порт TCP/8000.

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>Установка IIS

Добавьте поставщик `NanoServerPackage` из коллекции PowerShell. После того, как поставщик будет установлен и импортирован, можно установить пакеты Windows.

В созданном ранее сеансе PowerShell выполните следующие команды.

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

Чтобы быстро проверить, правильно ли настроен IIS, откройте URL-адрес `http://192.168.1.10/` и посмотрите, откроется ли страница приветствия. При установке IIS по умолчанию создается веб-сайт `Default Web Site`, прослушивающий порт 80.

## <a name="installing-the-aspnet-core-module-ancm"></a>Установка модуля ASP.NET Core (ANCM)

Модуль Core ASP.NET, IIS — это модуль 7.5 или более новой версии, отвечающий за управление процессами HTTP-прослушивателей ASP.NET Core и пропускание запросов к управляемым процессам через прокси-сервер. На данный момент модули ASP.NET Core для IIS устанавливается вручную. [Пакет размещения .NET Core Windows Server](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) необходимо установить на обычный компьютер (не Nano). После установки пакета на обычный компьютер необходимо будет скопировать следующие файлы в общую папку, созданную ранее.

На обычном сервере (не Nano) с IIS выполните следующие команды копирования.

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

На компьютере Windows 10 замените `C:\windows\system32\inetsrv` на `C:\Program Files\IIS Express`.

На стороне Nano необходимо скопировать из созданной ранее общей папки в допустимые расположения указанные ниже файлы. Для этого выполните следующие команды копирования.

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

В удаленном сеансе выполните следующий скрипт.

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
> После выполнения указанного выше шага удалите из общей папки файлы *aspnetcore.dll* и *aspnetcore_schema.xml*.

## <a name="installing-net-core-framework"></a>Установка .NET Core Framework

Если приложение публикуется как [развертывание, зависящее от платформы (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), нужно установить .NET Core на сервере. Используйте [PowerShell-скрипт dotnet-install.ps1](https://dot.net/v1/dotnet-install.ps1) в удаленном сеансе PowerShell и установите .NET Core на Nano Server. Версию для интерфейса командной строки передайте с параметром `-Version`:

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>Публикация приложения

Скопируйте опубликованные выходные данные существующего приложения в корневую общую папку.

Возможно, придется внести изменения в файл *web.config*, чтобы указать, куда был извлечен файл *dotnet.exe*. Кроме того, можно добавить файл *dotnet.exe* в параметр PATH.

Пример того, как будет выглядеть *web.config*, если *dotnet.exe* **не** входит в параметр PATH:

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

Выполните в удаленном сеансе следующие команды, чтобы создать для опубликованного приложения сайт в IIS с иным портом, чем у веб-сайта по умолчанию. Необходимо также открыть этот порт для доступа к Интернету. Для простоты в этом скрипте используется `DefaultAppPool`. Дополнительные рекомендации по работе в пуле приложений см. в разделе [Пулы приложений](xref:host-and-deploy/iis/index#application-pools).

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>Известная проблема при работе с интерфейсом командной строки .NET Core в Nano Server и ее решение
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>Запуск приложения

Доступ к опубликованному веб-приложению можно получить через браузер по адресу `http://192.168.1.10:8000`. Если вы настроили ведение журнала, как описано в разделе [Создание и перенаправление журналов](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), вы можете просмотреть журналы в папке *C:\PublishedApps\AspNetCoreSampleForNano\logs*.
