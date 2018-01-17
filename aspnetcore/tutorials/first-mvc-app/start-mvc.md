---
title: "Начало работы с MVC ASP.NET Core и Visual Studio"
author: rick-anderson
description: "Начало работы с MVC ASP.NET Core и Visual Studio"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 72852678a296107e6a2a146ed2324335a6e870ee
ms.sourcegitcommit: 77b8025c30ec2fd46d85ee2a2b497c44435d3009
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a>Начало работы с MVC ASP.NET Core и Visual Studio

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[!INCLUDE[consider RP](../../includes/razor.md)]

Существует 3 версии этого учебника:

* macOS: [Создание приложения ASP.NET Core MVC с помощью Visual Studio для Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Создание приложения ASP.NET Core MVC с помощью Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux и Windows: [Создание приложения MVC ASP.NET Core с помощью Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Установка Visual Studio и .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Установите Visual Studio Community 2017. Выберите загрузку сообщества. Если платформа Visual Studio 2017 уже установлена, пропустите этот шаг.

* [Установщик главной страницы Visual Studio 2017](https://www.visualstudio.com/)

Запустите программу установки и выберите следующие рабочие нагрузки.

* **ASP.NET и веб-разработка** в разделе **Интернет и облако**)
* **Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)

![**ASP.NET и веб-разработка** в разделе **Интернет и облако**)](start-mvc/_static/web_workload.png)

![**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Создание веб-приложения

В Visual Studio выберите меню **Файл > Создать > Проект**.

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

В диалоговом окне **Создание проекта** выполните следующие действия.

* В левой области выберите **.NET Core**.
* В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.
* Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).
* Нажмите кнопку **ОК**.

![Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.

* Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.-**.
* Выберите **Веб-приложение (модель-представление-контроллер)**.
* Нажмите кнопку **ОК**.

![Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.

* Выберите в раскрывающемся списке выбора версии **ASP.NET Core 1.1**.
* Выберите **Веб-приложение**.
* Сохраните значение по умолчанию **Без проверки подлинности**.
* Нажмите кнопку **ОК**.

![Новое веб-приложение ASP.NET Core](start-mvc/_static/p3.png)

---

В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC. Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров. Это простой начальный проект и хорошая стартовая точка.

Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или клавиши **CTRL-F5**, чтобы выбрать режим без отладки.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![Запуск приложения](start-mvc/_static/1.png)

* Visual Studio запускает [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение. Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт. На представленном выше снимке экрана используется порт номер 5000. URL-адрес в браузере отображает `localhost:5000`. При запуске приложения вы увидите другой номер порта.
* Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде. Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.
* Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.

![Меню отладки](start-mvc/_static/debug_menu.png)

* Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.

![IIS Express](start-mvc/_static/iis_express.png)

Шаблон по умолчанию включает рабочие ссылки **Домой, О программе** и **Контакты**. На представленном выше снимке экрана эти ссылки не отображаются. Если размер окна вашего браузера слишком мал, нажмите значок навигации, чтобы отобразить эти ссылки.

![Значок навигации в правом верхнем углу экрана](start-mvc/_static/2.png)

При запуске в режиме отладки нажмите клавиши **SHIFT+F5**, чтобы остановить отладку.

В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.

>[!div class="step-by-step"]
[Вперед](adding-controller.md)  
