---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Введение в ASP.NET MVC 4 | Документация Майкрософт
author: Rick-Anderson
description: Обновленную версию, если это руководство доступно здесь с помощью Visual Studio 2013. Новое учебное использует ASP.NET MVC 5, который обеспечивает множество улучшений t...
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: a718eea17458fa810897b35371085848f8dca6fe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810025"
---
<a name="intro-to-aspnet-mvc-4"></a>Введение в ASP.NET MVC 4
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Обновленную версию, если это руководство доступно [здесь](../../getting-started/introduction/getting-started.md) с помощью [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Новое учебное использует ASP.NET MVC 5, который содержит множество улучшений на этом руководстве.
> 
> Этот учебник поможет основы создания MVC 4 веб-приложения ASP.NET с помощью Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) или Visual Web Developer 2010 Express пакетом обновления 1. Visual Studio 2012 мы рекомендуем устанавливать ничего для работы с этим руководством не требуется. Если вы используете Visual Studio 2010 необходимо установить компонентов, перечисленных ниже. Все из них можно установить по ссылкам ниже:
> 
> - [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Установщик удостоверения рабочего Процесса для ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, установите [установщик удостоверения рабочего Процесса для ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) и: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Проект Visual Web Developer с исходным кодом C# — прилагаются в этом разделе. [Загрузить версию C#](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> В этом руководстве вы запустите приложение в Visual Studio. Вы можете также сделать приложение доступным через Интернет, развернув ее у поставщика услуг размещения. Корпорация Майкрософт предлагает бесплатные услуг хостинга до 10 веб-сайтов в [бесплатную пробную учетную запись Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Сведения о развертывании веб-проекта Visual Studio для веб-сайта Windows Azure, см. в разделе [Создание и развертывание веб-узла ASP.NET и базы данных SQL с помощью Visual Studio](https://docs.microsoft.com/dotnet/azure/). Этот учебник также показано, как использовать Entity Framework Code First Migrations для развертывания базы данных SQL Server в базу данных SQL Windows Azure (прежнее название — SQL Azure).
> 
> Это руководство было написано с Рик Андерсон ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Что вы создадите

> [!NOTE]
> Обновленную версию, если это руководство доступно [здесь](../../getting-started/introduction/getting-started.md) с помощью [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Новое учебное использует ASP.NET MVC 5, который содержит множество улучшений на этом руководстве.


Вы реализуете простое приложение списка фильмов, который поддерживает создание, изменение, поиска и список фильмов из базы данных. Ниже приведены два снимка экрана приложения, который вам предстоит создать. Он включает страницы, отображающей список фильмов из базы данных:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Приложение также позволяет добавить, изменить и удалить фильмов, а также сведения о см. в разделе сведений об отдельных проблемах. Все сценарии ввода данных включают проверку, чтобы гарантировать правильность данных, хранящихся в базе данных.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Начало работы

Запустить Visual Studio Express 2012 или Visual Web Developer 2010 Express. Большая часть снимков экрана в этой серии используется Visual Studio Express 2012, но вы можете изучать этот учебник с Visual Studio 2010 или с пакетом обновления 1, Visual Studio 2012, Visual Studio Express 2012 или Visual Web Developer 2010 Express. Выберите **новый проект** из **запустить** страницы.

Visual Studio является интегрированной среды разработки или интегрированной среды разработки. Так же, как использовать Microsoft Word для записи документов, вы используете интегрированную среду разработки для создания приложений. В Visual Studio есть панели инструментов в верхней, показывающий различные параметры, доступные для вас. Имеется также меню, которое предоставляет еще один способ выполнения задач в интегрированной среде разработки. (Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выберите **файл** &gt; **новый проект**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Создание первого приложения

Можно создавать приложения с помощью Visual Basic или Visual C# в качестве языка программирования. Выберите Visual C# в левой части, а затем выберите **веб-приложение ASP.NET MVC 4**. Присвойте проекту имя &quot;MvcMovie&quot; и нажмите кнопку **ОК**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

В **создания проекта ASP.NET MVC 4** выберите **веб-приложение**. Оставьте **Razor** как обработчик представлений по умолчанию.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Нажмите кнопку **ОК**. Visual Studio используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому у вас есть рабочее приложение прямо сейчас не выполняя никаких действий! Это простой &quot;Hello World!&quot; проекта, а вот отлично подходит для запуска приложения.

![](intro-to-aspnet-mvc-4/_static/image6.png)

В меню **Отладка** выберите пункт **Начать отладку**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Обратите внимание, что сочетание клавиш, чтобы начать отладку F5.

F5 приводит к Visual Studio для запуска IIS Express и выполнения веб-приложения. Затем Visual Studio запустит браузер и откроется домашняя страница приложения. Обратите внимание, что говорит в адресной строке браузера `localhost` и не что-либо типа `example.com`. Это потому, что `localhost` всегда указывает на локальном компьютере, который выполняется, в этом случае приложение, вы только что выполнили. При запуске веб-проекта Visual Studio для веб-сервера используется случайный порт. На рисунке ниже номер порта — 41788. При запуске приложения, вы, скорее всего, увидите другой номер порта.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Готовые этот шаблон по умолчанию позволяет дома, контактов и о страницы. Он также обеспечивает поддержку для регистрации и входа в, а также ссылки для Facebook и Twitter. Следующий шаг — изменить работу этого приложения и немного поговорим об ASP.NET MVC. Закройте обозреватель и изменим код.

> [!div class="step-by-step"]
> [Вперед](adding-a-controller.md)
