---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "Часть 8: Последней страницы, обработки исключений и выводу | Документы Microsoft"
author: JoeStagner
description: "Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 8 добавляет страницы контактов, о странице и исключение..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: fce1a20f9d1093b6c60542d8a786ddf54fdc922c
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Часть 8: Последней страницы, обработки исключений и выводу
====================
по [(Joe Stagner)](https://github.com/JoeStagner)

> Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET. Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.
> 
> Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 8 добавляет страницы контактов, о странице и обработка исключений. Это заключение ряда.


## <a id="_Toc260221680"></a>Обратитесь к странице (отправка по электронной почте из ASP.NET)

Создание новой страницы с именем ContactUs.aspx

С помощью конструктора, создайте следующую форму, выполнив особое Примечание для включения ToolkitScriptManager и элемент управления редактора, AjaxdControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Дважды щелкните кнопку «Отправить», чтобы создать обработчик событий click в файле кода программной части и реализуйте метод, чтобы отправить контактные данные в виде сообщения электронной почты.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Этот код требует, что ваш файл web.config содержит запись в раздел конфигурации, который указывает SMTP-сервера для отправки почты.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>О странице

Создайте страницу с именем AboutUs.aspx и добавьте любое содержимое, вам нравится.

## <a id="_Toc260221682"></a>Глобального обработчика исключений

Наконец в приложении возникает исключения при этом непредвиденных обстоятельств, холодного причина необработанных исключений в наш веб-приложения.

Мы никогда не будут необрабатываемое исключение отображаться для посетителей веб-сайта.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Помимо того, что взаимодействие с пользователем ужасных необработанные исключения также может быть проблема безопасности.

Чтобы решить эту проблему мы реализуем глобального обработчика исключений.

Чтобы сделать это, откройте файл Global.asax и запишите следующий обработчик событий, предварительно созданный.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Добавьте код для реализации приложения\_обработчик ошибок, как показано ниже.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Затем добавьте страницу с именем Error.aspx в решение и добавьте этот фрагмент кода разметки.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Теперь на странице\_загрузить извлечения обработчика событий, сообщения об ошибках из объекта запроса.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Заключение

Мы узнали, что веб-форм ASP.NET упрощает создание сложных веб-сайта с помощью доступа к базе данных, членство в AJAX, и т. д. довольно быстро.

Надеемся, у этого учебника вы получили средства, необходимые для начала создания приложений для собственных веб-форм ASP.NET!

>[!div class="step-by-step"]
[Назад](tailspin-spyworks-part-7.md)
