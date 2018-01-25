---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: "Запуск скриптов Windows PowerShell из файлов проекта MSBuild | Документы Microsoft"
author: jrjlee
description: "В этом разделе описывается выполнения сценария Windows PowerShell как часть процесса построения и развертывания. Сценарий можно запустить локально (другими словами, на б..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: afee7b0621df42a8bc70fc6f7c4a8fd0383fa83a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Запуск скриптов Windows PowerShell из файлов проекта MSBuild
====================
по [Джейсон Lee](https://github.com/jrjlee)

[Скачать PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> В этом разделе описывается выполнения сценария Windows PowerShell как часть процесса построения и развертывания. Можно запустить сценарий локально (другими словами, на сервере сборки) или удаленно, как и на веб-сервере назначения или сервер базы данных.
> 
> Существует множество причин, почему может потребоваться выполнить сценарий Windows PowerShell после развертывания. Например, можно сделать следующее:
> 
> - Добавление источника пользовательское событие в реестр.
> - Создайте каталог файловой системы для передачи файлов.
> - Очистите каталоги сборки.
> - Записи в пользовательский файл журнала.
> - Отправка сообщения электронной почты приглашение пользователей для нового веб-приложения.
> - Создайте учетные записи с соответствующими разрешениями.
> - Настройка репликации между экземплярами SQL Server.
> 
> В этом разделе показано, как запускать скрипты Windows PowerShell локально и удаленно из пользовательского целевого объекта в файле проекта Microsoft Build Engine (MSBuild).


Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения & #x 2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Службы Communication Foundation (WCF) и проект базы данных.

Метод развертывания, в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения управляется двух проектов файлы & #x 2014; один с построение инструкции, которые применяются для каждой целевой среде и, содержащий параметры построения и развертывания конкретной среды. Во время построения файла проекта среды объединяется в файл проекта зависит от среды, образуют полный набор инструкций построения.

## <a name="task-overview"></a>Общие сведения о задаче

Чтобы запустить сценарий Windows PowerShell в рамках процесса автоматической или один шаг развертывания, необходимо выполнить следующие высокоуровневые задачи:

- Добавление в сценарий Windows PowerShell в решение и системы управления версиями.
- Создание команды, который вызывает сценарий Windows PowerShell.
- Экранировать все зарезервированные символы XML, в команде.
- Создание целевого объекта в файле пользовательского проекта MSBuild и использовать **Exec** задачи для запуска вашей команды.

В этом разделе будет показано, как для выполнения этих процедур. Задачи и пошаговые руководства в этом разделе предполагается, что вы уже знакомы с целевые объекты MSBuild и свойства и понимания принципов использования пользовательского файла проекта MSBuild для управления процессом построения и развертывания. Дополнительные сведения см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Создание и Добавление скриптов Windows PowerShell

Задачи в этом разделе используется пример сценария Windows PowerShell с именем **LogDeploy.ps1** продемонстрировать запуска скриптов из MSBuild. **LogDeploy.ps1** сценарий содержит простую функцию, которая производит запись в одну строку в файл журнала:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


**LogDeploy.ps1** сценарий принимает два параметра. Первый параметр представляет полный путь к файлу журнала, в который требуется добавить запись, а второй параметр представляет назначение развертывания, который требуется записать в файл журнала. При запуске сценария он добавляет строки в файл журнала в формате:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Чтобы сделать **LogDeploy.ps1** скрипты для MSBuild, необходимо:

- Добавьте скрипт в систему управления версиями.
- Добавьте скрипт в решение в Visual Studio 2010.

Развертывание сценария с контентом решения, независимо от того, планируется ли запустить сценарий на сервере построения или на удаленном компьютере не требуется. Один из вариантов является добавление скрипта в папку решения. В примере диспетчера контактов так как вы хотите использовать скрипт Windows PowerShell как часть процесса развертывания, имеет смысл необходимо добавить скрипт в папку публикации решения.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Содержимое папки решений копируются сборки серверов, что и исходный материал. Тем не менее они образуют никакая часть выходных данных проекта.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Выполнение скрипта Windows PowerShell на сервере сборки

В некоторых сценариях может потребоваться запускать скрипты Windows PowerShell на компьютере, который создает проекты. Например сценарий Windows PowerShell может использовать для очистки папки сборки или записи в собственный файл журнала.

С точки зрения синтаксиса выполнив сценарий Windows PowerShell из файла проекта MSBuild совпадает со значением выполнения сценария Windows PowerShell из командной строки регулярного. Необходимо вызвать powershell.exe исполняемый файл и использовать **— команда** коммутатору для обеспечения нужные команды Windows PowerShell для выполнения. (В Windows PowerShell v2, можно также использовать **— файл** переключения). Этот формат должна выполнить команда:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Пример:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Если путь к скрипту содержит пробелы, необходимо заключить в одинарные кавычки, следующий за знаком амперсанда путь к файлу. Нельзя использовать двойными кавычками, так как вы уже использовали их следует заключать в команде:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


При вызове этой команды из MSBuild, существуют несколько моментов. Во-первых, необходимо включить **— NonInteractive** флаг, чтобы убедиться, что сценарий выполняется без вмешательства пользователя. Далее следует включать **-ExecutionPolicy** флаг со значением соответствующего аргумента. Указывает политику выполнения будет применяться к сценарию и позволяет переопределить политику выполнения по умолчанию, которое может помешать выполнения сценария Windows PowerShell. Можно выбрать один из этих значений аргумента:

- Значение **Unrestricted** позволит Windows PowerShell для выполнения сценария, независимо от того, имеет ли подпись скрипт.
- Значение **RemoteSigned** позволит Windows PowerShell для выполнения неподписанных скриптов, которые были созданы на локальном компьютере. Тем не менее должны быть подписаны скрипты, которые были созданы в другом месте. (На практике, вы создали сценарий Windows PowerShell локально на сервере сборки крайне маловероятно).
- Значение **AllSigned** позволит Windows PowerShell для выполнения только подписанные скриптов.

Политика выполнения по умолчанию — **Restricted**, который предотвращает запуск любых файлов скрипта Windows PowerShell.

Наконец необходимо экранировать все зарезервированные символы XML, которые происходят в вашей команды Windows PowerShell:

- Замените одинарные кавычки с  **&amp;apos;**
- Замените двойные кавычки с  **&amp;quot;**
- Замените амперсанды с  **&amp;amp;**

- При внесении этих изменений, команда будет выглядеть следующим образом:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


В рамках пользовательского файла проекта MSBuild, можно создать новую цель и использовать **Exec** задачи для выполнения этой команды:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


В этом примере обратите внимание, что:

- Переменные, такие как значения параметров и расположение исполняемого файла Windows PowerShell, объявленные как свойства MSBuild.
- Условия включаются, чтобы пользователь мог переопределить эти значения из командной строки.
- **MSDeployComputerName** свойство объявлено в другом месте в файле проекта.

При выполнении этот целевой объект как часть процесса построения, Windows PowerShell запустите команду и создать запись журнала в указанный файл.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Выполнение скрипта Windows PowerShell на удаленном компьютере

Windows PowerShell — могут выполняться скрипты в удаленных компьютеров с помощью [удаленное управление Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Чтобы сделать это, необходимо использовать [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) командлета. Это дает возможность выполнения сценария с одной или нескольких удаленных компьютерах без копирования скрипт на удаленных компьютерах. Результаты возвращаются на локальный компьютер, из которого запускается скрипт.

> [!NOTE]
> Прежде чем использовать **Invoke-Command** командлета Windows PowerShell выполнение скриптов на удаленном компьютере, необходимо настроить прослушиватель WinRM для принятия удаленных сообщений. Это можно сделать, выполнив команду **winrm quickconfig** на удаленном компьютере. Дополнительные сведения см. в разделе [установки и конфигурации для удаленного управления Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


В окне Windows PowerShell для запуска необходимо выполнить этот синтаксис **LogDeploy.ps1** сценария на удаленном компьютере:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Существует несколько способов использования **Invoke-Command** для выполнения сценария файл, но этот подход является наиболее простым для указания значений параметров и управления пути с пробелами.


При запуске из командной строки, необходимо вызвать исполняемый файл Windows PowerShell и использовать **— команда** параметр для указания:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Как и прежде, необходимо предоставить некоторые дополнительные ключи и экранировать любые зарезервированные символы XML, при выполнении команды из MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Наконец, как и раньше, можно использовать **Exec** задачу в целевой объект пользовательские MSBuild для выполнения команды:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


При выполнении этот целевой объект как часть процесса построения, Windows PowerShell ваш скрипт будет выполняться на компьютере, указанном в **– computername** аргумент.

## <a name="conclusion"></a>Заключение

В этом разделе описано, как запустить сценарий Windows PowerShell из файла проекта MSBuild. Этот подход можно использовать для выполнения сценария Windows PowerShell, локально или на удаленном компьютере, как часть автоматической или один шаг процесса построения и развертывания.

## <a name="further-reading"></a>Дополнительные сведения

Сведения о подписывании скриптов Windows PowerShell и управление политиками выполнения. в разделе [запуск скриптов Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Рекомендации по выполнение команд Windows PowerShell с удаленного компьютера см. в разделе [выполнение удаленных команд](https://technet.microsoft.com/library/dd819505.aspx).

Дополнительные сведения об использовании пользовательские файлы проекта MSBuild для управления процессом развертывания см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

>[!div class="step-by-step"]
[Назад](taking-web-applications-offline-with-web-deploy.md)
[Вперед](troubleshooting-the-packaging-process.md)
