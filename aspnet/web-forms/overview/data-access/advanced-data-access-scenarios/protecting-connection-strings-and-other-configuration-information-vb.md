---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Защита строки соединения и другие сведения о конфигурации (VB) | Документы Microsoft
author: rick-anderson
description: Приложения ASP.NET обычно хранят сведения о конфигурации в файле Web.config. Некоторые из этих сведений является конфиденциальной и гарантирует отсутствие защиты. По умолч...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 3372416dd9143afbfd442eaffb39cd807fae0de6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Защита строки соединения и другие сведения о конфигурации (Visual Basic)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) или [скачать PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Приложения ASP.NET обычно хранят сведения о конфигурации в файле Web.config. Некоторые из этих сведений является конфиденциальной и гарантирует отсутствие защиты. По умолчанию этот файл не будет обслуживаться для посетителя веб-сайта, но администратор или злоумышленник может получить доступ к файловой системе веб-сервера и просмотреть содержимое файла. В этом учебнике показано, что ASP.NET 2.0 позволяет защитить конфиденциальную информацию путем шифрования разделов файла Web.config.


## <a name="introduction"></a>Вступление

Сведения о конфигурации для приложений ASP.NET обычно хранятся в XML-файл с именем `Web.config`. В ходе этих учебников мы обновили `Web.config` небольшое число раз. При создании `Northwind` типизированного набора данных в [первом учебнике](../introduction/creating-a-data-access-layer-vb.md), например, строку подключения был автоматически добавлен к `Web.config` в `<connectionStrings>` раздела. Далее в [главные страницы и переходов](../introduction/master-pages-and-site-navigation-vb.md) учебнике мы вручную обновить `Web.config`, добавление `<pages>` элемент, указывая, что все страницы ASP.NET в данном проекте следует использовать `DataWebControls` темы.

Поскольку `Web.config` могут содержать конфиденциальные данные, например строки подключения, очень важно, содержимое `Web.config` храниться в безопасном и скрытым от несанкционированного просмотра. По умолчанию все HTTP-запрос в файл с `.config` расширения обрабатывается обработчиком ASP.NET, который возвращает *этот тип страницы не обслуживается* сообщение, показанное на рис. 1. Это означает, что посетители не может просматривать ваш `Web.config` s содержимое файлов, просто введя http://www.YourServer.com/Web.config в их в адресной строке браузера s.


[![Обход Web.config через браузер возвращает данный тип страницы не обслуживается сообщения](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Рис. 1**: посещения `Web.config` через браузер возвращает данный тип страницы не обслуживается сообщения ([Просмотр полноразмерное изображение](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))


Но что делать, если злоумышленник может найти некоторые уязвимости, позволяющий здесь для просмотра вашего `Web.config` s содержимое файлов? Что можно сделать с этой информацией злоумышленник и какие меры можно предпринять для обеспечения дополнительной защиты конфиденциальных сведений в `Web.config`? К счастью, большинство разделов в `Web.config` не содержат конфиденциальной информации. Какой вред может злоумышленник perpetrate зная имя темы, используемой страницах ASP.NET по умолчанию?

Некоторые `Web.config` разделы, однако содержат конфиденциальные сведения, которые могут включать строки подключения, имена пользователей, пароли, имена серверов, ключей шифрования и т. д. Эти сведения обычно находится в следующем `Web.config` разделы:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

В этом учебнике мы рассмотрим методы защиты такие конфигурации конфиденциальные сведения. Как мы увидим, .NET Framework версии 2.0 включает систему защищенных конфигураций, которые должны быть очень просто программного шифрования и расшифровки разделов выбранной конфигурации.

> [!NOTE]
> Этот учебник завершает рассматриваются рекомендации Майкрософт s для соединения с базой данных из приложения ASP.NET. Помимо шифрование строки подключения, вы можете помочь усиления безопасности системы за счет того, который подключается к базе данных безопасным образом.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Шаг 1: Изучение ASP.NET 2.0 s защищенные параметры конфигурации

ASP.NET 2.0 включает систему защищенной конфигурации для шифрования и расшифровки данных конфигурации. Сюда входят методы в платформе .NET Framework, который может использоваться для программного шифрования или расшифровки сведения о конфигурации. Защищенная конфигурация система использует [модель поставщика](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), что разработчики могут выбрать, какие шифрования реализация используется.

.NET Framework предлагает два поставщика защищенной конфигурации:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -использует асимметричного [алгоритм RSA](http://en.wikipedia.org/wiki/Rsa) для шифрования и расшифровки.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -использует Windows [API защиты данных (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) для шифрования и расшифровки.

Поскольку система защищенной конфигурации реализует шаблон проектирования поставщика, можно создать собственный поставщик защищенной конфигурации и подключить его в приложение. В разделе [реализация поставщика конфигурации защищенных](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) Дополнительные сведения об этом процессе.

Поставщики RSA и DPAPI клавишами их подпрограммы шифрования и расшифровки, и эти ключи можно хранить на компьютере пользователя уровня или. Ключи машинного идеально подходят для сценариев, где выполняется веб-приложение на отдельном выделенном сервере или если имеется несколько приложений на сервере, которым требуется совместное использование зашифрованных данных. Уровень пользователя — это более безопасный вариант в общих средах размещения, где другие приложения на том же сервере, не следует может расшифровывать разделы конфигурации s защищенные приложения.

В этом учебнике наши примеры будет использоваться поставщик DPAPI и ключей на уровне компьютера. В частности, мы рассмотрим шифрования `<connectionStrings>` статьи `Web.config`, хотя защищенная конфигурация системы может использоваться для шифрования большинство любой `Web.config` раздела. Сведения с помощью ключей на уровне пользователя, либо с помощью поставщика RSA можно найти ресурсы в разделе "Дополнительные материалы" в конце этого учебника.

> [!NOTE]
> `RSAProtectedConfigurationProvider` И `DPAPIProtectedConfigurationProvider` зарегистрированных поставщиков в `machine.config` файла с именами поставщика `RsaProtectedConfigurationProvider` и `DataProtectionConfigurationProvider`соответственно. При шифровании или расшифровке сведения о конфигурации, вам потребуется указать имя соответствующего поставщика (`RsaProtectedConfigurationProvider` или `DataProtectionConfigurationProvider`) вместо текущее имя типа (`RSAProtectedConfigurationProvider` и `DPAPIProtectedConfigurationProvider`). Можно найти `machine.config` файла в `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` папки.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Шаг 2: Программным путем шифрования и расшифровки разделов конфигурации

Всего несколько строк кода, мы можно зашифровать или расшифровать раздел конфигурации, определенного с помощью указанного поставщика. Код, как мы увидим, достаточно программно сослаться в соответствующей конфигурации разделе вызвать его `ProtectSection` или `UnprotectSection` метода, а затем вызовите `Save` метод для сохранения изменений. Кроме того платформа .NET Framework содержит полезные программа командной строки, способный шифровать и расшифровывать данные конфигурации. Мы рассмотрим эту программу командной строки на шаге 3.

Чтобы проиллюстрировать программным способом защиты сведений о конфигурации, позволяют s создавать страницы ASP.NET, которая содержит кнопки для шифрования и расшифровки `<connectionStrings>` раздела `Web.config`.

Сначала откройте `EncryptingConfigSections.aspx` страницы в `AdvancedDAL` папки. Перетащите текстовое поле из области элементов в конструктор параметр его `ID` свойства `WebConfigContents`, ее `TextMode` свойства `MultiLine`и его `Width` и `Rows` свойства 95% и 15, соответственно. Этот элемент управления будет отображать содержимое `Web.config` позволяет быстро увидеть, если содержимое зашифрованы или не. Конечно, в реальном приложении будет никогда не будет отображаться содержимое `Web.config`.

Под текстовое поле, добавьте два элемента управления Button с именем `EncryptConnStrings` и `DecryptConnStrings`. Установите для свойства Text для шифрования строк подключения и расшифровки строки подключения.

На этом этапе экран должен выглядеть как на рис.


[![Добавьте текстовое поле и две кнопки веб-элементов управления на страницу](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**На рисунке 2**: Добавьте текстовое поле и две кнопки веб-элементов управления на страницу ([Просмотр полноразмерное изображение](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))


Затем нужно написать код, который загружает и отображает содержимое `Web.config` в `WebConfigContents` текстового поля при первой страницы загрузки. Добавьте следующий код в класс страницы s кода. Этот код добавляет метод с именем `DisplayWebConfig` и вызывает его из `Page_Load` обработчик событий при `Page.IsPostBack` — `False`:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

`DisplayWebConfig` Использует метод [ `File` класса](https://msdn.microsoft.com/library/system.io.file.aspx) для открытия приложения `Web.config` файл [ `StreamReader` класса](https://msdn.microsoft.com/library/system.io.streamreader.aspx) для прочесть его содержимое в строку и [ `Path` класса](https://msdn.microsoft.com/library/system.io.path.aspx) создаваться физический путь к `Web.config` файла. Эти три классы находятся в [ `System.IO` пространства имен](https://msdn.microsoft.com/library/system.io.aspx). Следовательно, необходимо добавить `Imports``System.IO` в начало кода класса или, кроме того, эти класса имена с префиксом `System.IO.`

Затем нужно добавить обработчики событий для кнопок панели два `Click` событий и добавить необходимый код для шифрования и расшифровки `<connectionStrings>` статьи с использованием машинного ключа с помощью DPAPI поставщика. В конструкторе, дважды щелкните каждое из кнопок, чтобы добавить `Click` обработчика событий в коде программной класса, а затем добавьте следующий код:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Код, используемый в два обработчика событий очень похож. Они оба запустите, получение сведений о текущем s приложения `Web.config` файл через [ `WebConfigurationManager` класса](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` метод](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Этот метод возвращает файл веб-конфигурации для заданного виртуального пути. Далее, `Web.config` файл s `<connectionStrings>` раздел возможен только через [ `Configuration` класса](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` метод](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), который возвращает [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) объекта.

`ConfigurationSection` Объект включает в себя [ `SectionInformation` свойство](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) , предоставляющий дополнительные сведения и функциональные возможности, касающихся раздела конфигурации. Как код выше, можно определить, зашифрован ли раздел конфигурации, проверив `SectionInformation` свойство s `IsProtected` свойство. Кроме того, можно шифровать и расшифровывать через раздел `SectionInformation` свойство s `ProtectSection(provider)` и `UnprotectSection` методы.

`ProtectSection(provider)` Метод принимает в качестве входных данных строка, указывающая имя поставщика защищенной конфигурации для использования при шифровании. В `EncryptConnString` мы передаем DataProtectionConfigurationProvider в обработчик события для кнопки s `ProtectSection(provider)` метод, чтобы использовать поставщик DPAPI. `UnprotectSection` Метода определяется поставщик, который был использован для шифрования раздела конфигурации и поэтому не требуются все входные параметры.

После вызова метода `ProtectSection(provider)` или `UnprotectSection` метод, необходимо вызвать `Configuration` объект s [ `Save` метод](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) для сохранения изменений. Как только зашифрованные или расшифрованные данные конфигурации и сохранить изменения, мы называем `DisplayWebConfig` загрузить обновленный `Web.config` содержимое в элемент управления TextBox.

После ввода приведенный выше код, проверить его, посетив `EncryptingConfigSections.aspx` страницу в обозревателе. Первоначально вы увидите страницу, которая перечисляет содержимое `Web.config` с `<connectionStrings>` раздел отображается в виде обычного текста (см. рис. 3).


[![Добавьте текстовое поле и две кнопки веб-элементов управления на страницу](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Рис. 3**: Добавьте текстовое поле и две кнопки веб-элементов управления на страницу ([Просмотр полноразмерное изображение](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))


Нажмите кнопку шифрования строк подключения. Если проверка запросов включена, разметку обратная отправка от `WebConfigContents` TextBox сформирует `HttpRequestValidationException`, которого отображается сообщение, потенциально опасных `Request.Form` обнаружено значение от клиента. Проверку запросов, которая включена по умолчанию в ASP.NET 2.0, запрещает обратных передач, включающие без кодировки HTML и помогает предотвратить атаки путем внедрения скрипта. Эту проверку можно отключить на странице приложения на уровне или. Чтобы отключить для этой страницы, установите `ValidateRequest` параметру `False` в `@Page` директивы. `@Page` Директива находится в верхней части страницы s декларативная разметка.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Дополнительные сведения о проверке запроса, его назначение, как отключить его на страницу - и -уровне приложения, а также необходима кодировка HTML разметки см. в разделе [проверки запроса - предотвращение атак скрипт](../../../../whitepapers/request-validation.md).

После отключения проверки запроса страницы, попробуйте повторно нажмите кнопку шифрования строк подключения. При обратной передаче, файл конфигурации будет осуществляться доступ и его `<connectionStrings>` раздел, зашифрованные с помощью DPAPI поставщика. Текстовое поле обновляется для отображения нового `Web.config` содержимого. Как показано на рис. 4, `<connectionStrings>` информация теперь шифруется.


[![Щелкнув шифровать соединение строк кнопка шифрует &lt;connectionString&gt; раздела](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Рис. 4**: щелкнув шифровать соединение строк кнопка шифрует `<connectionString>` раздела ([Просмотр полноразмерное изображение](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))


Зашифрованный `<connectionStrings>` за раздел, созданный на компьютере, несмотря на то что некоторое содержимое в `<CipherData>` элемент был удален для краткости:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>` Элемент задает поставщик, используемый для шифрования (`DataProtectionConfigurationProvider`). Эта информация используется `UnprotectSection` метод при нажатии кнопки расшифровки строки подключения.


При обращении к строке подключения из `Web.config` - либо код, мы записи из элемента управления SqlDataSource, либо автоматически создаваемый код из адаптеров таблиц в нашем типизированных наборов данных — это автоматически расшифровывается. Иными словами, нам не нужно добавить любой дополнительный код или логику для расшифровки зашифрованного `<connectionString>` раздела. Чтобы продемонстрировать это, посетите один из предыдущих учебниках в настоящее время, например учебника простого отображения из раздела базовых отчетов (`~/BasicReporting/SimpleDisplay.aspx`). Как показано на рисунке 5, учебника работает точно так, как планируется, что его, указывающее на то, что данные строки зашифрованное соединение автоматически расшифровывается с помощью страницы ASP.NET.


[![Уровень доступа к данным автоматически расшифровывает данные строки подключения](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Рис. 5**: уровень доступа к данным автоматически расшифровывает данные строки подключения ([Просмотр полноразмерное изображение](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))


Чтобы восстановить `<connectionStrings>` статьи обратно в представление обычного текста, нажмите кнопку расшифровки строки подключения. При обратной передаче вы увидите строки подключения в `Web.config` в виде обычного текста. На этом этапе экран должен выглядеть, как и при первом посещении этой страницы (см. рис. 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Шаг 3: Шифрование разделов конфигурации с использованием`aspnet_regiis.exe`

Платформа .NET Framework включает ряд средств командной строки в `$WINDOWS$\Microsoft.NET\Framework\version\` папки. В [с помощью кэш-зависимости SQL](../caching-data/using-sql-cache-dependencies-vb.md) учебник, например, мы рассмотрели использование `aspnet_regsql.exe` средство командной строки для добавления инфраструктуры, необходимой для зависимости кэша SQL. Другое средство полезно командной строки в этой папке — [средство регистрации IIS ASP.NET (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Как и предполагает его имя, средство регистрации IIS ASP.NET в основном используется для регистрации приложения ASP.NET 2.0 с Microsoft s профессиональное веб-сервер IIS. В дополнение к его возможности, связанные с IIS, средство регистрации IIS ASP.NET может также использоваться для шифрования или расшифровки разделов конфигурации, указанный в `Web.config`.

В следующей инструкции показано общий синтаксис, используемый для шифрования раздела конфигурации с `aspnet_regiis.exe` средство командной строки:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*раздел* указан раздел конфигурации для шифрования (например, connectionStrings) *физической\_каталога* является полный физический путь корневую папку веб-приложения s, и *поставщика*  имя поставщика защищенной конфигурации для использования (например, DataProtectionConfigurationProvider). Кроме того Если веб-приложение регистрируется в службах IIS можно ввести виртуальный путь вместо физический путь, используя следующий синтаксис:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Следующие `aspnet_regiis.exe` пример выполняет шифрование `<connectionStrings>` статьи с помощью DPAPI поставщика с машинного ключа:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Аналогичным образом `aspnet_regiis.exe` средство командной строки можно использовать для расшифровки разделов конфигурации. Вместо использования `-pef` переход, использование `-pdf` (или вместо `-pe`, используйте `-pd`). Кроме того Обратите внимание, что имя поставщика не является необходимым для расшифровки.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Так как мы используем DPAPI поставщика, который использует ключи, связанных с компьютером, необходимо запустить `aspnet_regiis.exe` с одного компьютера, с которого отправляются на веб-страницах. Например если запустить программу командной строки с вашей локальной машины, а затем отправьте зашифрованный файл Web.config на рабочем сервере, рабочий сервер не будет возможность расшифровать строку подключения, так как он был зашифрован Использование ключей, определенных на компьютере разработки. Поставщик RSA имеет это ограничение, так как экспорт ключей RSA на другой компьютер.


## <a name="understanding-database-authentication-options"></a>Понимание параметров проверки подлинности базы данных

Прежде чем можно выполнять любое приложение `SELECT`, `INSERT`, `UPDATE`, или `DELETE` запросы к базе данных Microsoft SQL Server, базы данных необходимо определить, что запрашивающий объект. Этот процесс называется *проверки подлинности* и SQL Server предоставляет два метода проверки подлинности:

- **Проверка подлинности Windows** -процесс, в котором работает приложение используется для взаимодействия с базой данных. При запуске приложения ASP.NET с помощью Visual Studio 2005 s сервер разработки ASP.NET, приложение ASP.NET предполагает удостоверение пользователя, выполнившего вход. Для приложений ASP.NET на Microsoft Internet Information Server (IIS), приложений ASP.NET обычно подмены `domainName``\MachineName` или `domainName``\NETWORK SERVICE`, несмотря на то, что это может быть настроено.
- **Проверка подлинности SQL** -значения идентификатора и пароля пользователя, предоставляются учетные данные для проверки подлинности. С помощью проверки подлинности SQL идентификатор пользователя и пароль предоставляются в строке подключения.

Проверка подлинности Windows предпочтительнее проверки подлинности SQL, так как он является более безопасным. С проверкой подлинности Windows строки подключения будут свободны от имени пользователя и пароля и если веб-сервер и сервер базы данных находятся на двух разных компьютерах, учетные данные не отправляются по сети в виде обычного текста. С помощью проверки подлинности SQL Однако учетные данные проверки подлинности жестко запрограммированы в строке подключения и передаются с веб-сервера к серверу базы данных в виде обычного текста.

В этих учебниках используется проверка подлинности Windows. Чтобы узнать, какой режим проверки подлинности используется проверка строки подключения. Строка подключения в `Web.config` наши руководства был:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Встроенная безопасность = True и отсутствие имени пользователя и пароля показывают, что используется проверка подлинности Windows. В некоторые подключения строки термин доверенного соединения = Yes "или" Integrated Security = SSPI используется вместо встроенной безопасности = True, однако все три показывают использование проверки подлинности Windows.

Пример строки подключения, использующей проверку подлинности SQL. Примечание учетные данные, внедренные в строке подключения.

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Представьте, что злоумышленник доступны для просмотра вашего приложения s `Web.config` файла. При использовании проверки подлинности SQL для подключения к базе данных, доступном через Интернет, злоумышленник может использовать эту строку подключения для подключения к базе данных через среду SQL Management Studio или с помощью страницы ASP.NET в собственных веб-сайтов. Чтобы минимизировать эту угрозу, зашифровать строку подключения в `Web.config` с помощью защищенной конфигурации системы.

> [!NOTE]
> Дополнительные сведения о различных типах проверки подлинности, доступных в SQL Server см. в разделе [построение безопасности приложений ASP.NET: проверка подлинности, авторизации и безопасный обмен данными](https://msdn.microsoft.com/library/aa302392.aspx). Дополнительные примеры строк подключения, демонстрирующая различия между синтаксис проверки подлинности Windows и SQL, см. в разделе [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Сводка

По умолчанию файлы с `.config` расширения в приложении ASP.NET не может осуществляться через браузер. Эти типы файлов не возвращаются, так как они могут содержать конфиденциальные сведения, например строки подключения к базам данных, имена пользователей и пароли и т. д. Защищенная конфигурация системы в .NET 2.0 помогает дополнительной защиты конфиденциальных данных, позволяя разделов указанной конфигурации для шифрования. Существует два поставщика защищенной конфигурации, встроенный: один, который использует алгоритм RSA, а вторая использует API защиты данных Windows (DPAPI).

В этом учебнике мы рассматривали как зашифровать и расшифровать параметры конфигурации, с помощью DPAPI поставщика. Это можно сделать программно, как было показано в шаге 2, а также с помощью `aspnet_regiis.exe` средства командной строки, которое было выполнено в шаге 3. Дополнительные сведения о с помощью ключей на уровне пользователя или вместо этого поставщика RSA см в разделе "Дополнительные материалы".

Программирование довольны!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Построение приложения безопасного ASP.NET: Проверка подлинности, авторизации и безопасного обмена данными](https://msdn.microsoft.com/library/aa302392.aspx)
- [Шифрование данных конфигурации в ASP.NET 2.0 приложений](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Шифрование `Web.config` значений в ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Практическое руководство: Шифрование разделов конфигурации в ASP.NET 2.0 с помощью DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Практическое руководство: Шифрование разделов конфигурации в ASP.NET 2.0 с помощью RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [API конфигурации .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Защита данных Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Мерфи Тереза д и Рэнди Шмидт. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Назад](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [Вперед](debugging-stored-procedures-vb.md)
