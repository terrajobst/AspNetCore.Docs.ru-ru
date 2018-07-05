---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Введение в ASP.NET MVC | Документация Майкрософт
author: shanselman
description: Это руководство для начинающих, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, которое считывает и записывает в базу данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 408256d116a7e73e01c34b0a11881e14c5b8401d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385736"
---
<a name="intro-to-aspnet-mvc"></a>Введение в ASP.NET MVC
====================
по [(Scott hanselman)](https://github.com/shanselman)

> > [!NOTE]
> > Обновленную версию, если это руководство доступно [здесь](../../getting-started/introduction/getting-started.md) с помощью [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Новое учебное использует ASP.NET MVC 5, который содержит множество улучшений на этом руководстве.
> 
> 
> Это руководство для начинающих, в котором представлены основные сведения по ASP.NET MVC. Вы создадите простое веб-приложение, которое считывает и записывает в базу данных. Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.


Создадим наш первый веб-приложение ASP.NET MVC с помощью [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Сделаем небольшое приложение списка фильмов, сообщите нам создать и список фильмов.

## <a name="what-youll-build"></a>Что вы создадите

Ниже приведены два снимка экрана приложения, который вам предстоит создать. Вы получите простую таблицу фильмов с помощью различных столбцов.

[![Список фильмов - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

И вы получите Создание формы, поэтому мы можем добавить в список фильмов.

[![Создайте фильм - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Навыки, которые вы узнаете

Этом учебнике описываются основы создания веб-приложение ASP.NET MVC с помощью Visual Studio. Вы узнаете следующее.

- Как создать новый проект ASP.NET MVC
- Как создать новую базу данных с помощью SQL Server
- Как создать ASP.NET MVC контроллеры и представления
- Получение и отображение данных
- Включение проверки данных и изменения данных
- Как обновить схему базы данных

## <a name="get-started"></a>Приступая к работе

Запустить Visual Web Developer 2010 Express (назовем ее «VWD» теперь) и выберите новый проект из на начальном экране.

Visual Web Developer — это интегрированная среда разработки, или интегрированной среды разработки. Так же, как использовать Microsoft Word для записи документов, вы используете интегрированную среду разработки для создания приложений. Панель инструментов в верхней, отображаются различные параметры, доступные для вас, а также меню, вы также использовали выберите файл | Новый проект.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Создание первого приложения

Можно создавать приложения с помощью Visual Basic или Visual C#. Теперь выберите Visual C# с левой стороны экрана выберите «Веб-приложение ASP.NET MVC 2». Присвойте проекту имя «Movies» и нажмите кнопку ОК.

[![Новый проект](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

В области справа находится обозреватель решений, отображаются все файлы и папки в приложении. Большие окна в середине — изменения кода и большую часть времени. Visual Studio используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому у вас есть рабочее приложение прямо сейчас не выполняя никаких действий! Это связано с простых «Hello World! проект и является хорошей отправной точкой для нашего приложения.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Нажмите кнопку «Воспроизвести» на панели инструментов.

![Начало отладки](getting-started-with-mvc-part1/_static/image11.png)

Это зеленая стрелка, указывающая вправо, будет выполняться компиляция программы и запустить приложение в веб-браузере.

*Примечание: Можно вместо этого нажмите клавишу F5 на клавиатуре или выберите "Отладка" -&gt;запуск отладки из меню «Отладка».*

Это приведет к Visual Web Developer для запуска веб сервера разработки и выполнения нашего веб-приложения (не существует конфигурации или ручного действия, необходимые для этого). Затем он запустит браузер и настроить его для просмотра домашней страницы приложения. Обратите внимание, что ниже говорит что адресной строке браузера «localhost», а не что-то вида example.com. Том, что localhost всегда указывает на локальном компьютере — в этом случае работающего приложения, которые мы только что создали.

[![Домашняя страница](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

По умолчанию этот шаблон по умолчанию дает вам две страницы посетить и страницу основное имя для входа. Давайте изменить работу этого приложения и немного поговорим об ASP.NET MVC в процессе. Закройте обозреватель и позволяет изменить часть кода.

> [!div class="step-by-step"]
> [Вперед](getting-started-with-mvc-part2.md)
