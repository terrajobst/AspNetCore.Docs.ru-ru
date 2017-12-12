---
uid: web-pages/readme/beta3
title: "Веб-матрицы и ASP.NET Web Pages (Razor) о бета-версии 3 версии | Документы Microsoft"
author: rick-anderson
description: "Web Matrix и ASP.NET Web страницы (Razor) о бета-версии 3 выпуска"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5fad4b659dafe5470aeb84d320ff711b8840d1e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix и ASP.NET Web страницы (Razor) о бета-версии 3 выпуска
====================
> Web Matrix и ASP.NET Web страницы (Razor) о бета-версии 3 выпуска

9 ноября 2010 г.

## <a name="contents"></a>Описание

- [Обзор набора средств Visual Studio для Unity](#Overview)
- [Установка](#Installation_Notes)
- [Новые возможности, изменения и известные проблемы в бета-версии 3](#Known_Issues)

    - [Проблемы с установкой WebMatrix](#Known_Issues_Installation)
    - [Веб-страницы ASP.NET](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Установка приложений](#Known_Issues_Installing_Applications)
    - [Публикация приложений](#Known_Issues_Publishing_Applications)
    - [Другие проблемы](#Known_Issues_Other_Issues)
- [Дополнительные сведения](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Обзор

> Бета-версия Microsoft WebMatrix представляет собой стек бесплатной разработки, который устанавливается в минутах. Она интегрирует веб-сервера базы данных и платформами программирования для создания единой, интегрированной среды. Можно использовать для упрощения разработки кода, тестирования и публикация собственных ASP.NET или PHP, веб-сайт WebMatrix бета-версии или бета-версии WebMatrix можно использовать для создания нового веб-сайтов с помощью таких популярных приложений с открытым исходным кодом как DotNetNuke, Umbraco, WordPress и Joomla. Бета-версии WebMatrix использует же мощный веб-сервер, СУБД и платформы среды выполнения веб-сайта в Интернете, что упрощает и ускоряет переход из среды разработки в рабочей среде smooth и удобный.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Установка

> Чтобы установить WebMatrix бета-версии 3, используйте [платформы установщика Microsoft Web 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). После установки установщика веб-платформы, его можно использовать для установки WebMatrix бета-версии 3.
> 
> Если во время установки возникают проблемы, см. [Устранение неполадок с Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Инструкции по публикации приложения

> В разделе [пошаговые инструкции для публикации приложений](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Новые возможности, изменения, andKnown проблемы

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>WebMatrix бета-версии 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Проблема: WebMatrix бета-версия 3 доступна только на платформах, поддерживающих Microsoft .NET Framework 4

> .NET Framework версии 4 является обязательным для WebMatrix бета-версии. В некоторых случаях установщик бета-версии WebMatrix позволит попытается установить на платформу, которая не является частью набора поддерживаемых конфигураций. В частности Windows Vista без обновления SP1 позволит вам начать установку WebMatrix бета-версии, но компонент .NET Framework 4 не удастся и блокировать установку.
> 
> **Инструкции по решению**  
> Установите на поддерживаемых платформах, включая:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista с пакетом обновления 1 (SP1) или выше
> - Windows XP с пакетом обновления 3 (SP3)
> - Windows Server 2003 с пакетом обновления 2 (SP2)


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Проблема: Не удается установить WebMatrix бета-версии 3, если Microsoft Visual Studio 2008 устанавливается без Microsoft Visual Studio 2008 с пакетом обновления 1

> **Инструкции по решению**  
> Установка [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) из центра загрузки Майкрософт.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Проблема: Некоторые сборки для SQL Server Compact 4.0 не установлены в глобальном кэше СБОРОК

> Управляемые сборки для SQL Server Compact 4.0 не помещается в глобальный кэш сборок (GAC), при установке SQL Server Compact 4.0 на 64-разрядном компьютере и что компьютер имеет только профиле .NET Framework 3.5 с пакетом обновления 1 клиента установлен. Управляемые сборки, которые не установлены в глобальном кэше СБОРОК являются:
> 
> - *System.Data.SqlServerCe.dll* (поставщика ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Инструкции по решению**  
> Удаление SQL Server Compact 4.0. Загрузить и установить полную версию .NET Framework 3.5 с пакетом обновления 1 из следующего расположения:  
>   
> [Microsoft .NET Framework 3.5 пакетом обновления 1 (полный пакет)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Затем переустановите SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Проблема: Не удается удалить SQL Server Compact с помощью командной строки

> Удаление SQL Server Compact через параметры командной строки не работает в данном выпуске.
> 
> **Инструкции по решению**  
> Используйте *программы и компоненты* в панели управления Windows, чтобы удалить Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Веб-страницы ASP.NET

Этот раздел документа описывает новые возможности, изменения и известные проблемы с бета-версии 3 веб-страниц ASP.NET с синтаксисом Razor.

- [Новые возможности](#NewFeatures)
- [Изменения](#Changes)
- [Проблемы](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Новые возможности в бета-версии 3 для веб-страниц ASP.NET с синтаксисом Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Новинка! «Html.Raw» метод выводит разметку без кодировки

> Новый `Html.Raw` метод позволяет отобразить HTML-разметку как разметку вместо отображения закодированную выходную. (По умолчанию ASP.NET Razor кодирует строки перед их отображением.) Синтаксис выглядит следующим образом.
> 
> `Html.Raw(value)`
> 
> В следующем примере показано использование `Html.Raw`.
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Изменения в бета-версии 3 для веб-страниц ASP.NET с синтаксисом Razor

#### <a name="change-hrefattribute-method-removed"></a>Изменения: Удалить метод «HrefAttribute»

> `HrefAttribute` Метод `WebPage` класс был удален. Этот вспомогательный была использована для преобразования небезопасных символов в URL-адреса. Он больше не требуется, так как ASP.NET Razor автоматически кодирует строки. (Используйте новую `Html.Raw` метод для обработки строк без кодировки.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Изменение: Синтаксис для декларативной "@helper" Изменить вспомогательные методы

> В бета-версии 3, ASP.NET изменяет анализ вспомогательные методы, которые создаются с помощью `@helper` синтаксиса. По сути `@helper` синтаксис теперь анализируется как блок кода, а не как блок разметку, которая может включать код. Таким образом, код внутри модуля поддержки не нужно заключать в `@{ }` блоков. И наоборот, должно быть явно включено, в элементы HTML или ASP.NET Razor разметки внутри модуля поддержки `<text></text>` тегов.
> 
> Например, следующая `@helper` синтаксис работает в бета-версии 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> В бета-версии 3 необходимо изменить этого вспомогательного объекта должен выглядеть как в следующем примере:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Обратите внимание, что `@{ }` символы во вспомогательном методе исходный код больше не используется. Это так, как содержимое помощников рассматривается как блок кода по умолчанию. Вспомогательный объект отображает разметку, начинающуюся с открывающей `<a>` тег. Если вспомогательный объект должен представлять обычного текста или меток, которые будут отсутствовать закрывающий тег (например, `<meta>` тегов), должен быть содержимое для отображения в `<text></text>` тегов.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Изменений: Удаление «WebPageContext.HttpContext»

> `WebPageContext.HttpContext` Было удалено. Взамен рекомендуется использовать `HttpContext.Current` . ( `WebPageContext.HttpContext` Это просто оболочке свойство.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Изменение: Переместить в новый пакет вспомогательный «Facebook»

> `Facebook` Вспомогательный был перемещен в *Facebook.Helper* библиотеку, которая включает в себя `Facebook` вспомогательный метод и дополнительные функциональные возможности. Необходимо установить эту библиотеку как отдельный пакет, как описано в «Установка вспомогательных методов с помощью диспетчера пакетов» учебника [Приступая к работе со страницами ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Изменение: Членство, роли и безопасность типов перемещает новую сборку

> Следующие типы были перемещены в `WebMatrix.WebData` сборки:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>: Класс «TagBuilder» была перемещена на сборку System.Web.WebPages.dll

> `TagBuilde` r класса был перемещен в сборку System.Web.WebPages.dll. До настоящего времени в сборке, которая входила в состав платформы ASP.NET MVC. Это изменение означает, что не понадобится устанавливать ASP.NET MVC для использования `TagBuilder` класса.
> 
> Однако по-прежнему в класс является `System.Web.Mvc` пространства имен. Чтобы использовать `TagBuilder` класса (например, в пользовательских ASP.NET Razor вспомогательный) необходимо сослаться на пространство имен (например, путем добавления `@using System.Web.Mvc` в код).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Изменение: Запрос синтаксис проверки изменены; Удалить класс «Проверка»

> В бета-версии 3, чтобы отключить проверку для отдельных полей или набор полей, можно вызвать `Validation.Exclude` метод, передавая имя или имена полей, которые необходимо исключить из проверки. Новый синтаксис доступен в бета-версии 3 для обхода проверки. `Validation` Метод, используемый в бета-версии 3 были удалены.
> 
> > [!NOTE]
> > Если не отключить проверку запросов, если пользователь попытается отправить HTML-разметка (например, с помощью редактора форматированного текста на странице), веб-сайт сообщит об ошибке *от клиентаОбнаруженопотенциальноопасноезначениеRequest.Form*и входные данные пользователя не принят. Если отключить проверку запросов, необходимо вручную проверить ввод данных пользователем, чтобы убедиться в том, что он не содержит потенциально опасной разметки или с помощью примерно [Microsoft версии 4.0 библиотеки скриптов узла защиты между](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Чтобы отключить автоматическую проверку запросов, вызовите `Request.Unvalidated` метод, передав ему имя поля или другому объекту post, который вы хотите обойти проверку запросов для. Этот метод можно использовать для обхода проверки для всех элементов в `Form`, `QueryString`, `Cookies`, и `ServerVariables` коллекции. В следующих примерах показано использование `Unvalidated` метода:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Известные проблемы для веб-страницы ASP.NET с синтаксисом Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Проблема: Непредвиденное поведение при использовании пользовательские таблицы для членства

> Чтобы инициализировать поставщик членства для веб-сайта ASP.NET Razor, вызовите `WebSecurity.InitializeDatabaseConnection` метод. (В WebMatrix шаблоне начального сайта включает вызов этого метода в  *\_AppStart.cshtml* файл.) Если `autoCreateTables` параметр этого метода имеет значение в значение true (по умолчанию, ему присваивается значение true, если в шаблоне начального сайта), и если имя таблицы нераспознанный передается в метод (второй параметр), метод не выдает ошибку. Вместо этого он автоматически создадут таблицу.
> 
> Это может вызвать проблемы, если планируется использовать пользовательские таблицы членства, но передает имя неправильной таблицы в `WebSecurity.InitializeDatabaseConnection` метод. Поскольку метод по умолчанию вызывает ошибку, если таблицы не существует, и вместо этого он создает новую таблицу, приложение может появиться работает. Тем не менее код приложения, который основан на пользовательские таблицы (и поля в нем) со временем может завершиться непредвиденные ошибки.
> 
> **Инструкции по решению**  
> Убедитесь, что имя, переданное в `InitializeDatabaseConnection` метод соответствует таблице в базе данных членства профиль пользователя, или убедитесь, что `autoCreateTables` параметр имеет значение false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Ошибка, проблема: «не удалось сформировать пользовательский экземпляр SQL Server»

> Если выполняется IIS 7.5 в Windows 7 или Windows Server 2008 R2 WebMatrix веб-приложение использует SQL Server Express, может появиться сообщение о том, что SQL Server не удалось получить путь локального приложения пользователя во время выполнения.
> 
> **Инструкции по решению** убедитесь, что учетная запись Windows, то приложение запущено под (как правило, NETWORK SERVICE) имеет разрешения на чтение и запись для корневой папки приложения и вложенные папки например *приложения\_данных*. Более подробные сведения можно найти в статье базы знаний [проблемы с SQL Server Express пользователя при создании экземпляров и проектами веб-приложений ASP.net](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Проблема: В Visual Studio, пространства имен для пользовательских сборок (DLL) не импортируются автоматически

> Если вы используете пользовательские сборки в проект в Visual Studio, пространства имен, объявленные в эти сборки не импортируются автоматически во время разработки. В результате ссылки на пользовательские типы могут не распознаваться во время разработки, помечаются как не указано в Visual Studio (с помощью «волнистая линия»). Это происходит только во время разработки в Visual Studio. само приложение работает правильно.
> 
> **Инструкции по решению**  
> Включить `using` инструкции (`imports` в Visual Basic), ссылается на сущности, которые не распознаются во время разработки.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Проблема: Visual Studio IntelliSense и шаблоны проектов доступны только в ASP.NET MVC 3

> Установка веб-страниц ASP.NET не также устанавливает средства для Visual Studio как IntelliSense и шаблоны проектов для приложений веб-страниц ASP.NET.
> 
> **Инструкции по решению** для использования IntelliSense и шаблоны проектов для приложений веб-страниц ASP.NET в Visual Studio, установить ASP.NET MVC 3, версия-Кандидат либо с помощью установщика веб-платформы или [автономного установщика](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Проблема: «&lt;вспомогательный&gt; не удается найти класс» ошибка

> После обновления до бета-версии 3, может появится сообщение об ошибке, вспомогательный класс (например, `Facebook` класс) не удается найти. Начиная с версии Beta 2 и продолжить в бета-версии 3, были перемещены вспомогательные методы для пакетов, необходимо явно установить. Существующие узлы не будут обновлены до включать эти пакеты; Сюда входят узлы в *\My Documents\IISExpress* или *\My Documents\My веб-сайтов* папки. В частности, вы увидите эту ошибку при использовании сайта по умолчанию в *личных узлов* (WebSite1), которая содержит ссылку на `Twitter` вспомогательный.
> 
> **Инструкции по решению**  
> Закомментируйте вызовы все вспомогательные объекты на сайте, запустите  *\_администратора* и установить пакет или пакеты, содержащие вспомогательные методы, которые вы хотите использовать. После установки пакета вы можете раскомментировать строки, которые ссылаются вспомогательных методов.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Проблема: Развертывание сборок бета-версии 3 ASP.NET Razor в папку Bin может не работать для размещения сайтов

> При развертывании на веб-сайт веб-страниц ASP.NET на сайте, и в том случае, если развертывание сборок ASP.NET Razor бета-версии 3 на сайт *Bin* папки, могут возникнуть ошибки, включая следующие:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Это может произойти, если поставщик услуг размещения установил сборки ASP.NET Web Pages бета-версии 1 в кэш глобальной приложения сервера (GAC). Сборки в глобальном кэше СБОРОК получить более высокий приоритет над сборок, установленных локально в *Bin* папки.
> 
> **Инструкции по решению** свяжитесь с поставщиком услуг размещения для подтверждения того, вы видите ошибки из-за конфликта версий поставщика сборок и указанное имя. В этом случае запрос обновления, поставщик услуг размещения сборок в глобальном кэше СБОРОК на сервере.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Проблема: Чтение веб-каналов или другие внешние источники данных через прокси-сервер

> Если на сервере сайта находится за прокси-сервер, может потребоваться настроить сведения о прокси-сервера в *Web.config* файл, чтобы иметь возможность считывать данные, поступающие от вне локального узла. Например, если вы используете `ReCaptcha` вспомогательный, вспомогательное приложение взаимодействует со службой reCAPTCHA, но может быть заблокирован прокси-сервером. Аналогично веб-каналов, используемые веб-страницах ASP.NET, такие как веб-канал, используемый диспетчер пакетов, может потребоваться настройка прокси-сервера.
> 
> Если возникли проблемы в работе с внешней службы или при работе с пакетом, в веб-канала, помещаются следующие элементы в корневого каталога приложения *Web.config* файла:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Дополнительные сведения о настройке прокси-сервера см. в разделе [ &lt;прокси&gt; Element (Network Settings)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) на сайте MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Проблема: Ошибка «Не удается загрузить Microsoft.Web.Infrastructure.dll»

> Если вы ранее установили версию бета-версии 1 веб-страниц ASP.NET с синтаксисом Razor, а затем установить версию бета-версии 3, все соответствующие сборки будут установлены в глобальном кэше СБОРОК, за исключением *Microsoft.Web.Infrastructure.dll*. Как следствие, при запуске страницы ASP.NET Razor появится сообщение об ошибке, которое указывает, что *Microsoft.Web.Infrastructure.dll* не может быть загружена.
> 
> Эта проблема не возникает, если вы загрузили бета-версии 3 на чистом компьютере.
> 
> **Инструкции по решению**  
> На панели управления удалите веб-страниц ASP.NET. Затем переустановите бета-версии 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Проблема: Удаление .NET Framework версии 4 отключает веб-страниц ASP.NET с синтаксисом Razor

> Если удалить .NET Framework версии 4, а затем переустановите его, веб-страниц ASP.NET с синтаксисом Razor отключена. Страницы с *.cshtml* расширения работают правильно. Веб-страницы ASP.NET регистрирует сборку в корневой папке машины *Web.config* файла и удаление .NET Framework удаляет этот файл. Повторная установка .NET Framework устанавливает новую версию файла конфигурации, но не добавляет ссылку для сборки веб-страниц ASP.NET.
> 
> **Инструкции по решению** после повторной установки .NET Framework, переустановите ASP.NET Web Pages с синтаксисом Razor. При этом добавляется следующий элемент для *Web.config* файл в корне машины, которое обычно находится в следующем расположении:  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
>   
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Проблема: Приложения, развернутые ранее с помощью ASP.NET сборок в папке Bin возникать ошибки

> Во время развертывания, копии сборок веб-страниц ASP.NET (например, *Microsoft.WebPages.dll*) для *Bin* папку веб-сайта на сервере. (Это может произойти автоматически во время развертывания или потому, что разработчик явно копии сборок.) Тем не менее если установлена бета-версии 3, ошибки происходит, например ошибки, которые не удается найти определенных типов. Это происходит, поскольку несколько типов веб-страниц ASP.NET были перемещены в разных пространствах имен для бета-версии 3.
> 
> **Инструкции по решению**   
> Очистить *Bin* папке развернутого приложения, скопировать новые сборки в папку (или повторного развертывания приложения), а затем перезапустите приложение.


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Проблема: Без расширений URL-адреса не найдены файлы.cshtml/.vbhtml в IIS 7 или IIS 7.5

> В IIS 7 или IIS 7.5, запросы с URL-адреса следующего вида не удается найти страниц, имеющих *.cshtml* или *.vbhtml* расширения:  
>   
> `http://www.example.com/ExampleSite/ExampleFile`  
>   
> Проблема возникает из-за перезаписи URL-адресов не включена по умолчанию для IIS 7 или IIS 7.5. Возможная сценарий существует, вы не видите проблемы при тестировании локально с помощью IIS Express, но возникают при развертывании веб-сайта для размещения веб-сайта.
> 
> **Инструкции по решению**
> 
> - Если у вас есть контроль над компьютером сервера, на компьютере сервера установите обновление, описанное в [обновление доступно, что позволяет определенным обработчикам служб IIS 7.0 или IIS 7.5 обрабатывать запросы, URL-адреса не заканчиваться точкой](https://support.microsoft.com/kb/980368).
> - Если у вас по управлению компьютером сервера (например, развертывается для размещения веб-сайта), добавьте следующий на веб-сайт *Web.config* файла:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Проблема: С помощью проекта веб-приложения или страницы ASP.NET MVC и веб-ASP.NET в одном приложении

> При использовании веб-страниц ASP.NET в проект веб-приложения или в приложении ASP.NET MVC, может появится сообщение об ошибке, *WebPageHttpApplication* не удается найти.
> 
> **Инструкции по решению**  
> При появлении этой ошибки измените базовый класс, от которого наследует приложения. В *Global.asax* файл, измените следующую строку:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> на эту:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Это фактически отменяет изменения, которая была введена в выпуске бета-версии 1 веб-страниц ASP.NET с синтаксисом Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Проблема: Развертывание приложения на компьютере, не поддерживает SQL Server Compact установлена

> Приложения, включающие баз данных SQL Server Compact могут выполняться на компьютере, где SQL Server Compact не установлена. Microsoft WebMatrix бета-версии 3 автоматически копирует эти двоичные файлы для вас и выполняет соответствующее *Web.config* файла преобразования.
> 
> **Инструкции по решению** Если вам нужно скопировать эти файлы и сделать *Web.config* изменения файлов вручную, выполните следующие действия:
> 
> 1. Скопируйте сборки ядра базы данных для *Bin* папку (и вложенные папки), приложения на целевом компьютере: 
> 
>     - Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **для** *\Bin*
>     - Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **для** *\Bin\x86*
>     - Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **для** *\Bin\amd64*
> 2. В корневой папке веб-сайта, создайте или откройте *Web.config* файла. (В WebMatrix бета-версии 3, этот тип файлов доступна при выборе **все** в **выберите тип файла** диалоговое окно.)
> 3. Добавьте следующий элемент в качестве дочернего элемента  **&lt;конфигурации&gt;**  элемента (не внутри  **&lt;system.web&gt;**  элемент):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Проблема: Базы данных и WebGrid вспомогательные функции не работают со средним уровнем доверия в Visual Basic

> Если вы используете Visual Basic (создание *.vbhtml* файлы), `Database` и `WebGrid` вспомогательные методы не будет работать, если приложения настроен для использования со средним уровнем доверия.
> 
> **Инструкции по решению**  
> Временно задайте приложение для использования полного доверия.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Проблема: Свойство «Encrypt» не распознан

> SQL Server Compact 4.0 не распознает `Encrypt` свойство `SqlCeConnection` класса. Это свойство не следует использовать для шифрования файлов базы данных. `Encrypt` Свойство устарела в SQL Server Compact 3.5 выпуска и сохранен только для обратной совместимости. 
> 
> **Инструкции по решению**  
> Используйте `Encryption Mode` свойство `SqlCeConnection` класса для шифрования файлов базы данных SQL Server Compact 4.0. В следующем примере показано, как создание зашифрованные базы данных SQL Server Compact 4.0 с помощью `Encryption Mode` свойство:
>  
> [!code-csharp[Main](beta3/samples/sample11.cs)]
>  
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Смена режима шифрования существующей базы данных SQL Server Compact 4.0, выполните следующие действия.
>  
> [!code-csharp[Main](beta3/samples/sample13.cs)]
>  
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Шифрование незашифрованной базы данных SQL Server Compact 4.0, выполните следующие действия.
>  
> [!code-csharp[Main](beta3/samples/sample15.cs)]
>  
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Проблема: Необходимы библиотеки времени выполнения Microsoft Visual C++ 2008

> Собственные библиотеки DLL из SQL Server Compact 4.0 требуется Microsoft Visual C++ 2008 Runtime Libraries (x 86, IA64 и x 64) с пакетом обновления 1.
> 
> **Инструкции по решению**  
> Установите .NET Framework 3.5 SP1. Также устанавливает SP1 библиотеки среды выполнения Visual C++ 2008. Эти библиотеки можно загрузить из следующей папки:   
>   
> [Обновления безопасности ATL для распространяемого пакета Microsoft Visual C++ 2008 с пакетом обновления 1](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Обратите внимание, что установка .NET Framework 2.0, 3.0, или 4 не *не* установить SP1 библиотеки среды выполнения Visual C++ 2008.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Проблема: Если до установки .NET Framework на компьютере установлен SQL Server Compact, его неизменяемое имя поставщика не зарегистрирован в файле machine.config платформы .NET Framework

> SQL Server Compact может устанавливаться на компьютер, где не установлена платформа .NET Framework, так как SQL Server Compact требуется .NET framework. Если ни .NET Framework версии 3.5 и 4 не установлен перед установкой SQL Server Compact, Compact установки SQL Server не регистрирует его неизменяемое имя поставщика в *machine.config* файла. Все приложения, основанные на SQL Server Compact запись в *machine.config* файл не удастся. Неизменяемое имя регистрационной записи в *machine.config* выглядит как следующий пример:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Инструкции по решению**  
> Удаление SQL Server Compact 4.0 CTP-версия 1. Загрузите и установите полной версии платформы .NET Framework из следующего расположения:
> 
> [Microsoft .NET Framework 3.5 пакетом обновления 1 (полный пакет)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Microsoft .NET Framework 4.0 и выше (полный пакет)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Затем переустановите [SQL Server Compact 4.0 CTP-версия 1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Установка приложений

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Проблема: При установке приложения может занять много времени при пользовательской папке "Мои документы" перенаправляется в общую сетевую папку

> **Инструкции по решению**  
> Отсутствует. Приложение может занять некоторое время, чтобы установить, но устанавливается правильно.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Публикация приложений

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Проблема: Сайта может не работать после публикации, если поле «URL-адрес назначения» нет префикса http:// или https://

> В **параметры публикации** диалоговое окно, если URL-адрес назначения не начинается с `http://` или `https://`, сайт может не работать после развертывания.
> 
> **Инструкции по решению**  
> Убедитесь, что перед публикацией сайта, URL-адрес назначения в **параметры публикации** диалоговое окно начинается с `http://` или `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Проблема: Публикация базы данных MySQL завершается с ошибкой «не удалось опубликовать базу данных. Это может произойти, если удаленная база данных не может запустить сценарий.»

> Эта ошибка может возникнуть по ряду причин. Одна из причин этой ошибки можно увидеть, — если сценарий базы данных содержит символ одиночные кавычки (') и не является база данных MySQL назначения по умолчанию используется кодировка UTF-8.
> 
> **Инструкции по решению**  
> Задайте кодировку по умолчанию для удаленной базы данных MySQL в UTF-8.


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Другие проблемы

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Проблема: Поиска и фильтрации не работает в отчетах для Group By: тип проблемы

> При запуске отчета для сайта, если ввести текст в *фильтр по URL-адрес* и нажмите кнопку *поиска*, ничего не происходит. Это, поскольку этот элемент управления не работает при *Group By* присваивается состояние отчета *тип проблемы*, используемого по умолчанию.
> 
> **Инструкции по решению** в *Group By* вкладку ленты, нажмите кнопку *URL-адрес* для группировки записей по их URL-адрес источника. Текстовое поле и кнопку для фильтрации записей функциональна в этом состоянии.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Проблема: Приложений WCF не удастся выполнить с IIS Express

> Просмотр приложения WCF приводит к ошибке, как показано ниже:
> 
> *Не удалось загрузить файл или сборку ' Microsoft.Web.Administration, Version = 7.0.0.0, но, язык и региональные параметры = neutral, PublicKeyToken = 31bf3856ad364e35 "или одну из ее зависимостей. Не удается найти указанный файл.*
> 
> Это происходит, поскольку версия IIS Express бета-версия не поддерживает WCF по умолчанию.
> 
> **Инструкции по решению** , воспользуйтесь одним из следующих решений (инструкции по решению #2 требуется Microsoft Windows Vista или более поздней версии):
> 
> 
> 1. Копировать *Microsoft.Web.dll* и *Microsoft.Web.Administration.dll* сборки из расположения установки WebMatrix для *bin* каталог WCF приложение. По умолчанию, WebMatrix устанавливается в *Microsoft WebMatrix* вложенной папке системы *Program Files* папки.
> 2. В Microsoft Windows Vista или более поздней версии, создать символьную ссылку на сборки в *bin* каталог, используя следующие команды. (Этот подход имеет то преимущество, что он не создает копию сборок).
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Установите две сборки в глобальном кэше СБОРОК. С повышенными привилегиями выполните следующие команды:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Проблема: Не удалось выполнить определенные задачи, требующие повышения прав WebMatrix бета-версия 3

> WebMatrix бета-версии 3 не может выполнять определенные задачи, требующие повышения прав, таких как установка дополнительных компонентов в следующих ситуациях:
> 
> - В Windows Vista или Windows 7 вы вошли под учетной записью, у которого нет прав администратора, и управления учетных записей (UAC) отключено.
> - Вы используете Microsoft Windows XP или Microsoft Windows Server 2003.
> 
> **Инструкции по решению**  
> Большинство задач в бета-версии 3 WebMatrix необходимы разрешения администратора. Для тех, которые можно выполнять операции от имени администратора или выполните следующие действия:
> 
> - В Windows Vista или Windows 7 включите контроль учетных Записей.
> - В Windows XP добавьте пользователя в группу безопасности "Администраторы".


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Проблема: Отключен «Из галерею веб-узла»

> **Сайта из веб-коллекции** параметр отключен, если не установлен установщик веб-платформы 3.0.
> 
> **Инструкции по решению**  
> Установка [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Проблема: В Windows Server 2003, IIS Express не запускается для обычных пользователей

> В Windows Server 2003 при запуске страницы или запустить IIS Express, IIS Express не запускается. Для веб-страниц выводится сообщение об ошибке, указывающее, что приложение запущено пользователем без прав администратора.
> 
> **Инструкции по решению**  
> Пользователь с правами администратора для запуска WebMatrix бета-версии 3. Дополнительные сведения см. в следующей статье базы знаний:  
>   
> [Приложение, которое запускается пользователем без прав администратора не может прослушивать HTTP-трафика компьютера, на котором выполняется приложение в Windows Vista, Windows Server 2003 или Windows XP.](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Проблема: Google Chrome не доступен в качестве параметра запуска

> Google Chrome не отображается в списке браузеров в **запуска** на **Главная** вкладки.
> 
> **Инструкции по решению**  
> В некоторых версиях Google Chrome не регистрируют себя правильно с функцией программ по умолчанию в Windows. Чтобы избежать этого, Google Chrome, выберите пункт *Настройка и управление Google Chrome* меню, нажмите кнопку *параметры*и нажмите кнопку *марка Google Chrome обозреватель по умолчанию*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Проблема: Диалоговое окно «Foreign Key» не допускает ввод первичного ключа

> **Foreign Key** диалоговое окно не позволяет ввести имя первичного ключа из таблицы первичного ключа.
> 
> **Инструкции по решению**  
> Это сделано намеренно. Необходимо ввести имя первичного ключа из таблицы первичного ключа.


#### <a name="issue-the-relationships-button-is-disabled"></a>Проблема: Кнопка «Связей» отключена

> **Связи** под заголовком **таблицы** вкладке **баз данных** рабочей отключена для баз данных SQL Server Compact.
> 
> **Инструкции по решению**  
> Отсутствует. SQL Server Compact не поддерживает связи между таблицами.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Проблема: Параметризованные SQL-запросы исключений

> В SQL Server Compact 4.0, если не указать тип данных таких как `SqlDbType` или `DbType` для параметров в параметризованных запросах, создается исключение при выполнении запроса.
> 
> **Инструкции по решению**  
> Явно задать тип данных для параметров, таких как `SqlDbType` или `DbType`. Это очень важно в случае с типами данных больших двоичных ОБЪЕКТОВ (`image` и `ntext`). Используйте код, аналогичный следующему:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
>  
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Дополнительные сведения

Дополнительные сведения о WebMatrix бета-версии 3 см. ниже веб-сайтов:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/Web](https://www.microsoft.com/web)

* * *

© Корпорация Майкрософт, 2010. Все права защищены. [Условия использования](https://msdn.microsoft.com/en-us/cc300389.aspx).
