---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Включение проверки подлинности Windows в Katana | Документы Microsoft"
author: MikeWasson
description: "В этой статье показано, как включить проверку подлинности Windows в Katana. Рассматриваются два сценария: с помощью служб IIS для размещения Katana, а с помощью HttpListener резидентной Kat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: cc23a053fb1ba60ea84eca59e99f0e375fefc4cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="enabling-windows-authentication-in-katana"></a>Включение проверки подлинности Windows в Katana
====================
по [Mike Wasson](https://github.com/MikeWasson)

> В этой статье показано, как включить проверку подлинности Windows в Katana. Рассматриваются два сценария: с помощью служб IIS для размещения Katana, а с помощью HttpListener резидентной Katana в пользовательском процессе. Спасибо, что Барри Dorrans, Дэвид Matson и Крис Ross рецензирование статьи.


Katana является реализацией Майкрософт [OWIN](http://owin.org/), откройте веб-интерфейс для .NET. Можно читать введение OWIN и Katana [здесь](an-overview-of-project-katana.md). Архитектура OWIN состоит из нескольких уровней:

- Узел: Управляет процессом, в которой выполняется в конвейер OWIN.
- Сервер: Открывает к сетевому сокету и прослушивает запросы.
- По промежуточного слоя: Обрабатывает HTTP-запроса и ответа.

В настоящее время Katana предоставляет два сервера, которые поддерживают как встроенная проверка подлинности Windows.

- **Microsoft.Owin.Host.SystemWeb**. Использует службы IIS с помощью конвейера ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Использует [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx). Этот сервер в настоящее время является параметром по умолчанию, при размещении Katana.

> [!NOTE]
> Katana не предоставляет в настоящее время по промежуточного слоя OWIN для проверки подлинности Windows, так как эта функция уже доступна на серверах.


## <a name="windows-authentication-in-iis"></a>Проверка подлинности Windows в службах IIS

С помощью Microsoft.Owin.Host.SystemWeb, можно просто включите проверку подлинности Windows в службах IIS.

Давайте начнем с создания нового приложения ASP.NET с помощью шаблона проекта «Пустой веб-приложение ASP.NET».

![](enabling-windows-authentication-in-katana/_static/image1.png)

Добавьте пакеты NuGet. Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Теперь добавьте класс с именем `Startup` следующим кодом:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Это все, что нужно создать приложение «Hello world» для OWIN, на сервере IIS. Нажмите клавишу F5, чтобы начать отладку приложения. Должно появиться сообщение «Hello World!» в окне браузера.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Далее мы будем включите проверку подлинности Windows в IIS Express. Из **представление** последовательно выберите пункты **свойства**. Щелкните имя проекта в обозревателе решений для просмотра свойств проекта.

В **свойства** задайте **анонимную проверку подлинности** для **отключено** и задайте **проверки подлинности Windows** для  **Включить**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

При запуске приложения из Visual Studio, IIS Express потребуется учетные данные пользователя Windows. Это можно увидеть с помощью [Fiddler](http://fiddler2.com/home) или другой HTTP, средство отладки. Ниже приведен пример ответа HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Заголовки WWW-Authenticate в данном ответе указывают, что сервер поддерживает [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) протокола, который использует протокол Kerberos или NTLM.

Позже, при развертывании приложения на сервере, выполните [эти действия](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) Включение проверки подлинности Windows в службах IIS на этом сервере.

## <a name="windows-authentication-in-httplistener"></a>Проверка подлинности Windows в HttpListener

При использовании Microsoft.Owin.Host.HttpListener для самостоятельного размещения Katana проверку подлинности Windows можно включить непосредственно в **HttpListener** экземпляра.

Во-первых создайте новое консольное приложение. Добавьте пакеты NuGet. Из **средства** последовательно выберите пункты **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**. В окне консоли диспетчера пакетов введите следующую команду:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Теперь добавьте класс с именем `Startup` следующим кодом:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Этот класс реализует тот же пример «Hello world», от и до, но также задает проверку подлинности Windows, как схемы проверки подлинности.

Внутри `Main` функционировать, запуск конвейера OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Можно отправить запрос в Fiddler, чтобы убедиться, что приложение использует проверку подлинности Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Связанные разделы

[Общие сведения о проекте Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)

[Основные сведения об OWIN проверки подлинности форм в MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
