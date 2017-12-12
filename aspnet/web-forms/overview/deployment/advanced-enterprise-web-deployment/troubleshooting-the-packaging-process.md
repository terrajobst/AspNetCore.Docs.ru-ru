---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: "Устранение неполадок в процессе упаковки | Документы Microsoft"
author: jrjlee
description: "В этом разделе описывается, как собирать подробные сведения о процессе обработки пакетов с помощью свойства EnablePackageProcessLoggingAndAssert м..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 977077357eb5774193a40c55fabee9733dd5ab2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="troubleshooting-the-packaging-process"></a>Устранение неполадок в процессе упаковки
====================
по [Джейсон Lee](https://github.com/jrjlee)

[Скачать PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> В этом разделе описывается, как собирать подробные сведения о процессе обработки пакетов с помощью **EnablePackageProcessLoggingAndAssert** свойства в Microsoft Build Engine (MSBuild).
> 
> При задании **EnablePackageProcessLoggingAndAssert** свойства **true**, будет MSBuild:
> 
> - Добавление дополнительных сведений о процессе упаковки журналов построения.
> - Например, журнал ошибок при определенных условиях, при обнаружении повторяющихся файлов в списке пакетов.
> - Создайте каталог журнала в *ProjectName*\_пакет папки и использовать его для записи сведений об упаковке файлов.
> 
> Если вашего веб-развертывания пакеты не содержат файлы, которые предполагается, что произошел сбой обработки пакетов, можно использовать эти сведения для устранения неполадок процесса и ориентира где дела идут неправильный.
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert** свойство работает только при сборке проекта с помощью **отладки** конфигурации. Свойство учитывается в других конфигурациях.


Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения & #x 2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Службы Communication Foundation (WCF) и проект базы данных.

Метод развертывания, в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой процесс построения управляется двух проектов файлы & #x 2014; один с построение инструкции, которые применяются для каждой целевой среде и, содержащий параметры построения и развертывания конкретной среды. Во время построения файла проекта среды объединяется в файл проекта зависит от среды, образуют полный набор инструкций построения.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Основные сведения о свойстве EnablePackageProcessLoggingAndAssert

[Построение и упаковки проектов веб-приложений](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) описано, как конвейер публикации Web (WPP) предоставляет набор целевых объектов MSBuild, расширяющие функциональные возможности MSBuild и включить его для интеграции веб-служб Internet Information Services (IIS) Средства развертывания (Web Deploy). При создании пакета в проект веб-приложения, вызов WPP целевых объектов.

Большое количество этих целевых объектов WPP включать условную логику, осуществляющий запись в журнал дополнительные при **EnablePackageProcessLoggingAndAssert** свойству **true**. Например, при просмотре **пакета** целевым объектом, можно увидеть, он создает каталог журнала и выводит список файлов в текстовый файл, если **EnablePackageProcessLoggingAndAssert** равен **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Целевые объекты WPP определяются в *Microsoft.Web.Publishing.targets* файл в папке % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web. Можно открыть этот файл и просмотреть целевые объекты в Visual Studio 2010 или редакторе XML. Следует изменить содержимое файла.


## <a name="enabling-the-additional-logging"></a>Включение дополнительных ведения журнала

Можно задать значение для **EnablePackageProcessLoggingAndAssert** свойство различными способами, в зависимости от способа создания проекта.

При создании проекта из командной строки, можно задать значение для **EnablePackageProcessLoggingAndAssert** свойство в качестве аргумента командной строки:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Если вы используете пользовательском файле проекта для построения проектов, можно включить **EnablePackageProcessLoggingAndAssert** значение в **свойства** атрибут **MSBuild**задачи:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Если вы используете определение сборки Team Foundation Server (TFS) для построения проектов, можно задать значение для **EnablePackageProcessLoggingAndAssert** свойство в **Аргументы MSBuild** строки:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Дополнительные сведения о создании и настройке определений сборки см. в разделе [Создание сборки определение, поддерживает развертывания](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Кроме того, если вы хотите включить пакета в каждой сборки, можно изменить файл проекта для проекта веб-приложения задать **EnablePackageProcessLoggingAndAssert** свойства **true**. Свойства следует добавить к первому **PropertyGroup** элемент в файле с расширением CSPROJ или VBPROJ-файлу.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Просмотр файлов журналов

После построения и упаковки проекта веб-приложения с **EnablePackageProcessLoggingAndAssert** значение **true**, MSBuild создает дополнительную папку с именем входа *ProjectName* \_Папки пакета. Папка журнала содержит различные файлы.

![](troubleshooting-the-packaging-process/_static/image2.png)

Список файлов, которые вы видите будут различаться в зависимости от действий в проекте и процесс сборки. Тем не менее эти файлы обычно используются для записи списка файлов, которые собирает WPP пакетов на различных этапах процесса:

- *PreExcludePipelineCollectFilesPhaseFileList.txt* файла список файлов, которые собирает MSBuild упаковки, прежде чем будут удалены все файлы, указанные для исключения.
- *AfterExcludeFilesFilesList.txt* файл содержит список измененных файлов после удаляются все файлы, указанные для исключения.

    > [!NOTE]
    > Дополнительные сведения об исключении файлов и папок из обработки пакетов см. в разделе [за исключением файлов и папок из развертывания](excluding-files-and-folders-from-deployment.md).
- *AfterTransformWebConfig.txt* файла список файлов, собираемых для упаковки после любого *Web.config* преобразования были выполнены. В этом списке, все зависящие от конфигурации *Web.config* преобразования файлов, таких как *Web.Debug.config* и *Web.Release.config*, исключаются из списка файлов для Упаковка. Преобразовать один *Web.config* включается на их место.
- *PostAutoParameterizationWebConfigConnectionStrings.txt* файл содержит список файлов после строки подключения в *Web.config* файла были параметризованы. Это процесс, который позволяет заменить строки подключения нужные параметры для целевой среды при развертывании пакета.
- *Prepackage.txt* -файл содержит окончательные перед построением список файлов, которые будут включены в пакет.

> [!NOTE]
> Имена дополнительных файлов журналов обычно соответствуют WPP целевых объектов. Эти целевые объекты можно просмотреть с помощью проверки *Microsoft.Web.Publishing.targets* файл в папке % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


Если содержимое веб-пакет не соответствуют ожиданию, просмотр этих файлов может быть удобным способом для определения, на какой точке в процесс действия пошло не так.

## <a name="conclusion"></a>Заключение

В этом разделе описано, как использовать **EnablePackageProcessLoggingAndAssert** свойств в MSBuild для устранения неполадок в процессе обработки пакетов. Оно описано различными способами, в котором можно указать значение свойства для процесса сборки и ее описанных дополнительную информацию, которая регистрируется, если значение свойства **true**.

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения об использовании пользовательские файлы проекта MSBuild для управления процессом развертывания см. в разделе [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md) и [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Дополнительные сведения о WPP и управлении его упаковку в разделе [построения и упаковки проектов веб-приложений](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Рекомендации о том, как исключить определенные файлы и папки из веб-развертывания пакетов см. в разделе [за исключением файлов и папок из развертывания](excluding-files-and-folders-from-deployment.md).

>[!div class="step-by-step"]
[Назад](running-windows-powershell-scripts-from-msbuild-project-files.md)
