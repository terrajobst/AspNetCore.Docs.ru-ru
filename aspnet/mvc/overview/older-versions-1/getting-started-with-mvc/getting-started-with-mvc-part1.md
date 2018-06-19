---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Введение в ASP.NET MVC | Документы Microsoft
author: shanselman
description: Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, чтение и запись из базы данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2018
ms.locfileid: "30868494"
---
<a name="intro-to-aspnet-mvc"></a>Введение в ASP.NET MVC
====================
по [Скотт Хансельман](https://github.com/shanselman)

> > [!NOTE]
> > Обновленная версия, если этот учебник доступен [здесь](../../getting-started/introduction/getting-started.md) с помощью [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Новое учебное использует ASP.NET MVC 5 обеспечивает множество преимуществ по сравнению этого учебника.
> 
> 
> Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Вы создадите простое веб-приложение выполняет чтение и запись из базы данных. Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.


Создадим нашей первой веб-приложение ASP.NET MVC с помощью [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Мы выполним немного приложения список фильмов, сообщите нам создать и список фильмов.

## <a name="what-youll-build"></a>Что мы создадим

Вот приложение, которое мы создадим два снимка экрана. Имеется простой таблицы фильмов с различными столбцами.

[![Список фильмов - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

И необходимо создать форму, можно добавить в список фильмов.

[![Создание фильма - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Этот учебник поможет узнать основы создания веб-приложения ASP.NET MVC с помощью Visual Studio. Вы узнаете следующее.

- Создание нового проекта ASP.NET MVC
- Как создать новую базу данных с SQL Server
- Создание ASP.NET MVC контроллеры и представления
- Способы получения и отображения данных
- Включение проверки данных и изменение данных
- Как обновить схему базы данных

## <a name="get-started"></a>Приступая к работе

Запустить Visual Web Developer 2010 Express (назовем ее «VWD» теперь) и выберите новый проект с экрана "Пуск".

Visual Web Developer — это интегрированная среда разработки, или интегрированной среды разработки. Так же, как используется Microsoft Word для записи документов, воспользуемся интегрированной среды разработки для создания приложений. Панель инструментов в верхней, показывающий различные параметры, доступные для вас, а также меню, вы также использовали выберите файл | Новый проект.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Создание первого приложения

Можно создавать приложения, Visual Basic или Visual C#. Теперь выберите Visual C# в левой части экрана, затем выберите «Веб-приложение ASP.NET MVC 2». Присвойте проекту имя «Фильмов» и нажмите кнопку ОК.

[![Создание проекта](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

С правой стороны находится в обозревателе решений все файлы и папки в приложении. Большие окна в середине — где изменения кода и большую часть времени. Visual Studio используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому в данный момент есть рабочее приложение, не выполняя никаких действий! Это простое «Hello World! проект и является хорошей отправной точкой для нашего приложения.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Нажмите кнопку «Воспроизведение» на панели инструментов.

![Начало отладки](getting-started-with-mvc-part1/_static/image11.png)

Это зеленая стрелка вправо, который будет выполняться компиляция программы и запустить приложение в веб-браузере.

*Примечание: Можно вместо этого нажмите клавишу F5 на клавиатуре или выберите отладки -&gt;запуск отладки из меню «Отладка».*

Это приведет к Visual Web Developer для запуска веб сервера разработки и выполнения наших веб-приложения (не существует конфигурации или вручную шаги, необходимые для этого). Затем будет запустите обозреватель и настроить его для просмотра домашней страницы приложения. Обратите внимание, ниже говорится в адресной строке браузера, «localhost», а не примерно example.com. Это, так как localhost всегда указывает на локальном компьютере -, в этом случае работающего приложения, который мы только что создали.

[![Домашняя страница](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Без дополнительной настройки этот шаблон по умолчанию предоставляет основные входа и вы две страницы для перехода. Давайте изменить работу этого приложения и немного узнать о ASP.NET MVC в процессе. Закройте браузер, а также позволяет изменить часть кода.

> [!div class="step-by-step"]
> [Вперед](getting-started-with-mvc-part2.md)
