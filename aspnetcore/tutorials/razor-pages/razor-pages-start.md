---
title: Начало работы с Razor Pages в ASP.NET Core
author: rick-anderson
description: Основные сведения о создании веб-приложении Razor Pages в ASP.NET Core. Razor Pages рекомендуется для рабочих веб-нагрузок в ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278048"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Начало работы с Razor Pages в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

::: moniker range="= aspnetcore-2.0"

Мы рекомендуем руководствоваться версией этого учебника для ASP.NET Core 2.1. Она **гораздо** проще и рассматривает больше возможностей. Выберите **ASP.NET Core 2.1** в селекторе версий.

![Селектор версий в оглавлении](razor-pages-start/_static/v21.png)

::: moniker-end

В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core. Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.

Существуют три версии этого руководства:

* Windows: это руководство.
* MacOS: [Начало работы с Razor Pages в Visual Studio для Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS, Linux и Windows: [Начало работы с Razor Pages в ASP.NET Core с Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

* В меню **Файл** Visual Studio откройте **Создать** > **Проект**.
* Создайте новое веб-приложение ASP.NET Core. Назовите проект **RazorPagesMovie**. Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.
 ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2.1.png)
* Выберите в раскрывающемся списке **ASP.NET Core 2.1**, а затем **Веб-приложение**.

 ![Новое веб-приложение ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

Шаблон Visual Studio создает начальный проект:

![обозреватель решений](razor-pages-start/_static/se2.1.png)

Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика. Нажмите **Принять**, чтобы согласиться на отслеживание. Это приложение не отслеживает персональные данные. Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).

![Домашняя или индексная страница](razor-pages-start/_static/homeGDPR.png)

На следующем рисунке показано приложение после принятия отслеживания:

![Домашняя или индексная страница](razor-pages-start/_static/home2.1.png)

* Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение. В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Localhost обслуживает только веб-запросы с локального компьютера. Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт. На представленном выше снимке экрана используется номер порта 5000. При запуске приложения вы увидите другой номер порта.
* Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде. Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

* В меню **Файл** Visual Studio откройте **Создать** > **Проект**.
* Создайте новое веб-приложение ASP.NET Core. Назовите проект **RazorPagesMovie**. Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.
  ![Новое веб-приложение ASP.NET Core](../../razor-pages/index/_static/np.png)
* Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

Шаблон Visual Studio создает начальный проект:

![обозреватель решений](razor-pages-start/_static/se.png)

Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.

![Домашняя или индексная страница](razor-pages-start/_static/home.png)

* Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение. В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Localhost обслуживает только веб-запросы с локального компьютера. Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт. На представленном выше снимке экрана используется номер порта 5000. При запуске приложения вы увидите другой номер порта.
* Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде. Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [Далее: добавление модели](xref:tutorials/razor-pages/model)
