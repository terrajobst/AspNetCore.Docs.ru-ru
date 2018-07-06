---
uid: whitepapers/aspnet-web-deployment-content-map
title: Развертывание веб-ASP.NET — рекомендуемые ресурсы | Документация Майкрософт
author: rick-anderson
description: В этом разделе содержит ссылки на документацию, ресурсы о развертывании (публикации) ASP.NET веб-приложений в IIS с помощью Visual Studio 2010, Visual Web De...
ms.author: aspnetcontent
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: f46a588144fb1af958737b07b73acbff60318fe6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809424"
---
<a name="aspnet-web-deployment---recommended-resources"></a>Развертывание веб-ASP.NET — рекомендуемые ресурсы
====================
> В этом разделе содержит ссылки на документацию, ресурсы о развертывании (публикации) ASP.NET веб-приложений в IIS с помощью Visual Studio 2010, Visual Web Developer 2010 и более поздних версий.
> 
> Если известно, учета, замечательный блог [stackoverflow](http://stackoverflow.com) потока или любую ссылку, которая будет полезна, [отправьте нам сообщение электронной почты](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) со ссылкой.
> 
> > [!NOTE] 
> > 
> > Многие из этих ресурсов описания функций развертывания, которые доступны только в том случае, если установить последний выпуск [Visual Studio обновление веб-публикации](https://go.microsoft.com/fwlink/?LinkID=208120). Некоторые функции доступны только в Visual Studio 2012 или Visual Studio 2013.


В этом разделе содержатся следующие подразделы.

- [Основные сведения о параметрах развертывания для веб-проектов](#understanding)
- [Поиск размещения поставщиков для приложения ASP.NET](#findinghosting)
- [Развертывание веб-приложения из Visual Studio](#fromvs)
- [Развертывание веб-приложения необходимо создать и установить пакет веб-развертывания](#package)
- [Развертывание веб-приложения, с помощью процесса непрерывной интеграции (CI)](#ci)
- [С помощью преобразования Web.config, чтобы изменить параметры в файле app.config или в файл Web.config во время развертывания](#transforms)
- [С помощью веб-развертывания параметров для изменения параметров в конечное веб-приложение во время развертывания](#webdeployparms)
- [Убедившись, что приложение находится в автономном режиме во время развертывания](#appoffline)
- [Развертывание базы данных или изменения в базу данных как часть развертывания веб-приложений](#databasewithweb)
- [Развертывание базы данных отдельно от развертывания веб-приложений](#databaseseparate)
- [Развертывание веб-приложения, который использует приложение ASP.NET службах, таких как членство и профилирование](#aspnetmembership)
- [Предварительная компиляция для развертывания](#precompiling)
- [Развертывание веб-приложения интрасети](#intranet)
- [Автоматизация основных задач развертывания, которые не обрабатываются автоматически без дополнительной настройки](#automating)
- [Настройка веб-серверов, таким образом, разработчики могут развернуть веб-приложений к ним с помощью веб-развертывания](#configuringservers)
- [Настройка серверов для поставщика услуг размещения](#hostingprovider)
- [Устранение проблем с развертыванием](#troubleshooting)
- [Получение справки с помощью вопроса конкретного развертывания](#gettinghelp)
- [Дополнительные ресурсы](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Основные сведения о параметрах развертывания для веб-проектов

- [Веб-Общие сведения о развертывании для Visual Studio и ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Развертывание Windows Azure веб-сайта](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Описываются параметры и ссылки на ресурсы для развертывания веб-проектов для Windows Azure веб-сайтов, включая [непрерывной поставки](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (автоматических из [система управления версиями](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) а также с помощью Visual Studio.
- [Усовершенствования веб-публикации Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (видео Скотт Хансельман).
- [Общие сведения о Post для веб-развертывания в VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (блог работы). Более старые записи блога, но некоторые из ресурсов Visual Studio 2010 она связывает сведений, которые все еще актуальны для Visual Studio 2012.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Поиск размещения поставщиков для приложения ASP.NET

- [Размещение ASP.NET](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Развертывание веб-приложения из Visual Studio

- [Развертывание Windows Azure веб-сайта](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Описываются параметры и предоставляет ссылки на ресурсы для развертывания веб-проектов для Windows Azure Web Sites. Содержит раздел о развертывании из Visual Studio.
- [Веб-развертывание ASP.NET с помощью Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12, часть цикла руководств показано, как развертывать веб-приложения с базами данных SQL Server. Для базы данных в развертывании используется как поставщик dbDacFx, так и Entity Framework Code First Migrations. Также включает информацию о [преобразования файла Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [развертывание отдельных файлов](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [развертывания из командной строки](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), и [как Настройка Visual Studio web конвейера публикации путем изменения файлов .pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Применяется для всех веб-проектов ASP.NET, включая веб-форм, MVC и веб-API.)
- [Практическое: развертывание, публикация веб-проекта с помощью одним щелчком в Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (ссылаться информации для мастера веб-публикации Visual Studio).
- [Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Это более ранняя версия **веб-развертывание ASP.NET с помощью Visual Studio** перечисляются в верхней части этого раздела. Теперь главным образом используются сведения о развертывании базы данных SQL Server Compact и способы перехода с SQL Server Compact до полного выпуска SQL Server.
- [.NET многоуровневого приложения с помощью хранилища таблиц, очередей и больших двоичных объектов](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (сайт Microsoft Azure). 5 серии руководств, показано, как создать проект MVC и развернуть его в Windows Azure облачную службу.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Развертывание веб-приложения необходимо создать и установить пакет веб-развертывания

- [Практическое: Создание веб-пакета развертывания в Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Практическое: Install, создать пакет развертывания с помощью файла deploy.cmd средой Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [С помощью пакета веб-развертывания для развертывания в IIS на в среде разработки и на узле сторонних](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (блог Саид Хашими). Как использовать IIS Manager для установки пакета развертывания в IIS на локальном компьютере и на размещение компании, поддерживает диспетчер служб IIS для удаленного администрирования.
- [Создание Web развертывание пакета из Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET веб-сайт). Содержит инструкции для создания командной строки пакета и установки.
- [Упаковать один раз опубликовать в любом месте](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (блог Саид Хашими). Предоставляет пакет NuGet, который автоматизирует процесс преобразования файла Web.config для нескольких сред назначения, таким образом, чтобы можно было развернуть один пакет на несколько серверов. См. также [видео PackageWeb](https://www.youtube.com/watch?v=-LvUJFI8CzM) , Саид Хашими.

Также в следующем разделе.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Развертывание веб-приложения, с помощью процесса непрерывной интеграции (CI)

- [Непрерывная интеграция и непрерывная поставка (Создание реальных облачных приложений с помощью Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Глава электронной книги, которая познакомит непрерывной интеграции и непрерывной доставки.
- [Развертывание Windows Azure веб-сайта](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Описываются параметры и ссылки на ресурсы для развертывания веб-проектов для Windows Azure Web Sites. Включает раздел об автоматизации развертывания из системы управления версиями.
- [Развертывание веб-приложений в корпоративных сценариях](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). 40 серии руководств, показано, как для автоматизации развертывания в процесс непрерывной Интеграции, с помощью Visual Studio 2010 и Team Foundation Server 2010.
- [Внутри Microsoft Build Engine: использование MSBuild и Team Foundation Build, Саид Хашими и William Bartholomew](http://msbuildbook.com). Это книги, а не веб-ресурсу, но это начальное руководство о том, как настроить MSBuild для сценариев непрерывной интеграции.
- [MSBuild Extension Pack](https://github.com/mikefourie/MSBuildExtensionPack). Включает в себя задачи развертывания.
- [Team Foundation руководство по настройке сборки](https://aka.ms/vsarsolutions). Документация по ALM Rangers по настройке Team Foundation Server охватывает веб-развертывания и включает в себя учебники и видеоматериалы.
- [Преобразует SlowCheetah XML на сервере непрерывной Интеграции](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (блог Саид Хашими). В этой статье описывается использование SlowCheetah надстройки Visual Studio для преобразования app.config и другие файлы формата XML.

См. также [убедившись, что приложение находится в автономном режиме во время развертывания](aspnet-web-deployment-content-map.md#appoffline) ниже на этой странице.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>С помощью преобразования Web.config, чтобы изменить параметры в файле app.config или в файл Web.config во время развертывания

- [Преобразования файла Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Синтаксис преобразования файла Web.config для веб-проекта развертывания с помощью Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Веб-инструменты 2012.2 - преобразования web.config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (видео YouTube Саид Хашими). Показано, как настроить и предварительный просмотр преобразования Web.config.
- [Как отключить преобразования Web.config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Когда следует использовать параметры веб-развертывания вместо преобразования Web.config](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (преобразование документа XML), выпуск состоится codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (блог .NET веб-разработки и средства). Объявляет о доступности исходного кода для модуля преобразования файла Web.config и перечисляет некоторые средства, которые ее используют.
- [Windows Azure веб-сайты: Как приложение строки and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (блог Microsoft Azure). Преобразует альтернативой Web.config, если целевой среды является веб-сайтов Windows Azure и вы хотите преобразовать `appSettings` или `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>С помощью веб-развертывания параметров для изменения параметров в конечное веб-приложение во время развертывания

- [Практическое: использование веб-развертывания параметров в пакет веб-развертывания](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Как обновить параметры приложения на публикации, основанная на профиле публикации](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (блог Саид Хашими). Показано, как интегрировать параметров веб-развертывания в Visual Studio профилей публикации.
- [Веб-развертывание параметризации](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET веб-сайт).
- [Веб-развертывание параметризации в действии](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (блог работы).
- [Vs развертывание параметризации Web. Преобразования Web.config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (блог работы).
- [Windows Azure веб-сайты: Как приложение строки and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (блог Microsoft Azure). Альтернативы веб-развертывание параметров, если целевой среды веб-сайтов Windows Azure и требуется параметризовать соответствующие `appSettings` или `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Убедившись, что приложение находится в автономном режиме во время развертывания

- [Веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления кода](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). См. в разделе **взять приложение в автономный режим во время развертывания.**
- [Отключая приложения перед публикацией](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net сайт). Описание функции, встроенные в Web Deploy 3.0, автоматизирующий обработки приложения\_offline.htm файл. Эта функция не работает с помощью настраиваемого приложения\_offline.htm файлов.
- [Как использовать веб-приложение в автономный режим во время публикации](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (блог Саид Хашими). Как автоматизировать процесс использования пользовательского приложения\_offline.htm файл.
- [Веб-публикации обновлений для приложения в автономный режим и usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (блог веб-разработки Майкрософт). Другой вариант для автоматического использования приложения\_offline.htm файл.
- [Веб-развертывание RTW-версии 3.5](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net сайт). Новая функция в развертывание 3.5 Web для пользовательского приложения\_offline.htm файлов.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Развертывание базы данных или изменения в базу данных как часть развертывания веб-приложений

- [Настройка развертывания базы данных в Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Общие сведения о вариантах развертывания базы данных с веб-проекта.
- [Веб-развертывание ASP.NET с помощью Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 серии руководств, показано развертывание базы данных, используя поставщик dbDacFx и Entity Framework Code First Migrations.
- [Практическое: развертывание веб-публикации проекта с помощью одним щелчком в Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Развертывание безопасного приложения ASP.NET MVC 5 с членством, OAuth и базой данных SQL на Windows Azure веб-сайт](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Обширное руководство, который создает и развертывает приложение, которое используется один сервер SQL для базы данных для членства и данных приложения.
- [Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). 12, часть цикла руководств показано, как для развертывания базы данных SQL Server Compact и способы перехода с SQL Server Compact до полного выпуска SQL Server.

См. также развертывание веб-приложения путем создания и установки пакет веб-развертывания и развертывания веб-приложения с помощью процесса непрерывной интеграции (CI) ранее на этой странице.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Развертывание базы данных отдельно от развертывания веб-приложений

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Включение данных в проект базы данных SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (блог группы SQL Server Data Tools). Как развернуть схему и данные при развертывании базы данных.
- [Как развернуть базу данных в Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (сайт Microsoft Azure)
- [Миграция баз данных в базе данных SQL Azure (прежнее название — SQL Azure) для Windows](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Миграция базы данных SQL Azure с помощью SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (блог группы SQL Server Data Tools).
- [Перенос ориентированных на данные приложений в Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Миграция баз данных SQL Server в базу данных Azure SQL Windows](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Развертывание веб-приложения, который использует приложение ASP.NET службах, таких как членство и профилирование

- [Развертывание безопасного приложения ASP.NET MVC 5 с членством, OAuth и базой данных SQL на Windows Azure веб-сайт](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Обширное руководство, который создает и развертывает приложение, которое используется один сервер SQL для базы данных для членства и данных приложения.
- [ASP.NET Identity](https://asp.net/identity/). Ресурсы для ASP.NET Identity.
- [Веб-развертывание ASP.NET с помощью Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12, часть цикла руководств показано, как развернуть базу данных членства ASP.NET.
- [Настройка веб-сайта, использующего службы приложений](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Для веб-сайта проектов но это также касается проектов веб-приложений.
- [Пользователи и роли на рабочем веб-сайте](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Для веб-сайта проектов но это также касается проектов веб-приложений.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Предварительная компиляция для развертывания

- [Обзор предварительной компиляции проекта приложения ASP.NET Web](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Упаковка и публикация проекта веб-Tab, свойства проекта](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Advanced предварительной компиляции диалоговое окно параметров](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Развертывание веб-приложения интрасети

- [Используйте параметр локальной аутентификации в организации (AD FS) с ASP.NET в Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (блог по Витторио Берточчи.).
- [Создание сайта интрасети с использованием ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Более старые записанный пошагового руководства для Visual Studio 2010, не отражает значительных изменений в шаблоны проектов интрасети, появившихся в Visual Studio 2013.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Автоматизация основных задач развертывания, которые не обрабатываются автоматически без дополнительной настройки

- [Веб-развертывание ASP.NET с помощью Visual Studio: развертывание дополнительных файлов](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Установка разрешений для папки на веб-публикация](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (блог Саид Хашими).
- [Как увеличить размер файла целевых объектов, чтобы включить параметры реестра для проекта веб-пакет](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (блог средства веб-разработки).
- [Расширение преобразования XML (Web.config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (блог Саид Хашими). Показано, как для создания пользовательских преобразований XDT.
- [Веб-средства развертывания (MSDeploy) настраиваемый поставщик Take 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (блог Саид Хашими). В этой статье демонстрируется создание пользовательского поставщика веб-развертывания.
- [Как упаковать и развернуть компоненты COM](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (блог средства веб-разработки).
- [Как сборки .NET пакетов](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (блог средства веб-разработки). Как развертывать сборки в глобальный кэш сборок.
- [В сценарии все: Initialize вашей виртуальной Машине Windows Azure для вашей веб-сервера IIS, веб-развертывания и другие материалы](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (Тугберк Ugurlu блог).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Настройка веб-серверов, таким образом, разработчики могут развернуть веб-приложений к ним с помощью веб-развертывания

- [Установку и настройку веб-развертывание для администраторов и без прав администратора развертываний](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net сайт).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Настройка серверов для поставщика услуг размещения

- [Руководство по развертыванию размещение Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (Центр загрузки Майкрософт).
- [Создать файл XML профиля](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net сайт).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Устранение проблем с развертыванием

- [Устранение неполадок веб-сайтов Windows Azure в Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (сайт Microsoft Azure).
- [Веб-развертывание ASP.NET с помощью Visual Studio: Устранение неполадок](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Разрешение распространенных проблем с веб-развертывание](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Веб-развертывание коды ошибок](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net сайт).
- [Веб-развертывания часто задаваемые вопросы о Visual Studio и ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Основные различия между IIS и ASP.NET Development Server](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Распространенные различия конфигурации между средами разработки и эксплуатации](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Размещение приложений ASP.NET в со средним уровнем доверия](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guys с сайта Rolla).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Получение справки с помощью вопроса конкретного развертывания

- [Форум по конфигурации ASP.NET и развертывания](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Дополнительные ресурсы

Этот раздел содержит ссылки на дополнительные ресурсы, полезные Дополнительные сведения о том, как использовать средства развертывания Visual Studio и IIS.

Следующих блогах часто содержат сведения о веб-развертывание в Visual Studio.

- [Веб-инструменты для разработки в блоге Майкрософт](https://blogs.msdn.com/b/webdevtools/).
- [Блог саид Хашими](http://www.sedodream.com/).

Следующие ресурсы содержат документацию по веб-развертывания, платформу IIS, который используется для выполнения задачи развертывания проекта веб-приложений Visual Studio. Вы можете задавать вопросы о веб-развертывания в [форум средство веб-развертывания](https://go.microsoft.com/fwlink/?LinkId=149411) на веб-узле IIS.net.

- [Общие сведения о веб-развертывание](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Установка и настройка веб-развертывание](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Скрипты PowerShell для автоматизации Web развертывание установки](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Средство веб-развертывания](https://go.microsoft.com/fwlink/?LinkId=151481). Таблица верхнего уровня содержимого узла для веб-развертывания документации на сайте TechNet. Включает в себя полезную справочную информацию, но большая часть TechNet, страницы не были обновлены для лет.
- [Пространство имен Microsoft.Web.Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). Документация по API, не была обновлена со времен версии 1.0.
- [В блоге группы развертывания Microsoft Web](https://blogs.iis.net/msdeploy/default.aspx).
- [Вкладка Публикация на веб-узле IIS.net](https://www.iis.net/learn/publish).
