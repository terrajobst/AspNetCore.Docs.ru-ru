---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Запуск скриптов Windows PowerShell из файлов проекта MSBuild | Документация Майкрософт
author: jrjlee
description: В этом разделе описывается, как для выполнения сценария Windows PowerShell как часть процесса построения и развертывания. Сценарий можно запустить локально (другими словами, на б...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362899"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Запуск скриптов Windows PowerShell из файлов проекта MSBuild
====================
по [Джейсон Lee](https://github.com/jrjlee)

[Загрузить PDF-файл](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> В этом разделе описывается, как для выполнения сценария Windows PowerShell как часть процесса построения и развертывания. Можно запустить сценарий локально (другими словами, на сервере сборки) или удаленно, например на веб-сервере назначения или сервер базы данных.
> 
> Существует множество причин, почему может потребоваться выполнить скрипт Windows PowerShell после развертывания. Например, можно сделать следующее:
> 
> - Добавьте источник пользовательское событие в реестр.
> - Создайте каталог в файловой системе для передачи файлов.
> - Очистите каталогов сборки.
> - Записи в файл пользовательского журнала.
> - Отправка сообщения электронной почты приглашение пользователей подготовленные веб-приложения.
> - Создайте учетные записи с соответствующими разрешениями.
> - Настройка репликации между экземплярами SQL Server.
> 
> В этом разделе показано, как запустить сценарии Windows PowerShell, как локально, так и удаленно из пользовательский целевой объект в файл проекта Microsoft Build Engine (MSBuild).


Этот раздел является частью серии учебников, исходя из требования к развертыванию enterprise вымышленной компании Fabrikam, Inc. В этой серии руководств используется пример решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с более реалистичные уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.

Метод развертывания в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения управляется двумя файлы проекта&#x2014;один с Создание инструкции, которые применяются для каждой целевой среде и содержащего параметры построения и развертывания для конкретной среды. Во время сборки файл проекта для конкретной среды объединяется в файл проекта независимой от среды, чтобы полный набор инструкций построения.

## <a name="task-overview"></a>Общие сведения о задачах

Чтобы запустить сценарий Windows PowerShell как часть процесса автоматической или один шаг развертывания, необходимо выполнить следующие высокоуровневые задачи:

- Добавьте скрипт Windows PowerShell, в решение и в систему управления версиями.
- Создайте команду, которая вызывает сценарий Windows PowerShell.
- Экранировать все зарезервированные символы XML, в команде.
- Создание целевого объекта в пользовательский файл проекта MSBuild и использовать **Exec** задачу для выполнения команды.

В этом разделе будет показано, как для выполнения этих процедур. Задачи и пошаговые руководства в этом разделе предполагается, что вы уже знакомы с целевые объекты MSBuild и свойства, и понять, как использовать пользовательский файл проекта MSBuild для управления процессом построения и развертывания. Дополнительные сведения см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Создание и Добавление скриптов Windows PowerShell

Задачи в этом разделе используется пример сценария Windows PowerShell с именем **LogDeploy.ps1** Чтобы проиллюстрировать процесс для выполнения скриптов с MSBuild. **LogDeploy.ps1** скрипт содержит простой функции, которая производит запись в одну строку в файл журнала:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


**LogDeploy.ps1** скрипт принимает два параметра. Первый параметр представляет полный путь к файлу журнала, к которому требуется добавить запись, а второй параметр представляет назначение развертывания, который требуется записать в файл журнала. При выполнении сценарий, он добавляет строку в файл журнала в следующем формате:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Чтобы сделать **LogDeploy.ps1** скрипт для MSBuild, вам потребуется:

- Добавьте скрипт в систему управления версиями.
- Добавьте скрипт в решение в Visual Studio 2010.

Не нужно развернуть скрипт на ваше содержимое решения, независимо от того, планируете ли вы запустить скрипт на сервере сборки, или на удаленном компьютере. Один из вариантов — в том, чтобы добавить скрипт в папку решения. В примере диспетчера контактов так как вы хотите использовать скрипт Windows PowerShell как часть процесса развертывания, имеет смысл необходимо добавить скрипт в папку публикации решения.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Содержимое папки решений копируются серверов сборки в качестве исходного материала. Тем не менее они образуют нет никаких выходных данных проекта.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Выполнение скрипта Windows PowerShell на сервере сборки

В некоторых сценариях может потребоваться запускать сценарии Windows PowerShell на компьютере, который создает проекты. Например можно использовать сценарий Windows PowerShell для очистки папки сборки или записи в файл пользовательского журнала.

С точки зрения синтаксиса выполнив сценарий Windows PowerShell из файла проекта MSBuild совпадает со значением сценария Windows PowerShell из регулярного командной строки. Необходимо вызвать исполняемый файл powershell.exe и использовать **— команда** коммутатору для обеспечения команды Windows PowerShell для запуска. (В Windows PowerShell версии 2, можно также использовать **— файл** переключаться). Команду следует выполнить этот формат:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Пример:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Если путь к сценарию содержит пробелы, необходимо заключить в одинарные кавычки, следующий за знаком амперсанда путь к файлу. Нельзя использовать двойные кавычки, так как вы уже использовали их в нужно вложить команду:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


При вызове этой команды из MSBuild, существует несколько дополнительных факторов. Во-первых, следует включить **— NonInteractive** флажок, чтобы гарантировать, что сценарий выполняется без вмешательства пользователя. Далее следует включить **-ExecutionPolicy** флаг со значением соответствующего аргумента. Указывает политику выполнения, что Windows PowerShell будет применяться к сценарию и позволяет переопределить политику выполнения по умолчанию, которые могут препятствовать выполнение скрипта. Можно выбрать из значения аргументов:

- Значение **Unrestricted** позволит Windows PowerShell для выполнения скрипта, независимо от того, подписан ли сценарий.
- Значение **RemoteSigned** позволит Windows PowerShell для выполнения неподписанных скриптов, которые были созданы на локальном компьютере. Тем не менее должны быть подписаны скрипты, которые были созданы в другом месте. (На практике, вы скорее всего, для создания сценария Windows PowerShell локально на сервере сборки).
- Значение **AllSigned** позволит Windows PowerShell для выполнения только подписанные сценарии.

Политика выполнения по умолчанию — **Restricted**, который предотвращает запуск любых файлов скрипта Windows PowerShell.

Наконец вам потребуется экранировать все зарезервированные символы XML, которые происходят в вашей команды Windows PowerShell.

- Замените одинарные кавычки с  **&amp;apos;**
- Замените двойные кавычки с  **&amp;quot;**
- Замените амперсанды с  **&amp;amp;**

- При внесении этих изменений, команда будет выглядеть примерно следующим образом:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


В рамках пользовательского файла проекта MSBuild, можно создать новую цель и использовать **Exec** задачи для выполнения этой команды:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


В этом примере обратите внимание, что:

- Переменные, такие как значения параметров и расположение исполняемого файла Windows PowerShell, обозначаются как свойства MSBuild.
- Условия включены для Разрешите пользователям переопределять эти значения из командной строки.
- **MSDeployComputerName** свойство уже объявленного в файле проекта.

При выполнении данного целевого объекта как часть процесса построения, Windows PowerShell выполнения команды и записи в журнал событий в указанный файл.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Выполнение скрипта Windows PowerShell на удаленном компьютере

Windows PowerShell можно запускать скрипты на удаленных компьютерах с помощью [удаленное управление Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Чтобы сделать это, необходимо использовать [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) командлета. Это дает возможность выполнения скрипта для одного или нескольких удаленных компьютеров без копирования скрипт на удаленных компьютерах. Результаты возвращаются на локальный компьютер, из которого запускается скрипт.

> [!NOTE]
> Прежде чем использовать **Invoke-Command** командлета Windows PowerShell выполнить скрипты на удаленном компьютере, необходимо настроить прослушиватель WinRM для принятия удаленных сообщений. Это можно сделать с помощью команды **winrm quickconfig** на удаленном компьютере. Дополнительные сведения см. в разделе [Установка и настройка Windows удаленного управления](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


В окне Windows PowerShell, необходимо использовать этот синтаксис для запуска **LogDeploy.ps1** скрипт на удаленном компьютере:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Существует несколько способов использования **Invoke-Command** для выполнения сценария файл, но этот подход является самым простым для предоставления значений параметров и управления ими путей с пробелами.


При запуске из командной строки, необходимо вызвать исполняемый файл Windows PowerShell и используйте **— команда** параметр для предоставления собственных инструкций:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Как ранее, необходимо предоставить некоторые дополнительные параметры и экранировать любые зарезервированные символы XML, при выполнении команды из MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Наконец, как и раньше, вы можете использовать **Exec** задачу в пользовательский целевой объект MSBuild для выполнения команды:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


При выполнении данного целевого объекта как часть процесса построения, Windows PowerShell будет выполняться сценарий на компьютере, указанном в **– computername** аргумент.

## <a name="conclusion"></a>Заключение

В этом разделе описано, как запустить сценарий Windows PowerShell из файла проекта MSBuild. Этот подход можно использовать для выполнения сценария Windows PowerShell, локально или на удаленном компьютере, как часть автоматической или один шаг процесса построения и развертывания.

## <a name="further-reading"></a>Дополнительные сведения

Рекомендации о подписи скриптов Windows PowerShell и управление политиками выполнения, см. в разделе [запуск скриптов Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Рекомендации по выполнение команд Windows PowerShell с удаленного компьютера, см. в разделе [выполнение удаленных команд](https://technet.microsoft.com/library/dd819505.aspx).

Дополнительные сведения об использовании настраиваемых файлов проекта MSBuild для управления процессом развертывания, см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Назад](taking-web-applications-offline-with-web-deploy.md)
> [Вперед](troubleshooting-the-packaging-process.md)
