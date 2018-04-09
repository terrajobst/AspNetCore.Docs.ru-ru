---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Добавление контроллера (C#) | Документы Microsoft
author: Rick-Anderson
description: Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express Serivice с пакетом обновления 1, какие i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 963d3bbbadf408d7045c50bfd693069e4097d45d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller-c"></a>Добавление контроллера (C#)
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


Обозначает MVC *model-view-controller*. MVC представляет собой шаблон для разработки приложений, которые хорошо архитектурой и простые в обслуживании. Содержать MVC-приложений:

- Контроллеры: Классы, которые обрабатывают входящие запросы к приложению, получения данных модели, а затем укажите шаблонов представлений, возвращающих ответа клиенту.
- Модели: Классы, представляющие данные приложения, которые используют логику проверки для применения бизнес-правил для этих данных.
- Представления: Файлы шаблонов, которые приложение использует для динамического создания HTML-ответы.

Мы быть, охватывающие все эти компоненты этого учебника ряда и показано, как их использовать для построения приложения.

Давайте начнем с создания класса контроллера. В **обозревателе решений**, щелкните правой кнопкой мыши *контроллеров* папки, а затем выберите **добавить контроллер**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Имя вашего нового контроллера «HelloWorldController». Оставить как шаблон по умолчанию **пустой контроллер** и нажмите кнопку **добавить**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Обратите внимание на **обозревателе решений** что файл был создан именованный *HelloWorldController.cs*. Файл открыт в среде IDE.

![](adding-a-controller/_static/image5.png)

Внутри `public class HelloWorldController` блока, создайте два метода, которые выглядят как следующий код. Контроллер возвратит строку HTML-кода в качестве примера.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Контроллер называется `HelloWorldController` и первый способ называется `Index`. Давайте вызывать его из браузера. Запустите приложение (нажмите клавишу F5 или Ctrl + F5). В браузере добавления «HelloWorld» к пути в адресной строке. (Например, на рисунке ниже, он является `http://localhost:43246/HelloWorld.`) будет выглядеть страница в браузере, а следующий снимок экрана. В методе выше код непосредственно вернул строку. Вы сообщили система просто возвращается HTML-ФАЙЛ, как и!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC вызывает другой контроллер классы (и другие методы действия в них), в зависимости от входящего URL-адреса. Логика сопоставления по умолчанию, используемые ASP.NET MVC использует формат следующим образом, чтобы определить, какой код для вызова:

`/[Controller]/[ActionName]/[Parameters]`

Первая часть URL-адреса определяет класс контроллера для выполнения. Поэтому */HelloWorld* сопоставляется `HelloWorldController` класса. Во второй части URL-адрес определяет на класс для выполнения метода действия. Поэтому */HelloWorld/индекс* вызовет `Index` метод `HelloWorldController` класса для выполнения. Обратите внимание, что нам пришлось перейдите к */HelloWorld* и `Index` метод использовался по умолчанию. Это так, как метод с именем `Index` метод по умолчанию, который будет вызываться на контроллере, если тип не задан явно.

Перейдите по адресу `http://localhost:xxxx/HelloWorld/Welcome`. Выполняется метод `Welcome`, который возвращает строку "This is the Welcome action method..." (Это метод действия Welcome...). Сопоставление по умолчанию MVC `/[Controller]/[ActionName]/[Parameters]`. Для этого URL-адреса заданы контроллер `HelloWorld` и метод действия `Welcome`. Часть URL-адреса `[Parameters]` на данный момент еще не использовалась.

![](adding-a-controller/_static/image7.png)

Давайте немного изменить этот пример, чтобы некоторые сведения о параметрах можно передать URL-адреса контроллера (например, */HelloWorld/приветствия? имя = Скотт&amp;numtimes = 4*). Изменение вашей `Welcome` метод, чтобы включить два параметра, как показано ниже. Обратите внимание, что код использует функцию C# необязательный параметр указывает, что `numTimes` параметр по умолчанию 1, если не передаются значения для этого параметра.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Запустите приложение и откройте пример URL-адрес (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Можно попробовать разные значения для `name` и `numtimes` в URL-АДРЕСЕ. Система автоматически сопоставляет именованные параметры из строки запроса в адресной строке для параметров в методе.

![](adding-a-controller/_static/image8.png)

В обоих этих примерах контроллер затратил «VC» часть MVC — то есть, представления и контроллера работы. Контроллер вернул HTML напрямую. Обычно вы не хотите контроллеров, возвращение HTML напрямую, так как становится очень сложно в код. Вместо этого мы обычно используем отдельное представление файла шаблона для создания HTML-ответа. Давайте посмотрим, далее в том, как можно это сделать.

> [!div class="step-by-step"]
> [Назад](intro-to-aspnet-mvc-3.md)
> [Вперед](adding-a-view.md)
