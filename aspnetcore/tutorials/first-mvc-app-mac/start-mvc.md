---
title: Начало работы с MVC ASP.NET Core и Visual Studio для Mac
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core и Visual Studio
ms.author: riande
ms.date: 8/23/2017
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: 059ac1f7fa94d97adc958be3c0b936cdfa7f6d3e
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225477"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Начало работы с MVC ASP.NET Core и Visual Studio для Mac

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом учебнике представлены основы сборки веб-приложений MVC ASP.NET Core с помощью [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/). 

[!INCLUDE [consider RP](../../includes/razor.md)]

Существует 3 версии этого учебника:

* macOS: [Создание приложения MVC ASP.NET Core с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* Linux, macOS и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a>Создание веб-приложения

В Visual Studio выберите **Файл > Новое решение**.

![Новое решение macOS](../first-web-api-mac/_static/sln.png)

Выберите **Приложение .NET Core > ASP.NET Core > Веб-приложение ASP.NET Core (MVC) > Далее**.

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

> [!div class="step-by-step"]
> [Вперед](adding-controller.md)  
