---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Защита строк подключения и других сведений о конфигурации (Visual Basic) | Документация Майкрософт
author: rick-anderson
description: Приложения ASP.NET обычно хранят сведения о конфигурации в файл Web.config. Некоторые из этих сведений важна и гарантирует защиту. По умолч...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 9713bbd983c4e922273a23356cbbb3848a8b7c50
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839178"
---
<a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Защита строк подключения и других сведений о конфигурации (VB)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Скачать код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) или [скачать PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Приложения ASP.NET обычно хранят сведения о конфигурации в файл Web.config. Некоторые из этих сведений важна и гарантирует защиту. По умолчанию этот файл не будет обслуживаться чтобы посетитель веб-узла, но администратор или злоумышленник может получить доступ к файловой системе веб сервера и просмотреть содержимое файла. В этом руководстве мы Узнайте, что ASP.NET 2.0 позволяет защитить конфиденциальные данные путем их шифрования разделы файла Web.config.


## <a name="introduction"></a>Вступление

Сведения о конфигурации для приложений ASP.NET обычно хранятся в XML-файл с именем `Web.config`. В ходе этих учебниках мы обновили `Web.config` небольшое число раз. При создании `Northwind` типизированного набора DataSet в [руководства по использованию](../introduction/creating-a-data-access-layer-vb.md), например, строку подключения был автоматически добавлен к `Web.config` в `<connectionStrings>` разделе. Далее, в [главные страницы и переходы по узлу](../introduction/master-pages-and-site-navigation-vb.md) учебном курсе мы вручную обновили `Web.config`, добавление `<pages>` элемент, указывающий, что все страницы ASP.NET в наш проект должен использовать `DataWebControls` темы.

Так как `Web.config` могут содержать конфиденциальные данные, например строки подключения, очень важно, содержимое `Web.config` храниться безопасно, так и скрытых несанкционированный. По умолчанию любой HTTP-запрос в файл с `.config` расширение обрабатывается средой ASP.NET, который возвращает *страницы этого типа не обслуживается* сообщение, показанное на рис. 1. Это означает, что посетители не может просматривать ваш `Web.config` s содержимое файла, просто введя http://www.YourServer.com/Web.config в их адресную строку браузера s.


[![Посетив Web.config через браузер возвращает данный тип страницы не обслуживается сообщения](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Рис. 1**: посещения `Web.config` через браузер возвращает данный тип страницы не обслуживается сообщения ([Просмотр полноразмерного изображения](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))


Но что делать, если злоумышленнику удается найти некоторые эксплойта, она может просмотреть ваш `Web.config` s содержимое файла? Что может сделать с помощью этой информации злоумышленник и какие действия следует выполнить для обеспечения дополнительной защиты конфиденциальных сведений в `Web.config`? К счастью, большинство разделов в `Web.config` не содержат конфиденциальной информации. Какой вред можно злоумышленник этот если они знают имя темы страницы ASP.NET по умолчанию?

Определенные `Web.config` разделов, тем не менее, содержать конфиденциальные сведения, которые могут включать строки подключения, имена пользователей, пароли, имена серверов, ключей шифрования и т. д. Эти сведения обычно находится в следующем `Web.config` разделах:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

В этом руководстве мы рассмотрим методы защиты сведений такие конфиденциальные данные. Как мы увидим, .NET Framework версии 2.0 включает в себя защищенных конфигураций системы, которая дает программными средствами шифрования и расшифровки разделов выбранной конфигурации очень просто.

> [!NOTE]
> Этот учебник завершает с рассмотрения Microsoft s рекомендации по подключению к базе данных из приложения ASP.NET. В дополнение к шифрование строки подключения, вы можете помочь усиления защиты системы, гарантируя, который подключается к базе данных безопасным образом.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Шаг 1: Изучение ASP.NET 2.0 s защищенные параметры конфигурации

ASP.NET 2.0 входит система защищенной конфигурации для шифрования и расшифровки данных конфигурации. Сюда входят методы в платформе .NET Framework, который может использоваться программно шифровать или расшифровывать информацию о конфигурации. Система защищенной конфигурации использует [модель поставщика](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), что позволяет разработчикам выбрать, какие криптографических реализация используется.

.NET Framework предлагает два поставщика защищенной конфигурации:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -использует асимметричного [алгоритм RSA](http://en.wikipedia.org/wiki/Rsa) для шифрования и расшифровки.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -использует Windows [API защиты данных (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) для шифрования и расшифровки.

Так как система защищенной конфигурации реализует шаблон проектирования поставщика, можно создать собственный поставщик защищенной конфигурации и подключить его к приложению. См. в разделе [реализация поставщика защищенной конфигурации](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) Дополнительные сведения об этом процессе.

Поставщики RSA и DPAPI использовать ключи для их шифрования и расшифровки подпрограмм, и эти ключи можно хранить на компьютере пользователя уровне или уровне. Ключи уровня компьютера идеально подходят для сценариев, в котором выполняется веб-приложения на собственном специальном сервере, или если имеется несколько приложений на сервере, которым требуется совместное использование зашифрованных данных. Ключи уровня пользователя являются более безопасный вариант в общих средах размещения, где других приложений на одном сервере не сможете расшифровать разделы конфигурации s защищенные приложения.

В этом руководстве наши примеры будут использовать DPAPI поставщика и ключи уровня компьютера. В частности, мы рассмотрим шифрования `<connectionStrings>` статьи `Web.config`, несмотря на то, что система защищенной конфигурации может использоваться для шифрования большинство любой `Web.config` раздел. Сведения о ключей на уровне пользователя или с помощью поставщика RSA можно найти в источниках, в разделе «Дополнительные материалы» в конце этого руководства.

> [!NOTE]
> `RSAProtectedConfigurationProvider` И `DPAPIProtectedConfigurationProvider` поставщики регистрируются в `machine.config` файл с именами поставщика `RsaProtectedConfigurationProvider` и `DataProtectionConfigurationProvider`, соответственно. При шифровании или расшифровке сведения о конфигурации, необходимо указать имя соответствующего поставщика (`RsaProtectedConfigurationProvider` или `DataProtectionConfigurationProvider`) вместо имени фактического типа (`RSAProtectedConfigurationProvider` и `DPAPIProtectedConfigurationProvider`). Можно найти `machine.config` файл `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` папки.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Шаг 2: Программными средствами шифрования и расшифровки разделов конфигурации

С помощью нескольких строк кода мы шифрования или расшифровки определенного раздела настройки с помощью указанного поставщика. Код, как мы скоро увидим, что просто необходимо создавать программную ссылку на раздел соответствующую конфигурацию, вызовите его `ProtectSection` или `UnprotectSection` метод, а затем вызовите `Save` метод для сохранения изменений. Кроме того платформа .NET Framework содержит полезные программы командной строки, способный шифровать и расшифровывать данные конфигурации. Мы рассмотрим эта служебная программа командной строки на шаге 3.

Чтобы проиллюстрировать программным способом защиты информации о конфигурации, позволяющие s создать страницу ASP.NET содержит кнопки для шифрования и расшифровки `<connectionStrings>` статьи `Web.config`.

Сначала откройте `EncryptingConfigSections.aspx` странице в `AdvancedDAL` папку. Перетащите элемент управления TextBox с панели элементов в конструктор, установив его `ID` свойства `WebConfigContents`, ее `TextMode` свойства `MultiLine`и его `Width` и `Rows` свойства до 95% и 15, соответственно. Этот элемент управления будет отображаться содержимое `Web.config` позволяет быстро увидеть, если содержимое будут шифроваться или нет. Конечно, в реальном приложении бы никогда не будет отображаться содержимое `Web.config`.

Под текстовое поле, добавьте два элемента управления Button с именем `EncryptConnStrings` и `DecryptConnStrings`. Установите для свойства Text для строки подключения, шифрования и расшифровки строки подключения.

На этом этапе экран должен выглядеть как показано на рис. 2.


[![Добавьте текстовое поле и две кнопки веб-элементов управления на страницу](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Рис. 2**: Добавьте текстовое поле и две кнопки веб-элементов управления на страницу ([Просмотр полноразмерного изображения](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))


Далее нам нужно написать код, который загружает и отображает содержимое `Web.config` в `WebConfigContents` загрузки текстового поля при первой страницы. Добавьте следующий код в класс фонового кода страницы s. Этот код добавляет метод с именем `DisplayWebConfig` и вызывающего его из `Page_Load` обработчик событий при `Page.IsPostBack` — `False`:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

`DisplayWebConfig` Использует метод [ `File` класс](https://msdn.microsoft.com/library/system.io.file.aspx) для открытия приложения `Web.config` файл, [ `StreamReader` класс](https://msdn.microsoft.com/library/system.io.streamreader.aspx) чтения его содержимого в строку и [ `Path` класс](https://msdn.microsoft.com/library/system.io.path.aspx) создать физический путь к `Web.config` файл. Эти три класса находятся в [ `System.IO` пространства имен](https://msdn.microsoft.com/library/system.io.aspx). Следовательно, необходимо добавить `Imports``System.IO` в начало класса вспомогательного кода или, в качестве альтернативы они класса имена с префиксом `System.IO.`

Далее нам нужно добавить обработчики событий для два элемента управления Button `Click` событий и добавить необходимый код для шифрования и расшифровки `<connectionStrings>` разделе с помощью ключа уровня компьютера с поставщиком DPAPI. В конструкторе дважды щелкните каждую из кнопок, чтобы добавить `Click` обработчик событий в код программной части класса, а затем добавьте следующий код:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Код, используемый в два обработчика событий очень похож. Они оба сначала получите сведения о текущем s приложения `Web.config` файл через [ `WebConfigurationManager` класс](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` метод](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Этот метод возвращает файл веб-конфигурации для заданного виртуального пути. Далее, `Web.config` файл s `<connectionStrings>` разделе производится через [ `Configuration` класс](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` метод](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), который возвращает [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) объекта.

`ConfigurationSection` Объект включает [ `SectionInformation` свойство](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) , предоставляющий дополнительные сведения и функциональные возможности, касающиеся раздел конфигурации. Как показано в коде выше, мы можем определить, зашифрован ли раздел конфигурации, проверив `SectionInformation` свойство s `IsProtected` свойство. Кроме того, можно шифровать и расшифровывать с помощью разделе `SectionInformation` свойство s `ProtectSection(provider)` и `UnprotectSection` методы.

`ProtectSection(provider)` Метод принимает в качестве входных данных строка, указывающая имя поставщика защищенной конфигурации для использования при шифровании. В `EncryptConnString` мы передаем DataProtectionConfigurationProvider в обработчик события для кнопки s `ProtectSection(provider)` метод таким образом используется поставщик DPAPI. `UnprotectSection` Метод может определять поставщика, который был использован для шифрования раздела конфигурации и поэтому не требует входные параметры.

После вызова метода `ProtectSection(provider)` или `UnprotectSection` метод, необходимо вызвать `Configuration` объект s [ `Save` метод](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) для сохранения изменений. После того как зашифрованные или расшифрованные данные конфигурации и сохранить изменения, мы вызываем `DisplayWebConfig` загрузить обновленный `Web.config` содержимое в элемент управления TextBox.

Когда вы введете код, представленный выше, проверьте его, см. в статье `EncryptingConfigSections.aspx` страницы в обозревателе. Изначально вы увидите страницу, которая перечисляет содержимое `Web.config` с `<connectionStrings>` разделе, отображаются в виде обычного текста (см. рис. 3).


[![Добавьте текстовое поле и две кнопки веб-элементов управления на страницу](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Рис. 3**: Добавьте текстовое поле и две кнопки веб-элементов управления на страницу ([Просмотр полноразмерного изображения](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))


Нажмите кнопку шифрования строк подключения. Если включена проверка запросов, разметка обратная отправка от `WebConfigContents` создает текстовое поле `HttpRequestValidationException`, отображающий сообщение, потенциально опасных `Request.Form` обнаружено значение от клиента. Проверку запросов, которая включена по умолчанию в ASP.NET 2.0, не разрешены обратные передачи, которые включают незашифрованным HTML-кодом и предназначен для предотвращения атак путем внедрения скрипта. Эту проверку можно отключить на странице - или -уровне приложения. Чтобы отключить эту функцию для этой страницы, задайте `ValidateRequest` присвоить `False` в `@Page` директива. `@Page` Директива находится в верхней части страницы s декларативная разметка.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Дополнительные сведения о проверке запроса, его назначение, как отключить его на страницы - и -уровне приложения, а также в HTML кодирования разметки, см. в разделе [проверка запросов — предотвращение атак скрипт](../../../../whitepapers/request-validation.md).

После отключения проверки запросов для страницы, попробуйте еще раз нажмите кнопку шифрования строк подключения. При обратной передаче, будет осуществляться в файле конфигурации и его `<connectionStrings>` раздел, зашифрованные с помощью DPAPI поставщика. После этого текстового поля обновляется для отображения новых `Web.config` содержимого. Как показано на рис. 4, `<connectionStrings>` сведения шифруются.


[![Щелкнув шифровать соединение строк кнопку шифрует &lt;connectionString&gt; раздел](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Рис. 4**: щелкнув шифровать соединение строк кнопку шифрует `<connectionString>` раздел ([Просмотр полноразмерного изображения](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))


Зашифрованный `<connectionStrings>` следует раздел, созданный на компьютере, несмотря на то что некоторое содержимое в `<CipherData>` для краткости был удален элемент:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>` Элемент задает поставщик, используемый для шифрования (`DataProtectionConfigurationProvider`). Эта информация используется `UnprotectSection` метод при нажатии кнопки расшифровки строки подключения.


При обращении к строке подключения из `Web.config` - либо код, мы записи из элемента управления SqlDataSource, либо автоматически созданный код от адаптеров таблицы в нашей типизированных наборов DataSet - это автоматически расшифровываются. Короче говоря, мы не нужно добавлять дополнительный код или логику для расшифровки зашифрованных `<connectionString>` раздел. Чтобы продемонстрировать это, посетите один из предыдущих учебных курсах в настоящее время, например руководстве простого отображения из раздел основной отчетности (`~/BasicReporting/SimpleDisplay.aspx`). Как показано на рис. 5, учебника работает именно так, как ожидается, что он, указывающее, что зашифрованные строку подключения автоматически расшифровывается с помощью страницы ASP.NET.


[![Уровень доступа к данным автоматически расшифровывает данные строки подключения](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Рис. 5**: уровень доступа к данным автоматически расшифровывает данные строки подключения ([Просмотр полноразмерного изображения](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))


Чтобы отменить `<connectionStrings>` разделе обратно в представление в формате обычного текста, нажмите кнопку расшифровки строки подключения. При обратной передаче вы увидите строки подключения в `Web.config` в виде обычного текста. На этом этапе экран должен выглядеть точно так же при первом просмотре страницы (см. на рис. 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Шаг 3: Шифрование разделов конфигурации с использованием`aspnet_regiis.exe`

.NET Framework включает широкий набор средств командной строки в `$WINDOWS$\Microsoft.NET\Framework\version\` папку. В [использование зависимостей кэша SQL](../caching-data/using-sql-cache-dependencies-vb.md) учебник, к примеру, мы рассмотрели применение `aspnet_regsql.exe` средство командной строки, чтобы добавить инфраструктуру, необходимую для зависимости кэша SQL. Еще одно средство полезно командной строки в этой папке — [средство регистрации IIS ASP.NET (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Как понятно из названия, средство регистрации IIS ASP.NET главным образом используется для регистрации приложения ASP.NET 2.0 с помощью Microsoft s профессиональное веб-сервер IIS. В дополнение к ее возможности, относящиеся к IIS, средство регистрации IIS ASP.NET может также использоваться для шифрования или расшифровки указанным именем разделов конфигурации в `Web.config`.

В следующей инструкции показано общий синтаксис, используемый для шифрования раздела конфигурации с `aspnet_regiis.exe` средство командной строки:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*раздел* является раздел конфигурации для шифрования (например, connectionStrings), *физических\_directory* полных, физический путь к корневой каталог на веб-приложения s и *поставщика*  имя поставщика защищенной конфигурации (например, DataProtectionConfigurationProvider). Кроме того Если веб-приложения был зарегистрирован в службах IIS можно ввести виртуальный путь, а не физический путь, используя следующий синтаксис:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Следующие `aspnet_regiis.exe` пример выполняет шифрование `<connectionStrings>` разделе с помощью DPAPI поставщика с помощью ключа уровня компьютера:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Аналогичным образом `aspnet_regiis.exe` средство командной строки можно использовать для расшифровки разделов конфигурации. Вместо использования `-pef` переключения, используйте `-pdf` (или вместо `-pe`, использовать `-pd`). Кроме того Обратите внимание на то, что имя поставщика не является необходимым при расшифровке.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Так как мы используем поставщик API защиты данных, который использует ключи, от компьютера, необходимо запустить `aspnet_regiis.exe` с помощью одного компьютера, из которого обслуживаются веб-страниц. Например если вы запустите эту программу командной строки из вашей локальной машины, а затем отправьте зашифрованный файл Web.config на рабочий сервер, рабочий сервер не будет смог расшифровать данные строки подключения, так как он был зашифрован с помощью ключей, определенного на свой компьютер разработки. Поставщика RSA имеет это ограничение, так как это можно экспортировать ключи RSA на другой компьютер.


## <a name="understanding-database-authentication-options"></a>Возможности проверки подлинности базы данных

Прежде чем любое приложение может выдать `SELECT`, `INSERT`, `UPDATE`, или `DELETE` запросы к базе данных Microsoft SQL Server, базы данных сначала необходимо определить запрашивающей стороне. Этот процесс известен как *проверки подлинности* и SQL Server предоставляет два метода проверки подлинности:

- **Проверка подлинности Windows** -процесса, под которой выполняется приложение используется для взаимодействия с базой данных. При запуске приложения ASP.NET с помощью Visual Studio 2005 s сервер ASP.NET Development Server, приложение ASP.NET использует идентификатор текущего пользователя. Для приложений ASP.NET на Microsoft Internet Information Server (IIS), приложений ASP.NET обычно выступать от `domainName``\MachineName` или `domainName``\NETWORK SERVICE`, несмотря на то, что его можно настроить.
- **Проверка подлинности SQL** -значения, идентификатор и пароль пользователя передаются как учетные данные для проверки подлинности. С помощью проверки подлинности SQL идентификатор пользователя и пароль предоставляются в строке подключения.

Проверка подлинности Windows предпочтительнее проверку подлинности SQL, так как он является более безопасным. С проверкой подлинности Windows строка подключения предоставляется бесплатно, от имени пользователя и пароль, и если веб-сервер и сервер базы данных расположены на двух разных компьютерах, учетные данные не отправляются по сети в виде обычного текста. С помощью проверки подлинности SQL Однако учетные данные проверки подлинности жестко задано в строке подключения и передаются с веб-сервера на сервер базы данных в виде обычного текста.

Эти учебники использовали проверку подлинности Windows. Чтобы узнать, какой режим проверки подлинности используется, просмотрев строку подключения. Строку подключения в `Web.config` для наших учебниках было:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Встроенная безопасность = True и отсутствие имени пользователя и пароля указывают, что используется проверка подлинности Windows. В некоторых соединения строки термин доверенного подключения = Yes "или" Integrated Security = SSPI используется вместо встроенной безопасности = True, но все три указывает использование проверки подлинности Windows.

Пример строки подключения, использующей проверку подлинности SQL. Обратите внимание, учетные данные, внедренные в строке подключения:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Представьте, что злоумышленник имеет право просматривать ваши приложения s `Web.config` файл. Если вы используете проверку подлинности SQL для подключения к базе данных, доступную через Интернет, злоумышленник может использовать эту строку подключения для подключения к базе данных через SQL Management Studio или с помощью страницы ASP.NET в собственных веб-сайтов. Чтобы минимизировать эту угрозу, шифровать строку подключения в `Web.config` с использованием защищенной конфигурации системы.

> [!NOTE]
> Дополнительные сведения о различных типах проверки подлинности, доступных в SQL Server см. в разделе [построение безопасных приложений ASP.NET: проверка подлинности, авторизация и безопасное взаимодействие](https://msdn.microsoft.com/library/aa302392.aspx). Дополнительные примеры строк подключения, включая различия между синтаксис проверки подлинности Windows и SQL, см. в разделе [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Сводка

По умолчанию файлы с `.config` расширения в приложении ASP.NET не может осуществляться через браузер. Файлы этого типа не возвращаются, так как они могут содержать конфиденциальные сведения, такие как строки подключения к базам данных, имена пользователей и пароли и т. д. Защищенная конфигурация системы в .NET 2.0 помогает дополнительно защитить конфиденциальную информацию, позволяя указанным именем разделов конфигурации должны быть зашифрованы. Существует два поставщика защищенной конфигурации, встроенные: один, который использует алгоритм RSA, а вторая использует API защиты данных Windows (DPAPI).

В этом учебнике мы рассмотрели способы шифрования и расшифровки с помощью DPAPI поставщика параметров конфигурации. Это можно сделать программным образом, как мы уже выяснили на этапе 2, а также как с помощью `aspnet_regiis.exe` средства командной строки, которое было описано на шаге 3. Дополнительные сведения о ключей на уровне пользователя или вместо этого с помощью поставщика RSA см. в разделе ресурсов в разделе «Дополнительные материалы».

Счастливого вам программирования!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, обсуждавшимся в этом руководстве см. в следующих ресурсах:

- [Построение приложения Secure ASP.NET: Проверки подлинности, авторизации и безопасный обмен данными](https://msdn.microsoft.com/library/aa302392.aspx)
- [Шифрование данных конфигурации в ASP.NET 2.0 приложения](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Шифрование `Web.config` значений в ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Практическое руководство: Шифрование разделов конфигурации в ASP.NET 2.0 с помощью DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Практическое руководство: Шифрование разделов конфигурации в ASP.NET 2.0 с помощью RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [API конфигурации в среде .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Защита данных Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи книг по ASP/ASP.NET и основатель веб- [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Microsoft с 1998 года. Скотт — независимый консультант, преподаватель и автор. Его последняя книга — [ *Sams Teach ASP.NET 2.0 in 24 часа*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ним можно связаться по адресу [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Особая благодарность

В этой серии руководств пособий рецензировалась многими компетентными редакторами. Терезой Мерфи и Рэнди Шмидт, стали Лиз Шалок в этом руководстве. Хотите поработать с моих последующих статей для MSDN? Если Да, напишите мне [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [Вперед](debugging-stored-procedures-vb.md)
