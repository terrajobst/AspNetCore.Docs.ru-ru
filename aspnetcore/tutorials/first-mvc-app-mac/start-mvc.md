---
title: "Начало работы с MVC ASP.NET Core и Visual Studio для Mac"
author: rick-anderson
description: "Начало работы с MVC ASP.NET Core и Visual Studio"
keywords: "ASP.NET Core, MVC, Visual Studio для Mac, Entity Framework"
ms.author: riande
manager: wpickett
ms.date: 8/23/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 21f115eec924d5e4b21ad78398c8cbd99e02a0a8
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Начало работы с MVC ASP.NET Core и Visual Studio для Mac

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом учебнике представлены основы сборки веб-приложений MVC ASP.NET Core с помощью [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/). 

[!INCLUDE[consider RP](../../includes/razor.md)]

Существует 3 версии этого учебника:

* macOS: [Создание приложения MVC ASP.NET Core с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* Linux, macOS и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Предварительные требования

Для этого учебника требуется [пакет SDK для .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии.

Установите следующие компоненты:

- [пакет SDK для .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии;
- [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-web-app"></a>Создание веб-приложения

В Visual Studio выберите **Файл > Новое решение**.

![Новое решение macOS](../first-web-api-mac/_static/sln.png)

Выберите **Приложение .NET Core > ASP.NET Core > Веб-приложение > Далее**.

![Диалоговое окно "Новый проект" в macOS](start-mvc/1.png)

Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.

![Диалоговое окно "Новый проект" в macOS](start-mvc/2.png)

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio выберите **Выполнить > Запуск без отладки**, чтобы запустить приложение. Visual Studio запустит [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.

![Новый проект в браузере](start-mvc/b1.png)

* В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт. При запуске приложения вы увидите другой номер порта.
* В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.

Шаблон по умолчанию включает ссылки **Главная, О программе** и **Контакты**. На представленном выше снимке экрана эти ссылки не отображаются. Если размер окна вашего браузера слишком мал, щелкните значок навигации, чтобы отобразить эти ссылки.

![Новый проект в браузере](start-mvc/b2.png)

В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.

>[!div class="step-by-step"]
[Вперед](adding-controller.md)  
