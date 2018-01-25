---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: "Заметки о выпуске для ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012 | Документы Microsoft"
author: microsoft
description: "Этот документ описывает выпуска ASP.NET и Web Tools 2013.1 для Visual Studio 2012."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Заметки о выпуске для ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012
====================
по [Microsoft](https://github.com/microsoft)

> Этот документ описывает выпуска ASP.NET и Web Tools 2013.1 для Visual Studio 2012.


## <a name="contents"></a>Описание

- [Замечания по установке](#install)
- [Требования к программному обеспечению](#requirements)
- Новые возможности в ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Шаблоны](#templates)

        - [Шаблон ASP.NET MVC 5](#mvc5template)
        - [Шаблон ASP.NET Web API 2](#apitemplate)
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

        - [Просмотр файл cshtml просмотр с помощью или F5 приводит к возникновению ошибки сервера](#browseissue)
        - [Перезапись URL-адреса и Tilde(~)](#rewriteissue)
    - [Шаблоны](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Замечания по установке

[Установка](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET и веб-средств 2013.1 для Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Требования к программному обеспечению

Необходимо иметь Visual Studio 2012 или Visual Studio Express 2012 для Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Новые возможности в ASP.NET и веб-инструменты 2013.1 для Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>начальной загрузки

При формировании на основе скаффолдинга контроллеров MVC 5 и представлений, использует разметка для представления [начальной загрузки](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Шаблоны

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Шаблон ASP.NET MVC 5

Мы добавили новый шаблон MVC 5. Он ссылается на последнюю пакеты MVC 5 NuGet и формирование шаблонов можно использовать для добавления контроллеров и представлений.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Шаблон ASP.NET Web API 2

Мы добавили новый шаблон веб-API 2. Он ссылается на последнюю пакеты Web API 2 NuGet и формирование шаблонов можно использовать для добавления контроллеров и представлений.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Шаблоны элементов

Мы добавили новые шаблоны элементов для представления MVC 5, веб-страницы (Razor 3) и веб-API 2 контроллеров. Они установлены связанные пакеты NuGet в проект при добавлении новых элементов.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

При формировании на основе скаффолдинга контроллер MVC или веб-API, использующий Entity Framework, мы используем Framework 6. Дополнительные сведения об Entity Framework см. в разделе [истории версий Entity Framework](https://msdn.com/data/jj574253).

Можно также загрузить и установить средства платформы Entity Framework 6 для Visual Studio 2012. В разделе [получение платформы Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Формирование шаблонов ASP.NET

Формирование шаблонов ASP.NET — это платформа создания кода для веб-приложений ASP.NET. Он позволяет легко добавить стандартный код в проект, который взаимодействует с моделью данных.

В предыдущих версиях Visual Studio формирование шаблонов был ограничен проектов ASP.NET MVC. Благодаря этому обновлению теперь можно использовать формирование шаблонов для любого проекта ASP.NET, включая веб-форм. Это обновление не поддерживает создание страниц для проекта веб-форм, но по-прежнему можно использовать формирование шаблонов с веб-формами путем добавления зависимостей MVC в проект. Поддержка создания страниц для веб-формы будет добавлена в будущем обновлении.

При использовании формирования шаблонов мы убедитесь, что все требуемые зависимости устанавливаются в проекте. Например если для создания проекта веб-форм ASP.NET и затем использовать формирование шаблонов для добавления контроллера Web API, необходимые пакеты NuGet и ссылки добавляются в проект автоматически.

Чтобы добавить формирование шаблонов MVC в проект веб-форм, добавьте **новый элемент формирования шаблонов** и выберите **зависимостей MVC 5** в диалоговом окне. Существует два варианта для формирования шаблонов MVC; Минимальными и полное. При выборе минимальной только пакеты NuGet и ссылки для ASP.NET MVC добавляются в проект. При выборе параметра «полная» добавляются минимальные зависимости, а также необходимые файлы содержимого проекта MVC.

Поддержка асинхронных контроллеров формирование шаблонов использует новые функции асинхронного с Entity Framework 6.

Дополнительные сведения и учебники см. в разделе [Обзор формирование шаблонов ASP.NET](../2013/aspnet-scaffolding-overview.md). В следующих учебниках описано формирование шаблонов в Visual Studio 2013, но они также применимы к ASP.NET и 2013.1 Web Tools для Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Редактор Razor

Это обновление Visual Studio 2012 теперь поддерживает Razor 3 средства редактирования.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 включает широкий набор новых функций, которые описаны в статье [заметки о выпуске 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Эта версия NuGet устраняет необходимость для пользователей, чтобы явно разрешить NuGet восстановления отсутствующих пакетов. При установке NuGet 2.7, пользователи неявно даете согласие на автоматическое восстановление отсутствующих пакетов. Пользователей можно явным образом отказаться восстановление пакета NuGet параметры в Visual Studio. Это изменение упрощает работу восстановления пакета.

## <a name="known-issues-and-breaking-changes"></a>Известные проблемы и критические изменения

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Формирование шаблонов ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC и API формирование шаблонов-HTTP 404 не найдено ошибок

Если возникнет сообщение об ошибке при добавлении элемента формирования шаблонов в проект, возможно, проект может остаться в несогласованном состоянии. Некоторые изменения, внесенные быть формирование шаблонов будет выполнен откат, но другие изменения, например установленные пакеты NuGet, не будет выполнен откат. Если откат изменения конфигурации маршрутизации, пользователи получат ошибку HTTP 404 при навигации для формирования шаблонов элементов.

Чтобы устранить эту ошибку для MVC, добавить новый элемент формирования шаблонов и выберите зависимостей MVC 5 (минимум или полный). Этот процесс будет добавить все необходимые изменения в проект.

Чтобы устранить эту ошибку для веб-API:

1. Добавьте следующий класс WebApiConfig в проект.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Настройка WebApiConfig.Register в приложении\_Start-метод в файле Global.asax следующим образом:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 для Web перестает работать после добавления элемента формирования шаблонов

Если Visual Studio Express 2012 для Web перестает работать после добавления элемента формирования шаблонов с Entity Framework (например, Web API 2 контроллер с действиями, использующий Entity Framework), это может означать, что Visual Studio Express не удается загрузить сборки образа в машинном коде в зависимости от System.Web.Extensions.

Чтобы устранить эту проблему, настройте Visual Studio Express для работы с образа MSIL System.Web.Extensions:

1. Откройте командную строку с правами администратора.
2. Перейдите к 11.0\Common7\IDE %ProgramFiles%\Microsoft Visual Studio или 11.0\Common7\IDE % ProgramFiles(x86) %\Microsoft Visual Studio (для 64-разрядной Windows).
3. Откройте VWDExpress.exe.config в текстовом редакторе.
4. Добавьте следующую строку в списке &lt;конфигурации&gt;/&lt;среды выполнения&gt; элемента:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Перезапустите Visual Studio Express 2012 для Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Просмотр withBrowse файл cshtml WithorF5causes ошибка сервера

При создании проекта MVC 5 в Visual Studio 2012 (или откройте файл в проект Visual Studio 2012 MVC 5, созданный в Visual Studio 2013) и попытаться просмотреть файл cshtml с помощью просмотр с помощью либо нажмите клавишу F5, вы получите сообщение об ошибке с состояния — **ошибка сервера в Приложение «/»**. Пытается перейти к серверу`http://localhost:XXXX/Views/../XXXX.cshtml`

Чтобы устранить эту проблему, измените **действие при запуске** в проекте для **определенную страницу**. Необходимо указать значение для страницы.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

После этого изменения выбора F5 переходит в корневую папку приложения (`http://localhost:XXXX`). Такое поведение не так же, как для проектов MVC 5 в Visual Studio 2013, где **текущей страницы** параметр запускает откройте страницу.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Перезапись URL-адреса и Tilde(~)

После обновления до ASP.NET Razor 3 или MVC ASP.NET 5, нотации tilde(~) может перестать работать правильно при использовании операции перезаписи URL-адрес. Перезапись URL-адреса влияет нотация tilde(~) в HTML-элементов, например &lt;A /&gt;, &lt;СЦЕНАРИЯ /&gt;, &lt;ССЫЛКУ и&gt;, и поэтому больше не сопоставляет тильда в корневой каталог.

Например, если переписывать запросы для **asp.net/content** для **asp.net**, атрибут href в &lt;A href = «~/content/» /&gt; разрешается в **/content/ содержимое и** вместо  **/** . Чтобы отменить это изменение, можно задать **IIS\_WasUrlRewritten** контекста, значение false в каждой веб-страницы или в **приложения\_BeginRequest** в файле Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Шаблоны

При создании ASP.NET MVC проектов с помощью Visual Studio 2012 на Windows 8.1 или Windows Server 2012 R2, Visual Studio отображает сообщение об ошибке, указывающее, «Настройка Web [url] для ASP.NET 4.5 не удалось.»

![Ошибка конфигурации](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Эта ошибка возникает, поскольку Visual Studio 2012 не поддерживает компонент ASP.NET 4.5, установленные в этих версиях Windows. Чтобы включить ASP.NET 4.5, выполните действия, описанные в [Включение или отключение компонентов](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Включение и отключение компонентов Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Кроме того можно включить ASP.NET 4.5 через командную строку.

1. Откройте командную строку с правами администратора.
2. Выполните следующую команду, чтобы включить ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
