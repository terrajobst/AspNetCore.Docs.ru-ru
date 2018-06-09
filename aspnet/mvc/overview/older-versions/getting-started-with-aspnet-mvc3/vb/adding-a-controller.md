---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Добавление контроллера (VB) | Документы Microsoft
author: Rick-Anderson
description: Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 9a433083c31c7929f7599e52800c887f301d7727
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "30870613"
---
<a name="adding-a-controller-vb"></a>Добавление контроллера (Visual Basic)
====================
по [Рик Андерсон](https://github.com/Rick-Anderson)

> Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio. Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:
> 
> - [Необходимые компоненты Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с VB.NET исходный код доступен по следующему адресу. [Загрузить версию VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете C#, переключитесь в [C# версии](../cs/adding-a-controller.md) этого учебника.


Обозначает MVC *model-view-controller*. MVC представляет собой шаблон для разработки приложений, таким образом, что каждая часть имеет отдельный ответственности:

- Модель: Данные для приложения.
- Просмотр: Файлы шаблонов ваше приложение будет использовать для динамического создания HTML-ответы.
- Контроллеры: Классы, которые обрабатывают входящие запросы на URL-адрес для приложения, получения данных модели, а затем укажите шаблонов представлений, которые отображают ответа клиенту.

Мы быть, охватывающие все эти компоненты в этом учебнике и показано, как их использовать для построения приложения.

Создание нового контроллера, щелкнув правой кнопкой мыши *контроллеров* папки в **обозревателе решений** и выбрав **добавить контроллер**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Имя вашего нового контроллера &quot;HelloWorldController&quot; и нажмите кнопку **добавить**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Обратите внимание на **обозревателе решений** справа что автоматически создан новый файл с именем *HelloWorldController.cs* и что файл открыт в среде IDE.

Внутри новой `public class HelloWorldController` блокировки, создайте два новых метода, которые выглядят как следующий код. Мы вернемся строка HTML, непосредственно из контроллера в качестве примера.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Контроллер называется `HelloWorldController` новый метод с именем `Index`. Запустите приложение (нажмите клавишу F5 или Ctrl + F5). После запуска браузера append &quot;HelloWorld&quot; пути в адресной строке. (На компьютере, он имеет `http://localhost:43246/HelloWorld`) ваш браузер будет выглядеть на снимке экрана ниже. В методе выше код непосредственно вернул строку. Как отмечалось система просто возвращается HTML-ФАЙЛ, как и!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC вызывает другой контроллер классы (и другие методы действия в них), в зависимости от входящего URL-адреса. Контролировать, какой код вызывается логика сопоставления по умолчанию, используемые ASP.NET MVC используется формат следующим образом:

`/[Controller]/[ActionName]/[Parameters]`

Первая часть URL-адреса определяет класс контроллера для выполнения. Поэтому */HelloWorld* сопоставляется `HelloWorldController` класса. Во второй части URL-адрес определяет на класс для выполнения метода действия. Поэтому */HelloWorld/индекс* вызовет `Index` метод `HelloWorldController` класса для выполнения. Обратите внимание, что нам пришлось посетите */HelloWorld* выше и `Index` метод использовался по умолчанию. Это так, как метод с именем `Index` метод по умолчанию, который будет вызываться на контроллере, если тип не задан явно.

Перейдите по адресу `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Метод выполняется и возвращает строку, которая &quot;это метод приветствия действие... &quot;. Сопоставление по умолчанию MVC `/[Controller]/[ActionName]/[Parameters]`. Для этого URL-адреса — контроллер `HelloWorld` и `Welcome` метод. Мы не использовали `[Parameters]` частью URL-адрес еще.

![](adding-a-controller/_static/image6.png)

Давайте немного изменить этот пример, чтобы можно было передать некоторые сведения о параметрах в URL-адреса к контроллеру (например, */HelloWorld/начальной настройки? name = Скотт&amp;numtimes = 4*). Изменение вашей `Welcome` метод, чтобы включить два параметра, как показано ниже. Обратите внимание, что мы использовали функцию VB необязательный параметр указывает, что `numTimes` параметр по умолчанию 1, если не передаются значения для этого параметра.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Запустите приложение и перейдите к `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Можно попробовать разные значения для `name` и `numtimes`. Система автоматически сопоставляет именованные параметры из строки запроса в адресной строке для параметров в методе.

![](adding-a-controller/_static/image7.png)

В обоих этих примерах контроллер выполнения VC часть MVC — это работа представление и контроллер. Контроллер вернул HTML напрямую. Обычно мы не хотим контроллеров, возвращение HTML напрямую, так как становится очень сложно в код. Вместо этого мы обычно используем отдельное представление файла шаблона для создания HTML-ответа. Давайте взглянем на как можно это сделать.

> [!div class="step-by-step"]
> [Назад](intro-to-aspnet-mvc-3.md)
> [Вперед](adding-a-view.md)
