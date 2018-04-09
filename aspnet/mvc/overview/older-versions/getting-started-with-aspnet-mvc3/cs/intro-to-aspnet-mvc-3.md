---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Введение в ASP.NET MVC 3 (C#) | Документы Microsoft
author: Rick-Anderson
description: Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-3-c"></a>Введение в ASP.NET MVC 3 (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Доступна обновленная версия этого учебника [здесь](../../../getting-started/introduction/getting-started.md) , с использованием ASP.NET MVC 5 и Visual Studio 2013. Он является более безопасны, выполните гораздо проще и показаны дополнительные возможности.
> 
> 
> Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:
> 
> - [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с исходного кода C# доступна по следующему адресу. [Загрузка версии C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если используется Visual Basic, перейдите в [Visual Basic-версии](../vb/intro-to-aspnet-mvc-3.md) этого учебника.


## <a name="what-youll-build"></a>Что мы создадим

Необходимо реализовать простое приложение список фильмов, которое поддерживает создание, изменение и список фильмов из базы данных. Ниже приведены два снимка экрана приложения, которое будет собрано. Он включает страницы, отображающей список фильмов из базы данных.

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

Приложение также позволяет добавлять, изменять и удалить фильмов, а также подробные сведения см. в разделе об отдельных проблемах. Все сценарии ввода данных включите проверку убедитесь в правильности данных, хранящихся в базе данных.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Ниже приведен изучаемого материала.

- Как создать новый проект ASP.NET MVC.
- Инструкции по созданию ASP.NET MVC контроллеры и представления.
- Как создать новую базу данных с помощью Entity Framework Code First принципов.
- Способы получения и отображения данных.
- Информацию об изменении данных и включить проверку данных.

## <a name="getting-started"></a>Начало работы

Запуск, запустив Visual Web Developer 2010 Express («Visual Web Developer» для краткости) и выберите **новый проект** из **запустить** страницы.

Visual Web Developer — это интегрированная среда разработки, или интегрированной среды разработки. Так же, как используется Microsoft Word для записи документов, воспользуемся интегрированной среды разработки для создания приложений. В Visual Web Developer имеется панель инструментов в верхней, показывающий различные параметры, доступные для вас. Кроме того, имеется меню, которое предоставляет еще один способ выполнения задач в Интегрированной среде разработки. (Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выбрать **файл** &gt; **новый проект**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Создание первого приложения

Можно создавать приложения, с помощью Visual Basic или Visual C# в качестве языка программирования. Выберите Visual C# в левой части экрана, а затем выберите **веб-приложение ASP.NET MVC 3**. Назовите проект «MvcMovie» и нажмите кнопку **ОК**. (Если используется Visual Basic, перейдите в [Visual Basic-версии](../vb/intro-to-aspnet-mvc-3.md) данного руководства.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

В **нового проекта ASP.NET MVC 3** выберите **веб-приложение**. Проверьте **разметку HTML5 используйте** и оставить **Razor** как обработчик представлений по умолчанию.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Нажмите кнопку **ОК**. Visual Web Developer используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому в данный момент есть рабочее приложение, не выполняя никаких действий! Это простое «Hello World!» проекта, а вот хорошая можно запустить приложение.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

В меню **Отладка** выберите пункт **Начать отладку**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Обратите внимание, что сочетание клавиш, чтобы начать отладку F5.

F5 приводит Visual Web Developer для запуска веб-сервера разработки и выполнения веб-приложения. Затем Visual Web Developer запускает браузер и открывает домашнюю страницу приложения. Обратите внимание, что в адресной строке браузера говорит `localhost` , а не то, как `example.com`. Причина этого заключается в `localhost` всегда указывает на локальном компьютере, работающая в этом случае просто создано приложение. При запуске веб-проекта Visual Web Developer случайный порт используется для веб-сервера. На приведенном ниже рисунке произвольный номер порта — 43246. При запуске приложения, возможно, появится другой номер порта.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Сразу после распаковки этот шаблон по умолчанию предоставляет основные входа и две страницы, чтобы посетить. Следующий шаг — изменить работу этого приложения и немного узнать о ASP.NET MVC в процессе. Закройте браузер и изменим некоторый код.

> [!div class="step-by-step"]
> [Вперед](adding-a-controller.md)
