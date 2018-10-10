---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Включение проверки подлинности Windows в Katana | Документация Майкрософт
author: MikeWasson
description: 'В этой статье показано, как включить проверку подлинности Windows в Katana. Здесь рассматривается два сценария: использование IIS для узла Katana и с помощью HttpListener для резидентного размещения Kat...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8afa2c9dfbe03a9874513f7d083adf7608f4218f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910464"
---
<a name="enabling-windows-authentication-in-katana"></a>Включение проверки подлинности Windows в Katana
====================
по [Майк Уоссон](https://github.com/MikeWasson)

> В этой статье показано, как включить проверку подлинности Windows в Katana. Здесь рассматривается два сценария: использование IIS для узла Katana и с помощью HttpListener для резидентного размещения Katana в пользовательском процессе. Благодаря Barry Dorrans, Дэвид Matson и Крис Росс за рецензирование этой статьи.


Katana — это реализованный корпорацией Майкрософт [OWIN](http://owin.org/), Open Web Interface для .NET. Дополнительные общие сведения о OWIN и Katana [здесь](an-overview-of-project-katana.md). Архитектура OWIN имеет несколько уровней:

- Узел: Управляет процессом, в котором выполняется в конвейер OWIN.
- Сервер: Открывает к сетевому сокету и прослушивает запросы.
- По промежуточного слоя: Обрабатывает HTTP-запроса и ответа.

Katana в настоящее время предоставляет два сервера, каждый из которых поддерживает встроенную проверку подлинности Windows:

- **Microsoft.Owin.Host.SystemWeb**. Использует IIS с конвейером ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Использует [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Этот сервер в настоящее время является параметром по умолчанию, при резидентном размещении Katana.

> [!NOTE]
> Katana не поддерживает в настоящее время по промежуточного слоя OWIN для проверки подлинности Windows, так как эта функция уже доступна на серверах.

## <a name="windows-authentication-in-iis"></a>Проверка подлинности Windows в IIS

С помощью Microsoft.Owin.Host.SystemWeb, можно просто включить проверку подлинности Windows в службах IIS.

Начнем с создания нового приложения ASP.NET, с помощью шаблона проекта «Пустой веб-приложение ASP.NET».

![](enabling-windows-authentication-in-katana/_static/image1.png)

Добавьте пакеты NuGet. Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Теперь добавьте класс с именем `Startup` следующим кодом:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Это все, чтобы создать приложение «Hello world» для OWIN, выполняющегося на сервере IIS. Нажмите клавишу F5, чтобы начать отладку приложения. Вы увидите сообщение «Hello World!» в окне браузера.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Затем мы будем включите проверку подлинности Windows в IIS Express. Из **представление** меню, выберите **свойства**. Щелкните имя проекта в обозревателе решений, чтобы просмотреть свойства проекта.

В **свойства** окне **анонимную проверку подлинности** для **отключено** и задайте **проверки подлинности Windows** для  **Включить**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

При запуске приложения из Visual Studio, IIS Express потребуется учетные данные пользователя Windows. Это можно увидеть с помощью [Fiddler](http://fiddler2.com/home) или другой средство отладки HTTP. Ниже приведен пример ответа HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

В этом ответе заголовки WWW-Authenticate указывают, что сервер поддерживает [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) протокола, который использует Kerberos или NTLM.

Позже, при развертывании приложения на сервере, выполните [эти действия](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) для включения проверки подлинности Windows в службах IIS на этом сервере.

## <a name="windows-authentication-in-httplistener"></a>Проверка подлинности Windows в HttpListener

Если вы используете Microsoft.Owin.Host.HttpListener для резидентного размещения Katana, проверку подлинности Windows можно включить непосредственно на **HttpListener** экземпляра.

Во-первых создайте новое консольное приложение. Добавьте пакеты NuGet. Из **средства** меню, выберите **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Теперь добавьте класс с именем `Startup` следующим кодом:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Этот класс реализует тот же пример «Hello world», от и до, но он также задает проверку подлинности Windows схемы проверки подлинности.

Внутри `Main` функцию, запустите конвейер OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Можно отправить запрос в Fiddler, чтобы убедиться, что приложение использует проверку подлинности Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>См. также

[Обзор проекта Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Основные сведения о проверки подлинности форм OWIN в MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
