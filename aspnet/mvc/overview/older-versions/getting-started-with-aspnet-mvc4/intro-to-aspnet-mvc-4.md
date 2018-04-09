---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Введение в ASP.NET MVC 4 | Документы Microsoft
author: Rick-Anderson
description: Обновленная версия, если этот учебник доступен с помощью Visual Studio 2013. Новое в этом учебнике используется ASP.NET MVC 5 обеспечивает множество улучшений t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 519bac22ba2607931c5f3123b9b567859a2b3d1c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-4"></a>Введение в ASP.NET MVC 4
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Обновленная версия, если этот учебник доступен [здесь](../../getting-started/introduction/getting-started.md) с помощью [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Новое учебное использует ASP.NET MVC 5 обеспечивает множество преимуществ по сравнению этого учебника.
> 
> Этот учебник поможет узнать основы создания MVC 4 веб-приложения ASP.NET с помощью Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) или Visual Web Developer 2010 Express пакетом обновления 1. Visual Studio 2012 рекомендуется установить для завершения работы с учебником ничего не придется. Если вы используете Visual Studio 2010 необходимо установить компоненты ниже. Все из них можно установить, перейдите по следующим ссылкам:
> 
> - [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Установщик удостоверения рабочего Процесса для ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, установите [установщик удостоверения рабочего Процесса для ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) и: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Проект Visual Web Developer с исходного кода C# доступна по следующему адресу. [Загрузка версии C#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> В этом учебнике вы запустите приложение в Visual Studio. Вы можете также сделать приложение доступным через Интернет, развертывая ее поставщика услуг размещения. Корпорация Майкрософт предлагает бесплатные веб-размещения для до 10 веб-сайтов в [освободить пробную учетную запись Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Сведения о развертывании веб-проекта Visual Studio для веб-сайта Windows Azure см. в разделе [создания и развертывания веб-сайт ASP.NET и базы данных SQL с помощью Visual Studio](https://docs.microsoft.com/dotnet/azure/). Этот учебник также показано, как использовать Entity Framework Code First Migrations для развертывания базы данных SQL Server к базе данных SQL Windows Azure (прежнее название — SQL Azure).
> 
> Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Что мы создадим

> [!NOTE]
> Обновленная версия, если этот учебник доступен [здесь](../../getting-started/introduction/getting-started.md) с помощью [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Новое учебное использует ASP.NET MVC 5 обеспечивает множество преимуществ по сравнению этого учебника.


Необходимо реализовать простое приложение список фильмов, которое поддерживает создание, изменение, поиск и список фильмов из базы данных. Ниже приведены два снимка экрана приложения, которое будет собрано. Он включает страницы, отображающей список фильмов из базы данных.

![](intro-to-aspnet-mvc-4/_static/image1.png)

Приложение также позволяет добавлять, изменять и удалить фильмов, а также подробные сведения см. в разделе об отдельных проблемах. Все сценарии ввода данных включите проверку убедитесь в правильности данных, хранящихся в базе данных.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Начало работы

Запустить Visual Studio Express 2012 или Visual Web Developer 2010 Express. Большая часть снимков экрана в этой серии воспользуйтесь Visual Studio Express 2012, но можно выполнения заданий данного учебника с Visual Studio 2010 или с пакетом обновления 1, Visual Studio 2012, Visual Studio Express 2012 или Visual Web Developer 2010 Express. Выберите **новый проект** из **запустить** страницы.

Visual Studio — это интегрированная среда разработки, или интегрированной среды разработки. Так же, как используется Microsoft Word для записи документов, воспользуемся интегрированной среды разработки для создания приложений. В Visual Studio имеется панель инструментов в верхней, показывающий различные параметры, доступные для вас. Кроме того, имеется меню, которое предоставляет еще один способ выполнения задач в Интегрированной среде разработки. (Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выбрать **файл** &gt; **новый проект**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Создание первого приложения

Можно создавать приложения, с помощью Visual Basic или Visual C# в качестве языка программирования. Выберите Visual C# в левой части экрана, а затем выберите **веб-приложение ASP.NET MVC 4**. Присвойте проекту имя &quot;MvcMovie&quot; и нажмите кнопку **ОК**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

В **нового проекта ASP.NET MVC 4** выберите **веб-приложение**. Оставить **Razor** как обработчик представлений по умолчанию.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Нажмите кнопку **ОК**. Visual Studio используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому в данный момент есть рабочее приложение, не выполняя никаких действий! Это простой &quot;Hello World!&quot; проекта, а вот хорошая можно запустить приложение.

![](intro-to-aspnet-mvc-4/_static/image6.png)

В меню **Отладка** выберите пункт **Начать отладку**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Обратите внимание, что сочетание клавиш, чтобы начать отладку F5.

F5 в результате Visual Studio для запуска IIS Express и выполнения веб-приложения. Затем Visual Studio запускает браузер и открывает домашнюю страницу приложения. Обратите внимание, что в адресной строке браузера говорит `localhost` , а не то, как `example.com`. Причина этого заключается в `localhost` всегда указывает на локальном компьютере, работающая в этом случае просто создано приложение. При запуске веб-проекта Visual Studio для веб-сервера используется случайный порт. На рисунке ниже номер порта — 41788. При запуске приложения, возможно, появится другой номер порта.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Сразу после распаковки этот шаблон по умолчанию дает дома, контактов и о страницы. Он также обеспечивает поддержку для регистрации и входа и ссылки на Facebook и Twitter. Следующий шаг — изменить работу этого приложения и немного узнать о ASP.NET MVC. Закройте браузер и изменим некоторый код.

> [!div class="step-by-step"]
> [Вперед](adding-a-controller.md)
