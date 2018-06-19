---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: Устранение неполадок (12 12) | Документы Microsoft'
author: tdykstra
description: Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2a8342f026498a7cf3ff4a3c158ed177c15b7111
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890389"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Развертывание веб-приложения ASP.NET с SQL Server Compact с помощью Visual Studio или Visual Web Developer: Устранение неполадок (12 12)
====================
по [Tom Dykstra](https://github.com/tdykstra)

[Загрузите начальный проект](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Ряд учебниках показано развертывание ASP.NET (публикации) проекта веб-приложения, который содержит базу данных SQL Server Compact с помощью Visual Studio 2012 RC или Visual Studio Express 2012 RC для Web. Также можно использовать Visual Studio 2010, при установке обновления публикации Web. Введение ряда см. в разделе [в первом учебнике ряда](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Учебник, в котором показаны возможности развертывания, появившиеся после выпуска версии-КАНДИДАТА Visual Studio 2012, показано, как развернуть выпусках SQL Server, отличных от SQL Server Compact и показано, как развертывание веб-сайтов Azure для Windows, в разделе [развертывания веб-приложения ASP.NET с помощью Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


На этой странице описаны некоторые общие проблемы, которые могут возникнуть при развертывании веб-приложения ASP.NET с помощью Visual Studio. Для каждой из них приведены возможные причины ее возникновения и соответствующего решения.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Ошибка сервера в приложении '/' - текущая особая настройка ошибок не сведения об ошибке отображать удаленно

### <a name="scenario"></a>Сценарий

После развертывания узла к удаленному узлу, возникает сообщение об ошибке, указывается параметр customErrors в файле Web.config, но не указывает, было действительную причину ошибки:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

По умолчанию ASP.NET отображает подробные сведения об ошибке, только если веб-приложение выполняется на локальном компьютере. Обычно не нужно отображать подробные сведения об ошибке, если веб-приложение находится в открытом доступе через Интернет, возможно, злоумышленники могут использовать эти сведения для обнаружения уязвимостей приложения. Однако при развертывании обновления сайта или сайта иногда что-то работает неправильно, и вам необходимо получить реальное сообщение об ошибке.

Чтобы включить приложение для отображения подробных сообщений об ошибках, когда оно работает на удаленном узле, измените файл Web.config для задания `customErrors` режим отключен, повторного развертывания приложения и снова запустить приложение:

1. Если в файле Web.config приложения `customErrors` элемент в `system.web` элемент, изменение `mode` атрибут «OFF». В противном случае добавьте `customErrors` элемент в `system.web` элемент с `mode` атрибут «OFF», как показано в следующем примере:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Развертывание приложения.
3. Запустите приложение и повторите все, что это было ранее, причиной возникновения ошибок. Теперь можно увидеть, что такое реальное сообщение об ошибке.
4. После разрешения ошибки, восстановите исходное `customErrors` задание и повторного развертывания приложения.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Отказано в доступе на веб-странице, использует SQL Server Compact

### <a name="scenario"></a>Сценарий

При развертывании сайта, использующего SQL Server Compact и страница запускается в развернутом сайте, который обращается к базе данных, отображается следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Учетную запись СЕТЕВОЙ службы на сервере должен иметь возможность читать службы SQL Compact собственные двоичные файлы, которые находятся в *bin\amd64* или *bin\x86* папку, но он не имеет разрешения для этих папок. Набор для СЕТЕВОЙ службы на чтение *bin* папки, убедитесь, что предоставить разрешение на вложенные папки.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Не удалось прочитать файл конфигурации из-за недостаточных разрешений

### <a name="scenario"></a>Сценарий

При нажатии кнопки Visual Studio кнопка публикации для развертывания приложения в IIS на локальном компьютере, публикация завершается с ошибкой и **вывода** окне отображается сообщение об ошибке следующего вида:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Для использования одним щелчком опубликовать в службах IIS на локальном компьютере, необходимо использовать Visual Studio с правами администратора. Закройте Visual Studio и перезапустите его с разрешениями администратора.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Не удалось подключиться к целевому компьютеру... С помощью указанного процесса

### <a name="scenario"></a>Сценарий

При нажатии кнопки Visual Studio кнопка публикации для развертывания приложения, публикация завершается с ошибкой и **вывода** окне отображается сообщение об ошибке следующего вида:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Прокси-сервер прерывание связи с целевым сервером. С помощью панели управления Windows или в Internet Explorer выберите **обозревателя** и выберите **подключений** вкладки. В **свойства Интернета** диалоговое окно, нажмите кнопку **параметры локальной сети**. В **параметры локальной сети (LAN)** снимите флажок **автоматическое определение параметров** флажок. Нажмите кнопку "Опубликовать".

Если ошибка повторится, обратитесь к системному администратору, чтобы определить, что можно сделать с параметрами прокси-сервера или брандмауэра. Проблема возникает, так как веб-развертывание использует нестандартный порт для развертывания службы веб-управления (8172); для других соединений веб-развертывание использует порт 80. При развертывании стороннего поставщика услуг размещения обычно используется веб-службы управления.

## <a name="default-net-40-application-pool-does-not-exist"></a>По умолчанию .NET 4.0 приложения пул не существует

### <a name="scenario"></a>Сценарий

При развертывании приложения, которое требуется .NET Framework 4, отображается следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

В ASP.NET 4 не установлена в службах IIS. Если сервер, который выполняется развертывание находится на компьютере разработчика, Visual Studio 2010 установлена ASP.NET 4 установлена на компьютере, но не может быть установлен в службах IIS. На сервере, где выполняется развертывание откройте командную строку с повышенными привилегиями и установить ASP.NET 4 в службах IIS, выполнив следующие команды:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Кроме того, может потребоваться вручную установить версию .NET Framework по умолчанию пула приложений. Дополнительные сведения см. в разделе [развертывание в IIS в тестовой среде](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) учебника.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Формат строки инициализации не соответствует спецификации, начиная с индекса 0.

### <a name="scenario"></a>Сценарий

После развертывания приложения с помощью одного щелчка мыши публикации, при запуске страницы, которая обращается к базе данных получить следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Откройте *Web.config* файл в развернутом узле и проверьте, начинаются ли значения строк соединения с `$(ReplacableToken_`, как показано в следующем примере:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Если строки подключения будет выглядеть как в следующем примере, измените файл проекта и добавьте следующее свойство `PropertyGroup` элемент, для всех конфигураций сборки:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Затем повторного развертывания приложения.

## <a name="http-500-internal-server-error"></a>HTTP 500 Внутренняя ошибка сервера

### <a name="scenario"></a>Сценарий

При запуске развернутого сайта без конкретных сведений, причину ошибки, позволяющее определить, появится следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Существует множество причин ошибок 500, но является одной из возможных причин при использовании этих учебников поместить элемент XML, в том месте, в одном из файлов преобразования XML. Например, эта ошибка будет получена при поместить преобразование, которое вставляет `<location>` элемента под `<system.web>` вместо непосредственно под `<configuration>`. Решение — в этом случае исправьте преобразования XML-файл и повторного развертывания.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 Внутренняя ошибка сервера

### <a name="scenario"></a>Сценарий

При запуске развернутого сайта, может появиться следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Узел развертывания целей ASP.NET 4, но ASP.NET 4 не зарегистрирован в службах IIS на сервере. На сервере откройте командную строку с повышенными привилегиями и зарегистрировать ASP.NET 4, выполнив следующие команды:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Кроме того, может потребоваться вручную установить версию .NET Framework по умолчанию пула приложений. Дополнительные сведения см. в разделе [развертывание в IIS в тестовой среде](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) учебника.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Не удалось выполнить вход открытия экспресс базы данных SQL Server в приложении\_данных

### <a name="scenario"></a>Сценарий

Вам о новостях *Web.config* файла строки подключения, чтобы она указывала на базу данных SQL Server Express в качестве *.mdf* файла в вашей *приложения\_данные* папки и первым время запуска приложения, которые вы видите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Имя *.mdf* файла не может совпадать с именем любой базы данных SQL Server Express, когда-либо существует на компьютере, даже если вы удалили *.mdf* файл ранее существующей базы данных. Измените имя *.mdf* файл имени, которое никогда не используется в качестве имени базы данных и изменение *Web.config* файл для использования нового имени. В качестве альтернативы можно использовать [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) для удаления существующего ранее SQL Server Express баз данных.

## <a name="model-compatibility-cannot-be-checked"></a>Модель совместимости нельзя проверить

### <a name="scenario"></a>Сценарий

Вам о новостях *Web.config* файл строку подключения для указания в новой базе данных SQL Server Express и при первом запуске приложения вы увидите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Если имя базы данных, поместить в файл Web.config когда-либо использовался прежде, чем на компьютере, базы данных могут существовать с некоторые таблицы в ней. Выберите новое имя, которое не используется на компьютере, прежде чем и изменение *Web.config* в файле для использования этого нового имени базы данных. В качестве альтернативы можно использовать [Express служебной программы SQL Server](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) или [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) для удаления существующей базы данных.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Ошибка SQL при попытке создания пользователей или ролей сценария

### <a name="scenario"></a>Сценарий

Вы используете развертывания базы данных, настроенные на **упаковка и публикация SQL** вкладке скрипты SQL, которые выполняются во время развертывания включают команды Create User или Create Role и происходит сбой выполнения сценария при выполнении этих команд. Возможны более подробные сообщения, например следующие:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Если эта ошибка возникает при настройке развертывания базы данных в **веб-публикация** мастера, а не **упаковка и публикация SQL** создайте поток в [конфигурации и Развертывание](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) форум и решение будет добавлен на эту страницу по устранению неполадок.

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Учетная запись пользователя, которая использовалась для выполнения развертывания не имеет разрешения на создание пользователей или ролей. Например, назначить хостинга `db_datareader`, `db_datawriter`, и `db_ddladmin` ролей для учетной записи пользователя, он устанавливает для вас. Это достаточно для создания большинства объектов базы данных, но не для создания пользователей или ролей. Один из способов избежать этой ошибки является, исключив из развертывания базы данных пользователей и ролей. Это можно сделать, изменив `PreSource` элемент для базы данных автоматически созданного скрипта, чтобы он включал следующие атрибуты:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Сведения о редактировании `PreSource` в файле проекта см. в разделе [как: изменение параметров развертывания в файле проекта](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Если пользователи или роли в базе данных разработки должны быть в целевой базе данных, обратитесь в службу поставщика услуг размещения для получения помощи.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Ошибка времени ожидания SQL Server при выполнении скриптов во время развертывания

### <a name="scenario"></a>Сценарий

Вы указали пользовательские сценарии SQL для выполнения во время развертывания и веб-развертывания их, запускаемый истечения интервала ожидания.

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

При выполнении нескольких скриптов, имеющих режимы другой транзакции может привести к ошибкам времени ожидания. По умолчанию автоматически создаваемые скрипты запускаются в транзакции, но не пользовательские сценарии. При выборе **данных и (или) схему из существующей базы данных по запросу** параметр **упаковка и публикация SQL** вкладку, и при добавлении пользовательского скрипта SQL, необходимо изменить параметры транзакции некоторых скриптов, чтобы все скрипты используют одинаковые параметры транзакции. Дополнительные сведения см. в разделе [как: развертывание базы данных с проектом веб-приложения](https://msdn.microsoft.com/library/dd465343.aspx).

Если настроены параметры транзакции, таким образом, все же, но по-прежнему получить эту ошибку, возможное решение этой проблемы является запуск скриптов отдельно. В **скриптов базы данных** сетки в **упаковка и публикация** очистить вкладку SQL **Include** флажок для сценария, который приводит к возникновению ошибки времени ожидания, Опубликуйте проект. Затем перейдите обратно в **скриптов базы данных** сетки, выберите этот скрипт **Include** флажок и снимите **Include** флажки для других сценариев. Затем опубликуйте проект. Это время, при публикации выбранного пользовательского скрипта выполняется.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Потоковые данные манифеста сайта еще не доступен

### <a name="scenario"></a>Сценарий

При установке пакета с помощью *deploy.cmd* файл с `t` (тест) вы видите следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Сообщение об ошибке означает, что команда не удалось сформировать отчет теста. Тем не менее, команда может выполняться, если используется `y` (фактическая установка). Данное сообщение указывает только на проблему при выполнении команды в тестовом режиме.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Для этого приложения требуется ManagedRuntimeVersion версии 4.0

### <a name="scenario"></a>Сценарий

При попытке развертывания отображается следующее сообщение об ошибке:

 Ошибка: Поток данных "sitemanifest/dbFullSql [@path= «C:\TEMP\AdventureWorksGrant.sql']/sqlScript» пока недоступен. Пул приложений, который вы пытаетесь использовать имеет значение свойства «managedRuntimeVersion» значение «v2.0». Для этого приложения требуется «v4.0». 

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

В ASP.NET 4 не установлена в службах IIS. Если сервер, который выполняется развертывание находится на компьютере разработчика, Visual Studio 2010 установлена ASP.NET 4 установлена на компьютере, но не может быть установлен в службах IIS. На сервере, где выполняется развертывание откройте командную строку с повышенными привилегиями и установить ASP.NET 4 в службах IIS, выполнив следующие команды:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Невозможно выполнить приведение типа Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Сценарий

При развертывании пакета, отображается следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Вы пытаетесь развернуть из диспетчера служб IIS с помощью веб-развертывания 1.1 интерфейса на сервере, где установлена веб-развертывание 2.0. Если вы используете средства удаленного администрирования IIS для развертывания путем импорта пакета, проверка **новые возможности, доступные** диалоговое окно при установлении соединения. (Это диалоговое окно предназначено может будет отображаться только один раз при первой установке подключения. Чтобы очистить подключение и начать заново, закройте диспетчер IIS и запустить его снова, указав `inetmgr /reset` в командной строке.) Если одна из функций в списке является **развертывание веб-интерфейса пользователя**и он имеет номер версии меньше 8, развертываются на сервере может иметь веб-развертывания установлены версии 1.1 и 2.0. Для развертывания из клиент, имеющий 2.0 установлен, сервер должен иметь только Web Deploy 2.0 установлена. Необходимо связаться с поставщиком услуг размещения для решения этой проблемы.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Не удается загрузить собственные компоненты для SQL Server Compact

### <a name="scenario"></a>Сценарий

При запуске развернутого сайта, может появиться следующее сообщение об ошибке:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Развернутые сайт не имеет *amd64* и *x86* подпапки с сборки в машинном коде в их в приложения *bin* папки. На компьютере с SQL Server Compact установлена, машинные сборки находятся в *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Чтобы получить нужные файлы в нужных папках в проекте Visual Studio рекомендуется установить пакет NuGet SqlServerCompact. Установка пакета добавляет скрипт после построения, чтобы скопировать сборки в машинном коде в *amd64* и *x86*. Чтобы они должны быть развернуты тем не менее, необходимо вручную включить их в проект. Дополнительные сведения см. в разделе [развертывание SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) учебника.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Ошибка «Недопустимый путь» после развертывания приложения Entity Framework Code First

### <a name="scenario"></a>Сценарий

При развертывании приложения, использующий Entity Framework Code First Migrations и СУБД, такие как SQL Server Compact, который сохраняет свою базу данных в файл в приложении\_папки данных. У вас есть Code First Migrations настроен для создания базы данных после первого развертывания. При запуске приложения вы получаете сообщение об ошибке, как в следующем примере:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Код сначала предпринимается попытка создать базы данных, но приложение\_данных папка не существует. Либо не указаны файлы *приложения\_данные* папку при развертывании или выбранных **исключить приложения\_данные** на **Пакет/Публикация веб-сайта** вкладке **свойства проекта** окна. Процесс развертывания не создайте папку на сервере, если нет файлов в папке для копирования на сервер. Если уже имеется Настройка узла базы данных, процесс развертывания будут удалены файлы и *приложения\_данные* папку, если вы выбрали **удалить дополнительные файлы в месте назначения** в профиль публикации. Устранить неполадки, поместить файл-заполнитель, например TXT-файла в *приложения\_данные* папки, убедитесь, что у вас **исключить приложения\_данные** выбран и повторного развертывания. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>«Нельзя использовать COM-объект, который был отделен от своего базового RCW.»

### <a name="scenario"></a>Сценарий

Вы успешно созданы с помощью одного щелчка мыши публикации для развертывания приложения и затем запустить получение ошибки:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Закрытие и перезапустите Visual Studio обычно является все, что необходимо для устранения этой ошибки.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Развертывание завершается сбоем из-за пользователя учетные данные используются для публикации нет setACL центр

### <a name="scenario"></a>Сценарий

Публикации завершится с ошибкой, указывающей, вы не обладают правами задать разрешения на папки (учетная запись пользователя, которую вы используете не имеет полномочия setACL).

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

По умолчанию Visual Studio задает разрешения для корневой папки сайта разрешения чтения и записи в приложении\_папки данных. Если вы знаете, что указаны правильные разрешения по умолчанию для папки сайта и не требуется задать, это поведение можно отключить, добавив **&lt;назначения IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** файл профиля публикации (устанавливаемого в одном профиле) или файл wpp.targets (чтобы влияет на все профили). Сведения о редактировании этих файлов см. в разделе [как: изменение параметров развертывания в файлах профиля (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Когда приложение пытается выполнить запись в папку приложения, ошибок отказа в доступе

### <a name="scenario"></a>Сценарий

Ошибки приложения при попытке создания или изменения файла в одной из папок приложения, так как он не имеет полномочий на запись для этой папки.

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

По умолчанию Visual Studio задает разрешения для корневой папки сайта разрешения чтения и записи в приложении\_папки данных. Если приложению требуется доступ на запись к вложенной папке, можно задать разрешения для этой папки, как показано в [задании разрешений для папок](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) и [развертывание в рабочей среде](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) учебники. Если приложению требуется доступ на запись к корневой папке сайта, необходимо предотвратить доступ только для чтения установлен на корневую папку путем добавления **&lt;назначения IncludeSetACLProviderOn&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** файл профиля публикации (устанавливаемого в одном профиле) или файл wpp.targets (чтобы влияет на все профили). Сведения о редактировании этих файлов см. в разделе [как: изменение параметров развертывания в файлах профиля (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Ошибка конфигурации - атрибут targetFramework ссылается на версию, которая является более поздней, чем установленная версия платформы .NET Framework

### <a name="scenario"></a>Сценарий

Успешно опубликован веб-проекта, предназначенного для ASP.NET 4.5, но при запуске приложения (с `customErrors` режим значение «OFF» в файле Web.config) возникает следующая ошибка:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

Поле источника ошибки страницы ошибки перечислены следующие строки из файла Web.config как причина ошибки:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Возможная причина и решение

Сервер не поддерживает ASP.NET 4.5. Обратитесь к поставщику услуг, чтобы определить, когда и можно ли добавить поддержку для ASP.NET 4.5. Если обновление сервера не подходит, необходимо развернуть веб-проекта, предназначенного для ASP.NET 4 или более ранней версии вместо нее. Если в одно назначение развертывания ASP.NET 4 или более ранних веб-проекта, выберите **удалить дополнительные файлы в месте назначения** флажок на **параметры** вкладке **веб-публикация**мастера. Если вы не выбрали **удалить дополнительные файлы в месте назначения**, можно перейти на страницу ошибки конфигурации.

Проект **свойства** windows включает Target framework раскрывающегося списка, но не устраняет эту проблему, просто изменив, из **.NET Framework 4.5** для **платформы.NETFramework4**. Если изменить целевую платформу на более ранней версии framework, проект будет по-прежнему иметь ссылки на сборки в более поздней версии framework и не будет выполняться. Необходимо вручную изменить эти ссылки или создайте новый проект, в платформе .NET Framework 4 или более ранней версии. Дополнительные сведения см. в разделе [.NET Framework для различных версий для веб-сайтов](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Назад](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
