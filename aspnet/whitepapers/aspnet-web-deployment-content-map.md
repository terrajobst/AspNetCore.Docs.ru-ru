---
uid: whitepapers/aspnet-web-deployment-content-map
title: "Развертывание веб-ASP.NET — рекомендуется использовать ресурсы | Документы Microsoft"
author: rick-anderson
description: "Этот раздел содержит ссылки на документацию ресурсов о развертывании (публикации) ASP.NET веб-приложений в IIS с помощью Visual Studio 2010, Visual Web De..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 8bded273de1ca7b050d41ddd872d9a1aa68bb314
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment---recommended-resources"></a>Развертывание веб-ASP.NET — рекомендуется использовать ресурсы
====================
> Этот раздел содержит ссылки на документацию ресурсов о развертывании (публикации) ASP.NET веб-приложений в IIS с помощью Visual Studio 2010, Visual Web Developer 2010 и более поздних версиях.
> 
> Если вы знаете значительные записи блога [stackoverflow](http://stackoverflow.com) потоком, или любую ссылку, может потребоваться, [отправьте сообщение электронной почты](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) со ссылкой.
> 
> > [!NOTE] 
> > 
> > Многие из этих ресурсов описания функций развертывания, которые доступны только в том случае, если установить последний выпуск среды [Visual Studio веб-публикации обновление](https://go.microsoft.com/fwlink/?LinkID=208120). Некоторые возможности доступны только в Visual Studio 2012 или Visual Studio 2013.


В этом разделе содержатся следующие подразделы.

- [Основные сведения о параметрах развертывания для веб-проектов](#understanding)
- [Поиск поставщиков для приложения ASP.NET размещения](#findinghosting)
- [Развертывание веб-приложения из Visual Studio](#fromvs)
- [Развертывание веб-приложения необходимо создать и установить пакет веб-развертывания](#package)
- [Развертывание веб-приложения, используя процесс непрерывной интеграции (CI)](#ci)
- [С помощью преобразования Web.config, чтобы изменить параметры в файле app.config или в файл Web.config во время развертывания](#transforms)
- [С помощью параметров веб-развертывания для изменения параметров в конечное веб-приложение во время развертывания](#webdeployparms)
- [Убедившись, что приложение находится в автономном состоянии во время развертывания](#appoffline)
- [Развертывание базы данных или изменения в базе данных как часть развертывание веб-приложений](#databasewithweb)
- [Развертывание базы данных отдельно от развертывание веб-приложений](#databaseseparate)
- [Развертывание веб-приложения, который использует приложение ASP.NET служб, например членства и профилирование](#aspnetmembership)
- [Предварительная компиляция для развертывания](#precompiling)
- [Развертывание веб-приложения интрасети](#intranet)
- [Автоматизации общих задач развертывания, которые не производится автоматически без дополнительной настройки.](#automating)
- [Настройка веб-серверов, чтобы разработчики могут развертывать веб-приложений, к ним с помощью веб-развертывания](#configuringservers)
- [Настройка серверов для поставщика услуг размещения](#hostingprovider)
- [Устранение неполадок развертывания](#troubleshooting)
- [Получение справки с помощью специального вопроса конкретного развертывания](#gettinghelp)
- [Дополнительные ресурсы](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Основные сведения о параметрах развертывания для веб-проектов

- [Обзор развертывания веб-для Visual Studio и ASP.NET](https://msdn.microsoft.com/en-us/library/dd394698.aspx) (MSDN).
- [Развертывание Windows Azure веб-сайт](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Описываются параметры и ссылки на ресурсы для развертывания веб-проектов для Windows Azure веб-сайтов, включая [непрерывной поставки](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (автоматических из [система управления версиями](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) с помощью Visual Studio.
- [Visual Studio 2012 Web публикации усовершенствования](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (видео Скотт Хансельман).
- [Общие сведения о публикации для развертывания веб-приложения в VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (блог работы). Более старые записи блога, но некоторые из ресурсов Visual Studio 2010 приводятся ссылки на сведения, все еще актуальны для Visual Studio 2012.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Поиск поставщиков для приложения ASP.NET размещения

- [Размещение ASP.NET](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Развертывание веб-приложения из Visual Studio

- [Развертывание Windows Azure веб-сайт](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Описываются параметры, а также ссылки на ресурсы для развертывания веб-проектов веб-сайтов Azure для Windows. Содержит раздел, посвященный развертывание из Visual Studio.
- [ASP.NET веб-развертывания с помощью Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 учебника выпусков, показано, как развертывать веб-приложения с базами данных SQL Server. Для базы данных развертывания использует поставщик dbDacFx и Entity Framework Code First Migrations. Также включает информацию о [преобразования файлов Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [развертывание отдельных файлов](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [развертывания из командной строки](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), и [как Настройка веб-сайте Visual Studio конвейера публикации путем изменения файлов .pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Применяется для всех веб-проектов ASP.NET, включая веб-форм, MVC и веб-API)
- [Как: развертывание, публикация веб-проекта с помощью одного щелчка мыши в Visual Studio](https://msdn.microsoft.com/en-us/library/dd465337.aspx) (ссылаться информации для мастера веб-публикации Visual Studio).
- [Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Это более ранней версии **ASP.NET веб-развертывания с помощью Visual Studio** перечисляются в верхней части этого раздела. В основном полезно теперь сведения о развертывании баз данных SQL Server Compact и миграции с SQL Server Compact до полной версии SQL Server.
- [.NET многоуровневые приложения с помощью таблиц хранилища, очередей и больших двоичных объектов](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (сайт Microsoft Azure). 5 учебника выпусков, показано, как создать проект MVC и развернуть ее для облачных служб Windows Azure.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Развертывание веб-приложения необходимо создать и установить пакет веб-развертывания

- [Как: создайте пакет веб-развертывания в Visual Studio](https://msdn.microsoft.com/en-us/library/dd465323.aspx) (MSDN).
- [Способ: установите создан пакет развертывания, с помощью файла deploy.cmd, Visual Studio](https://msdn.microsoft.com/en-us/library/ff356104.aspx) (MSDN).
- [С помощью пакета веб-развертывания для развертывания служб IIS на компьютере разработки и на узле сторонних производителей](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (блог Sayed Hashimi). Использование диспетчера служб IIS для установки пакета развертывания в службах IIS на локальном компьютере и на размещение компании, поддерживает диспетчер IIS для удаленного администрирования.
- [Создание Web развертывание пакета из Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET веб-сайт). Содержит инструкции для создания командной строки пакета и установки.
- [После публикации в любом месте пакета](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (блог Sayed Hashimi). Предоставляет пакет NuGet, который автоматизирует процесс преобразования файла Web.config для нескольких сред назначения, чтобы один пакет можно развернуть на нескольких серверах. См. также [PackageWeb видео](https://www.youtube.com/watch?v=-LvUJFI8CzM) по Sayed Hashimi.

Также в следующем разделе.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Развертывание веб-приложения, используя процесс непрерывной интеграции (CI)

- [Непрерывной интеграции и непрерывного доставки (Создание реальных облачных приложений в Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Электронная книга главе, представлены непрерывной интеграции и непрерывного доставки.
- [Развертывание Windows Azure веб-сайт](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Описание параметров и ссылок на ресурсы для развертывания веб-проектов веб-сайтов Azure для Windows. Содержит раздел, посвященный автоматизации развертывания из системы управления версиями.
- [Развертывание веб-приложений в корпоративных сценариях](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). 40 учебника выпусков, показано, как автоматизировать развертывание в процесс непрерывной Интеграции с помощью Visual Studio 2010 и Team Foundation Server 2010.
- [Внутри Microsoft Build Engine: с помощью MSBuild и Team Foundation Build, Sayed Hashimi и Bartholomew Уильям](http://msbuildbook.com). Это книги, а не веб-ресурсу, но это начальное руководство о том, как настройка MSBuild для сценариев непрерывной интеграции.
- [Пакет расширения MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Включает в себя задачи развертывания.
- [Team Foundation руководство по настройке сборки](https://aka.ms/vsarsolutions). Документация по ALM Rangers по настройке Team Foundation Server охватывает веб-развертывания и включает учебники и видеоматериалы.
- [Преобразует SlowCheetah XML с сервера CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (блог Sayed Hashimi). Описание способов использования SlowCheetah надстройки Visual Studio для преобразования app.config и других XML-файлов.

См. также [убедившись, что приложение находится в автономном состоянии во время развертывания](aspnet-web-deployment-content-map.md#appoffline) ниже на этой странице.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>С помощью преобразования Web.config, чтобы изменить параметры в файле app.config или в файл Web.config во время развертывания

- [Файл Web.config преобразования](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Синтаксис преобразования файла Web.config для веб-проекта развертывания с помощью Visual Studio](https://msdn.microsoft.com/en-us/library/dd465326.aspx) (MSDN).
- [Веб-инструменты 2012.2 - преобразования web.config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (видео YouTube Sayed Hashimi). Показано, как настроить и просмотреть преобразования Web.config.
- [Отключение преобразования Web.config](https://msdn.microsoft.com/en-us/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Когда следует использовать параметры веб-развертывания вместо преобразования Web.config](https://msdn.microsoft.com/en-us/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (преобразовать документ XML) выпустила codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (блог .NET веб-разработка и инструменты). Объявляет о доступности исходного кода для модуля преобразования файлов Web.config и перечисляются некоторые средства, которые его используют.
- [Windows Azure веб-сайтов: Как приложение строки и соединения строк работает](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (блог Microsoft Azure). Преобразует альтернативой Web.config, если веб-сайтов Windows Azure к целевой среде, и необходимо преобразовать `appSettings` или `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>С помощью параметров веб-развертывания для изменения параметров в конечное веб-приложение во время развертывания

- [Как: использование веб-развертывания параметров в пакет веб-развертывания](https://msdn.microsoft.com/en-us/library/ff398068.aspx) (MSDN).
- [MSDeploy: Обновление параметров приложения на публикации, основанной на профиле публикации](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (блог Sayed Hashimi). Показано, как интегрировать веб-развертывания параметров в Visual Studio профилей публикации.
- [Веб-развертывание параметризации](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET веб-сайт).
- [Веб-развертывание параметризации в действии](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (блог работы).
- [Vs параметризации развертывание Web. Преобразования Web.config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (блог работы).
- [Windows Azure веб-сайтов: Как приложение строки и соединения строк работает](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (блог Microsoft Azure). Вместо веб-развертывание параметров, если веб-сайтов Windows Azure к целевой среде, и требуется параметризовать `appSettings` или `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Убедившись, что приложение находится в автономном состоянии во время развертывания

- [ASP.NET веб-развертывания с помощью Visual Studio: развертывание обновления кода](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). См. в разделе **взять приложение в автономный режим во время развертывания.**
- [Отключая приложения перед публикацией](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net сайта). Описание функции, встроенные в Web Deploy 3.0, автоматизирует обработку приложения\_offline.htm файл. Эта функция не работает с пользовательского приложения\_offline.htm файлов.
- [Как можно использовать веб-приложение в автономный режим во время публикации](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (блог Sayed Hashimi). Как автоматизировать процесс использования пользовательского приложения\_offline.htm файл.
- [Веб-публикация обновлений для приложения в автономный режим и usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (блог веб-разработки Майкрософт). Другой вариант для автоматического использования приложения\_offline.htm файл.
- [Веб-развертывание RTW 3.5](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net сайта). Новая функция в Web развертывание 3.5 для пользовательского приложения\_offline.htm файлов.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Развертывание базы данных или изменения в базе данных как часть развертывание веб-приложений

- [Настройка развертывания базы данных в Visual Studio](https://msdn.microsoft.com/en-us/library/dd394698.aspx#dbdeployment) (MSDN). Общие сведения о параметрах развертывания базы данных с веб-проекта.
- [ASP.NET веб-развертывания с помощью Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 учебника выпусков, показано развертывание базы данных, используя поставщик dbDacFx и Entity Framework Code First Migrations.
- [Как: развертывание веб-проекта с помощью одного щелчка мыши публикации в Visual Studio](https://msdn.microsoft.com/en-us/library/dd465337.aspx) (MSDN).
- [Развертывание приложения безопасного ASP.NET MVC 5 с членством, OAuth и базы данных SQL на Windows Azure веб-сайт](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Длинные учебник, в котором строит и развертывает приложение, которое используется один сервер SQL базы данных для членства и данных приложений.
- [Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). 12 учебника выпусков, показано, как развертывание баз данных SQL Server Compact и миграции с SQL Server Compact до полной версии SQL Server.

См. также развертывание веб-приложения путем создания и установки пакет веб-развертывания и развертывание веб-приложения, используя процесс непрерывной интеграции (CI) ранее на этой странице.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Развертывание базы данных отдельно от развертывание веб-приложений

- [SQL Server Data Tools](https://msdn.microsoft.com/en-us/library/hh272686(v=vs.103).aspx) (MSDN).
- [Включение данных в проект базы данных SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (блог группы SQL Server Data Tools). Том, как создавать схему и данные при развертывании базы данных.
- [Развертывание базы данных в Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (сайт Microsoft Azure)
- [Миграция баз данных в базу данных SQL Windows Azure (прежнее название — SQL Azure)](https://msdn.microsoft.com/en-us/library/windowsazure/ee730904.aspx) (MSDN).
- [Миграция базы данных SQL Azure с помощью SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (блог группы SQL Server Data Tools).
- [Перенос ориентированных на данные приложений в Windows Azure](https://msdn.microsoft.com/en-us/library/jj156154.aspx) (MSDN).
- [Миграция баз данных SQL Server к базе данных Azure SQL Windows](https://msdn.microsoft.com/en-us/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Развертывание веб-приложения, который использует приложение ASP.NET служб, например членства и профилирование

- [Развертывание приложения безопасного ASP.NET MVC 5 с членством, OAuth и базы данных SQL на Windows Azure веб-сайт](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Длинные учебник, в котором строит и развертывает приложение, которое используется один сервер SQL базы данных для членства и данных приложений.
- [ASP.NET Identity](https://asp.net/identity/). Ресурсы для ASP.NET Identity.
- [ASP.NET веб-развертывания с помощью Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 учебника выпусков, показано, как развертывание базы данных членства ASP.NET.
- [Настройка веб-сайт, использующий службы приложения](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Для веб-сайта проекты но также касается проектов веб-приложений.
- [Пользователи и роли на рабочий веб-сайт](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Для веб-сайта проекты но также касается проектов веб-приложений.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Предварительная компиляция для развертывания

- [Обзор предварительной компиляции проекта приложения ASP.NET Web](https://msdn.microsoft.com/en-us/library/aa983464.aspx) (MSDN).
- [Вкладка "веб" упаковки и публикации, свойства проекта](https://msdn.microsoft.com/en-us/library/dd410108.aspx) (MSDN).
- [Дополнительные параметры предварительной компиляции](https://msdn.microsoft.com/en-us/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Развертывание веб-приложения интрасети

- [Используйте параметр в локальной среде организации проверку подлинности (ADFS) с ASP.NET в Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (блог по Vittorio Bertocci).
- [Создание сайта интрасети с помощью ASP.NET MVC](https://msdn.microsoft.com/en-us/library/gg703322(VS.98).aspx) (MSDN). Более старые writen пошагового руководства для Visual Studio 2010 не отражает значительных изменений в шаблоны проектов интрасети, представленные в Visual Studio 2013.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Автоматизации общих задач развертывания, которые не производится автоматически без дополнительной настройки.

- [ASP.NET веб-развертывания с помощью Visual Studio: развертывание дополнительных файлов](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Установка разрешений для папки на веб-публикация](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (блог Sayed Hashimi).
- [Расширение целевого файла параметры реестра для проекта веб-пакета](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (блог Web Development Tools).
- [Расширение преобразование XML (Web.config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (блог Sayed Hashimi). Показано, как создавать пользовательские преобразования XDT.
- [Веб-средства развертывания (MSDeploy) настраиваемый поставщик Take 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (блог Sayed Hashimi). Показано, как создать настраиваемый поставщик веб-развертывания.
- [Упаковка и развертывание COM-компонентов](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (блог Web Development Tools).
- [Как сборки .NET пакета](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (блог Web Development Tools). Способ развертывания сборки в глобальный кэш сборок.
- [В сценарии все: инициализация вашей виртуальной Машине Windows Azure для вашего веб-сервера в IIS веб-развертывания и другие вещи](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (блог Тугберк Угурлу).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Настройка веб-серверов, чтобы разработчики могут развертывать веб-приложений, к ним с помощью веб-развертывания

- [Установка и настройка веб-развертывание для администратора и без прав администратора для развертывания](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net сайта).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Настройка серверов для поставщика услуг размещения

- [Руководство по развертыванию размещение Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (центра загрузки Майкрософт).
- [Создание файла профиля XML](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net сайта).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Устранение неполадок развертывания

- [Устранение неполадок веб-сайтов Windows Azure в Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (сайт Microsoft Azure).
- [ASP.NET веб-развертывания с помощью Visual Studio: Устранение неполадок](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Разрешение общих проблем с помощью веб-развертывания](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Веб-развертывание коды ошибок](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net сайта).
- [Веб-развертывания часто задаваемые вопросы о Visual Studio и ASP.NET](https://msdn.microsoft.com/en-us/library/ee942158.aspx) (MSDN).
- [Основные различия между IIS и ASP.NET Development Server](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Распространенные конфигурации различия между разработки и эксплуатации](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Размещение приложений ASP.NET со средним уровнем доверия](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guys Rolla сайта).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Получение справки с помощью специального вопроса конкретного развертывания

- [Форум по ASP.NET конфигурации и развертывания](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Дополнительные ресурсы

Этот раздел содержит ссылки на дополнительные ресурсы, которые можно использовать для изучения способов использования средств развертывания Visual Studio и IIS.

Следующих блогах часто содержат сведения о веб-развертывания Visual Studio.

- [Средства разработки веб-в блоге Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Блог sayed Hashimi](http://www.sedodream.com/).

Документация по веб-развертывания, платформу IIS, Visual Studio для выполнения задачи развертывания проекта веб-приложений на следующих ресурсах. Можно задать вопросы о веб-развертывания в [средство веб-развертывания форум](https://go.microsoft.com/fwlink/?LinkId=149411) на веб-сайте IIS.net.

- [Введение в веб-развертывания](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Установка и настройка Web развертывания](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Скрипты PowerShell для автоматизации Web развертывание установки](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Средство развертывания веб-](https://go.microsoft.com/fwlink/?LinkId=151481). Таблица верхнего уровня содержимого узла для веб-развертывания документации на сайте TechNet. Содержит полезную справочную информацию, но большая часть страниц не были обновлены лет TechNet.
- [Пространство имен Microsoft.Web.Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). Документация по API, начиная с версии 1.0 не обновлена.
- [Блог команды развертывания Microsoft Web](https://blogs.iis.net/msdeploy/default.aspx).
- [Вкладка Публикация на веб-сайте IIS.net](https://www.iis.net/learn/publish).
