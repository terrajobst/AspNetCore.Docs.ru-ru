---
title: Начало работы с MVC ASP.NET Core
author: rick-anderson
description: Сведения о начале работы с MVC ASP.NET Core.
ms.author: riande
ms.date: 12/12/2018
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: c09c06f55c4179e9e2174f0063ab7387b7e4c31b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899233"
---
# <a name="get-started-with-aspnet-core-mvc"></a>Начало работы с MVC ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

[!INCLUDE [consider RP](~/includes/razor.md)]

В этом учебнике приводятся основные сведения о веб-приложении MVC ASP.NET Core.

Это приложение служит для управления базой данных названий фильмов. Вы научитесь:

> [!div class="checklist"]
> * Создание веб-приложения.
> * Добавление модели и формирование шаблона.
> * Работа с базой данных.
> * Добавление поиска и проверки.

В конечном итоге вы получите приложение, позволяющее управлять данными фильмов и отображать их.

[!INCLUDE[](~/includes/mvc-intro/download.md)]

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-app"></a>Создание веб-приложения

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В Visual Studio выберите меню **Файл > Создать > Проект**.

![Файл > Создать > Проект](start-mvc/_static/alt_new_project.png)

В диалоговом окне **Создание проекта** выполните следующие действия.

* В левой области выберите **.NET Core**.
* В центральной области выберите **Веб-приложение ASP.NET Core (.NET Core)**.
* Назовите проект "MvcMovie" (имя "MvcMovie" необходимо присвоить для того, чтобы при копировании кода пространства имен совпали).
* Нажмите кнопку **ОК**.

![Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Заполните данные в диалоговом окне **Создание веб-приложения ASP.NET Core (.NET Core) — MvcMovie**.

* Выберите в раскрывающемся списке выбора версии **ASP.NET Core 2.2**.
* Выберите **Веб-приложение (модель — представление — контроллер)**
* Нажмите кнопку **ОК**.

![Диалоговое окно нового проекта, .NET Core в левой области, веб-узел ASP.NET Core ](start-mvc/_static/new_project22-21.png)

В Visual Studio используется только что созданный вами шаблон по умолчанию для проекта MVC. Чтобы приложение стало рабочим, осталось только указать имя проекта и выбрать несколько параметров. Это простой начальный проект и хорошая стартовая точка.

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

Для работы с этим руководством требуется знание VS Code. Дополнительные сведения см. в разделах [Начало работы с VS Code](https://code.visualstudio.com/docs) и [Справка по Visual Studio Code](#visual-studio-code-help).

* Откройте [Интегрированный терминал](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Смените каталог (`cd`) на папку, в которой будет содержаться проект.
* Выполните следующую команду:

   ```console
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * Появится диалоговое окно с предупреждением **В MvcMovie отсутствуют необходимые ресурсы для сборки и отладки. Добавить их?**  Выберите ответ **Да**.

  * `dotnet new mvc -o MvcMovie`: создает новый проект MVC ASP.NET Core в папке *MvcMovie*.
  * `code -r MvcMovie`: загружает файл проекта *MvcMovie.csproj* в Visual Studio Code.

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

* Выберите **Файл** > **Новое решение**.

  ![Новое решение macOS](~/tutorials/first-web-api-mac/_static/sln.png)

* Выберите **Приложение .NET Core** > **ASP.NET Core** > **Веб-приложение ASP.NET Core (MVC)** > **Далее**.

  ![Диалоговое окно "Новый проект" в macOS](~/tutorials/first-mvc-app-mac/start-mvc/1.png)

* В диалоговом окне **Настройка нового веб-API ASP.NET Core** оставьте установленное по умолчанию значение для параметра **Целевая платформа**, то есть **.NET Core 2.2*.

* Присвойте проекту имя **MvcMovie** и нажмите кнопку **Создать**.

---  
<!-- End of VS tabs -->

### <a name="run-the-app"></a>Запуск приложения

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Нажмите **CTRl+F5**, чтобы запустить приложение без отладки.

[!INCLUDE[](~/includes/trustCertVS.md)]

* Visual Studio запускает [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), а затем приложение. Обратите внимание на то, что в адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт.
* Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде. Многие разработчики предпочитают использовать режим без отладки, чтобы быстро запустить приложение и просмотреть изменения.
* Из меню **Отладка** можно запустить приложение в режиме с отладкой или без.

  ![Меню отладки](start-mvc/_static/debug_menu.png)

* Чтобы выполнить отладку приложения, нажмите кнопку **IIS Express**.

  ![IIS Express](start-mvc/_static/iis_express.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code) 

Нажмите клавиши CTRL+F5, чтобы выполнить запуск без отладчика.

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code запускает [Kestrel](xref:fundamentals/servers/kestrel), открывает браузер и переходит к `https://localhost:5001`. В адресной строке указывается `localhost:port:5001`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Localhost обслуживает только веб-запросы с локального компьютера.

  Запуск приложения с помощью клавиш CTRL+F5 (режим без отладки) позволяет внести изменения в код, сохранить файл, обновить браузер и увидеть изменения в коде. Многие разработчики предпочитают использовать режим без отладки, чтобы быстро обновлять страницу и просматривать изменения.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

Выберите **Выполнить** > **Запуск без отладки**, чтобы запустить приложение. Visual Studio для Mac запустит сервер [Kestrel](xref:fundamentals/servers/index#kestrel), откроет браузер и перейдет к `http://localhost:port`, где *port* — это номер порта, выбранный случайным образом.

[!INCLUDE[](~/includes/trustCertMac.md)]

* В адресной строке указывается `localhost:port#`, а не что-либо типа `example.com`. Это связано с тем, что `localhost` — стандартное имя узла для локального компьютера. Когда Visual Studio создает веб-проект, для веб-сервера используется случайный порт. При запуске приложения вы увидите другой номер порта.
* В меню **Запуск** можно запустить приложение в режиме с отладкой или без нее.

------

* Нажмите **Принять**, чтобы согласиться на отслеживание. Это приложение не отслеживает персональные данные. Созданный шаблоном код включает ресурсы для соблюдения [Общего регламента по защите данных (GDPR)](xref:security/gdpr).

  ![Домашняя или индексная страница](start-mvc/_static/privacy.png)

  На следующем рисунке показано приложение после принятия отслеживания:

  ![Домашняя или индексная страница](start-mvc/_static/home2.2.png)

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

В следующей части этого учебника мы поговорим об MVC и приступим к написанию кода.

> [!div class="step-by-step"]
> [Вперед](adding-controller.md)  
