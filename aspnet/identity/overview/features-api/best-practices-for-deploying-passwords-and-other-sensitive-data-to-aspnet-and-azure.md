---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: "Советы и рекомендации по развертыванию пароли и другие конфиденциальные данные, ASP.NET и службы приложений Azure | Документы Microsoft"
author: Rick-Anderson
description: "В этом учебнике показано, как код можно безопасно хранения и доступа к защищенным сведениям. Наиболее важно то, что никогда не следует хранить пароли и другие отправителя..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Рекомендации по развертыванию пароли и другие конфиденциальные данные в ASP.NET и службы приложений Azure
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> В этом учебнике показано, как код можно безопасно хранения и доступа к защищенным сведениям. Самое главное, никогда не следует хранить пароли и другие конфиденциальные данные в исходном коде и секретные данные в рабочей среде не следует использовать в режиме разработки и тестирования.
> 
> Пример кода является простое консольное приложение веб-задания и приложение ASP.NET MVC, которому требуется доступ к базе данных подключения строка пароля, Twilio, Google и SendGrid ключи безопасности.
> 
> Локальные параметры и PHP также упоминается.


- [Работа с паролями в среде разработки](#pwd)
- [Работа со строками подключения в среде разработки](#con)
- [Консольные приложения веб-заданий](#wj)
- [Развертывание секретные данные в Azure](#da)
- [Заметки о локальных и PHP](#not)
- [Дополнительные ресурсы](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Работа с паролями в среде разработки

Часто руководствах конфиденциальные данные в исходном коде, желательно с помнить, что никогда не следует хранить конфиденциальные данные в исходном коде. Например my [приложение ASP.NET MVC 5 с SMS и адрес электронной почты 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) учебнике показано ниже в *web.config* файл:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config* является файл исходного кода, поэтому эти секреты никогда не должны храниться в этом файле. К счастью `<appSettings>` элемент имеет `file` атрибут, который позволяет указать внешний файл, содержащий параметры конфигурации важных приложений. Можно переместить все секреты во внешний файл, при условии, что внешний файл не проверяется в дерево исходного кода. Например, в следующую разметку файла *AppSettingsSecrets.config* содержит секретные данные приложений:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Разметка во внешнем файле (*AppSettingsSecrets.config* в этом образце), находится в одном разметки *web.config* файла:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Среда выполнения ASP.NET объединяет содержимое внешнего файла разметку в &lt;appSettings&gt; элемента. Среда выполнения игнорирует атрибут файла, если не удается найти указанный файл.

> [!WARNING]
> Безопасность — не добавлять вашей *секреты .config* файл в проект или вернуть его в систему управления версиями. По умолчанию Visual Studio устанавливает `Build Action` для `Content`, то есть файл развертывания. Дополнительные сведения см. [почему не все файлы в папке проекта развертывания?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Несмотря на то, что можно использовать любое расширение для *секреты .config* файла, рекомендуется сохранить его *.config*, как файлы конфигурации не предоставляются службами IIS. Обратите внимание, что *AppSettingsSecrets.config* файл имеет два каталога уровня выше из *web.config* файл, поэтому оно не полностью входит в нужном каталоге решений. Путем перемещения файла из каталога решения &quot;добавить git \* &quot; не станем добавлять в репозиторий.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Работа со строками подключения в среде разработки

Visual Studio создает новые проекты ASP.NET, использующие [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB был создан специально для среды разработки. Он не требует пароля, поэтому не нужно ничего делать, чтобы предотвратить секретные данные проверяются в исходный код. Некоторые команды разработчиков используйте полной версии SQL Server (или других СУБД), требуется пароль.

Можно использовать `configSource` атрибут для замены всего `<connectionStrings>` разметки. В отличие от `<appSettings>` `file` атрибут, который объединяет разметки `configSource` атрибут заменяет разметку. В следующем разметки показан `configSource` атрибута в *web.config* файла:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Если вы используете `configSource` атрибута, как показано выше, чтобы переместить во внешний файл строки подключения и Visual Studio создаст новый веб-сайт, не сможет обнаружить используется база данных, и вы не сможете получить возможность настройки базы данных при вы pu публиковать в Azure из Visual Studio. Если вы используете `configSource` атрибута, можно использовать PowerShell для создания и развертывания веб-сайта и базы данных, или можно создать веб-сайта и базы данных на портале перед публикацией. [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) скрипт создает новый веб-сайт и базы данных.


> [!WARNING]
> Безопасность — в отличие от *AppSettingsSecrets.config* файл, файл строки внешнего соединения должен быть в том же каталоге, который является корнем *web.config* файл, поэтому вам придется принять меры предосторожности, чтобы убедиться, что не было вернуть его в хранилище версиями.


> [!NOTE]
> **Предупреждение системы безопасности в файле секреты:** рекомендуется не использовать секретные данные производства в тестирования и разработки. Использование паролей производства в испытательных и исследовательских утечки секретов.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Консольные приложения веб-заданий

*App.config* файла, используемого консольное приложение не поддерживает относительные пути, но он поддерживает абсолютные пути. Абсолютный путь можно использовать для перемещения своих секретов вне каталога проекта. В следующем примере показана секретов в *C:\secrets\AppSettingsSecrets.config* файла, а не конфиденциальные данные в *app.config* файла.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Развертывание секретные данные в Azure

При развертывании веб-приложения в Azure, *AppSettingsSecrets.config* файл не будет развернут (то есть, при необходимости). Может перейти к [портала управления Azure](https://azure.microsoft.com/services/management-portal/) и задать их вручную, чтобы это сделать:

1. Последовательно выберите пункты [https://portal.azure.com](https://portal.azure.com)и выполните вход с помощью учетных данных Azure.
2. Нажмите кнопку **Обзор &gt; веб-приложений**, затем щелкните имя веб-приложения.
3. Нажмите кнопку **все параметры &gt; параметры приложения**.

**Параметры приложения** и **строка подключения** значения переопределяют те же параметры в *web.config* файла. В нашем примере мы не были развернуты эти параметры в Azure, но если эти ключи были в *web.config* файла, параметры, отображаемые на портале бы больший приоритет.

Рекомендуется следовать [рабочий процесс DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) и использовать [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (или другой структуре, такие как [Chef](http://www.opscode.com/chef/) или [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) для Автоматизация установки этих значений в Azure. Следующий сценарий PowerShell использует [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) Экспорт зашифрованные секретные данные на диск:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

В приведенном выше сценарии «Имя» — имя секретного ключа, такие как "&quot;FB\_AppSecret&quot; или «TwitterSecret». Можно просмотреть файл «.credential», созданный скрипт в браузере. В приведенном ниже фрагменте проверяет каждый файл учетных данных и задает секреты именованный веб-приложения.

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Безопасность — не включать пароли и другие секретные данные в сценарии PowerShell, выполнив так противоречит цели с помощью сценария PowerShell для развертывания конфиденциальных данных. [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) командлет предоставляет безопасного механизма для получения пароля. С помощью строки пользовательского интерфейса можно предотвратить утечку пароль.


### <a name="deploying-db-connection-strings"></a>Развертывание базы данных строки подключения

Строки подключения базы данных обрабатываются аналогичным образом в настройках приложения. При развертывании веб-приложения из Visual Studio, строка соединения будут настроены автоматически. Это можно проверить на портале. Рекомендуемый способ настройки строки подключения — с помощью PowerShell. Пример сценария PowerShell создает веб-сайта и базы данных и задает строку подключения на веб-сайте загрузки [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) из [библиотеки Azure скрипт](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Примечания для PHP

Так как пары «ключ значение» для обоих **параметры приложения** и **строки подключения** хранятся в переменных среды в службе приложений Azure, разработчики, использующие любой web app платформы (например PHP) может легко получить эти значения. В разделе (Stefan Schackow) [веб-сайтов Windows Azure: как строки приложения и строк соединения работает](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) записи блога, в котором показан фрагмент PHP для чтения параметров приложения и строки подключения.

## <a name="notes-for-on-premises-servers"></a>Заметки о локальных серверов

При развертывании веб-серверов в локальной среде вы можете помочь безопасного секреты с [шифрование разделов конфигурации, файлов конфигурации](https://msdn.microsoft.com/library/ff647398.aspx). В качестве альтернативы можно использовать тот же подход рекомендуется для веб-сайтов Azure: оставьте настройки разработки в файлах конфигурации, а также использовать значения переменных среды для параметров рабочей. Таким образом, тем не менее, необходимо написать код приложения для функций, которые автоматически веб-сайтов Azure: извлечь параметры из переменных среды и использовать эти значения вместо параметров файла конфигурации или использовать параметры файла конфигурации при переменные среды не найдены.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

Пример PowerShell задает скрипт, создающий веб-приложения и базы данных, строка подключения + параметры приложения, загрузка [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) из [библиотеки Azure скрипт](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

В разделе (Stefan Schackow) [Windows Azure веб-сайтов: как работают строки приложения строки и соединения](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Выражаем особую благодарность Барри Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) и Carlos Farre для анализа.
