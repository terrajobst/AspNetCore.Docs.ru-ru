---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Добавление контроллера (VB) | Документация Майкрософт
author: Rick-Anderson
description: Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, который является...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b38f3c4051af426e471568b8a25a471986f07209
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576237"
---
<a name="adding-a-controller-vb"></a>Добавление контроллера (VB)
====================
по [Рик Андерсон]((https://twitter.com/RickAndMSFT))

> Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio. Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже. Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:
> 
> - [Необходимые компоненты для Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Обновление средств ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)
> 
> Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Проект Visual Web Developer с VB.NET исходный код доступен на следующей странице в этом разделе. [Загрузить версию VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Если вы предпочитаете C#, переключитесь в [C# версии](../cs/adding-a-controller.md) работы с этим руководством.


MVC означает *model-view-controller*. MVC — это шаблон для разработки приложений, таким образом, что каждая часть имеет отдельный ответственности:

- Модель: Данные для вашего приложения.
- Представления: Файлы шаблонов, что приложение будет использовать для динамического создания HTML-ответов.
- Контроллеры: Классы, которые обрабатывают входящие запросы на URL-адрес к приложению, извлекают данные модели, а затем укажите шаблоны представлений, которые отображают ответ клиенту.

Мы будет обсуждаться всеми этими понятиями в этом руководстве описано и показано, как их использовать для создания приложения.

Создать новый контроллер, щелкните правой кнопкой мыши *контроллеров* папку в **обозревателе решений** и выбрав **Добавление контроллера**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Назовите новый контроллер &quot;HelloWorldController&quot; и нажмите кнопку **добавить**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Обратите внимание, что в **обозревателе решений** справа, автоматически создан новый файл с именем *HelloWorldController.cs* и что файл открыт в интегрированной среде разработки.

Внутри новой `public class HelloWorldController` block, создайте два новых метода, которые выглядят аналогично следующему коду. Мы вернем строка HTML, непосредственно из контроллера в качестве примера.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Контроллер называется `HelloWorldController` новый метод с именем `Index`. Запустите приложение (нажмите клавишу F5 или Ctrl + F5). После запуска браузера добавления &quot;HelloWorld&quot; к пути в адресной строке. (На компьютере, он имеет `http://localhost:43246/HelloWorld`) в браузере будет выглядеть как на снимке экрана ниже. В методе выше код вернул строку непосредственно. Мы же говорили, система должна возвращать только некоторые HTML, как и!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC вызывает классы другой контроллер (и различных методов действий в них), в зависимости от входящего URL-адреса. Логика сопоставления по умолчанию, используемый платформой ASP.NET MVC использует формат следующим образом, чтобы контролировать, какой код вызывается:

`/[Controller]/[ActionName]/[Parameters]`

Первая часть URL-адреса определяет класс контроллера для выполнения. Поэтому */HelloWorld* сопоставляется `HelloWorldController` класса. Вторая часть URL-адреса определяет метод действия к классу для выполнения. Поэтому */HelloWorld/индекс* вызовет `Index` метод `HelloWorldController` класс для выполнения. Обратите внимание, что нам пришлось посетите */HelloWorld* выше и `Index` метод использовался по умолчанию. Это обусловлено тем, метод с именем `Index` является методом по умолчанию, который будет вызываться на контроллере, если тип не задан явным образом.

Перейдите по адресу `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Метод запускается и возвращает строку &quot;метод приветствия действие... &quot;. MVC по умолчанию осуществляется сопоставление `/[Controller]/[ActionName]/[Parameters]`. Этот URL-адрес, контроллер является `HelloWorld` и `Welcome` — это метод. Мы не использовали `[Parameters]` частью URL-адрес еще.

![](adding-a-controller/_static/image6.png)

Давайте немного изменить приведенный пример, таким образом, чтобы можно было передать сведения о параметрах в URL-адреса к контроллеру (например, */HelloWorld/Welcome? имя = Скотт&amp;; numtimes = = 4*). Изменение вашего `Welcome` метод включив два параметра, как показано ниже. Обратите внимание, что мы использовали функцию VB необязательный параметр указывает, что `numTimes` параметр должен по умолчанию 1, если не передаются значения для этого параметра.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Запустите приложение и перейдите к `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Вы можете попробовать различные значения для `name` и `numtimes`. Система автоматически сопоставляет именованные параметры из строки запроса в адресной строке параметров метода.

![](adding-a-controller/_static/image7.png)

В обоих этих примерах контроллер предоставляет VC часть MVC — это работа представления и контроллера. Контроллер возвращает HTML напрямую. Обычно мы не хотим контроллеров, возвращает HTML напрямую, поскольку в этом случае заметно усложняется написание кода. Вместо этого мы обычно используем файл шаблона отдельное представление для создания HTML-ответа. Давайте рассмотрим, как это можно сделать это.

> [!div class="step-by-step"]
> [Назад](intro-to-aspnet-mvc-3.md)
> [Вперед](adding-a-view.md)
