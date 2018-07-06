---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: задании разрешений для папок - 6, 12 | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 50b67c20ee374f1c7ab846446f836411132ddf78
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803685"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: задании разрешений для папок - 6, 12
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, если установить обновление веб-публикации. Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-приложения службы приложений Azure, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

В этом руководстве вы установите разрешения для *Elmah* папку в развернутой веб-сайта, чтобы приложения могли создавать файлы журналов в этой папке.

Если вы тестируете веб-приложения в Visual Studio с помощью Visual Studio Development Server (Cassini), то приложение запущено под вашу личность. Вы являетесь скорее администратора на компьютере разработчика и имеет полный полномочия на ничего делать, чтобы любой файл в любой папке. Но если приложение выполняется под управлением служб IIS, выполняется с удостоверением, определенных для пула приложений, назначенный сайт. Обычно это системный учетной записи с ограниченными разрешениями. По умолчанию он разрешения read и execute для приложения веб-файлов и папок, но он не имеет доступ на запись.

Это становится проблемой, если приложение создает или файлы обновлений, которые достаточно распространена в веб-приложениях. В приложении университета Contoso, Elmah создает XML-файлы в *Elmah* папку для сохранения сведений об ошибках. Даже если вы не используете примерно Elmah, сайт может разрешить пользователям загружать файлы или выполнять другие задачи, которые записывают данные в папку на узле.

Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Тестирование регистрации ошибок и создания отчетов

Чтобы увидеть, как приложение не работает правильно в службах IIS (несмотря на то, что и при тестировании в Visual Studio), может вызвать ошибку, которая обычно регистрируется, Elmah, а затем откройте журнал ошибок Elmah, чтобы просмотреть подробные сведения. Если Elmah не удалось создать XML-файл и сохранить сведения об ошибке, вы увидите пустой отчет.

Откройте браузер и перейдите к `http://localhost/ContosoUniversity`, а затем запросить недопустимый URL-адрес, например *Studentsxxx.aspx*. Отобразится страница ошибки, сформированные системой вместо *GenericErrorPage.aspx* странице, так как `customErrors` параметр в файле Web.config является «RemoteOnly» и службы IIS выполняются локально:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Теперь запустите *Elmah.axd* для просмотра отчета об ошибках. Так как не удалось создать XML-файл в Elmah, отобразится страница журнала пустой ошибки *Elmah* папку:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Параметр иметь разрешения на запись в папку Elmah

Можно вручную задать разрешения для папки или его можно сделать автоматически в процессе процесс развертывания. Делая автоматического требуется сложный код MSBuild и так, как только, что необходимо сделать это при первом развертывании, этот учебник только показано, как сделать это вручную. (Сведения о том, как сделать эту часть процесса развертывания, см. в разделе [установку разрешений на папку на веб-публикация](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) в блоге Саид Хашими.)

В **Windows Explorer**, перейдите к *C:\inetpub\wwwroot\ContosoUniversity*. Щелкните правой кнопкой мыши *Elmah* папку, выберите **свойства**, а затем выберите **безопасности** вкладки.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Если вы не видите **DefaultAppPool** в **имена групп или пользователей** списка, вы, вероятно, либо другим методом, отличной от указанной в этом руководстве для настройки используется IIS и ASP.NET 4 на вашем компьютере. В этом случае следует узнать, какие учетные данные используются пулом приложений, назначенных для приложения университета Contoso и предоставить разрешение на запись к этим удостоверением. См. по ссылкам о удостоверения пула приложений в конце этого руководства.)

Нажмите кнопку **Правка**. В **разрешения для Elmah** установите флажок **DefaultAppPool**, а затем выберите **записи** флажок в **Разрешить** столбца.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Нажмите кнопку **ОК** в обоих диалоговых окнах.

## <a name="retesting-error-logging-and-reporting"></a>Тестирование регистрации ошибок и создания отчетов

Проверьте, что вызывает ошибку снова таким же образом (запрос Неверный URL-адрес) и запустите **журнал ошибок** страницы. Это время ошибки отображается на странице.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Также необходимо разрешение на запись на *приложения\_данных* папку, так как у SQL Server Compact файлы базы данных в этой папке, и вы хотите иметь возможность обновлять данные в этих базах данных. В этом случае, тем не менее, не нужно выполнять никаких дополнительных компонентов так, как процесс развертывания автоматически устанавливает разрешения на запись на *приложения\_данных* папки.

Вы завершили все задачи, необходимые для получения университета Contoso работает надлежащим образом в IIS на локальном компьютере. В следующем учебном курсе будет сделать общедоступным сайта, развернув ее у поставщика услуг размещения.

## <a name="more-information"></a>Дополнительные сведения

В этом примере причина, почему не удается сохранить файлы журнала Elmah был довольно очевидным. Трассировку IIS можно использовать в случаях, когда причиной проблемы не столь очевидно; см. в разделе [устранения неполадок запросов с помощью сбойных в IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) на веб-узле IIS.net.

Дополнительные сведения о том, как предоставить разрешения для удостоверения пула приложений см. в разделе [удостоверения пула приложений](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) и [безопасный контент в IIS через файл списки управления доступом системы](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) на веб-узле IIS.net.

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
