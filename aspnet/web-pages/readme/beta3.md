---
uid: web-pages/readme/beta3
title: Web Matrix и ASP.NET Web Pages (Razor) о бета-версии 3 выпуска | Документация Майкрософт
author: rick-anderson
description: Web Matrix и ASP.NET Web Pages (Razor) о бета-версии 3 выпуска
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 3d729d1b0615533dddceff484acb3d42247f6cab
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839143"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Web Matrix и ASP.NET Web Pages (Razor) о бета-версии 3 выпуска
====================
> Web Matrix и ASP.NET Web Pages (Razor) о бета-версии 3 выпуска

9 ноября 2010 г.

## <a name="contents"></a>Описание

- [Обзор набора средств Visual Studio для Unity](#Overview)
- [Установка](#Installation_Notes)
- [Новые возможности, изменения и известные проблемы в бета-версии 3](#Known_Issues)

    - [Проблемы установки WebMatrix](#Known_Issues_Installation)
    - [Веб-страницы ASP.NET](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Установка приложений](#Known_Issues_Installing_Applications)
    - [Публикация приложений](#Known_Issues_Publishing_Applications)
    - [Другие проблемы](#Known_Issues_Other_Issues)
- [Дополнительные сведения](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Обзор

> Бета-версия Microsoft WebMatrix представляет собой стек разработки бесплатного, которая устанавливается за считанные минуты. Веб-сервера он интегрируется с базой данных и платформами программирования для создания единой, интегрированной среды. WebMatrix бета-версии можно использовать для упрощения разработки кода, тестирования и публикации собственного ASP.NET или PHP, веб-сайта или бета-версии WebMatrix можно использовать для создания нового веб-сайтов с помощью популярных приложений с открытым кодом как DotNetNuke, Umbraco, WordPress и Joomla. Бета-версии WebMatrix использует же мощный веб-сервер, СУБД и платформы среду, которая будет выполняться веб-сайта в Интернете, что упрощает переход от разработки в рабочей среде и ускоряет.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Установка

> Чтобы установить WebMatrix бета-версии 3, используйте [веб-платформы Microsoft Installer-3.0](https://go.microsoft.com/fwlink/?LinkID=194638). После установки установщика веб-платформы, его можно использовать для установки WebMatrix бета-версии 3.
> 
> При возникновении проблем во время установки, см. [Устранение неполадок с Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Инструкции для публикации приложений

> См. в разделе [пошаговые инструкции для публикации приложений](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Новые возможности, изменения, andKnown проблемы

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Установка WebMatrix бета-версия 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Проблема: WebMatrix бета-версия 3 доступна только на платформах, поддерживающих Microsoft .NET Framework 4

> В .NET Framework версии 4 является обязательным для WebMatrix бета-версии. В некоторых случаях установщик бета-версии WebMatrix позволит установке на платформу, которая не является частью набора поддерживаемых конфигураций. В частности Windows Vista без обновления SP1 позволяет начать установку WebMatrix бета-версии, но компонент .NET Framework 4 не удастся и блокировать установку.
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


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Проблема: Не удается установить WebMatrix бета-версии 3, если Microsoft Visual Studio 2008 установлено без Microsoft Visual Studio 2008 с пакетом обновления 1

> **Инструкции по решению**  
> Установка [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) из центра загрузки Майкрософт.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Проблема: Некоторые сборки для SQL Server Compact 4.0 не установлены в глобальном кэше СБОРОК

> Управляемые сборки для SQL Server Compact 4.0 не помещается в глобальный кэш сборок (GAC), при установке SQL Server Compact 4.0 на компьютере с 64-разрядной, и на компьютере установлена только профиль .NET Framework 3.5 с пакетом обновления 1 клиента установлен. Управляемые сборки, которые не установлены в глобальном кэше СБОРОК являются:
> 
> - *System.Data.SqlServerCe.dll* (поставщик ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Инструкции по решению**  
> Удаление SQL Server Compact 4.0. Скачайте и установите полную версию .NET Framework 3.5 SP1 из следующего расположения:  
>   
> [Microsoft .NET Framework 3.5 пакетом обновления 1 (полный пакет)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Затем переустановите SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Проблема: Не удается удалить SQL Server Compact с помощью командной строки

> Удаление SQL Server Compact через параметры командной строки не работает в этом выпуске.
> 
> **Инструкции по решению**  
> Используйте *программы и компоненты* на панели управления Windows, чтобы удалить Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Веб-страницы ASP.NET

Этот раздел документа описывает новые возможности, изменения и известные проблемы с бета-версии 3 веб-страниц ASP.NET с синтаксисом Razor.

- [Новые функции](#NewFeatures)
- [Изменения](#Changes)
- [Проблемы](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Новые возможности бета-версии 3 для веб-страниц ASP.NET с синтаксисом Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Новинка: «Html.Raw» метод выводит разметку без кодировки

> Новый `Html.Raw` метод позволяет отображать разметку HTML в виде разметки вместо отображения закодированную выходную. (По умолчанию ASP.NET Razor кодирует строки перед их отображением.) Синтаксис выглядит следующим образом.
> 
> `Html.Raw(value)`
> 
> В следующем примере показано использование `Html.Raw`.
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Изменения в бета-версии 3 для веб-страниц ASP.NET с синтаксисом Razor

#### <a name="change-hrefattribute-method-removed"></a>Изменение: Удален метод «HrefAttribute»

> `HrefAttribute` Метод `WebPage` класс был удален. Этот вспомогательный был использован для кодирования небезопасные символы URL-адреса. Он больше не требуется, так как ASP.NET Razor автоматически кодирует строки. (Использовать новый `Html.Raw` метод для обработки строк без кодировки.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Изменение: Синтаксис для декларативной "@helper" Изменить вспомогательные функции

> В выпуске бета-версии 3, ASP.NET изменяет анализе вспомогательные функции, которые создаются с помощью `@helper` синтаксис. По сути `@helper` синтаксис теперь анализируется как блок кода, а не как блок разметки, которая может включать код. Таким образом, код внутри модуля поддержки не нужно заключать в `@{ }` блоков. И наоборот, это разметка внутри модуля поддержки должно быть явно включено, в элементы HTML или ASP.NET Razor `<text></text>` теги.
> 
> Например, следующая `@helper` работает в выпуске бета-версии 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> В выпуске бета-версии 3 этого вспомогательного объекта необходимо изменить таким образом, как показано в следующем примере:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Обратите внимание, что `@{ }` символы исходный код во вспомогательном методе больше не используется. Это обусловлено содержимое из вспомогательных методов, обрабатываются как блок кода по умолчанию. Вспомогательное приложение выводит разметку, начинающуюся с открывающей `<a>` тега. Если вспомогательное приложение необходимо подготовить обычного текста или меток, которые будут отсутствовать закрывающий тег (например, `<meta>` тегов), визуализируемое содержимое должно быть в `<text></text>` теги.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Изменений: Удаление «WebPageContext.HttpContext»

> `WebPageContext.HttpContext` Свойство был удален. Взамен рекомендуется использовать `HttpContext.Current`. ( `WebPageContext.HttpContext` Свойство просто обернул это.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Изменение: Переместить в новый пакет вспомогательный «Facebook»

> `Facebook` Вспомогательный был перемещен в *Facebook.Helper* библиотеки, которая содержит `Facebook` вспомогательный метод и дополнительные функции. Необходимо установить эту библиотеку как отдельный пакет, как описано в «Установка вспомогательных функций с помощью диспетчера пакетов» в этом руководстве [Приступая к работе со страницами ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Изменение: Типы членства, ролей и безопасности перемещает новую сборку

> Следующие типы были перемещены `WebMatrix.WebData` сборки:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>: Класс «TagBuilder» перемещена на сборку System.Web.WebPages.dll

> `TagBuilde` r класс был перемещен в сборку System.Web.WebPages.dll. Ранее это было в сборке, который был частью ASP.NET MVC. Это изменение означает, что не нужно установить ASP.NET MVC, чтобы использовать `TagBuilder` класса.
> 
> Тем не менее, класс является по-прежнему в `System.Web.Mvc` пространства имен. Чтобы использовать `TagBuilder` класса (например, в пользовательских ASP.NET Razor вспомогательный объект), необходимо сослаться на пространство имен (например, путем добавления `@using System.Web.Mvc` в код).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Изменение: Запрос проверка синтаксиса изменены; Удален класс «Проверка»

> В выпуске бета-версии 3, чтобы отключить проверку для отдельных полей или набор полей, можно вызвать `Validation.Exclude` метод, передавая имя или имена полей для исключения из проверки. Новый синтаксис доступен в бета-версии 3 для обхода проверки. `Validation` Метод, используемый в бета-версии 3 были удалены.
> 
> > [!NOTE]
> > Если не отключить проверку запросов, если пользователь попытается отправить HTML-разметка (например, с помощью редактора форматированного текста на странице), веб-сайт сообщит об ошибке, например *было обнаружено потенциально опасных значения Request.Form от клиента*и входные данные пользователя не принят. Если вы отключите проверку запросов, необходимо самостоятельно записать после ввода, чтобы убедиться в том, что оно не должно содержать потенциально опасные разметки или скрипт с помощью примерно [Microsoft версии 4.0 библиотеки сценариев сайта защиты между](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Чтобы отключить автоматическую проверку запросов, вызовите `Request.Unvalidated` метод, передав ему имя поля или другой объект post, который вы хотите пропустить проверку запроса для. Этот метод можно использовать для обхода проверки для всех элементов в `Form`, `QueryString`, `Cookies`, и `ServerVariables` коллекций. Следующие примеры показывают, как использовать `Unvalidated` метод:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Известные проблемы веб-страниц ASP.NET с синтаксисом Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Проблема: Непредвиденное поведение при использовании пользовательского таблицы членства

> Чтобы инициализировать поставщик членства для на веб-сайте ASP.NET Razor, вызовите `WebSecurity.InitializeDatabaseConnection` метод. (В WebMatrix шаблоне начального сайта включает вызов этого метода в  *\_AppStart.cshtml* файл.) Если `autoCreateTables` задан параметр этого метода значение true (по умолчанию ему присваивается значение true, если в шаблоне начального сайта), и если нераспознанный имя_таблицы передается в метод (второй параметр), метод выдает ошибку. Вместо этого он автоматически создадут таблицу.
> 
> Это может стать проблемой, если вы планируете использовать пользовательские таблицы членства, но передать имя таблицы не так, чтобы `WebSecurity.InitializeDatabaseConnection` метод. Поскольку метод не по умолчанию создается ошибка, если таблицы не существует, а вместо этого он создает новую таблицу, приложение может показаться работать. Тем не менее код приложения, который основан на пользовательские таблицы (и поля в нем) со временем может завершаться непредвиденные ошибки.
> 
> **Инструкции по решению**  
> Убедитесь, что имя, переданное в `InitializeDatabaseConnection` метод совпадений, профиль пользователя из таблицы в базе данных членства, или убедитесь, что `autoCreateTables` параметр имеет значение false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Ошибка, проблема: «не удалось сформировать пользовательский экземпляр SQL Server»

> Если приложение WebMatrix использует SQL Server Express и работает IIS 7.5 на Windows 7 или Windows Server 2008 R2, вы увидите ошибку, указывающую, что SQL Server не удалось получить путь локального приложения пользователя во время выполнения.
> 
> **Инструкции по решению** убедитесь в том, что учетная запись Windows, то приложение запущено под (как правило, NETWORK SERVICE) имеет разрешения на чтение и запись для корневой папки приложения и вложенные папки например *приложения\_данных*. Более подробные сведения см. в статье базы знаний [проблемы с SQL Server Express пользователя при создании экземпляров и ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Проблема: В Visual Studio, пространства имен для пользовательских сборок (DLL) не импортируются автоматически

> Если вы используете пользовательские сборки в проекте в Visual Studio, пространства имен, объявленные в эти сборки не импортируются автоматически во время разработки. Таким образом ссылки на пользовательские типы могут не распознаваться во время разработки, помечаются как не указано в Visual Studio (с помощью «волнистая линия»). Это происходит только во время разработки в Visual Studio. само приложение работает правильно.
> 
> **Инструкции по решению**  
> Включить `using` инструкции (`imports` в Visual Basic), ссылающийся на сущности, которые не распознаются во время разработки.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Проблема: Visual Studio IntelliSense и шаблоны проектов доступны только в ASP.NET MVC версии 3

> Установка веб-страниц ASP.NET не также устанавливает средства для Visual Studio такие как IntelliSense и шаблоны проектов для приложений ASP.NET Web Pages.
> 
> **Инструкции по решению** использовать IntelliSense и шаблоны проектов для приложений веб-страниц ASP.NET в Visual Studio, установить версия-Кандидат ASP.NET MVC 3 с помощью установщика веб-платформы или [автономный установщик](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Проблема: "&lt;вспомогательный&gt; не удалось найти класс» ошибка

> После обновления до бета-версии 3, может появиться ошибка, вспомогательный класс (например, `Facebook` класс) не удается найти. Начиная с бета-версии 2 и продолжая в бета-версии 3, были перемещены для пакетов, необходимо явно установить вспомогательные функции. Существующие узлы не будут обновлены до включения этих пакетов; Сюда входят сайта в *\My Documents\IISExpress* или *\My Documents\My веб-сайтов* папки. В частности, вы увидите эту ошибку при использовании сайта по умолчанию в *My Sites* (WebSite1), который включает ссылку на `Twitter` вспомогательный.
> 
> **Инструкции по решению**  
> Закомментируйте вызовы все вспомогательные функции в сайт, запустите  *\_администратора* и установить пакет или пакеты, содержащие вспомогательные методы, которые вы хотите использовать. После установки пакета, Раскомментируйте строки, которые ссылаются на вспомогательные функции.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Проблема: Развертывание сборок бета-версии 3 ASP.NET Razor в папку Bin может не работать для размещения сайтов

> При развертывании на веб-сайте веб-страниц ASP.NET для размещения сайта, и в том случае, если развернуть сборки ASP.NET Razor бета-версии 3 на сайт *Bin* папки, могут возникнуть ошибки, включая следующие:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Это может произойти, если поставщик установил сборки ASP.NET Web Pages бета-версии 1 в кэш сервера глобального приложения (GAC). Сборки в глобальном кэше СБОРОК высокий приоритет по сравнению сборок, установленных локально в *Bin* папки.
> 
> **Инструкции по решению** обратитесь к поставщику услуг размещения, убедитесь, что ошибки возникают из-за конфликта версий поставщика сборок и выбираете. Если Да, запрашивают поставщика услуг размещения об обновлении сборки в глобальном кэше СБОРОК на сервере.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Проблема: Чтение веб-каналы или другие внешние источники данных через прокси-сервер

> Если на сервер сайта находится за прокси-сервер, может потребоваться настроить сведения прокси-сервера в *Web.config* файл, чтобы иметь возможность считывать данные, поступающие от за пределами вашего сайта. Например, если вы используете `ReCaptcha` helper, вспомогательное приложение взаимодействует со службой reCAPTCHA, но может быть заблокирован прокси-сервером. Аналогично веб-каналы, которые используются в веб-страниц в ASP.NET, такие как веб-канал, используемый диспетчером пакетов, может потребоваться настройка прокси-сервера.
> 
> Если возникают проблемы в работе с внешней службы или работе с веб-канал пакетов, поместите следующие элементы в корень приложения *Web.config* файла:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Дополнительные сведения о настройке прокси-сервер, см. в разделе [ &lt;прокси&gt; (сетевые параметры)](https://msdn.microsoft.com/library/sa91de1e.aspx) на сайте MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Проблема: Ошибка «Не удается загрузить Microsoft.Web.Infrastructure.dll»

> Если вы ранее установили бета-версии 1 веб-страниц ASP.NET с синтаксисом Razor и затем установить версию бета-версии 3, все соответствующие сборки установлены в глобальном кэше СБОРОК, за исключением *Microsoft.Web.Infrastructure.dll*. Как следствие, при запуске ASP.NET Razor pages, появится сообщение об ошибке, которое указывает, что *Microsoft.Web.Infrastructure.dll* не может быть загружен.
> 
> Эта проблема не возникает, если вы загрузили выпуск бета-версии 3 на чистом компьютере.
> 
> **Инструкции по решению**  
> В панели управления удалите веб-страниц ASP.NET. Затем переустановите бета-версии 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Проблема: Удаление .NET Framework версии 4 отключает веб-страниц ASP.NET с синтаксисом Razor

> Если удалить .NET Framework версии 4 и переустановите его, веб-страниц ASP.NET с синтаксисом Razor будет отключено. Страницы с *.cshtml* расширения не выполняются правильно. Веб-страницы ASP.NET регистрирует сборку в корневом каталоге компьютера *Web.config* файла и удаление .NET Framework удаляет этот файл. Повторная установка .NET Framework устанавливает новую версию файла конфигурации, но не добавляет ссылки для сборки веб-страниц ASP.NET.
> 
> **Инструкции по решению** после повторной установки .NET Framework, переустановите ASP.NET Web Pages с синтаксисом Razor. Это добавляет следующий элемент *Web.config* файл в корневой раздел компьютера, который обычно находится в следующем расположении:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Проблема: Ранее развернутое с использованием ASP.NET сборок в папке Bin приложения возникать ошибки

> Во время развертывания, копии сборок веб-страниц ASP.NET (например, *Microsoft.WebPages.dll*) для *Bin* папку веб-сайта на сервере. (Это может произойти автоматически во время развертывания или потому, что разработчик явно копии сборок.) Тем не менее при установке бета-версии 3, ошибки происходит, например ошибки, которые не удается найти определенные типы. Это происходит, поскольку несколько типов веб-страниц ASP.NET были перемещены в разных пространствах имен для бета-версии 3.
> 
> **Инструкции по решению**   
> Очистить *Bin* папке развернутого приложения, скопируйте новые сборки в папку (или повторного развертывания приложения), а затем перезапустите приложение.


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Проблема: URL-адреса без расширений приложения не найти файлы.cshtml/.vbhtml на IIS 7 или IIS 7.5

> В IIS 7 или IIS 7.5, запросы на URL-адрес, как показано ниже не смогут найти страниц, включающих *.cshtml* или *.vbhtml* расширения:  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Проблема возникает, так как переопределение URL-адресов не включена по умолчанию для IIS 7 или IIS 7.5. Возможная ситуация не возникает проблема, при тестировании локально с помощью IIS Express, что возникают при развертывании веб-сайта для размещения веб-сайта.
> 
> **Инструкции по решению**
> 
> - Если у вас есть контроль над на компьютере сервера, на компьютере сервера установите обновление, описанное в [обновление доступно, что позволяет определенным обработчикам служб IIS 7.0 или IIS 7.5 обрабатывать запросы URL-адреса которых не оканчиваются точкой](https://support.microsoft.com/kb/980368).
> - Если у вас нет контроля над на компьютере сервера (например, развертывается для размещения веб-сайта), добавьте следующий код веб сайта *Web.config* файла:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Проблема: С помощью проекта веб-приложения или ASP.NET MVC и ASP.NET Web pages в одном приложении

> При использовании веб-страниц ASP.NET в проекте веб-приложения или приложения ASP.NET MVC, может появиться ошибка, *WebPageHttpApplication* не удается найти.
> 
> **Инструкции по решению**  
> Если вы получаете эту ошибку, измените базовый класс, от которого наследует приложения. В *Global.asax* измените следующую строку:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> На эту:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Это фактически отменяет изменения, которая была введена для выпуска бета-версии 1 веб-страниц ASP.NET с синтаксисом Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Проблема: Развертывание приложения на компьютере, не SQL Server Compact установлена

> Приложения, включающие базы данных SQL Server Compact можно запустить на компьютере, где SQL Server Compact не установлено. Бета-версии 3 Microsoft WebMatrix автоматически копирует эти двоичные файлы для вас и выполняет соответствующий *Web.config* файл преобразования.
> 
> **Инструкции по решению** Если вам нужно скопировать эти файлы и сделать *Web.config* изменения файлов вручную, выполните следующие действия:
> 
> 1. Скопируйте сборки ядра базы данных для *Bin* папки (и вложенных папок), приложения на целевом компьютере: 
> 
>     - Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **для** *\Bin*
>     - Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **для** *\Bin\x86*
>     - Копировать *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **для** *\Bin\amd64*
> 2. В корневой папке веб-сайта, создайте или откройте *Web.config* файл. (В WebMatrix бета-версии 3, этот тип файлов доступна при нажатии кнопки **все** в **выберите тип файла** диалоговое окно.)
> 3. Добавьте следующий элемент в качестве дочернего элемента **&lt;конфигурации&gt;** элемент (не внутри **&lt;system.web&gt;** элемента):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Проблема: Базы данных и WebGrid вспомогательные функции не работают в со средним уровнем доверия в Visual Basic

> Если вы используете Visual Basic (создание *.vbhtml* файлы), `Database` и `WebGrid` вспомогательные функции не будут работать, если приложение настроено для использования со средним уровнем доверия.
> 
> **Инструкции по решению**  
> Временно настройте для приложения для использования полного доверия.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Проблема: Свойство «Шифрование» не распознан

> SQL Server Compact 4.0 не распознает `Encrypt` свойство `SqlCeConnection` класса. Это свойство не следует использовать для шифрования файлов базы данных. `Encrypt` Свойство устарела в SQL Server Compact 3.5 выпуск и сохранен только для обратной совместимости. 
> 
> **Инструкции по решению**  
> Используйте `Encryption Mode` свойство `SqlCeConnection` класса для шифрования файлов базы данных SQL Server Compact 4.0. В следующем примере показано, как создать зашифрованные базы данных SQL Server Compact 4.0 с помощью `Encryption Mode` свойство:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Чтобы изменить режим шифрования существующей базы данных SQL Server Compact 4.0, сделайте следующее:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Чтобы выполнить шифрование незашифрованной базы данных SQL Server Compact 4.0, сделайте следующее:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Проблема: Необходимы библиотеки времени выполнения Microsoft Visual C++ 2008

> Собственные библиотеки DLL из SQL Server Compact 4.0, потребуется Microsoft Visual C++ 2008 Runtime Libraries (x 86, IA64 и x 64), пакетом обновления 1.
> 
> **Инструкции по решению**  
> Установите .NET Framework 3.5 SP1. При этом также устанавливаются SP1 библиотеки среды выполнения Visual C++ 2008. Эти библиотеки можно загрузить из следующего расположения:   
>   
> [Обновление безопасности ATL для распространяемого пакета Microsoft Visual C++ 2008 с пакетом обновления 1](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Обратите внимание, что установка .NET Framework 2.0, 3.0, или 4 не *не* установить SP1 библиотеки среды выполнения Visual C++ 2008.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Проблема: Если перед установкой .NET Framework на компьютере установлен SQL Server Compact, его неизменяемое имя поставщика не зарегистрирован в файле machine.config .NET Framework

> SQL Server Compact можно установить на компьютере, который не поддерживает .NET Framework, установить, так как SQL Server Compact требуется .NET framework. Если ни .NET Framework версии 3.5 и 4 не установлена, перед установкой SQL Server Compact, Compact установки SQL Server не регистрирует его неизменяемое имя поставщика в *machine.config* файл. Любое приложение, которое использует SQL Server Compact записи в *machine.config* файла завершится ошибкой. Неизменяемое имя регистрационной записи в *machine.config* выглядит как в следующем примере:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Инструкции по решению**  
> Удаление SQL Server Compact 4.0 CTP-версия 1. Скачайте и установите полные версии платформы .NET Framework из следующего расположения:
> 
> [Microsoft .NET Framework 3.5 пакетом обновления 1 (полный пакет)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Выпуск Microsoft .NET Framework 4.0 (полный пакет)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Затем переустановите [SQL Server Compact 4.0 CTP-версия 1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Установка приложений

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Проблема: При установке приложения может занять много времени, если папки «Мои документы» перенаправляется в общую сетевую папку

> **Инструкции по решению**  
> Отсутствует. Приложение может занять некоторое время, чтобы установить, но правильную установку.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Публикация приложений

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Проблема: Сайта может не работать после публикации, если поле «URL-адрес назначения» нет префикса http:// или https://

> В **параметров публикации** диалоговое окно, если URL-адрес назначения не начинается с `http://` или `https://`, сайт может не работать после развертывания.
> 
> **Инструкции по решению**  
> Убедитесь, что прежде чем опубликовать сайт, URL-адрес назначения в **параметры публикации** диалоговое окно начинается с `http://` или `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Проблема: Публикация базы данных MySQL завершается с ошибкой «не удалось опубликовать базу данных. Это может произойти, если удаленная база данных нельзя запустить сценарий.»

> Эта ошибка может возникнуть по ряду причин. Одна из причин, можно увидеть, эта ошибка — Если скрипт базы данных одинарная кавычка (') и не может базы данных MySQL назначения по умолчанию используется кодировка UTF-8.
> 
> **Инструкции по решению**  
> Задайте кодировку по умолчанию для удаленной базы данных MySQL в UTF-8.


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Другие проблемы

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Проблема: Поиска и фильтрации не работает в отчетах для Group By: тип проблемы

> При запуске отчета для узла, когда вы вводите текст в *фильтровать по URL-адрес* поле и нажмите кнопку *поиска*, ничего не происходит. Это, поскольку этот элемент управления не работает при *Group By* присваивается состояние отчета *тип проблемы*, который используется по умолчанию.
> 
> **Инструкции по решению** в *Group By* вкладку ленты, нажмите кнопку *URL-адрес* для группировки записей по их URL-адрес источника. Текстовое поле и кнопку для фильтрации записей в этом состоянии функциональна.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Проблема: WCF приложения перестают работать с IIS Express

> Перейдя к приложения WCF вызовет ошибку, аналогичный приведенному ниже:
> 
> *Не удалось загрузить файл или сборку "Microsoft.Web.Administration, Version = 7.0.0.0, язык и региональные параметры = neutral, PublicKeyToken = 31bf3856ad364e35" или одну из ее зависимостей. Не удается найти указанный файл.*
> 
> Это происходит, потому что выпуска IIS Express бета-версия не поддерживает WCF по умолчанию.
> 
> **Инструкции по решению** , воспользуйтесь одним из следующих решений (инструкции по решению #2 требуется Microsoft Windows Vista или более поздней версии):
> 
> 
> 1. Копировать *Microsoft.Web.dll* и *файл Microsoft.Web.Administration.dll* сборки из папки установки WebMatrix, чтобы *bin* каталог WCF приложение. По умолчанию, WebMatrix устанавливается в *Microsoft WebMatrix* вложенной папке системы *Program Files* папки.
> 2. В Microsoft Windows Vista или более поздней версии, создайте символьную ссылку на сборки в *bin* каталог, используя следующие команды. (Данный подход имеет преимущество, что он не создает копию сборки).
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Установите две сборки в глобальном кэше СБОРОК. С повышенными привилегиями выполните следующие команды:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Проблема: Не удается выполнить определенные задачи, требующие повышения прав WebMatrix бета-версия 3

> WebMatrix бета-версии 3 не может выполнить определенные задачи, требующие повышения прав, таких как установка дополнительных компонентов в следующих ситуациях:
> 
> - В Windows Vista или Windows 7 вы выполняете вход с использованием учетной записи, которая не имеет прав администратора и контроля учетных записей (UAC) отключено.
> - При использовании Microsoft Windows XP или Microsoft Windows Server 2003.
> 
> **Инструкции по решению**  
> Большинство задач в WebMatrix бета-версии 3 не требуют прав администратора. Для тех, которые можно выполнить операцию с правами администратора или выполните следующие действия:
> 
> - В Windows Vista или Windows 7 включите контроль учетных Записей.
> - В Windows XP добавьте пользователя в группу безопасности администраторов.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Проблема: «Site from Web Gallery» отключена

> **Site from Web Gallery** параметр отключен, если не установлен установщик веб-платформы 3.0.
> 
> **Инструкции по решению**  
> Установка [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Проблема: В Windows Server 2003, IIS Express не запускается для обычных пользователей

> В Windows Server 2003 при открывает страницу или запуске IIS Express и IIS Express не запускается. Для веб-страниц выводится сообщение об ошибке, указывающее, что приложения был запущен пользователем без прав администратора.
> 
> **Инструкции по решению**  
> Запуск WebMatrix бета-версии 3, пользователь с правами администратора. Дополнительные сведения см. в следующей статье базы знаний:  
>   
> [Приложение, которое запускается с пользователь без прав администратора не может прослушивать HTTP-трафика компьютера, на котором приложение выполняется в Windows Vista, Windows Server 2003 или Windows XP.](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Проблема: Google Chrome не поддерживается в случае выполнения

> Google Chrome не отображается в списке браузеров в разделе **запуска** на **Главная** вкладки.
> 
> **Инструкции по решению**  
> В некоторых версиях Google Chrome не регистрируют себя правильно с функцией программы по умолчанию в Windows. Обойти это ограничение, Google Chrome, выберите пункт *Настройка и управление Google Chrome* меню, щелкните *параметры*, а затем нажмите кнопку *марки Google Chrome обозревателем по умолчанию*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Проблема: Диалоговое окно «Foreign Key» не позволяет ввести первичный ключ

> **Foreign Key** диалоговое окно не позволяет ввести имя первичного ключа из таблицы первичного ключа.
> 
> **Инструкции по решению**  
> Это сделано намеренно. Необходимо ввести имя первичного ключа из таблицы первичного ключа.


#### <a name="issue-the-relationships-button-is-disabled"></a>Проблема: Кнопка «Связи» отключена

> **Связи** под кнопкой **таблицы** вкладке **баз данных** рабочей области отключен для базы данных SQL Server Compact.
> 
> **Инструкции по решению**  
> Отсутствует. SQL Server Compact не поддерживает связи между таблицами.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Проблема: Параметризованные SQL-запросы исключения

> В SQL Server Compact 4.0, если вы не укажете тип данных таких как `SqlDbType` или `DbType` для параметров в параметризованных запросах, создается исключение при выполнении запроса.
> 
> **Инструкции по решению**  
> Явно задать тип данных для параметров, таких как `SqlDbType` или `DbType`. Это крайне важно в случае с типами данных больших двоичных ОБЪЕКТОВ (`image` и `ntext`). Используйте код, аналогичный следующему:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Дополнительные сведения

Дополнительные сведения о WebMatrix бета-версии 3 см. в разделе следующих веб-сайтах:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Все права защищены. [Условия использования](https://msdn.microsoft.cos/cc300389.aspx).
