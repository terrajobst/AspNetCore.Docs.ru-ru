---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Заметки о выпуске для ASP.NET and Web Tools 2013.1 для Visual Studio 2012 | Документация Майкрософт
author: microsoft
description: В этом документе описывается версия ASP.NET и Web Tools 2013.1 для Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 85cd45c25e0f2ad3c8d6d6de73a1a493533e7f7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374264"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Заметки о выпуске ASP.NET и Web Tools 2013.1 для Visual Studio 2012
====================
по [Microsoft](https://github.com/microsoft)

> В этом документе описывается версия ASP.NET и Web Tools 2013.1 для Visual Studio 2012.


## <a name="contents"></a>Описание

- [Замечания по установке](#install)
- [Требования к программному обеспечению](#requirements)
- Новые возможности в ASP.NET and Web Tools 2013.1 для Visual Studio 2012

    - [Начальная загрузка](#bootstrap)
    - [Шаблоны](#templates)

        - [Шаблон ASP.NET MVC 5](#mvc5template)
        - [Шаблон веб-API 2 ASP.NET](#apitemplate)
        - [Шаблоны элементов](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Формирование шаблонов ASP.NET](#scaffold)
    - [Редактор Razor](#razor)
    - [NuGet 2.7](#nuget)
- Известные проблемы и критические изменения

    - [Формирование шаблонов ASP.NET](#issuescaffolding)

        - [MVC и API формирование шаблонов-HTTP 404 не найдено ошибок](#404issue)
        - [Visual Studio Express 2012 для Web перестает работать после добавления элемента формирования шаблонов](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Просмотрите файл cshtml с просмотр с помощью или F5 приводит к ошибке сервера](#browseissue)
        - [Перезапись URL-адреса и Tilde(~)](#rewriteissue)
    - [Шаблоны](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Замечания по установке

[Установка](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Требования к программному обеспечению

Необходимо иметь Visual Studio 2012 или Visual Studio Express 2012 для Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Новые возможности в ASP.NET and Web Tools 2013.1 для Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Начальная загрузка

Когда вы позволяет формировать контроллеры MVC 5 и представления, разметка для представления использует [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Шаблоны

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Шаблон ASP.NET MVC 5

Мы добавили новый шаблон MVC 5. Он ссылается на последнюю версию пакетов MVC 5 NuGet и формирование шаблонов можно использовать для добавления контроллеров и представлений.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Шаблон веб-API 2 ASP.NET

Мы добавили новый шаблон веб-API 2. Он ссылается на последнюю версию пакетов Web API 2 NuGet и формирование шаблонов можно использовать для добавления контроллеров и представлений.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Шаблоны элементов

Мы добавили новые шаблоны элементов для представления MVC 5, веб-страниц (Razor 3) и контроллеры веб-API 2. Установке связанные пакеты NuGet в проект при добавлении новых элементов.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Если вы формирования шаблонов контроллера MVC или веб-API, с помощью Entity Framework, мы используем Framework 6. Дополнительные сведения о платформе Entity Framework см. в разделе [журнал версий Entity Framework](https://msdn.com/data/jj574253).

Кроме того, можно также загрузить и установить инструменты для Entity Framework 6 для Visual Studio 2012. См. в разделе [получение платформы Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Формирование шаблонов ASP.NET

Формирование шаблонов ASP.NET — это платформа создания кода для веб-приложений ASP.NET. Он позволяет легко добавлять стандартный код в проект, который взаимодействует с моделью данных.

В предыдущих версиях Visual Studio формирование шаблонов был ограничен проекты ASP.NET MVC. Благодаря этому обновлению теперь можно использовать формирование шаблонов для любого проекта ASP.NET, включая веб-форм. Это обновление не поддерживает создание страниц для проекта веб-форм, но по-прежнему можно использовать формирование шаблонов веб-формы, Добавление зависимостей MVC в проект. Поддержка создания страниц для веб-формы будет добавлена в будущем обновлении.

При использовании формирования шаблонов, мы убедитесь, что все необходимые зависимости устанавливаются в проекте. Например если для создания проекта веб-форм ASP.NET, а затем использовать формирование шаблонов, чтобы добавить контроллер веб-API, необходимые пакеты NuGet и ссылки добавляются в проект автоматически.

Чтобы добавить формирование шаблонов MVC в проект веб-форм, добавьте **создать шаблонный элемент** и выберите **зависимостей MVC 5** в диалоговом окне. Существует два варианта для формирования шаблонов MVC; Минимальный и полной. Если выбрать минимальным, только пакеты NuGet и ссылки для ASP.NET MVC добавляются в проект. При выборе параметра «полная» добавляются минимальные зависимости, а также содержимого файлов, необходимых для проекта MVC.

Поддержка формирования шаблонов контроллеров async использует новые возможности async из Entity Framework 6.

Дополнительные сведения и учебники см. в разделе [Обзор формирование шаблонов ASP.NET](../2013/aspnet-scaffolding-overview.md). В этих руководствах формирования шаблонов с помощью Visual Studio 2013, но они также могут применяться к ASP.NET и Web Tools 2013.1 для Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Редактор Razor

В этом обновлении Visual Studio 2012 теперь поддерживает Razor 3 средства редактирования.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 включает широкий набор новых функций, которые описаны подробно [заметки о выпуске NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Эта версия NuGet устраняет необходимость для пользователей, чтобы явно разрешить NuGet восстановить отсутствующие пакеты. При установке NuGet 2.7, пользователи неявно разрешать автоматически восстановить отсутствующие пакеты. Пользователи могут явно отключить восстановления пакета с помощью параметров NuGet в Visual Studio. Это изменение упрощает, как работает восстановление пакета.

## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Формирование шаблонов ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC и API формирование шаблонов-HTTP 404 не найдено ошибок

Если возникает ошибка при добавлении элемента формирования шаблонов в проект, это возможно, ваш проект остается в несогласованном состоянии. Некоторые изменения, внесенные быть формирования шаблонов будет выполнен откат, но другие изменения, например установленные пакеты NuGet, не будет выполнен откат. Если откат изменения конфигурации маршрутизации, пользователи будут получать ошибку HTTP 404 при переход к сформированные элементы.

Чтобы устранить эту ошибку для MVC, добавьте новый шаблонный элемент и выберите зависимостей MVC 5 (минимальное или полный). Этот процесс будет добавьте все необходимые изменения в проект.

Чтобы устранить эту ошибку для веб-API:

1. Добавьте следующий класс WebApiConfig в проект.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Настройка в приложении WebApiConfig.Register\_запустите метод в файле Global.asax, как показано ниже:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 для Web перестает работать после добавления элемента формирования шаблонов

Если Visual Studio Express 2012 для Web перестает работать после добавления элемента формирования шаблонов с платформой Entity Framework (например, контроллер Web API 2 с действиями, использующий Entity Framework), то, возможно, что Visual Studio Express не удалось загрузить сборки образов в машинном коде в зависимости от System.Web.Extensions.

Чтобы устранить эту проблему, настройте Visual Studio Express для работы с образа MSIL System.Web.Extensions:

1. Откройте командную строку в режиме администратора.
2. Перейдите к %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE или % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (для 64-разрядная версия Windows).
3. Откройте VWDExpress.exe.config в текстовом редакторе.
4. Добавьте следующую строку в разделе &lt;конфигурации&gt;/&lt;среды выполнения&gt; элемент:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Перезапустите Visual Studio Express 2012 для Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Просмотр withBrowse файл cshtml WithorF5causes ошибка сервера

При создании проекта MVC 5 в Visual Studio 2012 (или откройте в проекте Visual Studio 2012 MVC 5, который был создан в Visual Studio 2013) и попытайтесь просмотреть файл cshtml с помощью просмотр с помощью или F5, вы получите ошибку с сообщением - **ошибка сервера в Приложение «/»**. Сервер пытается перейти к `http://localhost:XXXX/Views/../XXXX.cshtml`

Чтобы устранить эту проблему, измените **действие при запуске** параметр проекта, чтобы **определенную страницу**. Необходимо указать значение для страницы.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

После внесения этого изменения, выбрав F5 переходит в корень приложения (`http://localhost:XXXX`). Такое поведение не так же, как для проектов MVC 5 в Visual Studio 2013, где **текущей страницы** параметр запускает откройте страницу.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Перезапись URL-адреса и Tilde(~)

После обновления до ASP.NET Razor 3 или ASP.NET MVC 5, нотации tilde(~) может не работать должным образом при использовании операции перезаписи URL-адрес. Перезапись URL-адреса влияет на нотация tilde(~) в HTML-элементов, например &lt;A /&gt;, &lt;СКРИПТ /&gt;, &lt;ССЫЛКУ /&gt;, и поэтому больше не сопоставляет тильда в корневой каталог.

Например, если вы переписать запросы **asp.net/content** для **asp.net**, атрибут href в &lt;A href = «~/content/» /&gt; разрешается **/content/ содержимое /** вместо **/**. Чтобы подавить это изменение, можно задать **IIS\_WasUrlRewritten** контекста, значение false, в каждой веб-странице или в **приложения\_BeginRequest** в файле Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Шаблоны

При создании ASP.NET MVC проектов с помощью Visual Studio 2012 на Windows 8.1 или Windows Server 2012 R2, Visual Studio отображает сообщение об ошибке о том, «Настройка Web [url] для ASP.NET 4.5 не удалось.»

![Ошибка конфигурации](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Эта ошибка возникает, так как Visual Studio 2012 не поддерживает компонент ASP.NET 4.5, установленные в этих версиях Windows. Чтобы включить ASP.NET 4.5, выполните действия, описанные в [или отключение компонентов Windows включить](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Включение и отключение компонентов Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Кроме того вы можете включить ASP.NET 4.5 через командную строку.

1. Откройте командную строку в режиме администратора.
2. Выполните следующую команду, чтобы включить ASP.NET версии 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
