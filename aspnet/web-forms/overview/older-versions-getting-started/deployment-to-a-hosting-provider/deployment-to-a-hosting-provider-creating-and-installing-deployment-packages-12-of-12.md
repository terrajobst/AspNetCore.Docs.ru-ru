---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: Устранение неполадок (12, 12) | Документация Майкрософт'
author: tdykstra
description: Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: bc3a412820638b347d2e5781c01481f622dd2033
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376353"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: Устранение неполадок (12, 12)
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Этой серии руководств показано, как развернуть (публикации) ASP.NET проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, если установить обновление веб-публикации. Введение в серии, см. в разделе [в первом учебнике серии](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны компоненты развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, содержит сведения о развертывании выпусков SQL Server, отличных от SQL Server Compact и содержит сведения о развертывании веб-сайтов Azure для Windows, см. в разделе [веб-развертывание ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


На этой странице описываются некоторые общие проблемы, которые могут возникнуть при развертывании веб-приложения ASP.NET с помощью Visual Studio. Для каждого из них предоставляются один или несколько возможных причин, так и соответствующие решения.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Ошибка сервера в приложении «/» - текущий особых параметров ошибок не сведения об ошибке отображать удаленно

### <a name="scenario"></a>Сценарий

После развертывания узла к удаленному узлу, вы получаете сообщение об ошибке, которая упоминается параметр customErrors в файле Web.config, но не указывает фактические причиной ошибки было:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

По умолчанию ASP.NET отображает подробные сведения об ошибке, только в том случае, если веб-приложение выполняется на локальном компьютере. Обычно не нужно отображать подробные сведения об ошибке, когда веб-приложения находится в открытом доступе в Интернете, поскольку хакеров, можно попытаться использовать эту информацию для обнаружения уязвимостей приложения. Тем не менее при развертывании обновления сайта или на сайт, иногда возникают проблемы, и вам нужно получить фактическое сообщение об ошибке.

Чтобы включить приложение для отображения подробных сообщений об ошибках при выполнении на удаленном узле, измените файл Web.config, чтобы задать `customErrors` режим отключен, повторное развертывание приложения и снова запустить приложение:

1. Если файл Web.config приложения `customErrors` элемент в `system.web` элемент, изменение `mode` атрибут «OFF». В противном случае добавьте `customErrors` элемент в `system.web` элемент с `mode` атрибут «OFF», как показано в следующем примере:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Разверните приложение.
3. Запустите приложение и повторите все, что раньше, возникает ошибка. Теперь вы увидите, что такое фактическое сообщение об ошибке.
4. После разрешения ошибки, восстановите исходные `customErrors` задание и повторного развертывания приложения.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Отказано в доступе на веб-странице, использует SQL Server Compact

### <a name="scenario"></a>Сценарий

При развертывании сайта, использующего SQL Server Compact и запустить страницу развернутого веб-сайта, обращается к базе данных, вы увидите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Необходимо иметь возможность читать SQL службы Compact собственные двоичные файлы, которые находятся в учетной записи СЕТЕВОЙ службы на сервере *bin\amd64* или *bin\x86* папки, но не имеют разрешения на чтение для этих папок. Набор разрешение на чтение данных для СЕТЕВОЙ службы *bin* папки, убедитесь, что предоставить разрешение на вложенные папки.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Не удается прочитать файл конфигурации из-за недостаточных разрешений

### <a name="scenario"></a>Сценарий

При нажатии кнопки в Visual Studio кнопка "Опубликовать" для развертывания приложения в IIS на локальном компьютере, публикация завершается ошибкой и **вывода** окне отображается сообщение об ошибке следующего вида:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Для использования одним щелчком публикация в IIS на локальном компьютере, необходимо использовать Visual Studio с правами администратора. Закройте Visual Studio и перезапустите его с правами администратора.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Не удалось подключиться к конечному компьютеру... С помощью указанного процесса

### <a name="scenario"></a>Сценарий

При нажатии кнопки в Visual Studio кнопка "Опубликовать" для развертывания приложения, публикация завершается ошибкой и **вывода** окне отображается сообщение об ошибке следующего вида:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Прокси-сервер прерывание связи с целевым сервером. С помощью панели управления Windows или в Internet Explorer, выберите **обозревателя** и выберите **подключений** вкладки. В **свойства Интернета** диалоговом окне щелкните **параметры локальной сети**. В **параметры локальной сети (LAN)** снимите флажок **автоматическое определение параметров** флажок. Нажмите кнопку "Опубликовать".

Если проблема сохранится, обратитесь к системному администратору, чтобы определить, что можно сделать с помощью параметров прокси-сервера или брандмауэра. Проблема возникает, так как веб-развертывание использует нестандартный порт для развертывания службы веб-управления (8172); для других соединений веб-развертывание использует порт 80. При развертывании стороннего поставщика услуг размещения, вы обычно используете веб-службы управления.

## <a name="default-net-40-application-pool-does-not-exist"></a>По умолчанию .NET 4.0 приложения пул не существует

### <a name="scenario"></a>Сценарий

Когда вы развертываете приложение, которому требуется .NET Framework 4, вы увидите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

ASP.NET 4 не установлена в IIS. Если сервер, который вы развертываете используется компьютер разработчика, и на нем установлен Visual Studio 2010, ASP.NET 4 установлен на компьютере, но не могут быть установлены в службах IIS. На сервере, где выполняется развертывание откройте командную строку с повышенными правами и установите ASP.NET 4 в службах IIS, выполнив следующие команды:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Кроме того, может потребоваться вручную задать версию .NET Framework по умолчанию пула приложений. Дополнительные сведения см. в разделе [развертывание в IIS в качестве тестовой среды](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) руководства.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Формат строки инициализации не соответствует спецификации, начиная с индекса 0.

### <a name="scenario"></a>Сценарий

После развертывания приложения с помощью одного щелчка публикации, когда вы запустите страницу, которая обращается к базе данных, вы получаете следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Откройте *Web.config* файла в развернутого сайта и проверьте, начинаются ли значения строки подключения с `$(ReplacableToken_`, как показано в следующем примере:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Если строки подключения будет выглядеть как в следующем примере, измените файл проекта и добавьте следующее свойство `PropertyGroup` элемент, для всех конфигураций сборки:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Затем повторно разверните приложение.

## <a name="http-500-internal-server-error"></a>HTTP 500 Внутренняя ошибка сервера

### <a name="scenario"></a>Сценарий

При запуске развернутого веб-сайта, вы увидите следующее сообщение об ошибке без конкретных сведений, указывающее, причина ошибки:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Существует множество причин ошибки с кодом 500, но одной из возможных причин, если вы следуете этих учебников — это поместить XML-элемент в неправильном месте в одном из файлов преобразования XML. Например, может появиться следующая ошибка ыло преобразование, которое вставляет `<location>` элемента под `<system.web>` вместо непосредственно под `<configuration>`. В этом случае рекомендуется исправить преобразования XML-файл и повторного развертывания.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 Внутренняя ошибка сервера

### <a name="scenario"></a>Сценарий

При запуске развернутого веб-сайта, вы увидите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Сайт, вы произвели целевых объектов, ASP.NET 4, но ASP.NET 4 не зарегистрирован в службах IIS на сервере. На сервере откройте командную строку с повышенными правами и зарегистрируйте ASP.NET 4, выполнив следующие команды:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Кроме того, может потребоваться вручную задать версию .NET Framework по умолчанию пула приложений. Дополнительные сведения см. в разделе [развертывание в IIS в качестве тестовой среды](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) руководства.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Ошибка входа в систему Открытие SQL Server Express базы данных в приложении\_данных

### <a name="scenario"></a>Сценарий

Можно обновить *Web.config* файл строки подключения для указания на базу данных SQL Server Express в качестве *.mdf* файлов в вашей *приложения\_данных* папку, а первая При запуске приложения, которое вы видите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Имя *.mdf* файла не может совпадать с именем любой базы данных SQL Server Express, когда-либо выпущенной на компьютере, даже если вы удалили *.mdf* файл ранее существующей базы данных. Измените имя *.mdf* файл с именем, который еще не использовался в качестве имени базы данных и изменить *Web.config* файл для использования нового имени. Кроме того, можно использовать [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) для удаления ранее существующий SQL Server Express баз данных.

## <a name="model-compatibility-cannot-be-checked"></a>Модель совместимости нельзя проверить

### <a name="scenario"></a>Сценарий

Можно обновить *Web.config* файл строки подключения для указания в новой базе данных SQL Server Express, и при первом запуске приложения вы увидите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Если имя базы данных, помещаемых в файле Web.config использовался, когда-либо прежде, чем на компьютере, базы данных могут существовать с некоторые таблицы в ней. Выберите новое имя, которое не используется на компьютере, прежде чем и изменение *Web.config* в файле для использования этого нового имени базы данных. Кроме того, можно использовать [Express служебной программы SQL Server](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) или [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) для удаления существующей базы данных.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Ошибка SQL, когда сценарий пытается создать пользователей или ролей

### <a name="scenario"></a>Сценарий

При использовании развертывания базы данных, настроенный на **упаковка и публикация SQL** вкладке скрипты SQL, которые выполняются во время развертывания включают команды Create User или Create Role и происходит сбой выполнения сценария при выполнении этих команд. Вы увидите, что более подробные сообщения, такие как следующие:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Если эта ошибка возникает, когда вы настроили развертывание базы данных в **веб-публикации** мастера, а не **упаковка и публикация SQL** создайте поток в [конфигурации и Развертывание](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) форум и решения будут добавляться на эту страницу по устранению неполадок.

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Учетная запись пользователя, которая использовалась для выполнения развертывания имеет разрешение на создание пользователей или ролей. Например, могут быть назначены размещающей компании `db_datareader`, `db_datawriter`, и `db_ddladmin` ролей с учетной записью пользователя, которая настраивает для вас. Это достаточно для создания большинства объектов базы данных, но не для создания пользователей или ролей. Одним из способов, чтобы избежать этой ошибки является исключение пользователей и роли из развертывания базы данных. Это можно сделать, изменив `PreSource` элемент для базы данных автоматически созданного скрипта таким образом, чтобы он включал следующие атрибуты:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Сведения о том, как изменить `PreSource` элемент в файле проекта, см. в разделе [как: изменение параметров развертывания в файле проекта](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Если пользователи или роли в базе данных разработки должны находиться в целевой базе данных, обратитесь в службу поставщика услуг размещения для получения помощи.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Ошибка времени ожидания SQL Server при выполнении пользовательских сценариев во время развертывания

### <a name="scenario"></a>Сценарий

Вы указали какие-либо пользовательские сценарии SQL для выполнения во время развертывания, и при веб-развертывания выполняет их, они истекло время ожидания.

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Выполнение нескольких сценариев, которые имеют различные транзакции режима может вызвать ошибки времени ожидания. По умолчанию автоматически создаваемые скрипты запускаются в транзакции, но пользовательские сценарии — нет. При выборе **запрос данных или схемы из существующей базы данных** параметр **упаковка и публикация SQL** вкладки, и при добавлении пользовательского скрипта SQL, необходимо изменить параметры транзакции некоторые скрипты, чтобы все сценарии используют одинаковые параметры транзакции. Дополнительные сведения см. в разделе [как: развертывание базы данных с проектом веб-приложения](https://msdn.microsoft.com/library/dd465343.aspx).

Если вы настроили параметры транзакции, таким образом, чтобы все совпадают, но по-прежнему получить эту ошибку, возможное решение — это выполнить сценарии отдельно. В **скрипты базы данных** сетки в **упаковка и публикация** вкладка SQL, снимите флажок **Include** флажок для скрипта, которые вызывают ошибку времени ожидания, затем опубликуйте проект. Затем перейдите обратно в **скрипты базы данных** сетки, выберите этот скрипт **Include** и снимите **Include** флажки для других скриптов. Затем опубликуйте проект еще раз. Это время, при публикации, выполняется только выбранный пользовательский скрипт.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Stream данных манифеста сайта еще не доступен

### <a name="scenario"></a>Сценарий

При установке пакета с помощью *deploy.cmd* файл с `t` (тест) вы увидите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Сообщение об ошибке означает, что команда не удалось сформировать отчет о тестировании. Тем не менее, команда может выполняться, если используется `y` (фактическая установка). Сообщение указывает только, что возникла проблема с помощью команды в тестовом режиме.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Для этого приложения требуется ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Сценарий

При попытке выполнить развертывание, вы увидите следующее сообщение об ошибке:

 Ошибка: Поток данных "sitemanifest/dbFullSql [@path= «C:\TEMP\AdventureWorksGrant.sql']/sqlScript» пока недоступен. Пул приложений, который вы пытаетесь использовать со свойством «managedRuntimeVersion» установлено значение «v2.0». Для этого приложения требуется «v4.0». 

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

ASP.NET 4 не установлена в IIS. Если сервер, который вы развертываете используется компьютер разработчика, и на нем установлен Visual Studio 2010, ASP.NET 4 установлен на компьютере, но не могут быть установлены в службах IIS. На сервере, где выполняется развертывание откройте командную строку с повышенными правами и установите ASP.NET 4 в службах IIS, выполнив следующие команды:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Не удается привести Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Сценарий

При развертывании пакета, вы увидите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Вы пытаетесь развернуть из диспетчера служб IIS с помощью веб-развертывание 1.1 Интерфейс на сервере, где установлены Web Deploy 2.0. Если вы используете средства удаленного администрирования IIS для развертывания путем импорта пакета, проверка **доступность новых функций** диалогового окна при создании подключения. (Это диалоговое окно может будет отображаться только один раз при первой установке подключения. Чтобы очистить подключения и начать заново, закройте диспетчер IIS и запустите ее снова, введя `inetmgr /reset` в командной строке.) Если одна из функций в списке является **веб-развертывание интерфейса**и он имеет номер версии ниже, чем 8 развертываются на сервере, возможно, веб-развертывания установлены версии 1.1 и 2.0. Для развертывания из клиента, имеющего 2.0 установлен, сервер должен иметь только Web Deploy 2.0 установлена. Обратитесь к поставщику услуг размещения для решения этой проблемы будет необходимо.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Не удалось загрузить собственные компоненты SQL Server Compact

### <a name="scenario"></a>Сценарий

При запуске развернутого веб-сайта, вы увидите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Не имеет развернутого веб-сайта *amd64* и *x86* вложенные папки с сборки в машинном коде в них в разделе приложения *bin* папки. На компьютере с SQL Server Compact установлена, машинные сборки находятся в *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Чтобы получить нужные файлы в правильные папки в проекте Visual Studio рекомендуется для установки пакета NuGet SqlServerCompact. При установке пакета добавляется сценарий после построения для копирования сборки в машинном коде в *amd64* и *x86*. Чтобы они должны быть развернуты тем не менее, необходимо вручную включить их в проект. Дополнительные сведения см. в разделе [развертывание SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) руководства.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Ошибка «Недопустимый путь», после развертывание приложения Entity Framework Code First

### <a name="scenario"></a>Сценарий

Развертывание приложения, использующего Entity Framework Code First Migrations и СУБД, такие как SQL Server Compact, который сохраняет свою базу данных в файл в приложении\_папку данных. У вас есть Code First Migrations, настроить для создания базы данных после первого развертывания. При запуске приложения вы получаете сообщение об ошибке, как в следующем примере:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Код сначала пытается создать базы данных, но приложение\_папки данных нет. Либо не указаны файлы *приложения\_данных* папку при развертывании, или вы выбрали **исключить приложение\_данных** на **пакета и публикация веб-** вкладке **свойства проекта** окна. Процесс развертывания не создайте папку на сервере, если нет файлов в папке для копирования на сервер. Если у вас уже есть база данных, настроить на сайте, процесс развертывания будут удалены файлы и *приложения\_данных* при выборе папки **удалять дополнительные файлы в месте назначения** в профиль публикации. Чтобы решить проблему, поместите файл-заполнитель как файл txt в *приложения\_данных* папки, убедитесь, что у вас нет **исключить приложение\_данных** выбран и повторное развертывание. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>«Невозможно использовать COM-объект, который был отделен от своего RCW.»

### <a name="scenario"></a>Сценарий

Вы успешно обновлены с помощью быстрой публикации для развертывания приложения и затем вы начнете получать эту ошибку:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Закрыть и перезапустить Visual Studio обычно является все, что требуется для устранения этой ошибки.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Развертывания завершается сбоем из-за пользователя учетные данные использовать для публикации нет setACL центра

### <a name="scenario"></a>Сценарий

Публикация завершается сбоем с ошибкой, указывающей, вы не полномочиями установки разрешений для папки (учетной записи пользователя, которую вы используете нет setACL центра).

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

По умолчанию Visual Studio задает разрешения на корневую папку сайта разрешения чтение и запись в приложении\_папку данных. Если вы знаете, что указаны правильные разрешения по умолчанию для папки сайта и не обязательно должны задаваться, отключить это поведение, добавив **&lt;назначения IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** файл профиля публикации (на одном профиле) или файл wpp.targets (чтобы влияет на все профили). Сведения о том, как изменить эти файлы, см. в разделе [как: изменение параметров развертывания в файлах профиля (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Ошибки запрета доступа, когда приложение пытается выполнить запись в папку приложения

### <a name="scenario"></a>Сценарий

Ошибки приложения при попытке создания или изменения файла в одной из папок приложения, так как он не имеет полномочия записи для этой папки.

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

По умолчанию Visual Studio задает разрешения на корневую папку сайта разрешения чтение и запись в приложении\_папку данных. Если приложению требуется доступ на запись во вложенную папку, можно задать разрешения для этой папки, как показано в [задании разрешений для папок](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) и [развертывание в рабочей среде](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) учебники. Если приложению требуется доступ на запись к корневой папке сайта, вам нужно запретить его задавать доступ только для чтения в корневой папке, добавив **&lt;назначения IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** файл профиля публикации (на одном профиле) или файл wpp.targets (чтобы влияет на все профили). Сведения о том, как изменить эти файлы, см. в разделе [как: изменение параметров развертывания в файлах профиля (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Ошибка конфигурации - атрибут targetFramework ссылается на версию, которая наступает позже, чем установленная версия платформы .NET Framework

### <a name="scenario"></a>Сценарий

Вы успешно опубликовали веб-проекта, предназначенного для ASP.NET 4.5, но при запуске приложения (с `customErrors` режим «OFF», в файле Web.config) возникает следующая ошибка:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

Окно источника ошибки страницы ошибки выделяются из файла Web.config следующую строку как причина ошибки:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Возможные причины и решения

Сервер не поддерживает ASP.NET 4.5. Обратитесь к поставщику услуг размещения для определения времени, и если можно добавить поддержку для ASP.NET 4.5. Если обновление сервера не подходит, необходимо развернуть веб-проект, который предназначен для ASP.NET 4 или более ранней версии вместо нее. Если в одно назначение развертывания ASP.NET 4 или более ранних веб-проект, выберите **удалять дополнительные файлы в месте назначения** флажок на **параметры** вкладке **веб-публикации**мастера. Если вы не выбрали **удалять дополнительные файлы в месте назначения**, вам придется продолжать перейти на страницу ошибки конфигурации.

Проект **свойства** windows включает в себя выберите в раскрывающемся списке требуемой версии, но не может устранить эту проблему, просто указав из **.NET Framework 4.5** для **платформы.NETFramework4**. Если изменить целевую платформу для более ранней версии платформы, проект по-прежнему будет содержать ссылки на сборки более поздней версии framework и не запустится. Необходимо вручную изменить эти ссылки или создайте новый проект, который предназначен для .NET Framework 4 или более ранней версии. Дополнительные сведения см. в разделе [версий .NET Framework для веб-сайтов](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
