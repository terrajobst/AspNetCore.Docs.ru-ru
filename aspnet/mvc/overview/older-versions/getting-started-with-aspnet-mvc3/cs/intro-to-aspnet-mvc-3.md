---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Введение в ASP.NET MVC 3 (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, который является...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: b0ff8da1911b36e6e74e5c7057f27d891ad57f61
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832293"
---
<a name="intro-to-aspnet-mvc-3-c"></a>Введение в ASP.NET MVC 3 (C#)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Обновленную версию этого учебника доступен [здесь](../../../getting-started/introduction/getting-started.md) , использующий ASP.NET MVC 5 и Visual Studio 2013. Она более безопасные, гораздо проще следовать и показаны дополнительные возможности.
> 
> 
> Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio. Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:
> 
> - [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с исходным кодом C# — прилагаются в этом разделе. [Загрузить версию C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете Visual Basic, переключитесь в [версии Visual Basic](../vb/intro-to-aspnet-mvc-3.md) работы с этим руководством.


## <a name="what-youll-build"></a>Что вы создадите

Вы реализуете простое приложение списка фильмов, который поддерживает создание, изменение и список фильмов из базы данных. Ниже приведены два снимка экрана приложения, который вам предстоит создать. Он включает страницы, отображающей список фильмов из базы данных:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

Приложение также позволяет добавить, изменить и удалить фильмов, а также сведения о см. в разделе сведений об отдельных проблемах. Все сценарии ввода данных включают проверку, чтобы гарантировать правильность данных, хранящихся в базе данных.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Вот, вы узнаете, как:

- Как создать новый проект ASP.NET MVC.
- Как создать ASP.NET MVC контроллеры и представления.
- Как создать новую базу данных с помощью Entity Framework Code First парадигма.
- Способы получения и отображения данных.
- Об изменении данных и проверки данных.

## <a name="getting-started"></a>Начало работы

Загрузите с Visual Web Developer 2010 Express («Visual Web Developer» для краткости), а затем выберите **новый проект** из **запустить** страницы.

Visual Web Developer — это интегрированная среда разработки, или интегрированной среды разработки. Так же, как использовать Microsoft Word для записи документов, вы используете интегрированную среду разработки для создания приложений. В Visual Web Developer имеется панель инструментов в верхней, показывающий различные параметры, доступные для вас. Имеется также меню, которое предоставляет еще один способ выполнения задач в интегрированной среде разработки. (Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выберите **файл** &gt; **новый проект**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Создание первого приложения

Можно создавать приложения с помощью Visual Basic или Visual C# в качестве языка программирования. Выберите Visual C# в левой части, а затем выберите **веб-приложение ASP.NET MVC 3**. Назовите проект «MvcMovie» и нажмите кнопку **ОК**. (Если вы предпочитаете Visual Basic, переключитесь в [версии Visual Basic](../vb/intro-to-aspnet-mvc-3.md) данного руководства.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

В **Создание проекта ASP.NET MVC 3** выберите **веб-приложение**. Проверьте **HTML5 использования разметки** и оставить **Razor** как обработчик представлений по умолчанию.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Нажмите кнопку **ОК**. Visual Web Developer используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому у вас есть рабочее приложение прямо сейчас не выполняя никаких действий! Это связано с простых «Hello World!» проекта, а вот отлично подходит для запуска приложения.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

В меню **Отладка** выберите пункт **Начать отладку**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Обратите внимание, что сочетание клавиш, чтобы начать отладку F5.

F5 приводит к Visual Web Developer для запуска веб-сервера разработки и выполнения веб-приложения. Затем Visual Web Developer запускает браузер и откроется домашняя страница приложения. Обратите внимание, что говорит в адресной строке браузера `localhost` и не что-либо типа `example.com`. Это потому, что `localhost` всегда указывает на локальном компьютере, который выполняется, в этом случае приложение, вы только что выполнили. При запуске веб-проекта Visual Web Developer для веб-сервера используется случайный порт. В приведенном ниже рисунке случайный номер порта является 43246. При запуске приложения, вы, скорее всего, увидите другой номер порта.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Готовые этот шаблон по умолчанию позволяет две страницы, чтобы перейти на страницу основное имя для входа. Следующий шаг — изменить работу этого приложения и немного поговорим об ASP.NET MVC в процессе. Закройте обозреватель и изменим код.

> [!div class="step-by-step"]
> [Вперед](adding-a-controller.md)
