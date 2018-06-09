---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: задании разрешений для папок - 6, 12 | Документы Microsoft'
author: tdykstra
description: Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 573e75221a1c0018bded7544e584b0c75f47d607
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "30887269"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: задании разрешений для папок - 6, 12
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, при установке обновления публикации Web. Введение ряда см. в разделе [в первом учебнике ряда](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны возможности развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, показано, как развернуть выпусках SQL Server, отличных от SQL Server Compact и показано, как для развертывания на веб-приложениях службы приложений Azure, в разделе [развертывания веб-приложения ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Обзор

В этом учебнике вы установите разрешения для *Elmah* папки в развернутой веб-сайта, чтобы приложения могли создавать файлы журналов в этой папке.

При тестировании веб-приложения в Visual Studio с помощью Visual Studio Development Server (Cassini), то приложение запущено под вашей личности. Скорее всего являетесь администратором на компьютере разработчика и имеют полный право выполнять в любой файл в любую папку. Однако при выполнении приложения в службах IIS, он выполняется с удостоверением, определенных для пула приложений, назначенный сайт. Обычно это системные учетную запись с ограниченными разрешениями. По умолчанию он разрешения чтение и выполнение для файлов и папок веб-приложения, но он не имеет доступа на запись.

Это становится проблемой, если приложение создает или файлы обновлений, которые обычное требуется в веб-приложениях. В приложении университета Contoso Elmah создает XML-файлы в *Elmah* папку для сохранения сведений об ошибках. Даже если вы не используете примерно Elmah, веб-узла может позволяет пользователям загружать файлы или выполнять другие задачи, которые записывают данные в папку на узле.

Напоминание: Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебник, не забудьте проверить [страницу устранения неполадок](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Проверка регистрации ошибок и отчетов

Чтобы увидеть, как приложение не работает правильно в службах IIS (несмотря на то, что и при его тестировании в Visual Studio), может вызвать ошибку, которая обычно регистрируется Elmah, а затем откройте журнал ошибок Elmah, чтобы просмотреть подробные сведения. Если Elmah не удалось создать XML-файл и сохранить сведения об ошибке, можно увидеть отчет об ошибке пустым.

Откройте браузер и перейдите к `http://localhost/ContosoUniversity`, и запросите недопустимый URL-адрес вида *Studentsxxx.aspx*. Отображается страница ошибки, созданные системой вместо *GenericErrorPage.aspx* страницы, так как `customErrors` параметр в файле Web.config «RemoteOnly» и службы IIS выполняются локально:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Теперь запустите *Elmah.axd* Чтобы просмотреть отчет об ошибке. Появиться страница журнала пустой ошибки, поскольку Elmah не удалось создать XML-файл в *Elmah* папки:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Параметр разрешения на запись для папки Elmah

Можно вручную задать разрешения для папки или его можно сделать автоматически в процессе развертывания. Делая автоматического требует сложного кода MSBuild и поскольку имеется только для этого при первом развертывании, этого учебника только показано, как сделать это вручную. (Сведения о том, как делать этого этапа процесса развертывания см. в разделе [установку разрешений на папку на веб-публикация](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) в блоге Sayed Hashimi.)

В **Проводник**, перейдите к *C:\inetpub\wwwroot\ContosoUniversity*. Щелкните правой кнопкой мыши *Elmah* выберите **свойства**, а затем выберите **безопасности** вкладку.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Если вы не видите **DefaultAppPool** в **имена групп или пользователей** список, возможно, используется другой метод, отличный от указанного в этом учебнике для настройки IIS и ASP.NET 4 на вашем компьютере. В этом случае Узнайте, какие учетные данные, используемые выделенной приложение Contoso университета и предоставить разрешение на запись с этим удостоверением пула приложений. См. по ссылкам о удостоверений пула приложений в конце данного руководства.)

Нажмите кнопку **Правка**. В **разрешения для Elmah** установите флажок **DefaultAppPool**, а затем выберите **записи** флажок в **Разрешить** столбца.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Нажмите кнопку **ОК** в обоих диалоговых окнах.

## <a name="retesting-error-logging-and-reporting"></a>Повторная проверка регистрации ошибок и отчетов

Проверьте, вызывающей ошибку снова таким же образом (запрос Неверный URL-адрес) и запустите **журнал ошибок** страницы. Это время, то ошибка появится на странице.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Также необходимо разрешение на запись на *приложения\_данные* папку, так как SQL Server Compact базы данных файлы в этой папке, и вы хотите иметь возможность обновлять данные в этих базах данных. В этом случае, однако, не нужно выполнять никаких дополнительных компонентов, поскольку процесс развертывания автоматически задает разрешения на запись на *приложения\_данные* папки.

Теперь завершена все задачи, необходимые для подготовки университета Contoso работает должным образом в IIS на локальном компьютере. В следующем уроке будет сделать общедоступными сайта, развертывая ее поставщика услуг размещения.

## <a name="more-information"></a>Дополнительные сведения

В этом примере причина, почему не удалось сохранить файлы журнала Elmah был довольно очевидным. Трассировку IIS можно использовать в случаях, когда причиной проблемы не столь очевидны; в разделе [устранения неполадок запросов с помощью сбойных в IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) на сайте IIS.net.

Дополнительные сведения о том, как предоставить разрешения для удостоверения пула приложений см. в разделе [удостоверений пула приложений](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) и [безопасный контент в IIS через файл списки управления доступом системы](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) на сайте IIS.net.

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Вперед](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
