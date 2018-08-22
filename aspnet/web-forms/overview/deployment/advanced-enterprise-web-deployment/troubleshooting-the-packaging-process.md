---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Устранение неполадок в процессе упаковки | Документация Майкрософт
author: jrjlee
description: В этом разделе описывается, как собирать подробные сведения о процессе упаковки с помощью свойства EnablePackageProcessLoggingAndAssert в M...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 22be1ccc5a1ec52d143bedffd79264918c1a45e1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837638"
---
<a name="troubleshooting-the-packaging-process"></a>Устранение неполадок в процессе упаковки
====================
по [Джейсон Lee](https://github.com/jrjlee)

[Загрузить PDF-файл](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> В этом разделе описывается, как собирать подробные сведения о процессе упаковки с помощью **EnablePackageProcessLoggingAndAssert** свойства в Microsoft Build Engine (MSBuild).
> 
> При задании **EnablePackageProcessLoggingAndAssert** свойства **true**, MSBuild будет:
> 
> - Добавьте дополнительные сведения о процессе упаковки в журналы построения.
> - Например, журнал ошибок, при определенных условиях, найденные файлы-дубликаты в списке пакетов.
> - Создайте каталог журнала *имя_проекта*\_упаковать папки и использовать его для записи сведений о файлах, упаковке.
> 
> Если ваш веб-развертывания пакетов не содержат файлы, которые предполагается, что произошел сбой в процессе упаковки, можно использовать эти сведения для устранения неполадок процесса и pinpoint, где ход выполнения процесса не так.
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert** свойство работает только при построении проекта с помощью **Отладка** конфигурации. Свойство учитывается в других конфигурациях.


Этот раздел является частью серии учебников, исходя из требования к развертыванию enterprise вымышленной компании Fabrikam, Inc. В этой серии руководств используется пример решения&#x2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;для представления веб-приложения с более реалистичные уровень сложности, включая приложения ASP.NET MVC 3, Windows Communication Служба Foundation (WCF) и проект базы данных.

Метод развертывания в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения управляется двумя файлы проекта&#x2014;один с Создание инструкции, которые применяются для каждой целевой среде и содержащего параметры построения и развертывания для конкретной среды. Во время сборки файл проекта для конкретной среды объединяется в файл проекта независимой от среды, чтобы полный набор инструкций построения.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Основные сведения о свойстве EnablePackageProcessLoggingAndAssert

[Сборка и упаковка проектов веб-приложений](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) описано, как конвейер публикации Web (WPP) предоставляет набор целевых объектов MSBuild, которые расширяют функциональные возможности MSBuild и включить его, чтобы интегрировать веб-службы Internet Information Services (IIS) Средство развертывания (веб-развертывания). При создании пакета в проект веб-приложения, вызываемой WPP целевых объектов.

Множество из этих целевых объектов WPP включать условную логику, которая записывает в журнал Дополнительные сведения при **EnablePackageProcessLoggingAndAssert** свойству **true**. Например, если вы просмотрите **пакета** целевой объект, вы увидите его создает дополнительный журнал directory и выводит список файлов в текстовый файл, если **EnablePackageProcessLoggingAndAssert** равен **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Целевые объекты WPP определяются в *Microsoft.Web.Publishing.targets* файл в папке % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web. Можно открыть этот файл и просмотрите целевые объекты в Visual Studio 2010 или редакторе XML. Постарайтесь не измените содержимое файла.


## <a name="enabling-the-additional-logging"></a>Включить дополнительное ведение журнала

Можно указать значение для **EnablePackageProcessLoggingAndAssert** свойства различными способами, в зависимости от того, как при создании проекта.

При построении проекта из командной строки можно указать значение для **EnablePackageProcessLoggingAndAssert** свойство аргумент командной строки:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Если вы используете пользовательском файле проекта для сборки проектов, можно включить **EnablePackageProcessLoggingAndAssert** значение в **свойства** атрибут **MSBuild**задачи:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Если вы используете определение сборки Team Foundation Server (TFS) для построения проектов, можно просто указать значение для **EnablePackageProcessLoggingAndAssert** свойство в **Аргументы MSBuild** строки:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Дополнительные сведения о создании и настройке определений сборки см. в разделе [Создание сборки определение что поддерживает развертывания](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Кроме того, если вы хотите включить пакет в каждой сборки, можно изменить файл проекта для проекта веб-приложения задать **EnablePackageProcessLoggingAndAssert** свойства **true**. Следует добавить свойство к первому **PropertyGroup** элемент в файле с расширением CSPROJ или VBPROJ-файл.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Просмотр файлов журнала

Во время сборки и упаковать проект веб-приложения с помощью **EnablePackageProcessLoggingAndAssert** присвоено **true**, MSBuild создает дополнительную папку с именем входа *имя_проекта* \_Папку пакета. В папке журнала содержит различные файлы.

![](troubleshooting-the-packaging-process/_static/image2.png)

Список файлов, которые вы видите будет различаться в зависимости от указанные действия в проекте и процесс сборки. Тем не менее эти файлы обычно используются для записи списка файлов, которые собирает WPP для упаковки, на различных этапах процесса:

- *PreExcludePipelineCollectFilesPhaseFileList.txt* файл список файлов, которые собирает MSBuild для упаковки, пока не будут удалены все файлы, указанные для исключения.
- *AfterExcludeFilesFilesList.txt* файл содержит список измененных файлов, после удаления все файлы, указанные для исключения.

    > [!NOTE]
    > Дополнительные сведения об исключении файлов и папок из в процессе упаковки см. в разделе [за исключением файлов и папок из развертывания](excluding-files-and-folders-from-deployment.md).
- *AfterTransformWebConfig.txt* файл список файлов, собираемых для упаковки после любых *Web.config* выполнения преобразований. В этом списке, а все зависящие от конфигурации *Web.config* преобразования файлов, например *Web.Debug.config* и *Web.Release.config*, исключаются из списка файлов для Упаковка. Преобразование одного *Web.config* входит в их место.
- *PostAutoParameterizationWebConfigConnectionStrings.txt* файл содержит список файлов после строки подключения в *Web.config* файла были параметризованы. Это процесс, который позволяет заменить строки подключения с помощью настройки прав для целевой среды при развертывании пакета.
- *Prepackage.txt* файл содержит окончательные перед построением списка файлов для включения в пакет.

> [!NOTE]
> Имена дополнительных файлов журналов обычно соответствуют WPP целевых объектов. Вы можете просмотреть эти целевые объекты с помощью проверки *Microsoft.Web.Publishing.targets* файл в папке % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


Если содержимое веб-пакет не вашим ожиданиям, просмотр этих файлов может быть удобно для определения, на какой момент в процесс вещей пошло не так.

## <a name="conclusion"></a>Заключение

В этом разделе описано, как можно использовать **EnablePackageProcessLoggingAndAssert** свойства в MSBuild для устранения неполадок в процессе упаковки. Оно описывает различные способы, в котором можно указать значение свойства к процессу сборки и описывались дополнительной информацией, которая записывается, когда задано свойство **true**.

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения об использовании настраиваемых файлов проекта MSBuild для управления процессом развертывания, см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Дополнительные сведения о конвейере публикации в Интернете и управлению в процессе упаковки см. в разделе [сборка и упаковка проектов веб-приложений](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Рекомендации о том, как исключить определенные файлы и папки из веб-развертывания пакетов, см. в разделе [за исключением файлов и папок из развертывания](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Назад](running-windows-powershell-scripts-from-msbuild-project-files.md)
