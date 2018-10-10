---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Приступая к работе с OWIN и Katana | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 9920861da0e67d9304a944cacfb8ff8685267cd6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913181"
---
<a name="getting-started-with-owin-and-katana"></a>Приступая к работе с OWIN и Katana
====================
по [Майк Уоссон](https://github.com/MikeWasson)

[Откройте веб-интерфейс для .NET (OWIN)](http://owin.org/) абстракцию между веб-серверов .NET и веб-приложений. Отделив веб-сервера из приложения, OWIN упрощает создание по промежуточного слоя для разработки веб-приложений .NET. Кроме того, OWIN упрощает порт веб-приложений на другие узлы&#8212;к примеру, Резидентное размещение в службе Windows или другой процесс.

OWIN — это спецификация принадлежащих сообщества, не реализацию. Проект Katana — это набор компонентов OWIN с открытым кодом, разработанная корпорацией Майкрософт. Общие сведения о OWIN и Katana, см. в разделе [Обзор проекта Katana](an-overview-of-project-katana.md). В этой статье я перейти непосредственно в код, чтобы приступить к работе.

В этом руководстве используется [версии-кандидата Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566), но можно также использовать Visual Studio 2012. Некоторые действия различаются в Visual Studio 2012, который я примечание ниже.

## <a name="host-owin-in-iis"></a>Размещение OWIN в IIS

В этом разделе будут размещены OWIN в службах IIS. Этот параметр обеспечивает гибкость и возможность компоновки конвейер OWIN вместе с набор зрелой функций IIS. При использовании этого параметра приложение OWIN, выполняется в конвейере запросов ASP.NET.

Во-первых создайте новый проект веб-приложения ASP.NET. (В Visual Studio 2012, используйте тип проекта пустое веб-приложение ASP.NET).

![](getting-started-with-owin-and-katana/_static/image1.png)

В **новый проект ASP.NET** диалоговом окне выберите **пустой** шаблона.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Добавьте пакеты NuGet

Затем добавьте необходимые пакеты NuGet. Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Добавьте класс запуска

Добавьте класс запуска OWIN. В обозревателе решений щелкните правой кнопкой мыши проект и выберите **добавить**, а затем выберите **новый элемент**. В **Добавление нового элемента** диалоговом окне выберите **класс запуска Owin**. Дополнительные сведения о настройке класс startup см. в разделе [определение класса запуска OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Добавьте следующий код в метод `Startup1.Configuration`:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Этот код добавляет просто блок по промежуточного слоя в конвейер OWIN, реализованную как функцию, которая получает **Microsoft.Owin.IOwinContext** экземпляра. Когда сервер получает HTTP-запроса, в конвейер OWIN вызывает по промежуточного слоя. Задает тип содержимого для ответа по промежуточного слоя и записывает текст ответа.

> [!NOTE]
> Шаблон класса запуска OWIN доступен в Visual Studio 2013. Если вы используете Visual Studio 2012, просто добавьте пустой класс с именем `Startup1`и вставьте в него следующий код:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Запуск приложения

Нажмите клавишу F5, чтобы начать отладку. Visual Studio открывает окно браузера, чтобы `http://localhost:*port*/`. Страница должна выглядеть следующим образом:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>OWIN резидентной в консольном приложении

Его можно легко преобразовать это приложение из размещение в службах IIS для размещения на собственном сервере в пользовательском процессе. С помощью размещение в службах IIS, IIS действует как HTTP-сервер и как процесс, на котором размещена служба. С вариантом с размещением, приложение создает процесс и использует **HttpListener** класс как HTTP-сервера.

В Visual Studio создайте новое консольное приложение. В окне консоли диспетчера пакетов введите следующую команду:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Добавление `Startup1` класс из части 1 этого руководства в проект. Не нужно изменять данный класс.

Реализация приложения `Main` метод следующим образом.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

При запуске консольного приложения, сервер начинает прослушивать `http://localhost:9000`. Если перейти по этому адресу в веб-браузере, вы увидите, что на странице «Hello world».

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Добавление диагностики OWIN

Пакет Microsoft.Owin.Diagnostics содержит промежуточный слой, который перехватывает необработанные исключения и отображает HTML-страницу с сообщением об ошибке. Этот страничных функций как ошибку в странице ASP.NET, иногда называется "[желтый экраном смерти](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Как YSOD страницы ошибки Katana полезно использовать во время разработки, но рекомендуется его отключить в рабочем режиме.

Чтобы установить пакет диагностики в проекте, введите следующую команду в окне консоли диспетчера пакетов:

`install-package Microsoft.Owin.Diagnostics –Pre`

Измените код в ваш `Startup1.Configuration` метод следующим образом:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Используйте сочетание клавиш CTRL + F5 для запуска приложения без отладки, таким образом, чтобы Visual Studio не произойдет останов на исключение. Приложение работает так же, как и раньше, пока вы переходите `http://localhost/fail`, после чего приложение выдает исключение. Промежуточное по страницы ошибки будет перехватить исключение и отображать HTML-страницу с информацией об ошибке. Можно щелкнуть вкладки, чтобы просмотреть стек, строка запроса, файлы cookie, заголовка запроса и переменных среды OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Следующие шаги

- [Определение класса запуска OWIN](owin-startup-class-detection.md)
- [Использование OWIN для резидентного размещения веб-API ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Использование OWIN для резидентного размещения SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
