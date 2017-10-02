---
title: "Начало работы с Razor Pages в ASP.NET Core"
author: rick-anderson
description: "Начало работы с Razor Pages в ASP.NET Core"
keywords: ASP.NET Core,Razor Pages,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1d8d7805aafbf28fef044d09369a1dc76108b141
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a>Начало работы с Razor Pages в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core. Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:mvc/razor-pages/index). Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.
* Создайте новое веб-приложение ASP.NET Core. Назовите проект **RazorPagesMovie**. Очень важно, чтобы проект имел имя *RazorPagesMovie*, так как пространства имен при копировании и вставке кода должны совпасть.
  ![Новое веб-приложение ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)
* Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.
  ![Веб-приложение (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)

Шаблон Visual Studio создает начальный проект:

![обозреватель решений](razor-pages-start/_static/se.png)

Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.

![Домашняя или индексная страница](razor-pages-start/_static/home.png)

* Visual Studio запускает [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем ваше приложение. В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Localhost обслуживает только веб-запросы с локального компьютера. Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт. На представленном выше снимке экрана используется номер порта 5000. При запуске приложения вы увидите другой номер порта.
* Запуск приложения с помощью клавиш **CTRL+F5** (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде. Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[Далее: добавление модели](xref:tutorials/razor-pages/modelz)
