---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: "Введение в ASP.NET MVC 3 (VB) | Документы Microsoft"
author: Rick-Anderson
description: "Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: f0717e8d635818322be8b3242efe756a20351340
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Введение в ASP.NET MVC 3 (Visual Basic)
====================
По [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:
> 
> - [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с VB.NET исходный код доступен по следующему адресу. [Загрузить версию VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете C#, переключитесь в [C# версии](../cs/intro-to-aspnet-mvc-3.md) этого учебника.


Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:

- [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)

Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Проект Visual Web Developer с VB исходный код доступен по следующему адресу. [Загрузите версию VB](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Если вы предпочитаете CSharp, переключитесь в [версии c#](../cs/intro-to-aspnet-mvc-3.md) этого учебника.

## <a name="what-youll-build"></a>Что мы создадим

Необходимо реализовать простое приложение список фильмов, которое поддерживает создание, изменение и список фильмов из базы данных. Ниже приведены два снимка экрана приложения, которое будет собрано. Он включает страницы, отображающей список фильмов из базы данных.

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Приложение также позволяет добавлять, изменять и удалить фильмов, а также подробные сведения см. в разделе об отдельных проблемах. Все сценарии ввода данных включите проверку убедитесь в правильности данных, хранящихся в базе данных.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Ниже приведен изучаемого материала.

- Создание нового проекта ASP.NET MVC
- Создание новой базы данных с использованием Entity Framework сначала код
- Создание ASP.NET MVC контроллеры и представления
- Способы получения и отображения данных
- Включение проверки данных и изменение данных

## <a name="getting-started"></a>Начало работы

Запуск, запустив Visual Web Developer 2010 Express («VWD» для краткости) и выберите **новый проект** из **запустить** страницы.

Visual Web Developer — это интегрированная среда разработки, или интегрированной среды разработки. Так же, как используется Microsoft Word для записи документов, воспользуемся интегрированной среды разработки для создания приложений. В Visual Web Developer имеется панель инструментов в верхней, показывающий различные параметры, доступные для вас. Кроме того, имеется меню, которое предоставляет еще один способ выполнения задач в Интегрированной среде разработки. (Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выбрать **файл** &gt; **новый проект**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Создание первого приложения

Можно создавать приложения, используя язык программирования Visual Basic или Visual C#. В этом учебнике выберите Visual Basic в левой части экрана, а затем **веб-приложение ASP.NET MVC 3**. Назовите проект «MvcMovie» и нажмите кнопку **ОК**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

В **нового проекта ASP.NET MVC 3** выберите **веб-приложение**. Оставить **Razor** как обработчик представлений по умолчанию.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Нажмите кнопку **ОК**. Visual Web Developer используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому в данный момент есть рабочее приложение, не выполняя никаких действий! Это простое «Hello World!» проекта, а вот хорошая можно запустить приложение.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

В меню **Отладка** выберите пункт **Начать отладку**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Обратите внимание, что сочетание клавиш, чтобы начать отладку F5.

F5 приводит Visual Web Developer для запуска веб-сервера разработки и выполнения веб-приложения. Затем VWD запускается браузер и открывает домашнюю страницу приложения. Обратите внимание, что в адресной строке браузера говорит `localhost` , а не то, как `example.com`. Причина этого заключается в `localhost` всегда указывает на локальном компьютере, работающая в этом случае просто создано приложение. При запуске веб-проекта VWD случайный порт используется для проекта. На приведенном ниже рисунке произвольный номер порта — 43246. Проект, скорее всего, будет использовать другой номер порта.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Без дополнительной настройки этот шаблон по умолчанию предоставляет основные входа и вы две страницы для перехода. Давайте изменить работу этого приложения и немного узнать о ASP.NET MVC в процессе. Закройте браузер и изменим некоторый код.

>[!div class="step-by-step"]
[Вперед](adding-a-controller.md)
