---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и службы приложений Azure | Документация Майкрософт
author: Rick-Anderson
description: Этом руководстве показано, как ваш код можно безопасно хранить и получить доступ к защищенным сведениям. Наиболее важно то, что вы никогда не следует хранить пароли и другие замечания...
ms.author: aspnetcontent
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 445e7c32baf4316273b0a5901a776684a6c5d73f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832459"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Рекомендации по развертыванию паролей и других конфиденциальных данных в ASP.NET и службы приложений Azure
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этом руководстве показано, как ваш код можно безопасно хранить и получить доступ к защищенным сведениям. Наиболее важно, никогда не следует хранить пароли и другие конфиденциальные данные в исходном коде, а не следует использовать секреты производства в режиме разработки и тестирования.
> 
> Пример кода — это простое консольное приложение веб-задания и приложения ASP.NET MVC, которому требуется доступ к базе данных подключения строка пароля, Twilio, Google и SendGrid ключи безопасности.
> 
> В локальной среде параметры и PHP также упоминается.


- [Работать с паролями в среде разработки](#pwd)
- [Работа со строками соединения в среде разработки](#con)
- [Консольные приложения веб-заданий](#wj)
- [Развертывание секреты в Azure](#da)
- [Заметки о локальных и PHP](#not)
- [Дополнительные ресурсы](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Работать с паролями в среде разработки

Часто руководствах конфиденциальные данные в исходном коде, будем надеяться, что с исключением, что никогда не следует хранить конфиденциальные данные в исходном коде. Например Мой [приложения ASP.NET MVC 5 с помощью SMS и электронной почты 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) руководстве показано, следующий код в *web.config* файла:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config* файл является исходный код, поэтому эти секреты никогда не должны храниться в этом файле. К счастью `<appSettings>` элемент имеет `file` атрибут, который позволяет указать внешний файл, содержащий параметры конфигурации важные приложения. Все секреты можно переместить во внешний файл до тех пор, пока внешнего файла не возвращались в дерево исходного кода. Например, в следующей разметке, файл *AppSettingsSecrets.config* содержит все секреты приложения:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Разметка в файле внешних (*AppSettingsSecrets.config* в этом примере), та же разметка находится в *web.config* файла:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Среда выполнения ASP.NET объединяет содержимое внешнего файла с разметкой в &lt;appSettings&gt; элемент. Среда выполнения игнорирует атрибут файла, если не удается найти указанный файл.

> [!WARNING]
> Безопасность — не добавляйте к *.config секреты* файла проекта или вернуть его в систему управления версиями. По умолчанию Visual Studio устанавливает `Build Action` для `Content`, то есть файл развертывается. Дополнительные сведения см. в разделе [почему не все файлы в папке проекта развернуты?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Несмотря на то, что можно использовать любое расширение для *.config секреты* файл, лучше всего оставить *.config*, как файлы конфигурации не предоставляются службами IIS. Обратите внимание, что *AppSettingsSecrets.config* файл имеет два уровня каталогов вверх от *web.config* файла, поэтому оно полностью не входит в каталоге решения. Путем перемещения файла из каталога решений, &quot;git добавьте \* &quot; , чтобы не добавлять его в репозиторий.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Работа со строками соединения в среде разработки

Visual Studio создает проекты ASP.NET, использующих [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB был создан специально для среды разработки. Он не требует пароля, поэтому не нужно ничего делать, чтобы предотвратить секреты из проверки в исходный код. Некоторые команды разработчиков использовать полные версии SQL Server (или других СУБД), которые требуется пароль.

Можно использовать `configSource` атрибут для замены всего `<connectionStrings>` разметки. В отличие от `<appSettings>` `file` атрибут, который объединяет разметку, `configSource` атрибут заменяет разметку. Следующая разметка показывает `configSource` атрибут в *web.config* файла:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Если вы используете `configSource` атрибута, как показано выше, чтобы переместить во внешний файл строки подключения и Visual Studio создаст новый веб-сайт, он не сможет обнаружить при использовании базы данных, и вы не получите возможность так настроить базы данных при вы pu публиковать в Azure из Visual Studio. Если вы используете `configSource` атрибута, можно использовать PowerShell для создания и развертывания веб-сайта и базы данных, или можно создать веб-сайта и базы данных на портале перед публикацией. [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) скрипт создает новый веб-сайт и базы данных.


> [!WARNING]
> Безопасность — в отличие от *AppSettingsSecrets.config* файл, файл строки внешнего соединения должен быть в том же каталоге, в корневом *web.config* файла, поэтому вам придется принимать меры предосторожности, чтобы убедиться, что не устанавливайте флажок в исходном репозитории.


> [!NOTE]
> **Предупреждение системы безопасности на файл секретов:** рекомендуется не используйте секреты производства в разработки и тестирования. С помощью паролей производства в тестирования или разработки утечки секретов.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Консольные приложения веб-заданий

*App.config* файл, используемый в консольное приложение не поддерживает относительные пути, однако он поддерживает абсолютные пути. Абсолютный путь можно использовать для перемещения секретов из каталога проекта. В следующей разметке показан секреты в *C:\secrets\AppSettingsSecrets.config* файл, а не конфиденциальные данные в *app.config* файл.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Развертывание секреты в Azure

При развертывании веб-приложения в Azure, *AppSettingsSecrets.config* файл не будет развернут (то есть, то, что нужно). Может перейти к [портала управления Azure](https://azure.microsoft.com/services/management-portal/) и задать их вручную, чтобы сделать это:

1. Перейдите к [ https://portal.azure.com ](https://portal.azure.com)и войдите, используя свои учетные данные Azure.
2. Нажмите кнопку **Обзор &gt; веб-приложений**, затем щелкните имя веб-приложения.
3. Нажмите кнопку **все параметры &gt; параметры приложения**.

**Параметры приложения** и **строку подключения** значения переопределяют значения в *web.config* файл. В нашем примере мы не были развернуты эти параметры в Azure, но если эти ключи были в *web.config* файл, параметры, отображаемые на портале имеет приоритет.

Мы рекомендуем следовать [рабочий процесс DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) и использовать [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (или другой платформы, такие как [Chef](http://www.opscode.com/chef/) или [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) для Автоматизация установки этих значений в Azure. Следующий сценарий PowerShell использует [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) Экспорт зашифрованные секретные данные на диск:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

В приведенном выше сценарии «Name» — это имя секретного ключа, такие как "&quot;FB\_AppSecret&quot; или «TwitterSecret». Можно просмотреть файл «.credential», созданный с помощью скрипта в браузере. В следующем фрагменте проверяет каждый файл учетных данных и задает секреты именованный веб-приложения.

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Безопасность — не будут содержать пароли и другие секретные данные в сценарии PowerShell, выполнив так сравнениях цель с помощью сценария PowerShell для развертывания конфиденциальных данных. [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) командлет предоставляет безопасный механизм для получения пароля. С помощью запросов пользовательского интерфейса можно предотвратить утечку пароль.


### <a name="deploying-db-connection-strings"></a>Развертывание строки подключения базы данных

Строки подключения базы данных обрабатываются аналогичным образом к параметрам приложения. При развертывании веб-приложения из Visual Studio, строка подключения будет настроен автоматически. Это можно проверить на портале. Чтобы задать строку подключения рекомендуется с помощью PowerShell. Пример сценария PowerShell создает веб-сайта и базы данных, а также задает строку подключения на веб-сайте загрузки [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) из [библиотеки сценариев Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Заметки о PHP

Так как пары ключ значение для обоих **параметры приложения** и **строки подключения** хранятся в переменных среды в службе приложений Azure, разработчики, использующие любой web app платформ (например PHP) может легко получить эти значения. См. в разделе (Stefan Schackow) [Windows Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) записи блога, в котором показан фрагмент кода PHP для считывания параметров приложения и строки подключения.

## <a name="notes-for-on-premises-servers"></a>Заметки о на локальных серверах

При развертывании в локальной веб-серверы, вы можете помочь безопасного секреты, [шифрование разделов конфигурации файлов конфигурации](https://msdn.microsoft.com/library/ff647398.aspx). Кроме того, можно использовать тот же подход рекомендуется для веб-сайтов Azure: оставьте разработки в файлах конфигурации и использовать значения переменных среды вступили в рабочей среде. Таким образом, однако необходимо написать код приложения для функции, которая выполняется автоматически в веб-сайтов Azure: извлечь параметры из переменных среды и использовать их вместо параметров файла конфигурации или использовать параметры файла конфигурации при переменные среды не найдены.

<a id="addRes"></a>
## <a name="additional-resources"></a>Дополнительные ресурсы

Пример PowerShell задает скрипт, создающий веб-приложения + базы данных, строку подключения и параметры приложения, загрузка [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) из [библиотеки сценариев Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

См. в разделе (Stefan Schackow) [Windows Azure Web Sites: как работают строки строки приложения и подключения](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Особую благодарность выражаю Barry Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) и Carlos Farre за рецензирование.
