---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Приступая к работе с OWIN и Katana | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2018
---
<a name="getting-started-with-owin-and-katana"></a>Приступая к работе с OWIN и Katana
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Открыть веб-интерфейс .NET (OWIN)](http://owin.org/) определяет абстракцию между .NET веб-серверов и веб-приложений. Посредством разделения веб-сервера из приложения, OWIN упрощает создание по промежуточного слоя для разработки веб-приложений .NET. Кроме того, OWIN для упрощения порт веб-приложений на другие узлы&#8212;, Резидентное размещение в службе Windows или другой процесс.

OWIN — это спецификация принадлежащие сообщества, не реализацию. Katana проект — это набор компонентов OWIN открытым исходным кодом, разработанная корпорацией Майкрософт. Общие сведения о OWIN и Katana, в разделе [Обзор проекта Katana](an-overview-of-project-katana.md). В этой статье я будет выполнен переход вправо в код, чтобы приступить к работе.

В этом учебнике используется [версии-кандидата Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566), но можно также использовать Visual Studio 2012. В Visual Studio 2012, который я примечание ниже отличаются некоторых шагов.

## <a name="host-owin-in-iis"></a>Узел OWIN в службах IIS

В этом разделе мы будем размещения OWIN в службах IIS. Данный параметр предоставляет гибкость и возможность компоновки конвейер OWIN, а также набор зрелой функциональных возможностей IIS. При использовании этого параметра приложение OWIN, выполняется в конвейер запросов ASP.NET.

Во-первых создайте новый проект веб-приложения ASP.NET. (В Visual Studio 2012, используйте тип проекта пустое веб-приложение ASP.NET.)

![](getting-started-with-owin-and-katana/_static/image1.png)

В **новый проект ASP.NET** диалогового окна выберите **пустой** шаблона.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Добавление пакетов NuGet

Добавьте необходимые пакеты NuGet. Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Добавьте класс запуска

Добавьте класс запуска OWIN. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**. В **Добавление нового элемента** диалогового окна выберите **запуска Owin класса**. Дополнительные сведения о настройке класс запуска см. в разделе [определение класса запуска OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Добавьте следующий код в метод `Startup1.Configuration`:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Этот код добавляет простой часть по промежуточного слоя в конвейере OWIN, реализованную как функцию, которая получает **Microsoft.Owin.IOwinContext** экземпляра. Когда сервер получает запрос HTTP, конвейер OWIN вызывается по промежуточного слоя. Задает тип содержимого для ответа по промежуточного слоя и записывает текст ответа.

> [!NOTE]
> Шаблон класса запуска OWIN доступен в Visual Studio 2013. Если вы используете Visual Studio 2012, просто добавьте новый пустой класс с именем `Startup1`и вставьте следующий код:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Запуск приложения

Нажмите клавишу F5, чтобы начать отладку. Visual Studio откроет окно браузера для `http://localhost:*port*/`. Страница должна выглядеть следующим образом:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>OWIN резидентной в консольном приложении

Можно легко преобразовать это приложение из размещения в службах IIS для размещения на собственном сервере в пользовательском процессе. С размещения в службах IIS, IIS работает и как HTTP-сервера и другой процесс, на котором размещена служба. С резидентной, приложение создает процесс и использует **HttpListener** класс как HTTP-сервера.

В Visual Studio создайте новое консольное приложение. В окне консоли диспетчера пакетов введите следующую команду:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Добавить `Startup1` класс из части 1 этого руководства в проект. Нет необходимости изменять данный класс.

Реализовать приложение `Main` метод следующим образом.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

При запуске консольного приложения, сервер начинает прослушивать `http://localhost:9000`. Если перейти на этот адрес в веб-браузер, вы увидите страницу «Hello world».

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Добавить OWIN Diagnostics

Пакета Microsoft.Owin.Diagnostics содержит по промежуточного слоя, который перехватывает необработанные исключения и отображает HTML-страницу с описанием ошибки. Функции этой страницы, так же, как страница ошибки ASP.NET, который иногда называют «[желтый экрана смерти](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)» (YSOD). Как YSOD страницы ошибки Katana используется во время разработки, но рекомендуется отключить его в рабочий режим.

Чтобы установить пакет диагностики в вашем проекте, введите следующую команду в окне консоли диспетчера пакетов:

`install-package Microsoft.Owin.Diagnostics –Pre`

Измените код в вашей `Startup1.Configuration` метод следующим образом:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Используйте сочетание клавиш CTRL + F5 для запуска приложения без отладки, так, что Visual Studio не произойдет останов на исключение. Приложение работает так же, как и раньше, пока не будет выполнен переход к `http://localhost/fail`, после чего приложение вызывает исключение. Промежуточное по страницы ошибки будет перехватить исключение и отображать на HTML-странице с информацией об ошибке. Можно щелкнуть вкладки, чтобы просмотреть стек, строки запроса, файлы cookie, заголовок запроса и переменных среды OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Следующие шаги

- [Определение класса запуска OWIN](owin-startup-class-detection.md)
- [Используйте OWIN для самостоятельного размещения веб-API ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Используйте OWIN для самостоятельного размещения SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
