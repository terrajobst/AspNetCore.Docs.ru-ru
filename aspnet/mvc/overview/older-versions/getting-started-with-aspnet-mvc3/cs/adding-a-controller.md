---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Добавление контроллера (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express Serivice пакетом обновления 1, какие i...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 0c13248960384e8d773f9c5ba3d927b6a8502938
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805863"
---
<a name="adding-a-controller-c"></a>Добавление контроллера (C#)
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


MVC означает *model-view-controller*. MVC — это шаблон для разработки приложений, которые хорошо продуманной архитектурой и простые в обслуживании. Приложения на основе MVC содержат:

- Контроллеры: Классы, которые обрабатывают входящие запросы к приложению, извлекают данные модели, а затем укажите шаблоны представлений, которые возвращают ответ клиенту.
- Модели: Классы, представляющие данные приложения и используют логику проверки для реализации бизнес-правил к этим данным.
- Представления: Файлы шаблонов, которые приложение использует для динамического создания HTML-ответов.

Мы будет обсуждаться в этой серии руководств, все эти концепции и показать, как их использовать для создания приложения.

Начнем с создания класса контроллера. В **обозревателе решений**, щелкните правой кнопкой мыши *контроллеров* папку, а затем выберите **Добавление контроллера**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Назовите новый контроллер «HelloWorldController». Оставьте как шаблон по умолчанию **пустой контроллер** и нажмите кнопку **добавить**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Обратите внимание, что в **обозревателе решений** что новый файл был создан именованный *HelloWorldController.cs*. Файл открыт в интегрированной среде разработки.

![](adding-a-controller/_static/image5.png)

Внутри `public class HelloWorldController` block, создайте два метода, которые выглядят аналогично следующему коду. Контроллер будет возвращена строка HTML, в качестве примера.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Контроллер называется `HelloWorldController` и первый способ называется `Index`. Давайте вызвать его из браузера. Запустите приложение (нажмите клавишу F5 или Ctrl + F5). В браузере добавьте «HelloWorld» к пути в адресной строке. (Например, на рисунке ниже, его `http://localhost:43246/HelloWorld.`) страница в браузере будет выглядеть как на следующем снимке экрана. В методе выше код вернул строку непосредственно. Вы сказали, что система должна возвращать только некоторые HTML, как и!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC вызывает классы другой контроллер (и различных методов действий в них), в зависимости от входящего URL-адреса. Логика сопоставления по умолчанию, используемый платформой ASP.NET MVC использует формат следующим образом, чтобы определить, какой код для вызова:

`/[Controller]/[ActionName]/[Parameters]`

Первая часть URL-адреса определяет класс контроллера для выполнения. Поэтому */HelloWorld* сопоставляется `HelloWorldController` класса. Вторая часть URL-адреса определяет метод действия к классу для выполнения. Поэтому */HelloWorld/индекс* вызовет `Index` метод `HelloWorldController` класс для выполнения. Обратите внимание, что нам пришлось перейдите к */HelloWorld* и `Index` метод использовался по умолчанию. Это обусловлено тем, метод с именем `Index` является методом по умолчанию, который будет вызываться на контроллере, если тип не задан явным образом.

Перейдите по адресу `http://localhost:xxxx/HelloWorld/Welcome`. Выполняется метод `Welcome`, который возвращает строку "This is the Welcome action method..." (Это метод действия Welcome...). MVC по умолчанию осуществляется сопоставление `/[Controller]/[ActionName]/[Parameters]`. Для этого URL-адреса заданы контроллер `HelloWorld` и метод действия `Welcome`. Часть URL-адреса `[Parameters]` на данный момент еще не использовалась.

![](adding-a-controller/_static/image7.png)

Давайте немного изменить приведенный пример, чтобы передать сведения о параметрах из URL-адрес в контроллер (например, */HelloWorld/Welcome? имя = Скотт&amp;; numtimes = = 4*). Изменение вашего `Welcome` метод включив два параметра, как показано ниже. Обратите внимание, что код использует функцию необязательного параметра C#, чтобы указать, что `numTimes` параметр должен по умолчанию 1, если не передаются значения для этого параметра.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Запустите приложение и перейдите к пример URL-адреса (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Вы можете попробовать различные значения для `name` и `numtimes` в URL-адрес. Система автоматически сопоставляет именованные параметры из строки запроса в адресной строке параметров метода.

![](adding-a-controller/_static/image8.png)

В обоих этих примерах контроллер предоставляет «VC» часть MVC — то есть работу представления и контроллера. Контроллер возвращает HTML напрямую. Обычно это не требуется Возвращает HTML напрямую, поскольку в этом случае заметно усложняется написание кода. Вместо этого мы обычно используем файл шаблона отдельное представление для создания HTML-ответа. Давайте взглянем далее в том, как это можно сделать.

> [!div class="step-by-step"]
> [Назад](intro-to-aspnet-mvc-3.md)
> [Вперед](adding-a-view.md)
